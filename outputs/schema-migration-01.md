---
title: Schema Migration 01
type: review
sources:
  - "current schema.md"
  - "original script.md"
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [review, schema, sql, postgres, migration, ddl]
---

# Schema Migration 01 — restore the original script, correct the status model

Runnable SQL taking `raw/current schema.md` back to `raw/original script.md`'s constraint set, plus the status-model corrections that document could not have made because its own status lists were wrong. Companion to [[current-schema-scrutiny]], which diagnoses the defects; this page fixes them.

Every statement here is either **(a)** a verbatim restoration of something `raw/original script.md` already had, or **(b)** grounded in a customer statement quoted inline. Nothing is my inference — the items that need a team decision are listed at the end, outside the script.

## The naming question: keep `Awaiting Arrangements`, do not use `Blocked`

Short answer: **no.** Two grounded reasons.

**1. `awaiting arrangements` is the customer's own word.** [[event-planning-venue-booking-system|brief §4]], *Event Status Management*, names the statuses outright:

> [!quote] Customer wording — [[event-planning-venue-booking-system|brief §4]]
> Events progress through suitable statuses so that users can understand their current stage, for example draft, submitted, under review, approved, planning, **awaiting arrangements**, confirmed, completed, cancelled, or rejected.

**2. `blocked` is already taken, and means something else.** The same §4 uses it for *venue availability*:

> [!quote] Customer wording — [[event-planning-venue-booking-system|brief §4]]
> Venue Availability Calendar — Authorised users can view when venues are **available, tentatively held, confirmed, blocked, or otherwise unavailable**.

Reusing `Blocked` as an event status would collide with an established term for a venue-calendar state inside the same system. A coordinator reading "blocked" would have to ask which kind. The customer supplied a distinct word for each; keep them distinct.

Worth noting §4 says "suitable statuses" and "for example", so the list is *proposable*, not binding — the team may add or rename. But `Blocked` is the one rename with an active cost, and `Awaiting Arrangements` is the one name that arrives pre-approved.

## Grounding of each recommendation

| # | Change | Grounded in | In script |
|---|---|---|---|
| 1 | Split the ten §4 statuses across `event_request` and `event`, one table each | [[event-planning-venue-booking-system\|brief §4]] status list; [[event-request-workflow\|brief §5]] "An event request is approved, rejected, or returned"; §4 *Draft Event Requests* ("save an **event request** before submission") | ✅ Part 1 |
| 2 | Add `Planning`, `Awaiting Arrangements`; drop `In Progress` | §4 names both; `In Progress` occurs in no source | ✅ Part 1 |
| 3 | Add `Returned`; drop `Withdrawn` from `event_request` | [[event-request-workflow\|brief §5]] step 5 names *returned*; `Withdrawn` appears in the brief only for **registration** withdrawal | ✅ Part 1 |
| 4 | Fix the malformed `event.status` default | It violates its own `CHECK`; the table is uninsertable | ✅ Part 1 |
| 5 | Restore 48 `on delete` clauses | Verbatim from `raw/original script.md` (49 occurrences) | ✅ Part 2 |
| 6 | Restore 12 dropped constraints and 3 table-level uniques | Verbatim from `raw/original script.md` | ✅ Part 3 |
| 7 | Enforce the double-booking hard block | [#35](https://github.com/SinYang13/IS212-2026/discussions/35) "blocked outright, not warned"; [#41](https://github.com/SinYang13/IS212-2026/discussions/41) one active hold/confirmed per venue and overlapping period. The original marked options (a)/(b) in a comment and picked neither — this takes (a), as that comment recommended | ✅ Part 4 |
| 8 | Restore 41 FK indexes, `updated_at` triggers, RLS | Verbatim from `raw/original script.md` | ✅ Part 5 |
| — | De-duplicate technical support / `decision_record` | No customer statement assigns an owner | ❌ team decision |
| — | Guest registration; split attendance out of `registration.status` | 🔶 [#22](https://github.com/SinYang13/IS212-2026/discussions/22) is open; the attendance split is required by §4 but its *shape* is not stated | ❌ team decision |
| — | Completion tracking on `event_essential_arrangement` | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) requires the gate, but names no storage shape | ❌ team decision |

### How the ten statuses divide

All ten of §4's example statuses survive, each on **exactly one** table, plus `Returned` from §5:

| Status | Goes to | Why that table |
|---|---|---|
| Draft | `event_request` | §4 *Draft Event Requests*: "save an **event request** before submission" |
| Submitted, Under Review | `event_request` | pre-decision stages of the review in [[event-request-workflow\|brief §5]] |
| Approved, Rejected, Returned | `event_request` | §5: "An event **request** is approved, rejected, or returned" |
| Planning, Awaiting Arrangements | `event` | §4 status list; the [#80](https://github.com/SinYang13/IS212-2026/discussions/80) gate lives between them |
| Confirmed, Completed | `event` | §4 status list; [#71](https://github.com/SinYang13/IS212-2026/discussions/71) completion |
| Cancelled | `event` | §4 status list; [#68](https://github.com/SinYang13/IS212-2026/discussions/68) coordinator-only, any stage |

> [!info] This settles W1, it does not dodge it
> [[current-schema-scrutiny]] flagged that [[event]]'s state machine reproduces the pre-approval states. It does so because §4's list genuinely puts all ten on the *event*. But §5 just as plainly attaches approve/reject/return to the *request*, and §4's *Draft Event Requests* row says "event request" in as many words. The allocation above is the only one that leaves every customer-named status somewhere and duplicates none. [[event]] and [[event-request]] should be updated to match **before** this migration is run.

---

## Part 0 — Preflight (read-only, run first)

Run this before anything else. Parts 1 and 3 will fail on non-conforming rows, and you want to know that now rather than mid-transaction.

```sql
-- Rows whose status must be remapped by Part 1
select 'event: needs remap' as check_name, status, count(*)
  from public.event
 where status in ('Draft', 'Approved', 'In Progress')
 group by status
union all
select 'event: unmappable', status, count(*)
  from public.event
 where status not in ('Draft','Approved','In Progress','Confirmed','Completed','Cancelled')
 group by status
union all
select 'event_request: UNMAPPABLE — team must decide', status, count(*)
  from public.event_request
 where status = 'Withdrawn'
 group by status;

-- Rows that would violate the constraints Part 3 restores. All must return 0.
select 'event.end<=start'            as violation, count(*) from public.event                      where end_time  <= start_time
union all select 'event.reg window',   count(*) from public.event                      where registration_close_date < registration_open_date
union all select 'event.self reuse',   count(*) from public.event                      where reused_from_event_id = event_id
union all select 'comment self parent',count(*) from public.event_comment              where parent_comment_id = comment_id
union all select 'session dup seq',    count(*) from (select event_id, sequence_no from public.session group by 1,2 having count(*)>1) d
union all select 'session.end<=start', count(*) from public.session                    where end_time <= start_time
union all select 'booking no scope',   count(*) from public.booking                    where event_id is null and session_id is null
union all select 'booking no decider', count(*) from public.booking                    where status <> 'Requested' and decided_by_user_account_id is null
union all select 'slot dup',           count(*) from (select booking_id, slot_date, slot from public.booking_slot group by 1,2,3 having count(*)>1) d
union all select 'eqres no scope',     count(*) from public.equipment_reservation      where event_id is null and session_id is null
union all select 'eqline dup item',    count(*) from (select equipment_reservation_id, equipment_item_id from public.equipment_reservation_line group by 1,2 having count(*)>1) d
union all select 'eqline over-reserve',count(*) from public.equipment_reservation_line where quantity_reserved > quantity_requested
union all select 'reg checkin skew',   count(*) from public.registration               where (checked_in_at is null) <> (checked_in_method is null)
union all select 'notif sent no ts',   count(*) from public.notification               where status = 'Sent' and sent_at is null
union all select 'audit empty',        count(*) from public.audit_record               where action is null and field_changed is null
union all select 'DOUBLE BOOKINGS',    count(*) from (
    select b.venue_id, s.slot_date, s.slot
      from public.booking_slot s join public.booking b using (booking_id)
     where b.status in ('Tentative Hold','Confirmed')
     group by 1,2,3 having count(*) > 1) d;
```

> [!warning] `event_request.status = 'Withdrawn'` has no grounded target
> `Withdrawn` is not a customer-named request state, so there is no source-backed value to map it to. If the preflight returns any such rows, the team must decide what they meant (most likely `Rejected`, possibly a genuinely new state the customer should be asked about) **before** running Part 1. The script below deliberately does not guess — it aborts.

## Part 1 — Status model

```sql
begin;

-- --------------------------------------------------------------------------
-- 1a. Abort rather than guess at ungrounded data (see warning above).
-- --------------------------------------------------------------------------
do $$
declare n bigint;
begin
  select count(*) into n from public.event_request where status = 'Withdrawn';
  if n > 0 then
    raise exception
      'Migration aborted: % event_request row(s) have status ''Withdrawn'', which is not a customer-named state and has no grounded target. Decide the mapping, then remove this guard.', n;
  end if;
end;
$$;

-- --------------------------------------------------------------------------
-- 1b. event_request: drop the invented 'Withdrawn', restore §5's 'Returned'.
-- --------------------------------------------------------------------------
alter table public.event_request drop constraint if exists event_request_status_chk;
alter table public.event_request add  constraint event_request_status_chk
  check (status in ('Draft', 'Submitted', 'Under Review', 'Approved', 'Rejected', 'Returned'));

-- --------------------------------------------------------------------------
-- 1c. event: remap pre-approval and invented values onto the post-approval
--     lifecycle. An `event` row only exists because a request was approved,
--     so the earliest state it can legitimately hold is 'Planning'.
--     'In Progress' -> 'Confirmed': a running event stays Confirmed until
--     Completed, and §4 names no in-flight state.
-- --------------------------------------------------------------------------
update public.event set status = 'Planning'  where status in ('Draft', 'Approved');
update public.event set status = 'Confirmed' where status = 'In Progress';

alter table public.event drop constraint if exists event_status_chk;

-- Fix the malformed default. It was '''Planning''::text'::text — a literal
-- string containing quote marks, which violated the CHECK, so every INSERT
-- omitting status failed.
alter table public.event alter column status set default 'Planning';

alter table public.event add constraint event_status_chk
  check (status in ('Planning', 'Awaiting Arrangements', 'Confirmed', 'Completed', 'Cancelled'));

commit;
```

## Part 2 — Restore the 48 `on delete` clauses

Generated by matching every `CONSTRAINT ... FOREIGN KEY` in `raw/current schema.md` against the same column's action in `raw/original script.md`. All 48 matched; none invented.

```sql
begin;

alter table public.user_account drop constraint if exists user_account_client_organisation_id_fkey;
alter table public.user_account add  constraint user_account_client_organisation_id_fkey
  foreign key (client_organisation_id) references public.client_organisation (client_organisation_id) on delete restrict;
alter table public.user_account_role drop constraint if exists user_account_role_user_account_id_fkey;
alter table public.user_account_role add  constraint user_account_role_user_account_id_fkey
  foreign key (user_account_id) references public.user_account (user_account_id) on delete cascade;
alter table public.user_account_role drop constraint if exists user_account_role_role_id_fkey;
alter table public.user_account_role add  constraint user_account_role_role_id_fkey
  foreign key (role_id) references public.role (role_id) on delete restrict;
alter table public.event_request drop constraint if exists event_request_requesting_user_account_id_fkey;
alter table public.event_request add  constraint event_request_requesting_user_account_id_fkey
  foreign key (requesting_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.event_request drop constraint if exists event_request_client_organisation_id_fkey;
alter table public.event_request add  constraint event_request_client_organisation_id_fkey
  foreign key (client_organisation_id) references public.client_organisation (client_organisation_id) on delete restrict;
alter table public.event drop constraint if exists event_event_request_id_fkey;
alter table public.event add  constraint event_event_request_id_fkey
  foreign key (event_request_id) references public.event_request (event_request_id) on delete set null;
alter table public.event drop constraint if exists event_reused_from_event_id_fkey;
alter table public.event add  constraint event_reused_from_event_id_fkey
  foreign key (reused_from_event_id) references public.event (event_id) on delete set null;
alter table public.event drop constraint if exists event_assigned_coordinator_user_account_id_fkey;
alter table public.event add  constraint event_assigned_coordinator_user_account_id_fkey
  foreign key (assigned_coordinator_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.event drop constraint if exists event_owning_organiser_user_account_id_fkey;
alter table public.event add  constraint event_owning_organiser_user_account_id_fkey
  foreign key (owning_organiser_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.event drop constraint if exists event_client_organisation_id_fkey;
alter table public.event add  constraint event_client_organisation_id_fkey
  foreign key (client_organisation_id) references public.client_organisation (client_organisation_id) on delete restrict;
alter table public.event_essential_arrangement drop constraint if exists event_essential_arrangement_event_id_fkey;
alter table public.event_essential_arrangement add  constraint event_essential_arrangement_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.event_comment drop constraint if exists event_comment_event_id_fkey;
alter table public.event_comment add  constraint event_comment_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.event_comment drop constraint if exists event_comment_author_user_account_id_fkey;
alter table public.event_comment add  constraint event_comment_author_user_account_id_fkey
  foreign key (author_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.event_comment drop constraint if exists event_comment_parent_comment_id_fkey;
alter table public.event_comment add  constraint event_comment_parent_comment_id_fkey
  foreign key (parent_comment_id) references public.event_comment (comment_id) on delete cascade;
alter table public.supporting_document drop constraint if exists supporting_document_event_id_fkey;
alter table public.supporting_document add  constraint supporting_document_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.supporting_document drop constraint if exists supporting_document_uploaded_by_user_account_id_fkey;
alter table public.supporting_document add  constraint supporting_document_uploaded_by_user_account_id_fkey
  foreign key (uploaded_by_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.session drop constraint if exists session_event_id_fkey;
alter table public.session add  constraint session_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.venue_supported_layout drop constraint if exists venue_supported_layout_venue_id_fkey;
alter table public.venue_supported_layout add  constraint venue_supported_layout_venue_id_fkey
  foreign key (venue_id) references public.venue (venue_id) on delete cascade;
alter table public.venue_supported_layout drop constraint if exists venue_supported_layout_room_layout_id_fkey;
alter table public.venue_supported_layout add  constraint venue_supported_layout_room_layout_id_fkey
  foreign key (room_layout_id) references public.room_layout (room_layout_id) on delete restrict;
alter table public.booking drop constraint if exists booking_venue_id_fkey;
alter table public.booking add  constraint booking_venue_id_fkey
  foreign key (venue_id) references public.venue (venue_id) on delete restrict;
alter table public.booking drop constraint if exists booking_event_id_fkey;
alter table public.booking add  constraint booking_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.booking drop constraint if exists booking_session_id_fkey;
alter table public.booking add  constraint booking_session_id_fkey
  foreign key (session_id) references public.session (session_id) on delete cascade;
alter table public.booking drop constraint if exists booking_requested_by_user_account_id_fkey;
alter table public.booking add  constraint booking_requested_by_user_account_id_fkey
  foreign key (requested_by_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.booking drop constraint if exists booking_decided_by_user_account_id_fkey;
alter table public.booking add  constraint booking_decided_by_user_account_id_fkey
  foreign key (decided_by_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.booking drop constraint if exists booking_suggested_alternative_venue_id_fkey;
alter table public.booking add  constraint booking_suggested_alternative_venue_id_fkey
  foreign key (suggested_alternative_venue_id) references public.venue (venue_id) on delete set null;
alter table public.booking_slot drop constraint if exists booking_slot_booking_id_fkey;
alter table public.booking_slot add  constraint booking_slot_booking_id_fkey
  foreign key (booking_id) references public.booking (booking_id) on delete cascade;
alter table public.equipment_reservation drop constraint if exists equipment_reservation_event_id_fkey;
alter table public.equipment_reservation add  constraint equipment_reservation_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.equipment_reservation drop constraint if exists equipment_reservation_session_id_fkey;
alter table public.equipment_reservation add  constraint equipment_reservation_session_id_fkey
  foreign key (session_id) references public.session (session_id) on delete cascade;
alter table public.equipment_reservation drop constraint if exists equipment_reservation_reviewed_by_user_account_id_fkey;
alter table public.equipment_reservation add  constraint equipment_reservation_reviewed_by_user_account_id_fkey
  foreign key (reviewed_by_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.equipment_reservation_line drop constraint if exists equipment_reservation_line_equipment_reservation_id_fkey;
alter table public.equipment_reservation_line add  constraint equipment_reservation_line_equipment_reservation_id_fkey
  foreign key (equipment_reservation_id) references public.equipment_reservation (equipment_reservation_id) on delete cascade;
alter table public.equipment_reservation_line drop constraint if exists equipment_reservation_line_equipment_item_id_fkey;
alter table public.equipment_reservation_line add  constraint equipment_reservation_line_equipment_item_id_fkey
  foreign key (equipment_item_id) references public.equipment_item (equipment_item_id) on delete restrict;
alter table public.support_request drop constraint if exists support_request_event_id_fkey;
alter table public.support_request add  constraint support_request_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.support_request_assignment drop constraint if exists support_request_assignment_support_request_id_fkey;
alter table public.support_request_assignment add  constraint support_request_assignment_support_request_id_fkey
  foreign key (support_request_id) references public.support_request (support_request_id) on delete cascade;
alter table public.support_request_assignment drop constraint if exists support_request_assignment_technical_support_user_account__fkey;
alter table public.support_request_assignment add  constraint support_request_assignment_technical_support_user_account__fkey
  foreign key (technical_support_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.registration drop constraint if exists registration_event_id_fkey;
alter table public.registration add  constraint registration_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.registration drop constraint if exists registration_session_id_fkey;
alter table public.registration add  constraint registration_session_id_fkey
  foreign key (session_id) references public.session (session_id) on delete cascade;
alter table public.registration drop constraint if exists registration_attendee_user_account_id_fkey;
alter table public.registration add  constraint registration_attendee_user_account_id_fkey
  foreign key (attendee_user_account_id) references public.user_account (user_account_id) on delete cascade;
alter table public.waiting_list_entry drop constraint if exists waiting_list_entry_registration_id_fkey;
alter table public.waiting_list_entry add  constraint waiting_list_entry_registration_id_fkey
  foreign key (registration_id) references public.registration (registration_id) on delete cascade;
alter table public.change_request drop constraint if exists change_request_event_id_fkey;
alter table public.change_request add  constraint change_request_event_id_fkey
  foreign key (event_id) references public.event (event_id) on delete cascade;
alter table public.change_request drop constraint if exists change_request_requesting_user_account_id_fkey;
alter table public.change_request add  constraint change_request_requesting_user_account_id_fkey
  foreign key (requesting_user_account_id) references public.user_account (user_account_id) on delete restrict;
alter table public.notification drop constraint if exists notification_recipient_user_account_id_fkey;
alter table public.notification add  constraint notification_recipient_user_account_id_fkey
  foreign key (recipient_user_account_id) references public.user_account (user_account_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_event_id_fkey;
alter table public.notification add  constraint notification_related_event_id_fkey
  foreign key (related_event_id) references public.event (event_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_event_request_id_fkey;
alter table public.notification add  constraint notification_related_event_request_id_fkey
  foreign key (related_event_request_id) references public.event_request (event_request_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_booking_id_fkey;
alter table public.notification add  constraint notification_related_booking_id_fkey
  foreign key (related_booking_id) references public.booking (booking_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_equipment_reservation_id_fkey;
alter table public.notification add  constraint notification_related_equipment_reservation_id_fkey
  foreign key (related_equipment_reservation_id) references public.equipment_reservation (equipment_reservation_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_registration_id_fkey;
alter table public.notification add  constraint notification_related_registration_id_fkey
  foreign key (related_registration_id) references public.registration (registration_id) on delete cascade;
alter table public.notification drop constraint if exists notification_related_change_request_id_fkey;
alter table public.notification add  constraint notification_related_change_request_id_fkey
  foreign key (related_change_request_id) references public.change_request (change_request_id) on delete cascade;
alter table public.audit_record drop constraint if exists audit_record_actor_user_account_id_fkey;
alter table public.audit_record add  constraint audit_record_actor_user_account_id_fkey
  foreign key (actor_user_account_id) references public.user_account (user_account_id) on delete set null;

commit;
```

## Part 3 — Restore the dropped constraints

Every statement is verbatim from `raw/original script.md`. Run Part 0 first — these fail on non-conforming rows.

```sql
begin;

-- event
alter table public.event add constraint event_time_order_chk
  check (start_time is null or end_time is null or end_time > start_time);
alter table public.event add constraint event_registration_window_chk
  check (registration_open_date is null or registration_close_date is null
         or registration_close_date >= registration_open_date);
alter table public.event add constraint event_no_self_reuse_chk
  check (reused_from_event_id is distinct from event_id);

-- event_comment
alter table public.event_comment add constraint event_comment_no_self_parent_chk
  check (parent_comment_id is distinct from comment_id);

-- session
alter table public.session add constraint session_event_id_sequence_no_key
  unique (event_id, sequence_no);
alter table public.session add constraint session_time_order_chk
  check (start_time is null or end_time is null or end_time > start_time);

-- booking
alter table public.booking add constraint booking_scope_chk
  check (event_id is not null or session_id is not null);
alter table public.booking add constraint booking_decider_chk
  check (status = 'Requested' or decided_by_user_account_id is not null);

-- booking_slot
alter table public.booking_slot add constraint booking_slot_booking_id_slot_date_slot_key
  unique (booking_id, slot_date, slot);

-- equipment_reservation
alter table public.equipment_reservation add constraint equipment_reservation_scope_chk
  check (event_id is not null or session_id is not null);

-- equipment_reservation_line
alter table public.equipment_reservation_line
  add constraint equipment_reservation_line_resv_item_key
  unique (equipment_reservation_id, equipment_item_id);
alter table public.equipment_reservation_line
  add constraint equipment_reservation_line_quantity_chk
  check (quantity_reserved <= quantity_requested);

-- registration
alter table public.registration add constraint registration_checkin_chk
  check ((checked_in_at is null) = (checked_in_method is null));

create unique index if not exists registration_unique_per_event_uidx
  on public.registration (event_id, attendee_user_account_id)
  where session_id is null and status not in ('Cancelled', 'Withdrawn');

create unique index if not exists registration_unique_per_session_uidx
  on public.registration (session_id, attendee_user_account_id)
  where session_id is not null and status not in ('Cancelled', 'Withdrawn');

-- notification
alter table public.notification add constraint notification_sent_at_chk
  check (status <> 'Sent' or sent_at is not null);

-- audit_record
alter table public.audit_record add constraint audit_record_kind_chk
  check (action is not null or field_changed is not null);

commit;
```

> [!warning] The two registration indexes no longer cover every row
> They were written when `attendee_user_account_id` was `not null`. It is now nullable, and Postgres treats each `null` as distinct — so guest registrations (`attendee_name`/`attendee_email` only) are **not** de-duplicated by these indexes. Restoring them is still strictly better than nothing, but closing the gap needs the guest-registration decision listed at the end.

## Part 4 — Enforce the double-booking hard block

The one place this migration goes beyond the original script — because the original left the block as a commented choice between (a) and (b) and picked neither, while [#35](https://github.com/SinYang13/IS212-2026/discussions/35) is a settled, non-proposable rule (it is in [[index]]'s hard-rules callout). This implements option (a), which that comment recommended: denormalise `venue_id` and `status` onto `booking_slot`, kept in step by triggers, then a partial unique index.

```sql
begin;

alter table public.booking_slot add column if not exists venue_id       bigint;
alter table public.booking_slot add column if not exists booking_status text;

update public.booking_slot s
   set venue_id = b.venue_id, booking_status = b.status
  from public.booking b
 where b.booking_id = s.booking_id
   and (s.venue_id is distinct from b.venue_id
     or s.booking_status is distinct from b.status);

alter table public.booking_slot alter column venue_id       set not null;
alter table public.booking_slot alter column booking_status set not null;

alter table public.booking_slot drop constraint if exists booking_slot_venue_id_fkey;
alter table public.booking_slot add  constraint booking_slot_venue_id_fkey
  foreign key (venue_id) references public.venue (venue_id) on delete restrict;

-- Keep the denormalised copy correct on insert/update of a slot.
create or replace function booking_slot_sync_from_booking()
returns trigger language plpgsql as $$
begin
  select b.venue_id, b.status into new.venue_id, new.booking_status
    from public.booking b where b.booking_id = new.booking_id;
  return new;
end;
$$;

drop trigger if exists booking_slot_sync on public.booking_slot;
create trigger booking_slot_sync
  before insert or update of booking_id on public.booking_slot
  for each row execute function booking_slot_sync_from_booking();

-- Propagate a booking's venue/status change down to its slots.
create or replace function booking_propagate_to_slots()
returns trigger language plpgsql as $$
begin
  update public.booking_slot
     set venue_id = new.venue_id, booking_status = new.status
   where booking_id = new.booking_id;
  return new;
end;
$$;

drop trigger if exists booking_propagate on public.booking;
create trigger booking_propagate
  after update of venue_id, status on public.booking
  for each row when (old.venue_id is distinct from new.venue_id
                  or old.status   is distinct from new.status)
  execute function booking_propagate_to_slots();

-- The hard block itself (#35, #41): at most one live booking per
-- venue + date + slot. Releasing or rejecting a booking frees the slot.
create unique index if not exists booking_slot_no_double_booking_uidx
  on public.booking_slot (venue_id, slot_date, slot)
  where booking_status in ('Tentative Hold', 'Confirmed');

commit;
```

## Part 5 — Restore indexes, triggers and RLS

```sql
begin;

create index if not exists user_account_client_org_idx on user_account (client_organisation_id);
create index if not exists user_account_role_role_idx on user_account_role (role_id);
create index if not exists event_request_user_idx on event_request (requesting_user_account_id);
create index if not exists event_request_client_org_idx on event_request (client_organisation_id);
create index if not exists event_client_org_idx on event (client_organisation_id);
create index if not exists event_coordinator_idx on event (assigned_coordinator_user_account_id);
create index if not exists event_organiser_idx on event (owning_organiser_user_account_id);
create index if not exists event_reused_from_idx on event (reused_from_event_id);
create index if not exists event_status_idx on event (status);
create index if not exists event_comment_event_idx on event_comment (event_id);
create index if not exists event_comment_author_idx on event_comment (author_user_account_id);
create index if not exists event_comment_parent_idx on event_comment (parent_comment_id);
create index if not exists supporting_document_event_idx on supporting_document (event_id);
create index if not exists supporting_document_uploader_idx on supporting_document (uploaded_by_user_account_id);
create index if not exists session_event_idx on session (event_id);
create index if not exists venue_supported_layout_layout_idx on venue_supported_layout (room_layout_id);
create index if not exists booking_venue_idx on booking (venue_id);
create index if not exists booking_event_idx on booking (event_id);
create index if not exists booking_session_idx on booking (session_id);
create index if not exists booking_requester_idx on booking (requested_by_user_account_id);
create index if not exists booking_decider_idx on booking (decided_by_user_account_id);
create index if not exists booking_alt_venue_idx on booking (suggested_alternative_venue_id);
create index if not exists booking_status_idx on booking (status);
create index if not exists booking_slot_booking_idx on booking_slot (booking_id);
create index if not exists booking_slot_date_idx on booking_slot (slot_date, slot);
create index if not exists equipment_reservation_event_idx on equipment_reservation (event_id);
create index if not exists equipment_reservation_session_idx on equipment_reservation (session_id);
create index if not exists equipment_reservation_reviewer_idx on equipment_reservation (reviewed_by_user_account_id);
create index if not exists equipment_line_reservation_idx on equipment_reservation_line (equipment_reservation_id);
create index if not exists equipment_line_item_idx on equipment_reservation_line (equipment_item_id);
create index if not exists support_request_event_idx on support_request (event_id);
create index if not exists support_request_assignment_user_idx on support_request_assignment (technical_support_user_account_id);
create index if not exists registration_event_idx on registration (event_id);
create index if not exists registration_session_idx on registration (session_id);
create index if not exists registration_attendee_idx on registration (attendee_user_account_id);
create index if not exists change_request_event_idx on change_request (event_id);
create index if not exists change_request_requester_idx on change_request (requesting_user_account_id);
create index if not exists notification_recipient_idx on notification (recipient_user_account_id);
create index if not exists notification_status_idx on notification (status) where status = 'Pending';
create index if not exists audit_record_entity_idx on audit_record (entity_type, entity_id);
create index if not exists audit_record_actor_idx on audit_record (actor_user_account_id);

-- updated_at triggers
create or replace function set_updated_at()
returns trigger language plpgsql as $$
begin
  new.updated_at = now();
  return new;
end;
$$;

do $$
declare t text;
begin
  foreach t in array array[
    'client_organisation', 'user_account', 'event_request', 'event', 'session',
    'venue', 'booking', 'equipment_item', 'equipment_reservation',
    'support_request', 'registration', 'change_request'
  ]
  loop
    execute format('drop trigger if exists %I_set_updated_at on public.%I', t, t);
    execute format(
      'create trigger %I_set_updated_at before update on public.%I
         for each row execute function set_updated_at()', t, t);
  end loop;
end;
$$;

-- Row Level Security: default-deny everywhere. Policies still depend on the
-- role model (#56) and are deliberately not written here.
do $$
declare t text;
begin
  foreach t in array array[
    'client_organisation', 'role', 'user_account', 'user_account_role',
    'event_request', 'event', 'event_essential_arrangement', 'event_comment',
    'supporting_document', 'session', 'venue', 'room_layout',
    'venue_supported_layout', 'booking', 'booking_slot', 'equipment_item',
    'equipment_reservation', 'equipment_reservation_line', 'support_request',
    'support_request_assignment', 'registration', 'waiting_list_entry',
    'change_request', 'notification', 'audit_record'
  ]
  loop
    execute format('alter table public.%I enable row level security', t);
  end loop;
end;
$$;

commit;
```

> [!warning] Enabling RLS with no policies default-denies every table
> Supabase blocks all access through the anon and authenticated keys the moment RLS is on and no policy matches. If the application currently reads through those keys, **it will stop working** until policies exist. Run Part 5's RLS block last, and only once policies are ready — or split it out and run Parts 1–4 first.

## Part 6 — Verify

```sql
select 'FKs without on delete' as check_name, count(*) as should_be_zero
  from pg_constraint where contype = 'f' and connamespace = 'public'::regnamespace
   and confdeltype = 'a'
union all
select 'check constraints', count(*)          -- expect >= 23
  from pg_constraint where contype = 'c' and connamespace = 'public'::regnamespace
union all
select 'indexes', count(*)                    -- expect >= 44
  from pg_indexes where schemaname = 'public'
union all
select 'tables without RLS', count(*)         -- expect 0 after Part 5
  from pg_tables where schemaname = 'public' and not rowsecurity;

-- The default now satisfies its own CHECK. Catalog check, no side effects:
select column_default as event_status_default   -- expect: 'Planning'::text
  from information_schema.columns
 where table_schema = 'public' and table_name = 'event' and column_name = 'status';

-- Optional live insert test. Needs an existing user_account and
-- client_organisation, and rolls itself back.
begin;
  insert into public.event (name, owning_organiser_user_account_id, client_organisation_id)
  select 'smoke test', u.user_account_id, c.client_organisation_id
    from public.user_account u cross join public.client_organisation c limit 1
  returning event_id, status;   -- expect status = 'Planning'
rollback;
```

## Deliberately not in this migration

Each needs a decision that no source supplies. Putting them in SQL would be inventing requirements.

| Item | Why it is not here | What is needed |
|---|---|---|
| Technical-support duplication (`event.technical_support_flag`/`_description` vs `support_request.support_needed_flag`/`description`) | No source names an owner | Team picks the owner; drop the other pair |
| `decision_record` on both `event_request` and `event` | [[event-request-workflow\|brief §5]] step 5 attaches the record to the decision but not to a table | Team picks; on the Part 1 split it belongs to the request |
| Guest registration (`attendee_name`/`attendee_email`, nullable account) | Answers 🔶 [#22](https://github.com/SinYang13/IS212-2026/discussions/22), which is open | Decide on [[registration]] first, then add an identity `check` and a guest-aware unique index |
| `Attended` / `No Show` in `registration.status` | §4 requires registration and attendance be distinguishable, but names no shape | Move to a separate column or table; then `registration.status` returns to Registered/Waitlisted/Withdrawn |
| Completion tracking on `event_essential_arrangement` | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) requires the gate but names no storage | Decide how `programme` and `other` are evaluated — they have no backing table |
| `equipment_item` pooled quantity vs single `operational_status` | 🔶 [#13](https://github.com/SinYang13/IS212-2026/discussions/13) defers pooled-vs-serialised | Resolve [#13](https://github.com/SinYang13/IS212-2026/discussions/13); the current shape cannot express "3 of 10 in maintenance" |
| A unique login identity on `user_account` | 🔶 [#62](https://github.com/SinYang13/IS212-2026/discussions/62) leaves the auth method open | Needed under every method; propose with the [#62](https://github.com/SinYang13/IS212-2026/discussions/62) answer |
| `related_support_request_id` on `notification` | [[notifications\|brief §6]] implies it; no discussion confirms | Confirm with the customer |

## See also

- [[current-schema-scrutiny]] — the defect review this migration answers
- [[supabase-schema-ddl]] — the wiki's candidate DDL, itself carrying the wrong status lists
- [[event]], [[event-request]] — update these to the Part 1 split before running
