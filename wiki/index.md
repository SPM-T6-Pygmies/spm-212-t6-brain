---
title: Index
type: index
sources: []
tags: [meta, index]
---

# Index

Content catalog of every wiki page. Update whenever a page is created, renamed, or removed.

**37 pages** — 3 sources, 15 concepts, 15 domain objects, 3 entities, 1 review. All raw documents ingested as of 2026-09-02.

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

## Sources

One summary page per ingested document in `raw/`.

| Page | Document | Status |
|---|---|---|
| [[customer-brief-no-1]] | `Customer Brief no. 1.md` | Baseline requirements |
| [[is212-discussions-2026-08-26]] | `IS212 2026 Discussions Aug 26.md` | ⚠️ Superseded snapshot |
| [[is212-discussions-2026-08-31]] | `IS212 2026 Discussions Aug 31.md` | ✅ Authoritative |

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
| [[non-functional-requirements]] | Performance, security, usability, reliability, scalability, auditability, maintainability |
| [[reporting]] | Event, venue usage and registration reports; history; dashboards |
| [[open-questions]] | ⭐ Consolidated register of everything left to the team |

## Domain objects

The object model — what each thing *is*, as opposed to what happens to it. Start at [[domain-model]] for the ER diagram and the register of settled and deferred modelling decisions.

### Event flow

| Page | Covers |
|---|---|
| [[event]] | The central aggregate; attributes, 11 statuses, essential arrangements, comments, documents |
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
| [[registration]] | Registration period, waiting list, 3-day window, check-in |

### Cross-cutting

| Page | Covers |
|---|---|
| [[notification]] | 14 trigger scenarios, channels, batch delivery, reminders |
| [[audit-record]] | Activity history vs change history; retention boundary |

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

## Ingestion status

| Raw file | Status |
|---|---|
| `raw/Customer Brief no. 1.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 26.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 31.md` | ✅ Ingested 2026-09-02 |
| `raw/llm-wiki.md` | ⏸️ Not ingested — methodology, not domain content; see log |
