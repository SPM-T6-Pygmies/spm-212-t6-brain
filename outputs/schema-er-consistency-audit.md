---
title: Schema and ER Diagram Consistency Audit
type: review
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "Week 4 - Customer Briefing.md"
tags: [review, schema, ddl, er-diagram, consistency, lint]
---

# Schema and ER Diagram Consistency Audit

A scrutiny pass over [[schema]]'s 26-table DDL and [[domain-model]]'s two ER diagrams, checked against the fourteen domain-object pages, the sixteen concept pages, the migration artefacts and the raw sources. Dated 2026-09-09.

**The DDL is mechanically sound.** 26 `create table` against a 26-entry RLS loop; every index resolves to a real table and column; every `default` satisfies its own `CHECK`; the `updated_at` trigger list matches exactly the tables carrying that column. Nothing here is a broken script.

**The defects are all at the boundary between the model and the wiki that is supposed to govern it.** They fall into one pattern: the Sept 8 batch was absorbed by the concept and domain layers but only partially by the physical layer, and several structures predating that batch were never reconciled at all. 44 discussion answers cited by domain-object pages appear in *neither* [[schema]] nor [[domain-model]] — including [#99](https://github.com/SinYang13/IS212-2026/discussions/99), [#109](https://github.com/SinYang13/IS212-2026/discussions/109), [#110](https://github.com/SinYang13/IS212-2026/discussions/110), [#115](https://github.com/SinYang13/IS212-2026/discussions/115), [#120](https://github.com/SinYang13/IS212-2026/discussions/120) and [#121](https://github.com/SinYang13/IS212-2026/discussions/121).

> [!warning] Six status enumerations contain values no source supports
> Across `equipment_item`, `equipment_reservation`, `booking`, `registration`, `notification` and `waiting_list_entry`, the DDL enumerates values that appear on no domain page, and omits values that pages mark `✅ Specified`. Because [[schema]] declares itself "source of truth for … status enumerations", these silently overrule the pages that hold the citations.

## Critical

### C1 — `equipment_item.operational_status` contradicts its only source

| Side | Value set |
|---|---|
| [[equipment-item]], `✅ Specified`, [#23](https://github.com/SinYang13/IS212-2026/discussions/23) | available, unavailable, damaged, under maintenance |
| `equipment_item_operational_status_chk` | Available, **Reserved**, **In Use**, Maintenance, **Defective**, **Retired** |

`Unavailable` — named on three pages — is absent. `Damaged` was renamed `Defective` and `Under maintenance` to `Maintenance` with no note. `Reserved`, `In Use` and `Retired` are inventions, and two of them collapse a distinction [[equipment-item]] states twice: operational status "is a different concept from an item being transiently unavailable because it is already committed to another event's D-1 → Return Day + 1 window". `Reserved`/`In Use` put the commitment window into the very column the page says does not hold it. Corroborated by [[equipment-management]] and [[technical-support]].

### C2 — `registration.status` re-conflates what the brief requires separable

`registration_status_chk` admits `Cancelled`, `Attended` and `No Show`. None appears in [[registration]] or [[attendee-registration]] — the States table has exactly three rows (Registered, Waitlisted, Withdrawn). `Attended`/`No Show` are worse than ungrounded: [[registration]] states twice that "Registration status and attendance information must be kept distinguishable" ([[event-planning-venue-booking-system|brief §4]]), and the DDL *already* models attendance separately via `checked_in_at`/`checked_in_method`. Attendance now has two independently writable homes.

### C3 — The capacity hard cap is neither enforced nor computable

[#99](https://github.com/SinYang13/IS212-2026/discussions/99) is one of the register's non-negotiables: VIPs may be added manually to a full event, "but the overall event/venue capacity must still not be exceeded". Nothing in the DDL counts live registrations against `event.event_capacity` (nullable) or `session.attendance_limit`, and it is absent from [[schema]]'s "Constraints that are not declarative" list.

The *venue* half cannot be computed at all. `event` has no `venue_id`; venue is reachable only through `booking`, which may be `Rejected`/`Released`/`Cancelled`; per-layout capacity lives on `venue_supported_layout.capacity`; and no booking records which layout it assumes. So a registration-time check has no path to the layout figure and can only reach the scalar `venue.capacity` — the field [[venue-suitability]] calls "the wrong field". **The registration cap and the suitability check would read two different, independently writable numbers.**

### C4 — `equipment_reservation` can compute one end of the availability window, not both

D1 added `return_date`, closing the Return Day + 1 end. The **D-1 collection end has no anchor**: there is no collection date, event date or window-start column on the reservation or its lines, and the only route to D is a join to `event.preferred_date`/`event.start_time` or `session.start_time` — all nullable, with `equipment_reservation_scope_chk` permitting `event_id` to be null.

[[schema]]'s own D1 row names all three gaps ("no return, collection or period"), records the fix as `return_date` alone, and leaves the impression feature 16 is now answerable. [[equipment-management]] inherits it: "the rule is representable and feature 16 has something to compute from". It needs both ends. Relatedly, "no same-day sharing" ([#5](https://github.com/SinYang13/IS212-2026/discussions/5)) has no exclusion constraint and is missing from the non-declarative register.

### C5 — The guest-registration uniqueness index rests on a premise its own DDL falsifies

```
-- 'Registered' rather than the broader exclusion above: Release 1 has no
-- waiting list and no check-in, so it is the only live state this path can
-- produce. Widen it when waitlisting lands.
```

The same file ships `'Waitlisted'` in `registration_status_chk`, the whole `waiting_list_entry` table with its five-state machine, and `checked_in_at`/`checked_in_method`. Concretely: a guest can hold a `Waitlisted` row **and** a `Registered` row for one event, or two `Waitlisted` rows, and nothing blocks it.

[[registration]] then describes a symmetry that does not exist — "enforced on the account id and, for guests, on the lower-cased email":

| | account-holder index | guest index |
|---|---|---|
| statuses treated as live | `not in ('Cancelled','Withdrawn')` | `= 'Registered'` only |
| session-scoped variant | yes | **none** |

### C6 — `equipment_reservation.status` cannot store the re-check the customer requires

[[equipment-reservation]] names five states; the DDL enumerates six, overlapping on three.

- **`Under review` is absent.** It is the target of `Reserved → Under review`, the edge required by [#16](https://github.com/SinYang13/IS212-2026/discussions/16) and [#114](https://github.com/SinYang13/IS212-2026/discussions/114) (equipment requirements may change after reservation; TSS re-checks). The DDL cannot record a reservation sent back for re-check.
- **`Recorded` is absent**, replaced by `Requested` as the default — contradicting [[equipment-reservation]]'s opening line ("there is **no formal request → approval workflow**", [#19](https://github.com/SinYang13/IS212-2026/discussions/19)) and the DDL's own comment three lines above it.
- `Partially Reserved` re-introduces reservation-level fulfilment that the page explicitly rejects in favour of per-line judgement ([#90](https://github.com/SinYang13/IS212-2026/discussions/90)), duplicating `equipment_reservation_line.fulfilment_status`.

## High

### H1 — Both ER diagrams are missing the coordinator edge they are recorded as having

[[domain-model]]'s callout and `log.md` both state the diagram was "**re-drawn**" for the 2026-09-09 migration. The *column* `event_request.assigned_coordinator_user_account_id` was added to the `EVENT_REQUEST` block; the *edge* was not added to either diagram. Diffing all 50 FKs against both diagrams, this is the only shortfall in the relational one:

```
user_account <- event_request: 2 FK cols but 1 edge
```

`EVENT` correctly carries two labelled edges. Given [[schema]] devotes a section to "Coordinator assignment spans both tables", the diagrams contradict it.

### H2 — Session ↔ Booking is drawn three ways, and all three overstate it

| Source | Says |
|---|---|
| Conceptual diagram | `SESSION \|\|--\|\| BOOKING` |
| Relational diagram | `SESSION \|o--\|\| BOOKING` |
| [[booking]] | `1 : 1 (own)` |
| [[venue]] | `1 : many (via booking)` |
| The DDL | nullable `session_id`, no unique index → 0..N |

[#109](https://github.com/SinYang13/IS212-2026/discussions/109) settles it against the strict readings: "Only one final venue booking should normally be confirmed for the same event/session, **unless the event requires multiple venues**." [[booking]] also concedes elsewhere that "an event may go through several bookings over its life", which its own `1 : 1` forbids. The DDL is right; the two diagrams and [[booking]]'s relationship row are wrong.

### H3 — `booking.status` carries an ungrounded sixth value

The DDL enumerates `Cancelled`; [[booking]] names five states and routes cancellation to `Released` ("a confirmed booking was cancelled/postponed"). `Cancelled` is redundant with `Released` and sourced nowhere.

### H4 — Venue per-slot availability has no table, so a maintenance block cannot be recorded

[[venue]] lists "Availability (per slot)" as `✅ Specified` with five values including `Blocked`, and [[venue-booking]] states "**Blocking is at slot granularity**" ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)). There is no venue-slot table: `booking_slot` is a child of `booking` and carries no `venue_id`, and a block cannot be stored as a booking because `booking_scope_chk` demands an `event_id` or `session_id` that a maintenance block does not have.

### H5 — "Two rules … this DDL cannot express" — there are at least eight

The section presents itself as exhaustive. Missing from it: the capacity cap (C3); no same-day equipment sharing (C4); only the assigned coordinator may cancel ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)); no direct Organiser edit after submission ([#102](https://github.com/SinYang13/IS212-2026/discussions/102)); `Withdrawn` reachable only from `Under review` ([#103](https://github.com/SinYang13/IS212-2026/discussions/103)); auto-completion at end date + 7 days ([#71](https://github.com/SinYang13/IS212-2026/discussions/71)); and the padded-occupancy refinement of the double-booking rule below.

### H6 — Effective occupancy is not materialised, so the proposed double-booking index under-blocks

[[booking]] and [[venue-booking]] both state that occupancy is the published window **plus** setup and turnaround padding, and that "conflict detection must reason about this padded window". The proposed index keys on `booking_slot` rows only, and nothing requires padding slots to exist as rows. Compounding it, [#108](https://github.com/SinYang13/IS212-2026/discussions/108) fixes one slot as the smallest unit of setup and turnaround, but the DDL stores `setup_time_minutes`/`turnaround_time_minutes` as free integers — nothing forbids `45`, which the rule prohibits for Release 1.

### H7 — `booking_decider_chk` makes a coordinator-placed hold unstorable

`check (status = 'Requested' or decided_by_user_account_id is not null)` requires a Venue Staff decider for every non-`Requested` row. But [[booking]] has the *coordinator* place a tentative hold, and `Released` is also reachable by hold expiry — neither has a decider. [[booking]] scopes the field precisely to "the Venue Staff who approved or rejected the request", i.e. `Confirmed` and `Rejected` only. The constraint is one predicate too broad.

### H8 — `schema-migration-01` argues against the value `schema` now ships

[[schema-migration-01]] has a titled section — "**keep `Awaiting Arrangements`, do not use `Blocked`**" — and ships `check (status in ('Planning', 'Awaiting Arrangements', …))`. [[schema]] ships `Blocked`. Migration 02 never touches `event_status_chk`, so **the two migrations cannot produce the current DDL**, and migration 01 still carries the original argument unamended while migration 02's reversal callout covers only the `Withdrawn` reversal.

### H9 — The migration pages do not account for the deployed DDL

- **In migration 01, absent from `schema`:** the entire double-booking implementation — `booking_slot.venue_id`/`booking_status`, two sync functions, two triggers, and `booking_slot_no_double_booking_uidx`. [[schema]] keeps that index only as a comment.
- **In `schema`, created by neither migration:** the whole attendee-identity block (`registration_reference`, `attendee_name`/`attendee_email`, the XOR check, the email index). [[schema]] attributes it to `migrations/20260907132238_attendee_registration_identity.sql`, a third migration no page documents and no artefact links.

### H10 — "nine values in total" became ten, in the same migration that documents it

D4's `change_request_item.decision` added `Accepted`, already used by `waiting_list_entry`. Counted directly: pre-D4 = 9, now = 10 (`Accepted`, `Cancelled`, `Confirmed`, `Pending`, `Rejected`, `Released`, `Requested`, `Reserved`, `Returned`, `Withdrawn`). The prose was not updated by the migration that invalidated it.

## Medium

| # | Finding |
|---|---|
| M1 | **Two cited raw sources do not exist.** `raw/current schema.md` (the subject of [[current-schema-scrutiny]]) and `raw/original script.md` (from which [[schema-migration-01]] restores 48 `ON DELETE` clauses "verbatim") are both absent. `raw/` is the immutable source of truth, so the provenance chain behind "the applied physical model" is broken. |
| M2 | **Stale counts.** `index.md` still calls [[schema]] a "25-table" model — in the catalog row *for that page*. `index.md` and [[current-schema-scrutiny]] both say "the fifteen `wiki/domain/` pages"; there are 16, and `index.md` says so two lines earlier. [[open-questions]] says "all fifteen domain objects" against [[domain-model]]'s fourteen. |
| M3 | **[[event]]'s `✅ Specified` "Outstanding actions" has no column anywhere**, and `preferred_time` exists on `event_request` but not `event` — contradicting [[schema]]'s own claim that the request/event split is symmetric across `preferred_date`, `expected_attendance`, `venue_requirements`, `equipment_requirements` and `accessibility_needs`. |
| M4 | **`audit_record.actor_user_account_id` is nullable with `on delete set null`**, against [[audit-record]]'s "Every entry has an actor" and "must capture what changed, **who** changed it, and when". Deleting a user erases the "who" from existing rows. The sibling `audit_record_kind_chk` shows this is not a general policy of leaving audit fields loose. |
| M5 | **Notification is the least reconciled table.** `Queued` — [[notification]]'s state for batched delivery — is absent, so batch delivery (the customer's own first answer, [#55](https://github.com/SinYang13/IS212-2026/discussions/55)) is unrepresentable; `Read` is modelled although the page says read-tracking "is **not** modelled here"; `channel` has no `CHECK`, cannot express "both" ([#37](https://github.com/SinYang13/IS212-2026/discussions/37)), and its global `default 'email'` is precisely the mass-configuration [#37](https://github.com/SinYang13/IS212-2026/discussions/37) rules out; "Delivery timing" (`✅ Specified`) has no column. |
| M6 | **`booking_scope_chk` and `equipment_reservation_scope_chk` are OR where every page states XOR**, and neither requires `session.event_id = booking.event_id` — a booking may name event A and a session of event B. |
| M7 | **`equipment_item.transfer_time_minutes` is a per-item constant** that cannot vary by destination, though [[equipment-item]] defines it as time "from its current location to the event venue". The page lists three candidate representations; the DDL silently picks an unlisted fourth that cannot compute the quantity the rule defines. |
| M8 | **Pooled `quantity` plus a single `operational_status` cannot express partial maintenance** — three of ten projectors under repair is inexpressible, so available quantity is underivable. That defeats the one part of [#13](https://github.com/SinYang13/IS212-2026/discussions/13) that is *not* open ("the system needs to know the quantity of equipment available"). |
| M9 | **`venue.booking_horizon_days` invents a per-venue, day-granular horizon** for a rule [[venue-booking]] records as "**No booking horizon has been specified**" ([#57](https://github.com/SinYang13/IS212-2026/discussions/57)), enforces nothing, and is not listed among the structures [[schema]] flags as encoding an open decision. |
| M10 | **`event.technical_support_flag`/`technical_support_description` duplicate `support_request.support_needed_flag`/`description`**, both claimed by [[support-request]] as its own `✅ Specified` attributes, with no precedence stated. |
| M11 | **Indefinite postponement has no representable state.** [#121](https://github.com/SinYang13/IS212-2026/discussions/121) confirms an event may be postponed with no new date. [[event]] and [[event-lifecycle]] both flag that the five `event_status_chk` values contain nothing for it; [[schema]] — the page a reader consults for the status enumerations — is silent. |
| M12 | **[[spm-24-implementation-decisions]]' deviations are unabsorbed and recorded on one side only:** `uuid` PKs vs `bigint identity`; lowercase status values vs Title Case; `timestamptz` registration window vs `date`; plural `registrations` vs singular; and the open XOR-vs-always-present attendee identity. [[schema]] carries no reciprocal note for any of them. |
| M13 | **Three of [[schema-migration-02]]'s five open items were silently dropped** from both [[schema]] and [[domain-model]]: which booking a layout applies to (alive in [[domain-model]] prose only), from which states withdrawal is reachable (filed in [[domain-model]]'s *settled* register), and what populates `return_date` (no register row). Worse, the `change_request` dual representation is **stated as settled** in [[schema]] — "no longer authoritative once a request has item rows" — when migration 02's entire point is that nothing enforces it. |
| M14 | **The 3-day acceptance window ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)) is two unconstrained nullable timestamps.** No default, no `CHECK`, and nothing requires them non-null at `Offered`. `waiting_list_entry` also has no `created_at` and no uniqueness scope on `position`, so the FIFO ordering [[registration]] calls structurally necessary is unenforceable. Separately, `waiting_list_entry.Withdrawn` resolves an open team decision [[registration]] still records as open. |

## Low and lint

- **Seven bare `§N` citations outside code fences in [[domain-model]]** (lines 450, 457×2, 458×3, 533). Per `CLAUDE.md` these variant spellings must be normalised to a linked `brief §N`. Three sit in prose columns whose *Ref* column on the same row is correctly linked. [[schema]] is clean.
- **The relational ER blocks omit timestamp columns inconsistently** — `EVENT_REQUEST`/`EVENT`/`BOOKING` show `created_at` but not `updated_at`; `SESSION`/`VENUE`/`EQUIPMENT_ITEM` show neither; `SUPPORT_REQUEST_ASSIGNMENT` omits `assigned_at`. No stated rule covers it.
- **Two self-referencing FKs are undrawn** — `event.reused_from_event_id` and `event_comment.parent_comment_id` — though both have their own rows in the *Schema design decisions* table, and `venue.suggested_alternative_venue_id` **is** drawn.
- **`CHANGE_REQUEST_ITEM` is declared before `CHANGE_REQUEST`** in the relational diagram — the only child-before-parent block.
- **[[current-schema-scrutiny]] uses [[supabase-schema-ddl]] as its live reference standard** at eight points, including an actionable "Restore … from [[supabase-schema-ddl]]", with no supersession banner on the page itself.
- **[[schema]] carries no release-scope marker and no equivalent disclaimer**, though it ships `📋 Backlog` structures (waiting list, check-in). [[domain-model]] handles this explicitly ("It carries no scope marker of its own"); [[schema]] does not.
- **`registration.checked_in_method` and `trigger_scenario` have no `CHECK`** despite closed enumerations on their pages, and no `checked_in_by_user_account_id` exists, so the schema cannot say who performed a check-in. [[schema]]'s claim that the six notification FKs let "the DB enforce integrity" overclaims: nothing requires at least one to be set.

## Checked and dismissed

- **`booking_slot_value_chk check (slot in ('AM','PM','Night'))` is not a violation of [[venue]]'s "do not hard-code the missing labels".** The customer named the three slots in the opening sentence of [#50](https://github.com/SinYang13/IS212-2026/discussions/50); what is inferred is which *hour range* maps to PM and which to Night. The DDL stores a label and no hours, so it does not encode the inferred mapping. `venue.operating_hours_start`/`_end` are venue-level and unrelated.
- **`client_organisation`'s missing primary-contact and administrator columns** are correctly absent — both are `🔶 Team decision` ([#65](https://github.com/SinYang13/IS212-2026/discussions/65), [#81](https://github.com/SinYang13/IS212-2026/discussions/81)) and neither role is confirmed to exist.
- **`support_request` has no status column**, matching [[support-request]]'s refusal to propose a lifecycle no source names.
- **`venue.capacity`**: no page in the venue or equipment cluster still treats the scalar as authoritative — the demotion propagated cleanly. The residual exposure is C3, via registration.

## Recommended order

1. **C3, C4, C6** — committed customer rules the model cannot answer. These block Release 1 features 16, 17 and 18.
2. **C1, C2, H3** — remove or ground the invented enum values before anything is built against them; each one currently overrules a cited page.
3. **C5, H7, M6** — constraints that are wrong rather than missing, so they will silently admit or reject the wrong rows.
4. **H1, H2** — re-draw the two diagram defects and reconcile [[booking]]'s `1 : 1` against [#109](https://github.com/SinYang13/IS212-2026/discussions/109).
5. **H5, H8, H9, H10, M13** — repair the provenance and self-description layer, so the next migration starts from an accurate account of this one.
6. **M1, M2 and the lint items** — cheap, and M1 is the one that undermines every claim of "verified against the deployed schema".

See [[schema]] for the DDL under review, [[domain-model]] for the diagrams, and [[discussions-sept-8-critical-diffs]] for the ingestion report that produced D1–D4.
