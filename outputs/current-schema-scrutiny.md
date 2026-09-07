---
title: Current Schema Scrutiny
type: review
sources:
  - "current schema.md"
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [review, schema, sql, postgres, data-model, scrutiny]
---

# Current Schema Scrutiny

A defect review of `raw/current schema.md` — the 25-table Postgres schema currently deployed — against the wiki's structural reference ([[domain-model]], the fifteen `wiki/domain/` pages) and against [[supabase-schema-ddl]], the candidate DDL the deployed schema was evidently derived from.

**Verdict:** table inventory is complete (all 25 objects present, correctly named, correctly keyed). The damage is in everything *below* the column list — status enumerations, constraints and referential actions. **Zero of 48 foreign keys carry an `ON DELETE` clause**, 12 of the DDL's constraints were dropped in transit, and two status enumerations were replaced with values that appear in no source document. One of those replacements makes the table uninsertable.

> [!warning] Two defect classes, one of them not the schema's fault
> C1–C6 and H1 are drift introduced *after* [[supabase-schema-ddl]]. But the `In Progress` / `Withdrawn` status values (C2, C3) were already wrong **in the wiki's own DDL**, and the root cause (W1) is a contradiction between [[event]] and [[event-request]] that predates both. Fixing the schema without fixing W1 will just reintroduce the same values on the next regeneration.

## Severity summary

| # | Finding | Table(s) | Severity |
|---|---|---|---|
| C1 | `event.status` default is a malformed nested literal; violates its own `CHECK` | `event` | 🔴 Critical |
| C2 | `event.status` omits `Planning` / `Awaiting Arrangements`, invents `In Progress` | `event` | 🔴 Critical |
| C3 | `event_request.status` drops `Returned`, invents `Withdrawn` | `event_request` | 🔴 Critical |
| C4 | Request/event state split only half-applied — `event` still carries `Draft`, `Approved` | `event`, `event_request` | 🔴 Critical |
| C5 | Double-booking hard block unenforceable — no uniqueness anywhere on venue/date/slot | `booking_slot` | 🔴 Critical |
| C6 | All 48 FKs default to `NO ACTION`; every deliberate cascade/restrict/set-null lost | all | 🔴 Critical |
| C7 | Anonymous registration silently introduced; both uniqueness indexes lost | `registration` | 🔴 Critical |
| H1 | Registration status conflates registration with attendance | `registration` | 🟠 High |
| H2 | Duplicate — 13 request/event field pairs, 6 renamed, 1 retyped | `event_request`, `event` | 🟠 High |
| H3 | Duplicate — technical support stored on both `event` and `support_request` | `event`, `support_request` | 🟠 High |
| H4 | Duplicate — `decision_record` on both request and event | `event_request`, `event` | 🟠 High |
| H5 | Essential arrangements record *which*, never *whether complete* | `event_essential_arrangement` | 🟠 High |
| H6 | Scope checks lost — bookings/reservations attachable to nothing | `booking`, `equipment_reservation` | 🟠 High |
| M1–M9 | Lost uniqueness, ordering, time-order and sanity constraints | various | 🟡 Medium |
| W1–W3 | Wiki-side contradictions that caused C2–C4 | `wiki/` | 🟠 High |

## Critical

### C1 — `event.status` default is malformed and unsatisfiable

```sql
status text NOT NULL DEFAULT '''Planning''::text'::text CHECK (status = ANY (ARRAY['Draft', ..., 'Cancelled']))
```

The default is a **doubly-quoted literal**: it evaluates to the eleven-character string `'Planning'::text`, quote marks and cast included, not to `Planning`. That value is not in the `CHECK` list — and neither is `Planning`, even correctly quoted (C2).

**Consequence:** every `INSERT INTO event` that omits `status` fails with a check-constraint violation. The table cannot be written to through its own default path. This is almost certainly the artefact of a status value being edited in a GUI and re-quoted on save.

### C2 — `event.status` deletes the two states [#80](https://github.com/SinYang13/IS212-2026/discussions/80) exists to create

| Source | Values |
|---|---|
| [[event]] (States) | Draft, Submitted, Under Review, Approved, Returned, Rejected, **Planning**, **Awaiting Arrangements**, Confirmed, Completed, Cancelled |
| Deployed schema | Draft, Approved, Confirmed, **In Progress**, Completed, Cancelled |

`Planning` and `Awaiting Arrangements` are the states that carry the single most-cited settled decision in the domain — *approval is not confirmation, and confirmation is blocked while any essential arrangement is incomplete* ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)). Without `Awaiting Arrangements` there is no state in which an event is approved-but-not-yet-confirmable, so the gate has nowhere to live and `Approved → Confirmed` becomes a legal single step. That inverts the customer's answer.

`In Progress` appears in **no source document, no domain page and no discussion thread**. [[event]] documents no in-flight state; an event that is running is `Confirmed` until `Completed`.

`Submitted`, `Under Review`, `Returned` and `Rejected` are also absent — defensible if the request/event split were fully applied, but it is not (C4).

### C3 — `event_request.status` deletes `Returned` and invents `Withdrawn`

| Source | Values |
|---|---|
| [[event-request]] (States) | Draft, Submitted, Under Review, Approved, Rejected, **Returned** |
| Deployed schema | Draft, Submitted, Under Review, Approved, Rejected, **Withdrawn** |

`Returned` is the coordinator's *return for amendment or clarification* — [[event-request-workflow|brief §5]] step 5 names it as one of three review outcomes alongside approve and reject, and both [[event]] and [[event-request]] model the `Returned → Submitted` resubmission edge.

**Consequence:** the entire return-and-resubmit loop is unrepresentable. A coordinator who needs more information has only `Rejected` (terminal) available, which destroys the request. Meanwhile `Withdrawn` — organiser-initiated abandonment — is stated nowhere by the customer; it is an invented state that displaced a specified one.

### C4 — the request/event split is only half-applied

This is the structural finding. [[event-request]] is explicit:

> Approval converts a request into a plannable [[event]]; the two are modelled as **separate records** because approval commits ConnectSphere to nothing ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)).

…and marks `Approved` as *"Terminal for this record — see [[event]]"*. On that model, `event.status` should begin at the post-approval stages only. The deployed schema instead keeps `Draft` and `Approved` on **both** tables:

| Status value | On `event_request` | On `event` | Correct owner |
|---|---|---|---|
| Draft | ✅ | ✅ | request only |
| Submitted | ✅ | ✗ | request |
| Under Review | ✅ | ✗ | request |
| Approved | ✅ | ✅ | request (terminal) → hands off to event |
| Rejected | ✅ | ✗ | request |
| Returned | ✗ (C3) | ✗ | request |
| Planning / Awaiting Arrangements | ✗ | ✗ (C2) | event |
| Confirmed / Completed | ✗ | ✅ | event |
| Cancelled | ✗ | ✅ | event (any stage, [#68](https://github.com/SinYang13/IS212-2026/discussions/68)) |

**Consequence:** `event_request.status = 'Approved'` and `event.status = 'Approved'` can coexist on a linked pair with no rule stating which is authoritative, and an `event` row can sit in `Draft` — a state that, on the wiki's own model, means "not yet submitted for review", which a post-approval record cannot be in. Either the split is real and `event` loses `Draft`/`Approved` in favour of `Planning`/`Awaiting Arrangements`/`Confirmed`, or the split is abandoned and `event_request` is folded into `event`. The current schema does neither, so the boundary is undefined at exactly the point [[event-request]] calls *"the single largest open modelling decision in the event cluster."*

### C5 — the double-booking hard block is unenforceable

Double-booking is a **hard block, not an overridable warning** ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)) — a settled decision in [[booking]]'s register. The deployed `booking_slot` has a surrogate PK and *nothing else*:

```sql
CREATE TABLE public.booking_slot (
  booking_slot_id bigint GENERATED ALWAYS AS IDENTITY NOT NULL,
  booking_id bigint NOT NULL,
  slot_date date NOT NULL,
  slot text NOT NULL CHECK (slot = ANY (ARRAY['AM','PM','Night'])),
  ...
);
```

[[supabase-schema-ddl]] at least carried `unique (booking_id, slot_date, slot)` plus a documented choice between denormalising `venue_id`/`status` onto `booking_slot` or a guard trigger. Both are gone. The deployed schema does not even prevent the *same booking* from claiming the same slot twice, let alone two `Confirmed` bookings colliding on one venue/date/slot.

**Consequence:** the customer's one unambiguous hard block is enforced by nothing. Two coordinators confirming the same room for the same Tuesday afternoon both succeed.

### C6 — every referential action was dropped

48 `FOREIGN KEY` constraints; **0 `ON DELETE` clauses**. Postgres defaults all of them to `NO ACTION`. [[supabase-schema-ddl]] assigned these deliberately — *"cascade on child/junction tables, restrict on master-data references"*.

| Intended | Now | Effect |
|---|---|---|
| `event_comment.event_id` → cascade | no action | deleting an event errors instead of removing its comment thread |
| `supporting_document.event_id` → cascade | no action | same |
| `booking_slot.booking_id` → cascade | no action | a released booking cannot be deleted while slots exist |
| `equipment_reservation_line.*` → cascade | no action | same |
| `event.event_request_id` → set null | no action | a request can never be deleted once converted |
| `event.reused_from_event_id` → set null | no action | a template event is undeletable forever ([#75](https://github.com/SinYang13/IS212-2026/discussions/75)) |
| `audit_record.actor_user_account_id` → set null | no action | a departed user's account is undeletable, breaking [[non-functional-requirements\|brief §8]] retention handling |
| `venue.*`, `equipment_item.*` → restrict | no action | accidentally correct — `NO ACTION` behaves like `RESTRICT` here |

The last row is the only one where the loss is harmless. Everywhere else, cleanup and cascade paths now fail at runtime rather than at review.

### C7 — anonymous registration introduced; both uniqueness guards lost

The deployed `registration` differs from every wiki source:

```sql
attendee_user_account_id bigint,                                   -- was NOT NULL
registration_reference uuid NOT NULL DEFAULT gen_random_uuid() UNIQUE,  -- new
attendee_name text,                                                 -- new
attendee_email text,                                                -- new
```

[[registration]] states the relationship as **many : 1, mandatory** — *"Each registration belongs to exactly one Attendee account"* ([[event-planning-venue-booking-system|brief §4]]; [#48](https://github.com/SinYang13/IS212-2026/discussions/48)) — and files "what information an Attendee must supply" as an **open** team decision (🔶 [#22](https://github.com/SinYang13/IS212-2026/discussions/22)). Adding `attendee_name`/`attendee_email` answers [#22](https://github.com/SinYang13/IS212-2026/discussions/22) unilaterally, in the schema, without the decision being recorded on the domain page or in the deferred-modelling register.

Three concrete defects follow:

1. **No identity is required at all.** All three of `attendee_user_account_id`, `attendee_name`, `attendee_email` are nullable with no `CHECK` that at least one is set. A registration attached to nobody inserts cleanly.
2. **Duplicate registrations are unblocked.** [[supabase-schema-ddl]]'s two partial unique indexes (`registration_unique_per_event_uidx`, `registration_unique_per_session_uidx`) are absent. One attendee can hold twenty live registrations for one event, and each consumes a capacity slot.
3. **Waiting-list ordering is unguarded.** `waiting_list_entry.position` has no uniqueness per event, so two entries can hold position 1 and the [#44](https://github.com/SinYang13/IS212-2026/discussions/44) offer sequence is nondeterministic.

Guest registration may well be the right call for [#33](https://github.com/SinYang13/IS212-2026/discussions/33)'s VIP-list flow — but it is a modelling decision that must be made on [[registration]] first, then mirrored here.

## High

### H1 — registration status conflates registration with attendance

[[event-planning-venue-booking-system|brief §4]] requires *"registration and attendance information to be distinguished"* — [[registration]] models them as **two independent axes**: a three-state registration status (Registered / Waitlisted / Withdrawn) and a separate check-in fact.

The deployed schema stores attendance **twice, in contradictory forms**:

| Representation | Values |
|---|---|
| `status` | Registered, Waitlisted, **Cancelled**, Withdrawn, **Attended**, **No Show** |
| `checked_in_at` / `checked_in_method` | timestamp + method |

`Attended` and `No Show` are attendance outcomes occupying the registration-status column. Once a row is set to `No Show`, whether the person had been `Registered` or promoted from `Waitlisted` is destroyed — the two axes collapse into one, which is the precise thing [[event-planning-venue-booking-system|brief §4]] forbids. And nothing keeps `status = 'Attended'` in step with `checked_in_at IS NOT NULL`; [[supabase-schema-ddl]]'s `registration_checkin_chk` (both null or both set) is also gone. `Cancelled` is a fourth invention — [[registration]] has only `Withdrawn`.

### H2 — duplicate: 13 request/event field pairs, six renamed, one retyped

The user-reported duplication. `event_request` and `event` carry the same 13 planning fields. Duplication is *defensible* — the request is an immutable statement of what was asked for, the event is what is being arranged, and [[event]] notes preferred date/time "may diverge from what is ultimately arranged". What is **not** defensible is that the two tables spell them differently:

| `event_request` | `event` | Drift |
|---|---|---|
| `event_name` | `name` | renamed |
| `accessibility_needs` | `accessibility_requirements` | renamed |
| `room_layout_preferences` | `room_layout_preference` | renamed + pluralisation |
| `general_programme` | `programme_agenda` | renamed |
| `other_special_arrangements` | `special_arrangements` | renamed |
| `registration_requirements` (text) | `registration_enabled_flag` (boolean) | renamed **and retyped** |
| `preferred_time` (text) | `start_time`/`end_time` (timestamptz) | restructured |

**Consequence:** copy-on-approval cannot be a mechanical column-for-column copy; it needs a hand-maintained mapping that will rot. The `registration_requirements → registration_enabled_flag` pair is lossy — free-text registration requirements cannot be squeezed into a boolean, and [[event]] already marks `registration_enabled_flag` as `⚠️ Partial`. Nothing in the schema records *whether* a field has diverged post-approval, so "what did the client originally ask for" and "what changed since" are only answerable via `audit_record`, which has no FK to either table.

### H3 — duplicate: technical support stored on two tables

| `event` | `support_request` |
|---|---|
| `technical_support_flag boolean` | `support_needed_flag boolean` |
| `technical_support_description text` | `description text` |

Both are inherited faithfully from [[supabase-schema-ddl]], so this is a wiki-level duplicate, not deployment drift — but it is a duplicate either way. No constraint, trigger or FK keeps them in step; `event.technical_support_flag = true` with zero `support_request` rows, or `false` with three, are both insertable. [[support-request]] gives no rule for which wins.

### H4 — duplicate: `decision_record` on both request and event

`event_request.decision_record` and `event.decision_record` both cite [[event-request-workflow|brief §5]] step 5 — the coordinator's approve/reject/return record. On the request/event split that record belongs to the **request**, since the decision is what ends the request's life. [[event]] claims it too. One column is dead; which one is undecided.

### H5 — essential arrangements have no completion state

```sql
CREATE TABLE public.event_essential_arrangement (
  event_id bigint, arrangement_type text, is_essential boolean DEFAULT true
);
```

The table records **which** arrangement types matter for this event — correctly per-event, per [#80](https://github.com/SinYang13/IS212-2026/discussions/80). It does not record **whether any of them is done**. The rule the table exists to serve is *"confirmation is blocked while any arrangement essential to this event is incomplete"*, and that predicate has no data to read.

For `venue`, `equipment` and `registration`, completeness is *derivable* (a `Confirmed` booking, a `Reserved` reservation, an open registration window). For `technical_support` it is weakly derivable via `support_request_assignment`. For **`programme` and `other` there is no backing table at all** — those two arrangement types can never be evaluated. The gate is unimplementable for two of its six declared types.

### H6 — scope checks lost: bookings and reservations attachable to nothing

`booking.event_id` and `booking.session_id` are both nullable; so are `equipment_reservation.event_id` and `.session_id`. [[supabase-schema-ddl]] guarded both with `check (event_id is not null or session_id is not null)` precisely because *"a row referencing neither is meaningless"*. Both checks are gone. A `Confirmed` booking against a venue, belonging to no event and no session, inserts cleanly and blocks the slot forever with no owner to release it.

## Medium

| # | Finding | Consequence |
|---|---|---|
| M1 | `session` lost `unique (event_id, sequence_no)` | two sessions can both be "session 2"; programme ordering is nondeterministic |
| M2 | `equipment_reservation_line` lost `unique (reservation_id, item_id)` **and** `quantity_reserved <= quantity_requested` | the same item can appear twice on one reservation with no aggregate; 5 units reservable against a request for 2, contradicting [#90](https://github.com/SinYang13/IS212-2026/discussions/90)'s line-by-line fulfilment |
| M3 | All time-order checks dropped — `event` and `session` (`end > start`), `event` registration window (`close >= open`), `event_no_self_reuse`, `event_comment_no_self_parent` | events ending before they start; an event reused from itself; a comment parented to itself (infinite thread) |
| M4 | `booking` lost `booking_decider_chk` | a `Confirmed` or `Rejected` booking with `decided_by_user_account_id` null — no accountable approver, breaking [[non-functional-requirements\|brief §8]] auditability |
| M5 | `notification` lost `notification_sent_at_chk`; has no `related_support_request_id` | `status = 'Sent'` with null `sent_at`; support-request notifications ([[notifications\|brief §6]]) cannot be linked to their subject |
| M6 | `audit_record` lost `audit_record_kind_chk`; `entity_type` has no `CHECK` | empty audit rows (neither `action` nor `field_changed`); any typo is a valid entity type, so the polymorphic join silently returns nothing |
| M7 | `equipment_item` — pooled `quantity` with a **single** `operational_status` | 10 projectors of which 3 are in `Maintenance` is inexpressible; the row is either all-Available or all-Maintenance. [#13](https://github.com/SinYang13/IS212-2026/discussions/13) defers pooled-vs-serialised but requires the system *always know available quantity* — which this shape cannot deliver |
| M8 | `user_account` has **no unique identity column** | `contact_details text` is an undifferentiated blob; there is no email or username to authenticate against or to enforce one-account-per-person. [#62](https://github.com/SinYang13/IS212-2026/discussions/62) leaves the *method* open, but a unique login handle is needed under every method |
| M9 | Not present in the dump: 41 FK indexes, `updated_at` triggers, RLS enablement | If the dump is complete rather than table-only, **no RLS means every table is readable through the Supabase anon key** — a direct breach of [[access-control]] and [[non-functional-requirements\|brief §8]]. Verify before treating as a finding |

## Wiki-side corrections

Per the Review workflow, defects that are the *wiki's* rather than the raw document's. These are the root cause of C2–C4 — patching the schema alone will regress on the next regeneration.

### W1 — [[event]] and [[event-request]] contradict each other on state ownership

[[event-request]] declares six states and marks `Approved` *"terminal for this record"*. [[event]]'s own `stateDiagram-v2` nonetheless still opens `[*] --> Draft --> Submitted --> Under Review --> Approved --> Planning` — reproducing all five pre-approval states that [[event-request]] claims exclusively, including `Returned` and `Rejected`.

So the wiki asserts the split in prose and then denies it in the diagram. [[event-request]]'s own warning callout ("The brief blurs Event Request and Event") diagnoses the ambiguity in the *brief* but does not notice that [[event]] has reproduced it. Both DDLs then guessed differently, which is how `In Progress` and `Withdrawn` got in.

**Recommended fix:** trim [[event]]'s state machine to the post-approval lifecycle — `Approved → Planning ⇄ Awaiting Arrangements → Confirmed → Completed`, with `Cancelled` reachable from all ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)) — and leave Draft/Submitted/Under Review/Returned/Rejected to [[event-request]] alone. This is a modelling decision, not a typo fix, so it is flagged here rather than applied.

### W2 — [[supabase-schema-ddl]]'s status lists never matched the domain pages

Its "Known gaps" table concedes *"every status value list is a best reading of the owning domain page"*. But `In Progress` and `Withdrawn` are not readings of anything — neither string occurs in [[event]], [[event-request]], the brief, or any discussion. They are inventions that the deployed schema then inherited verbatim. Both `check` lists need regenerating from the domain pages once W1 is settled.

### W3 — [[domain-model]] contradicts its own ERD on staff availability

The Schema design decisions section states that Support Request's staff-availability representation (🔶 [#89](https://github.com/SinYang13/IS212-2026/discussions/89)) *"is left off `USER_ACCOUNT` entirely, since no source specifies what an availability record would even contain."* The ERD block immediately above lists `string availability` on `USER_ACCOUNT`. Both DDLs followed the ERD. Either the prose or the ERD is wrong; [[user-account]] carries the attribute as `⚠️ Partial` / `📋 Backlog`, which favours keeping the column and correcting the prose.

## Recommended order of work

| Priority | Action |
|---|---|
| 1 | Settle W1 — decide the request/event state boundary on the domain pages |
| 2 | Regenerate both `status` check lists from the settled pages (fixes C1, C2, C3, C4, W2) |
| 3 | Restore the 12 dropped constraints and the `ON DELETE` clauses from [[supabase-schema-ddl]] (fixes C6, H6, M1–M6) |
| 4 | Close the double-booking block with the denormalised partial unique index option (C5) |
| 5 | Decide guest registration on [[registration]], then mirror; restore both unique indexes and add an identity check (C7) |
| 6 | Split attendance out of `registration.status` (H1) |
| 7 | Resolve the four duplicate pairs — pick an owner for each (H2, H3, H4) |
| 8 | Add completion tracking to `event_essential_arrangement`, or specify how `programme` and `other` are evaluated (H5) |
| 9 | Verify M9 against the live database, not the dump |

## See also

- [[supabase-schema-ddl]] — the candidate DDL this schema drifted from
- [[domain-model]] — the ER diagram, design-decision rationale and deferred-modelling register
- [[event]], [[event-request]] — the two pages whose contradiction drives C2–C4
- [[open-questions]] — the requirements-level register
