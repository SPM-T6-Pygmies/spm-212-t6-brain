---
title: Index
type: index
sources: []
tags: [meta, index]
---

# Index

Content catalog of every wiki page. Update whenever a page is created, renamed, or removed.

**21 pages** — 3 sources, 15 concepts, 3 entities. All raw documents ingested as of 2026-09-02.

> [!important] Hard rules the customer actually committed to
> Most of this brief is deliberately open, so the few fixed constraints matter. Do not design around these — they are stated, not proposable.
> - Venues book in **AM / PM / Night** slots; full day is all three ([[venue-booking]], #50)
> - Double-booking must be **blocked outright**, not warned ([[venue-booking]], #35)
> - Only **one** active tentative hold or confirmed booking per venue and overlapping period ([[venue-booking]], #41)
> - Equipment is collectable from **D-1** and available again only from **Return Day + 1**; no same-day sharing ([[equipment-management]], #5)
> - Confirmation must be **blocked** while an arrangement essential to that event is incomplete ([[event-readiness]], #80)
> - Only the **assigned Event Coordinator** may cancel, at any stage ([[event-lifecycle]], #68)
> - Waiting-list acceptance window is **3 days** ([[attendee-registration]], #44)
> - Events auto-complete **7 days** after the end date if not closed manually ([[event-lifecycle]], #71)
> - **Singapore time only**; English only ([[non-functional-requirements]], #36, #28)

## Start here

| If you want to… | Read |
|---|---|
| Understand what is being built | [[event-planning-venue-booking-system]] |
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

## Entities

| Page | Type | Covers |
|---|---|---|
| [[connectsphere-event-services]] | Organisation | The mock customer; current state, operating rules, drivers |
| [[event-planning-venue-booking-system]] | Product | Functionality areas, scope status, explicit exclusions |
| [[is212-g9-section]] | Course / process | Personas, board rules, groups, timeline |

## Ingestion status

| Raw file | Status |
|---|---|
| `raw/Customer Brief no. 1.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 26.md` | ✅ Ingested 2026-09-02 |
| `raw/IS212 2026 Discussions Aug 31.md` | ✅ Ingested 2026-09-02 |
| `raw/llm-wiki.md` | ⏸️ Not ingested — methodology, not domain content; see log |
