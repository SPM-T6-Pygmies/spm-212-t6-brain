---
title: Schema Migration 02
type: review
sources:
  - "IS212 2026 Discussions Sept 8.md"
  - "Customer Brief no. 1.md"
tags: [review, schema, sql, postgres, migration, ddl]
---

# Schema Migration 02 — close the four divergences the Sept 8 clarifications opened

Runnable SQL for the five structural changes applied to [[schema]] on 2026-09-09. Companion to [[discussions-sept-8-critical-diffs]], which diagnoses the gaps; this page records the fix.

**Status: applied.** Unlike [[schema-migration-01]], which was written as a proposal, this migration is already reflected in [[schema]]'s DDL. This page exists so the delta is reviewable in isolation and re-runnable against any environment still on the pre-migration schema.

Every statement is grounded in a customer answer quoted inline. The items that remain open are listed at the end, **outside** the script — none of them is guessed at here.

## Grounding

| # | Change | Grounded in | Divergence |
|---|---|---|---|
| 1 | `equipment_reservation.return_date` — nullable `date` | [#113](https://github.com/SinYang13/IS212-2026/discussions/113) "There is currently no fixed default return day"; the rule it feeds is [#5](https://github.com/SinYang13/IS212-2026/discussions/5) | D1 |
| 2 | `venue_supported_layout.capacity` — nullable `integer` | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) "Venue capacity varies by room layout… **will be provided** as part of the venue attributes" | D2 |
| 3 | `'Withdrawn'` added to `event_request_status_chk` | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) the withdrawal is "record[ed] in the system" and must "remain distinguishable from draft, submitted, approved, rejected, or other request states" | D3 |
| 4 | `change_request_item` child table | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) "a single change request may cover several fields, and some changes may be accepted while others are not… the relevant reason or follow-up should be communicated" | D4 |
| 5 | `event_request.assigned_coordinator_user_account_id` | [#73](https://github.com/SinYang13/IS212-2026/discussions/73) the manager assigns a coordinator after submission; [#94](https://github.com/SinYang13/IS212-2026/discussions/94) reassignment is the manager's | — |

Change 5 was not one of the four divergences. It closes a gap that predates the Sept 8 batch: [[event-request-workflow]] Step 3 assigns a coordinator *before* Review (Step 4) and Approval (Step 5), but no `event` row exists that early, so nothing recorded who was reviewing a request between `Submitted` and `Approved`.

> [!warning] Change 3 reverses a decision [[schema-migration-01]] made deliberately
> Migration 01's grounding table reads: *"Add `Returned`; **drop `Withdrawn`** from `event_request`… `Withdrawn` appears in the brief only for **registration** withdrawal."* That was correct on the evidence then available, and [[current-schema-scrutiny]] independently flagged the invented `Withdrawn` as critical finding **C3**. [#103](https://github.com/SinYang13/IS212-2026/discussions/103) supplied the grounding that was missing. This is not a defect in migration 01 — it is the customer answering a question that had not been asked yet. Recorded here so a reader diffing the two migrations does not conclude one of them is wrong.

## Script

```sql
begin;

-- ---------------------------------------------------------------------------
-- 1. D1 — equipment_reservation.return_date  (#113, feeds #5's Return Day + 1)
--    Nullable by necessity: the customer states there is no fixed default,
--    so a not-null column would force an invented value on every row.
-- ---------------------------------------------------------------------------
alter table equipment_reservation
  add column if not exists return_date date;

-- ---------------------------------------------------------------------------
-- 2. D2 — venue_supported_layout.capacity  (#112)
--    Supplied per layout as master data, never estimated by the system.
--    venue.capacity is intentionally left in place — see Open items.
-- ---------------------------------------------------------------------------
alter table venue_supported_layout
  add column if not exists capacity integer;

alter table venue_supported_layout
  drop constraint if exists venue_supported_layout_capacity_chk;
alter table venue_supported_layout
  add constraint venue_supported_layout_capacity_chk
  check (capacity is null or capacity >= 0);

-- ---------------------------------------------------------------------------
-- 3. D3 — event_request gains a seventh state, 'Withdrawn'  (#103)
--    Coordinator-actioned. The Organiser asks outside the system; only the
--    coordinator performs and records the withdrawal.
-- ---------------------------------------------------------------------------
alter table event_request
  drop constraint if exists event_request_status_chk;
alter table event_request
  add constraint event_request_status_chk
  check (status in ('Draft', 'Submitted', 'Under Review',
                    'Approved', 'Rejected', 'Returned', 'Withdrawn'));

-- ---------------------------------------------------------------------------
-- 4. D4 — per-field change outcomes  (#104)
--    change_request's flat significance_flag / fields_affected /
--    coordinator_decision columns are retained but cease to be authoritative
--    once a request has item rows. See Open items.
-- ---------------------------------------------------------------------------
create table if not exists change_request_item (
  change_request_item_id bigint generated always as identity primary key,
  change_request_id      bigint not null
                           references change_request (change_request_id)
                           on delete cascade,
  field_name             text not null,
  requested_value        text,
  significance_flag      boolean not null default false,
  decision               text not null default 'Pending',
  decision_reason        text,
  created_at             timestamptz not null default now(),
  updated_at             timestamptz not null default now(),
  constraint change_request_item_decision_chk
    check (decision in ('Pending', 'Accepted', 'Rejected')),
  -- #104: "where a change is not accepted, the relevant reason or follow-up
  -- should be communicated" — enforced, not left to the application.
  constraint change_request_item_reason_chk
    check (decision <> 'Rejected' or decision_reason is not null)
);

create index if not exists change_request_item_change_request_idx
  on change_request_item (change_request_id);

-- ---------------------------------------------------------------------------
-- 5. Coordinator assignment on the request  (#73 assign, #94 reassign)
--    Frozen once the request leaves review; event carries the live value.
-- ---------------------------------------------------------------------------
alter table event_request
  add column if not exists assigned_coordinator_user_account_id bigint
    references user_account (user_account_id) on delete restrict;

create index if not exists event_request_coordinator_idx
  on event_request (assigned_coordinator_user_account_id);

-- ---------------------------------------------------------------------------
-- 6. Housekeeping for the new table: updated_at trigger + RLS
-- ---------------------------------------------------------------------------
create trigger change_request_item_set_updated_at
  before update on change_request_item
  for each row execute function set_updated_at();

alter table change_request_item enable row level security;

commit;
```

## Verification

Static checks only — no Postgres server is available here, so **this script was not executed** and this page does not claim it runs. What was verified against [[schema]]'s post-migration DDL:

| Check | Result |
|---|---|
| `create table` count | 26 (was 25) |
| Tables listed in the RLS loop | 26 — `change_request_item` present |
| Tables listed in the `updated_at` trigger loop | includes `change_request_item` |
| FK indexes for every new FK | `change_request_item_change_request_idx`, `event_request_coordinator_idx` — both present |
| Every status column's default satisfies its own `CHECK` | `change_request_item.decision` defaults `'Pending'`, which is in its check list |
| `event_request.status` default | `'Draft'` — still in the widened seven-value list |

## Open items — deliberately not in the script

These need a decision, not SQL. Guessing at them here is what [[schema-migration-01]] avoided and this migration avoids too.

| # | Open item | Why it is not resolved here |
|---|---|---|
| 1 | **What populates `return_date`?** | [#113](https://github.com/SinYang13/IS212-2026/discussions/113) states there is no fixed default and leaves capture — auto-populated or manual — to the team. Storage is now solved; population is not. Feature 16 returns an undefined availability answer for any reservation whose return date is null |
| 2 | **What is `venue.capacity` for, now?** | The scalar was retained alongside the per-layout figure, so capacity lives in two places with **no stated precedence**. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) says suitability reads the layout-specific value; whether the scalar becomes derived (max across layouts) or is dropped is undecided. Until then, treat `venue.capacity` as untrusted |
| 3 | **Which booking a layout applies to** | `booking` records no chosen room layout. The system therefore knows every venue's per-layout capacities but not which layout a given booking assumes — so change 2 cannot yet be used to validate a booking, only to filter a search |
| 4 | **The dual representation on `change_request`** | The flat columns still exist beside `change_request_item`. Nothing in the schema enforces which wins, so an implementer can write to one and read the other. A follow-up should either drop the flat columns or add a constraint |
| 5 | **From which states is withdrawal reachable?** | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) names only `Under review`; the question that prompted it said "before it is reviewed". The check constraint permits the value but no transition rule is encoded — as is already true of every other status on this schema |

## Related pages

- [[schema]] — the post-migration physical model, and the authority for it
- [[discussions-sept-8-critical-diffs]] — why D1–D4 existed
- [[schema-migration-01]] — the previous migration, whose `Withdrawn` decision change 3 reverses
- [[current-schema-scrutiny]] — the defect review that produced migration 01
- [[event-request]], [[change-request]], [[equipment-reservation]], [[venue]] — the domain pages that own these attributes
