---
title: ConnectSphere Event Services
type: entity
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "Week 7 Customer Changes.md"
tags: [entity, organisation, customer, connectsphere]
---

# ConnectSphere Event Services

The mock customer organisation for the IS212 project and the commissioning client for the [[event-planning-venue-booking-system]].

## Profile

| Attribute | Value |
|---|---|
| Type | Regional event planning and venue services provider |
| Market | Organisations across Southeast Asia |
| Facilities | Owns and operates several event facilities **within Singapore** |
| Headcount | ~500 operational staff |
| Event types | Conferences, seminars, workshops, exhibitions, training sessions, meetings, networking events |
| Assets | Event venues, plus equipment and supporting resources |

Because all owned venues are in Singapore, the system operates on **Singapore time only** (Source: is212-discussions-2026-08-31, [#36](https://github.com/SinYang13/IS212-2026/discussions/36)).

## Internal roles

The ~500 staff take on the roles of Event Coordinator, Venue Staff, and Technical Support Staff (Source: customer-brief-no-1). An **Event Operations Manager** role also exists in practice but appears only in the clarifications, not the brief. The Week 7 change notice then introduces two further internal roles, the **Event Coordinator Lead** and the **Safety Officer** ([[week-7-customer-changes|Week 7 C5–C6]]); whether the Lead replaces the Manager is unanswered. See [[user-roles]].

## Current state

Event coordination runs on a mixture of email, spreadsheets, shared calendars, messaging applications, online forms, and manually maintained documents. A single event's information is therefore spread across several places and owned by different employees.

The specific tools in use were never named, and integration with them is out of scope; the new system becomes the authoritative source for the event information it manages (Source: is212-discussions-2026-08-31, [#79](https://github.com/SinYang13/IS212-2026/discussions/79)). There is no data migration requirement (Source: is212-discussions-2026-08-31, [#12](https://github.com/SinYang13/IS212-2026/discussions/12)).

## Operating rules worth remembering

| Rule | Detail | Ref |
|---|---|---|
| Venue slots | Bookings are made in AM / PM / Night slots; full day is all three | [#50](https://github.com/SinYang13/IS212-2026/discussions/50) |
| Equipment turnaround | Collectable from D-1; available again only from Return Day + 1 | [#5](https://github.com/SinYang13/IS212-2026/discussions/5) |
| Double-booking | Must be blocked outright | [#35](https://github.com/SinYang13/IS212-2026/discussions/35) |
| Cancellation authority | Only the assigned Event Coordinator | [#68](https://github.com/SinYang13/IS212-2026/discussions/68) |
| Coordinator assignment | Event Operations Manager, per an SOP held outside the system | [#93](https://github.com/SinYang13/IS212-2026/discussions/93), [#94](https://github.com/SinYang13/IS212-2026/discussions/94) |
| Contracts and payments | Handled physically, outside the system | [#24](https://github.com/SinYang13/IS212-2026/discussions/24) |
| Equipment health checklist | Kept outside the system | [#85](https://github.com/SinYang13/IS212-2026/discussions/85) |

## Business drivers

Expansion has increased the volume of events, venues, resources, clients and participants beyond what the current tooling can coordinate. The brief lists twelve concrete failure modes of the status quo — duplicated requests, divergent spreadsheets, unclear approval status, stale requirements, accidental double-booking, insufficient turnaround, changes not reaching affected parties, cancelled events still holding reservations, and no record of who changed what and when.

The system is expected to support anticipated growth over the next **three years** across events, users, client organisations, venues, equipment and registrations. See [[non-functional-requirements]].

## Relationships

- Serves external client organisations, each of which may have multiple Event Organisers and several past and upcoming events. See [[user-roles]].
- Requirements captured in [[customer-brief-no-1]], clarified through [[is212-discussions-2026-08-31]].
