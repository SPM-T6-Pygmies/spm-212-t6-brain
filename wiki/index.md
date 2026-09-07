---
title: Index
type: index
sources: []
tags: [meta, index]
---

# Index

Content catalog of every wiki page. Update whenever a page is created, renamed, or removed.

**47 pages** — 4 sources, 16 concepts, 16 domain objects, 3 entities, 8 reviews. All raw documents ingested as of 2026-09-05.

> [!important] Hard rules the customer actually committed to
> Most of this brief is deliberately open, so the few fixed constraints matter. Do not design around these — they are stated, not proposable.
> - Venues book in **AM / PM / Night** slots; full day is all three ([[venue-booking]], [#50](https://github.com/SinYang13/IS212-2026/discussions/50))
> - Double-booking must be **blocked outright**, not warned ([[venue-booking]], [#35](https://github.com/SinYang13/IS212-2026/discussions/35))
> - Only **one** active tentative hold or confirmed booking per venue and overlapping period ([[venue-booking]], [#41](https://github.com/SinYang13/IS212-2026/discussions/41))
> - Equipment is collectable from **D-1** and available again only from **Return Day + 1**; no same-day sharing ([[equipment-management]], [#5](https://github.com/SinYang13/IS212-2026/discussions/5))
> - Confirmation must be **blocked** while an arrangement essential to that event is incomplete ([[event-readiness]], [#80](https://github.com/SinYang13/IS212-2026/discussions/80))
> - Only the **assigned Event Coordinator** may cancel, at any stage ([[event-lifecycle]], [#68](https://github.com/SinYang13/IS212-2026/discussions/68))
> - Waiting-list acceptance window is **3 days** ([[attendee-registration]], [#44](https://github.com/SinYang13/IS212-2026/discussions/44))
> - Events auto-complete **7 days** after the end date if not closed manually ([[event-lifecycle]], [#71](https://github.com/SinYang13/IS212-2026/discussions/71))
> - **Singapore time only**; English only ([[non-functional-requirements]], [#36](https://github.com/SinYang13/IS212-2026/discussions/36), [#28](https://github.com/SinYang13/IS212-2026/discussions/28))

## Start here

| If you want to… | Read |
|---|---|
| Understand what is being built | [[event-planning-venue-booking-system]] |
| Understand the object model | [[domain-model]] |
| Understand who it is for | [[connectsphere-event-services]], [[user-roles]] |
| See what the customer has *not* decided | [[open-questions]] |
| See the latest customer answers | [[is212-discussions-2026-08-31]] |
| Understand the course process and deadlines | [[is212-g9-section]] |
| See what's actually in the Week 12 release | [[release-1-scope]] |

## Sources

One summary page per ingested document in `raw/`.

| Page | Document | Status |
|---|---|---|
| [[customer-brief-no-1]] | `Customer Brief no. 1.md` | Baseline requirements |
| [[is212-discussions-2026-08-26]] | `IS212 2026 Discussions Aug 26.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-08-31]] | `IS212 2026 Discussions Aug 31.md` | ✅ Authoritative |
| [[week-4-customer-briefing]] | `Week 4 - Customer Briefing.md` | ✅ Authoritative on Release 1 scope |

## Concepts

### Event flow

| Page | Covers |
|---|---|
| [[event-request-workflow]] | The 14-step process, draft to completion; coordinator assignment |
| [[event-lifecycle]] | Statuses; approval vs confirmation; completion, cancellation, postponement |
| [[event-readiness]] | Essential arrangements; the confirmation gate; outstanding actions |
| [[change-impact-management]] | Significant vs ordinary edits; rescheduling; revalidation |

### Resources

| Page | Covers |
|---|---|
| [[venue-booking]] | Slot model, booking approval, tentative holds, conflict blocking, turnaround |
| [[venue-suitability]] | Search and filtering vs suitability checking; capacity, layout, accessibility |
| [[equipment-management]] | Catalogue, the two availability rules, reservation, maintenance, defects |
| [[technical-support]] | Equipment review flow, clarification routing, staff assignment and rostering |

### People and access

| Page | Covers |
|---|---|
| [[user-roles]] | The five briefed roles plus the Event Operations Manager gap |
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
| [[event-request]] | The pre-approval artefact; the request-vs-event modelling fork |
| [[session]] | First-class child of an event, with its own venue, equipment and attendance limit |
| [[change-request]] | Significant vs ordinary changes; the revalidation trigger |

### Resources

| Page | Covers |
|---|---|
| [[venue]] | AM/PM/Night slots, capacity, layouts, per-venue turnaround, unavailability |
| [[booking]] | The booking record, tentative holds, slot exclusivity, conflict blocking |
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
| [[user-stories-assignment-scrutiny]] | `raw/User Stories Assignment.md` checked against `raw/IS212 User Story Standard — v1.0.md` and against this wiki's sourced requirements; proposed fixes for every story plus three recommended additions to Registration |
| [[user-stories-proposed-rewrites]] | Every proposed rewrite and recommended new card from [[user-stories-assignment-scrutiny]], consolidated into one build-ordered, ready-to-paste replacement backlog |
| [[release-1-candidate-user-stories]] | Candidate backlog derived row-by-row from [[release-1-scope]]'s `🎯`/`🧩` register — full coverage table, new cards for every uncarded area, and Release 1-scoped rewrites of Reg1/Reg5 now that waiting list and check-in are `📋 Backlog` |
| [[supabase-schema-ddl]] | ⚠️ **Superseded by [[schema]]** — the earlier DDL draft, kept for provenance. Its `event.status` (`Draft/Approved/Confirmed/In Progress/Completed/Cancelled`) and `event_request.status` (…`Withdrawn`) predate the one-per-table status split and contain two ungrounded values |
| [[user-story-coverage-stocktake]] | Cross-references Linear's 76 `SPM - IS212` issues against [[release-1-candidate-user-stories]], [[user-stories-proposed-rewrites]] and [[event-planning-venue-booking-system]]'s functionality-area table; both flagged Release 1 anomalies now resolved, and all seventeen `📋 Backlog`/undecided areas with a citation but no card are now carded in [[backlog-candidate-user-stories]] |
| [[backlog-candidate-user-stories]] | Draft cards for every uncarded area [[user-story-coverage-stocktake]] found — nine epics, 30 stories and one spike covering Technical Support, Reporting, cross-event visibility, history browsing, event content, tentative holds, readiness chasing and reminders; every card carries its blocking unknown, and ten questions are escalated to the customer |
| [[current-schema-scrutiny]] | `raw/current schema.md` — the deployed 25-table Postgres schema — checked against [[domain-model]], the fifteen domain pages and [[supabase-schema-ddl]]; seven critical defects including a `CHECK`-violating `event.status` default, two invented status values, an unenforceable double-booking block and zero `ON DELETE` clauses across 48 FKs, plus three wiki-side contradictions that caused them |
| [[schema-migration-01]] | Runnable SQL fixing every defect in [[current-schema-scrutiny]] that a source actually grounds — restores 48 `on delete` clauses, 12 constraints and 41 indexes verbatim from `raw/original script.md`, reallocates [[event-planning-venue-booking-system\|brief §4]]'s ten named statuses one-per-table across `event_request` and `event`, and enforces the [#35](https://github.com/SinYang13/IS212-2026/discussions/35) double-booking block; eight further items are listed as team decisions rather than guessed at |

## Ingestion status

| Raw file | Status |
|---|---|
| `raw/Customer Brief no. 1.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 26.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 31.md` | ✅ Ingested 2026-09-02 |
| `raw/Week 4 - Customer Briefing.md` | ✅ Ingested 2026-09-05 |
| `raw/llm-wiki.md` | ⏸️ Not ingested — methodology, not domain content; see log |
