---
title: Index
type: index
sources: []
tags: [meta, index]
---

# Index

Content catalog of every wiki page. Update whenever a page is created, renamed, or removed.

**62 pages** — 9 sources, 17 concepts, 18 domain objects, 3 entities, 15 reviews. All raw documents ingested as of 2026-10-04, plus three discussions ([#127](https://github.com/SinYang13/IS212-2026/discussions/127)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129)) read live from the board because no export contains them yet — see [[is212-discussions-2026-10-04-addendum]]. Release 1 is now the customer's "26 core": the Week 4 twenty plus the six changes in [[week-7-customer-changes]]. [[schema]] migrated 2026-09-09; the gaps from the 2026-09-16 batch and Week 7 (E1–E3, W1–W8) are logged there, not yet migrated.

> [!important] Hard rules the customer actually committed to
> Most of this brief is deliberately open, so the few fixed constraints matter. Do not design around these — they are stated, not proposable.
> - ~~Venues book in **AM / PM / Night** slots; full day is all three~~ — **superseded 2026-10-04**: the smallest unit of booking is **15 minutes** ([[venue-booking]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128), [[week-7-customer-changes\|Week 7 C1]])
> - Double-booking must be **blocked outright**, not warned ([[venue-booking]], [#35](https://github.com/SinYang13/IS212-2026/discussions/35))
> - Only **one** active tentative hold or confirmed booking per venue and overlapping period ([[venue-booking]], [#41](https://github.com/SinYang13/IS212-2026/discussions/41))
> - Equipment is collectable from **D-1** and available again only from **Return Day + 1**; no same-day sharing ([[equipment-management]], [#5](https://github.com/SinYang13/IS212-2026/discussions/5)) — but there is **no default return day**, so [[schema]]'s `equipment_reservation.return_date` is nullable and how it gets populated is still open ([#113](https://github.com/SinYang13/IS212-2026/discussions/113))
> - Confirmation must be **blocked** while an arrangement essential to that event is incomplete ([[event-readiness]], [#80](https://github.com/SinYang13/IS212-2026/discussions/80))
> - Only the **assigned Event Coordinator** may cancel, at any stage ([[event-lifecycle]], [#68](https://github.com/SinYang13/IS212-2026/discussions/68))
> - Waiting-list acceptance window is **3 days** ([[attendee-registration]], [#44](https://github.com/SinYang13/IS212-2026/discussions/44))
> - Events auto-complete **7 days** after the end date if not closed manually ([[event-lifecycle]], [#71](https://github.com/SinYang13/IS212-2026/discussions/71))
> - **Singapore time only**; English only ([[non-functional-requirements]], [#36](https://github.com/SinYang13/IS212-2026/discussions/36), [#28](https://github.com/SinYang13/IS212-2026/discussions/28))
> - ~~One slot is the smallest unit of setup and turnaround; an event across two slots consumes both in full~~ — **superseded** by the 15-minute unit ([[venue-booking]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128))
> - Each venue has a **configurable setup time and turnaround time**, counted in availability and conflict detection; a booking occupies `[start − setup, end + turnaround)` ([[venue-booking]], [[week-7-customer-changes\|Week 7 C1]]). Replaces the one-adjacent-slot rule of [#123](https://github.com/SinYang13/IS212-2026/discussions/123)
> - Retiring a venue **does not cancel or auto-reschedule** its future bookings; they are flagged for review and Coordinators notified ([[venue-booking]], [#122](https://github.com/SinYang13/IS212-2026/discussions/122))
> - Venue capacity **varies by room layout**, supplied as master data, not computed ([[venue]], [#112](https://github.com/SinYang13/IS212-2026/discussions/112))
> - The **Event Operations Manager**, now titled the **Event Coordinator Lead**, must be a separate system role; it assigns, reassigns, and views all coordinators' assignments and active events ([[user-roles]], [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#128](https://github.com/SinYang13/IS212-2026/discussions/128), [[week-7-customer-changes\|Week 7 C5]])
> - Registration must not exceed event/venue capacity — **even for a manually added VIP** ([[registration]], [#99](https://github.com/SinYang13/IS212-2026/discussions/99))
> - Every tentative hold carries an **expiry**; an expired hold is never a confirmed booking ([[booking]], [[week-7-customer-changes\|Week 7 C4]])
> - An event **cannot proceed to preparation** until a Safety Officer has approved it; the Officer always responds ([[operational-safety-check]], [[week-7-customer-changes\|Week 7 C6]])
> - One event may hold **several simultaneous venue bookings**, each checked independently ([[booking]], [[week-7-customer-changes\|Week 7 C3]])

## Start here

| If you want to… | Read |
|---|---|
| See what Week 7 changed | [[week-7-customer-changes]], [[week-7-change-impact-review]] |
| Understand what is being built | [[event-planning-venue-booking-system]] |
| Understand the object model | [[domain-model]] |
| Understand who it is for | [[connectsphere-event-services]], [[user-roles]] |
| See what the customer has *not* decided | [[open-questions]] |
| See the latest customer answers | [[is212-discussions-2026-10-04-addendum]], then [[is212-discussions-2026-10-01]] |
| See what the newest batch changed our mind about | [[discussions-sept-8-critical-diffs]] |
| Understand the course process and deadlines | [[is212-g9-section]] |
| See what's actually in the Week 12 release | [[release-1-scope]] |

## Sources

One summary page per ingested document in `raw/`.

| Page | Document | Status |
|---|---|---|
| [[customer-brief-no-1]] | `Customer Brief no. 1.md` | Baseline requirements |
| [[is212-discussions-2026-08-26]] | `IS212 2026 Discussions Aug 26.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-08-31]] | `IS212 2026 Discussions Aug 31.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-09-08]] | `IS212 2026 Discussions Sept 8.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-09-16]] | `IS212 2026 Discussions Sept 16.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-10-01]] | `IS212 2026 Discussions Oct 1.md` | ✅ Authoritative snapshot |
| [[is212-discussions-2026-10-04-addendum]] | Discussions [#127](https://github.com/SinYang13/IS212-2026/discussions/127)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129), live from the board | ✅ Authoritative — **not yet exported to `raw/`** |
| [[week-7-customer-changes]] | `Week 7 Customer Changes.md` | ✅ Authoritative; binding for Release 1 |
| [[week-4-customer-briefing]] | `Week 4 - Customer Briefing.md` | ✅ Authoritative on Release 1 scope |

## Concepts

### Event flow

| Page | Covers |
|---|---|
| [[event-request-workflow]] | The 14-step process, draft to completion; coordinator assignment |
| [[event-lifecycle]] | Statuses; approval vs confirmation; completion, cancellation, postponement |
| [[event-readiness]] | Essential arrangements; the confirmation gate; the separate safety gate; outstanding actions |
| [[operational-safety-check]] | The Safety Officer's review between technical arrangements and preparation: inputs, outcomes, the gate, open questions |
| [[change-impact-management]] | Significant vs ordinary edits; rescheduling; revalidation |

### Resources

| Page | Covers |
|---|---|
| [[venue-booking]] | The 15-minute booking unit, booking approval, expiring tentative holds, conflict blocking, per-venue setup/turnaround, multi-venue events, temporary unavailability, venue retirement |
| [[venue-suitability]] | Search and filtering vs suitability checking; capacity, layout, accessibility |
| [[equipment-management]] | Catalogue, the two availability rules, reservation, maintenance, defects |
| [[technical-support]] | Equipment review flow, clarification routing, staff assignment and rostering |

### People and access

| Page | Covers |
|---|---|
| [[user-roles]] | The seven roles — five briefed, the Event Coordinator Lead (the Operations Manager renamed) and the Safety Officer |
| [[access-control]] | Authentication, protected information, client-organisation visibility |

### Participation and communication

| Page | Covers |
|---|---|
| [[attendee-registration]] | Registration period, capacity, waiting list, withdrawal, attendance |
| [[notifications]] | Trigger scenarios, channels, batch vs real-time, reminders |

### Cross-cutting

| Page | Covers |
|---|---|
| [[release-1-scope]] | ⭐ Authoritative in/adjacent/out register for the Week 12 release — every functionality area, marked 🎯/🧩/📋 |
| [[non-functional-requirements]] | Performance, security, usability, reliability, scalability, auditability, maintainability |
| [[reporting]] | Event, venue usage and registration reports; history; dashboards |
| [[open-questions]] | ⭐ Consolidated register of everything left to the team |

## Domain objects

The object model — what each thing *is*, as opposed to what happens to it. Start at [[domain-model]] for the ER diagram and the register of settled and deferred modelling decisions, and [[schema]] for the applied DDL.

### Event flow

| Page | Covers |
|---|---|
| [[event]] | The central aggregate; attributes, the five post-approval statuses, essential arrangements, comments, documents |
| [[event-request]] | The pre-approval artefact; the seven request states including `Withdrawn`; the request-vs-event modelling fork |
| [[safety-check]] | One Safety Officer review round on an event — proposed, not in the deployed schema |
| [[session]] | First-class child of an event, with its own venue, equipment and attendance limit |
| [[change-request]] | Significant vs ordinary changes; per-field partial approval; the revalidation trigger |

### Resources

| Page | Covers |
|---|---|
| [[venue]] | The 15-minute booking unit, per-layout capacity, layouts, configurable setup/turnaround, unavailability, retired status |
| [[booking]] | The booking record, start and end on a 15-minute grid, padded occupancy, expiring tentative holds, several bookings per event, conflict blocking |
| [[venue-unavailability]] | A period a venue is marked temporarily unavailable, with one of five reasons — proposed, not in the deployed schema |
| [[equipment-item]] | Catalogue, statuses, the two availability rules, pooled-vs-serialised |
| [[equipment-reservation]] | Requirement lines, partial fulfilment, defect logging |
| [[support-request]] | Technical support requests and staff assignment |

### People and participation

| Page | Covers |
|---|---|
| [[user-account]] | Roles, multi-role, the capability matrix, coordinator assignment rules |
| [[client-organisation]] | The two stated cardinalities; cross-organisation access |
| [[registration]] | Registration period, waiting list, 3-day window, check-in, account-less (guest) registrants |

### Cross-cutting

| Page | Covers |
|---|---|
| [[notification]] | 14 trigger scenarios, channels, batch delivery, reminders |
| [[audit-record]] | Activity history vs change history; retention boundary |
| [[schema]] | ⭐ The applied physical model — 25-table Postgres/Supabase DDL, keys, check constraints, indexes, RLS; source of truth for column types, defaults and status enumerations |

## Entities

| Page | Type | Covers |
|---|---|---|
| [[connectsphere-event-services]] | Organisation | The mock customer; current state, operating rules, drivers |
| [[event-planning-venue-booking-system]] | Product | Functionality areas, scope status, explicit exclusions |
| [[is212-g9-section]] | Course / process | Personas, board rules, groups, timeline |

## Reviews

One-off scrutiny artefacts — not domain knowledge, kept here so they aren't orphaned.

| Page | Covers |
|---|---|
| [[discussions-sept-8-critical-diffs]] | ⭐ The incremental-ingestion report for `raw/IS212 2026 Discussions Sept 8.md` — establishes the 28-item delta against Aug 31, then grades the seven answers that contradict or close a standing wiki position (three critical, three high, two medium) and the two pre-existing schema defects the batch made undeniable. Start here to see what the newest customer answers changed our mind about |
| [[linear-ticket-audit-sept-16-batch]] | The `SPM - IS212` Linear board checked against the two Venues answers in [[is212-discussions-2026-09-16]] — eleven comments posted (three on in-cycle cards), one card whose premise is superseded (SPM-23), one open question answered (SPM-20), and the venue-throughput consequence of adjacent-slot buffers. Comments only; nothing edited or created |
| [[linear-ticket-audit-sept-8-batch]] | ⭐ The `SPM - IS212` Linear board checked against [[discussions-sept-8-critical-diffs]] — nine acceptance criteria that contradicted a customer answer (one of them in active development), three systemic defects that cut across many cards, and a required feature 18 verb (*view registration status*) that had no ticket at all. Records the 27 tickets changed, the 14 created, and the ~35 Tier 2 edits identified but not applied |
| [[user-stories-assignment-scrutiny]] | `raw/User Stories Assignment.md` checked against `raw/IS212 User Story Standard — v1.0.md` and against this wiki's sourced requirements; proposed fixes for every story plus three recommended additions to Registration |
| [[user-stories-proposed-rewrites]] | Every proposed rewrite and recommended new card from [[user-stories-assignment-scrutiny]], consolidated into one build-ordered, ready-to-paste replacement backlog |
| [[release-1-candidate-user-stories]] | Candidate backlog derived row-by-row from [[release-1-scope]]'s `🎯`/`🧩` register — full coverage table, new cards for every uncarded area, and Release 1-scoped rewrites of Reg1/Reg5 now that waiting list and check-in are `📋 Backlog` |
| [[supabase-schema-ddl]] | ⚠️ **Superseded by [[schema]]** — the earlier DDL draft, kept for provenance. Its `event.status` (`Draft/Approved/Confirmed/In Progress/Completed/Cancelled`) and `event_request.status` (…`Withdrawn`) predate the one-per-table status split and contain two ungrounded values |
| [[user-story-coverage-stocktake]] | Cross-references Linear's 76 `SPM - IS212` issues against [[release-1-candidate-user-stories]], [[user-stories-proposed-rewrites]] and [[event-planning-venue-booking-system]]'s functionality-area table; both flagged Release 1 anomalies now resolved, and all seventeen `📋 Backlog`/undecided areas with a citation but no card are now carded in [[backlog-candidate-user-stories]] |
| [[backlog-candidate-user-stories]] | Draft cards for every uncarded area [[user-story-coverage-stocktake]] found — nine epics, 30 stories and one spike covering Technical Support, Reporting, cross-event visibility, history browsing, event content, tentative holds, readiness chasing and reminders; every card carries its blocking unknown. Revised 2026-09-09 against the 2026-09-08 batch, which reaches twenty of the thirty-one items without adding or deleting a card: three escalated customer questions closed, two opened, nine live |
| [[current-schema-scrutiny]] | `raw/current schema.md` — the deployed 25-table Postgres schema — checked against [[domain-model]], the fifteen domain pages and [[supabase-schema-ddl]]; seven critical defects including a `CHECK`-violating `event.status` default, two invented status values, an unenforceable double-booking block and zero `ON DELETE` clauses across 48 FKs, plus three wiki-side contradictions that caused them |
| [[schema-migration-02]] | The five structural changes applied to [[schema]] on 2026-09-09 — `equipment_reservation.return_date`, per-layout `venue_supported_layout.capacity`, the seventh `event_request` state `Withdrawn`, the `change_request_item` child table with its enforced decline reason, and coordinator assignment on the request. Records five open items the SQL deliberately does not guess at, including the retained `venue.capacity` scalar and the missing booking-layout link |
| [[schema-migration-01]] | Runnable SQL fixing every defect in [[current-schema-scrutiny]] that a source actually grounds — restores 48 `on delete` clauses, 12 constraints and 41 indexes verbatim from `raw/original script.md`, reallocates [[event-planning-venue-booking-system\|brief §4]]'s ten named statuses one-per-table across `event_request` and `event`, and enforces the [#35](https://github.com/SinYang13/IS212-2026/discussions/35) double-booking block; eight further items are listed as team decisions rather than guessed at |
| [[schema-er-consistency-audit]] | ⭐ Scrutiny pass over [[schema]]'s 26-table DDL and [[domain-model]]'s two ER diagrams, checked against the fourteen domain pages, the migration artefacts and the raw sources. The DDL is mechanically sound; the defects sit at the model/wiki boundary. Six status enumerations carry values no source supports, the [#99](https://github.com/SinYang13/IS212-2026/discussions/99) capacity cap is neither enforced nor computable, `equipment_reservation` can compute only one end of the D-1 → Return Day + 1 window, and both ER diagrams are missing the coordinator edge the log records as drawn. Two cited `raw/` sources do not exist |
| [[week-7-change-impact-review]] | ⭐ `raw/Week 7 Customer Changes.md`'s six mandatory Release 1 changes, run through `raw/Managing Changes – A Guide.md` against the Linear board and [[schema]]. **Revised the same day for [#128](https://github.com/SinYang13/IS212-2026/discussions/128)**: the 15-minute unit replaces the slots, the Lead is the Operations Manager renamed, and four tickets moved to Done. Now 15 new stories (45 pts), 24 edits, 6 relabels, **no reopens**, plus a proposed migration 03 and 8 open customer questions. Nothing applied to Linear or the DDL |
| [[week-7-user-stories]] | The user stories from [[week-7-change-impact-review]] on their own, for reading and pasting. It has the 21 existing stories that Week 7 modifies, with each change highlighted (`==added==`, `~~removed~~`) against the live Linear text, and the 15 new W7 cards (45 pts). A quality-pass table lists the eight cards that still need work, led by SPM-45 (13 ACs) and W7-12 (the role is the business, not a user). Nothing applied to Linear |
| [[spm-24-implementation-decisions]] | Decisions taken building SPM-24 (register for a confirmed event) in the `connectsphere` repository — the public attendee view, the fixed name-and-email field set, duplicate and capacity refusal, and deviations from the candidate DDL; reconciled against [[schema]], with the one surviving disagreement (XOR vs always-present attendee identity) listed as open |

## Ingestion status

| Raw file | Status |
|---|---|
| `raw/Customer Brief no. 1.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 26.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 31.md` | ✅ Ingested 2026-09-02 |
| `raw/Week 4 - Customer Briefing.md` | ✅ Ingested 2026-09-05 |
| `raw/IS212 2026 Discussions Sept 8.md` | ✅ Ingested 2026-09-08 — incremental; only the 28-item delta against Aug 31 |
| `raw/IS212 2026 Discussions Sept 16.md` | ✅ Ingested 2026-09-21 — incremental; only the 2-item delta against Sept 8 ([#122](https://github.com/SinYang13/IS212-2026/discussions/122), [#123](https://github.com/SinYang13/IS212-2026/discussions/123)) |
| `raw/Week 7 Customer Changes.md` | ✅ Ingested 2026-10-04 — [[week-7-customer-changes]]; reviewed in [[week-7-change-impact-review]] |
| `raw/IS212 2026 Discussions Oct 1.md` | ✅ Ingested 2026-10-04 — incremental; the 3-item delta against Sept 16 ([#124](https://github.com/SinYang13/IS212-2026/discussions/124), [#125](https://github.com/SinYang13/IS212-2026/discussions/125), [#126](https://github.com/SinYang13/IS212-2026/discussions/126)) — [[is212-discussions-2026-10-01]] |
| `raw/Managing Changes – A Guide.md` | ✅ Summarised 2026-10-04 under *Change management* in [[is212-g9-section]] — course process, so no source page |
| `raw/llm-wiki.md` | ⏸️ Not ingested — methodology, not domain content; see log |
| Discussions [#127](https://github.com/SinYang13/IS212-2026/discussions/127)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129) | ⚠️ Ingested 2026-10-04 from the **live board**, not a `raw/` file — [[is212-discussions-2026-10-04-addendum]]. Re-export the board to make it checkable |
