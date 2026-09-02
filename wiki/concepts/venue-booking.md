---
title: Venue Booking
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
tags: [venue, booking, availability, scheduling, business-rules, connectsphere]
---

# Venue Booking

**Objects:** [[booking]] · [[venue]]

How [[connectsphere-event-services]] venues are catalogued, held, requested, approved, and protected from double-booking in the [[event-planning-venue-booking-system]]. Covers steps 6–7 of the planning process (see [[event-lifecycle]]). Assessment of whether a given venue *fits* an event is covered separately in [[venue-suitability]].

## Venue Catalogue

Venue Staff own venue master data — location, capacity, facilities, accessibility, supported room layouts, operating hours, and per-venue setup/teardown/turnaround time; see [[venue]] for the full attribute table (Source: customer-brief-no-1). Capacity drives suitability — see [[venue-suitability]].

All venues are in Singapore and the system operates on Singapore time (Source: is212-discussions-2026-08-31, [#36](https://github.com/SinYang13/IS212-2026/discussions/36)) — see [[non-functional-requirements]].

## Three-Slot Booking Model

Bookings are made against the AM / PM / Night slots defined on [[venue]] — a full-day booking is all three slots, and partial blocking (e.g. mornings only during a renovation) is expressed by blocking individual slots (Source: is212-discussions-2026-08-31, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)).

> [!warning] Slot labels — see [[venue]] for the unresolved ambiguity
> The customer's answer to [#50](https://github.com/SinYang13/IS212-2026/discussions/50) pairs three time ranges with only one label ("AM"); which range is PM and which is Night is this wiki's inference, not a stated fact. [[venue]] carries the verbatim source text, the resulting ambiguity, and the unexplained 12pm–1pm / 6pm–7pm gaps in full — this page does not restate the inferred hours, to avoid the two pages drifting apart.

**Team decision required:** the exact UX and implementation of slot selection are left open ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)) — including whether slots are stored as an enum or as concrete start/end timestamps, and how a multi-day or [[event-lifecycle|multi-session]] event maps onto slots.

## Booking Request and Approval

1. The Event Coordinator identifies candidate venues (search, filter, suitability check — see [[venue-suitability]]).
2. The Event Coordinator submits a **venue booking request** carrying the event timing and venue requirements (Source: customer-brief-no-1).
3. **Venue Staff** review and decide. They may approve, reject, or reject with a suggested alternative venue or arrangement (Source: customer-brief-no-1; is212-discussions-2026-08-31, [#45](https://github.com/SinYang13/IS212-2026/discussions/45)).

Rejection with an alternative is **not** a formal counter-offer object in the system. Venue Staff suggest; **the Event Coordinator then makes the changes** and re-requests (Source: is212-discussions-2026-08-31, [#45](https://github.com/SinYang13/IS212-2026/discussions/45)). This keeps the coordinator as the single point of control over event arrangements, consistent with [[event-request-workflow]].

Booking request, approval, rejection, and change are all notification triggers (see [[notifications]]).

## Tentative Holds

A venue may be temporarily held while an event is still being finalised (Source: customer-brief-no-1). The hold/confirm/release states, the one-active-hold-or-booking-per-venue rule, and the unresolved hold-expiry question are all defined on [[booking]] — see States and Binding rules.

> [!question]- Team decision required — tentative hold expiry and release
> Expiry period and release mechanism for tentative holds are explicitly for the team to propose (Source: is212-discussions-2026-08-31, [#39](https://github.com/SinYang13/IS212-2026/discussions/39) and [#41](https://github.com/SinYang13/IS212-2026/discussions/41)). This matters because a hold that never expires is functionally a confirmed booking and will silently block the venue — a stated pain point in the brief ("cancelled events may continue to occupy venue or equipment reservations").

Competing holds are therefore impossible by rule: the first hold wins the slot until it is confirmed or released. Multiple coordinators cannot shortlist the same slot in parallel.

## Conflict Detection and Double-Booking

The system must **identify and block** incompatible or overlapping bookings (Source: is212-discussions-2026-08-31, [#35](https://github.com/SinYang13/IS212-2026/discussions/35)).

- This is a **hard block**, not a warning.
- The question explicitly offered a soft-warning-with-override option (override by Venue Staff for VIP organisations); the customer did **not** take it.
- No override or exception path for privileged clients is authorised. Any such path would need fresh clarification.

Overlap detection must account for setup and turnaround time, not only the published event start and end times (Source: customer-brief-no-1). See [[venue-suitability]] and [[event-readiness]].

## Setup and Turnaround Time

- Venue Staff record **setup, teardown and turnaround requirements per venue** (Source: is212-discussions-2026-08-31, [#43](https://github.com/SinYang13/IS212-2026/discussions/43)).
- It is a per-venue attribute, not a global constant and not (currently) variable by event type or size.
- Insufficient turnaround between consecutive events is a named problem in the current manual process (Source: customer-brief-no-1).
- Extending an event may invalidate the turnaround before the next booking — a [[change-impact-management]] trigger.

Note the tension with the fixed three-slot model: if slots are indivisible, per-venue turnaround may be partly absorbed by the 12pm–1pm and 6pm–7pm gaps. Whether per-venue turnaround adds *on top of* slot boundaries is not stated and should be resolved by the team.

## Venue Unavailability and Blocking

- Venue Staff can block venues for maintenance, renovation, safety issues, internal activities, or other operational reasons (Source: customer-brief-no-1).
- Blocking is at slot granularity, enabling partial-day unavailability ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)).
- When a venue becomes unavailable **after** a booking is confirmed, Venue Staff are responsible for updating availability. The availability change affecting an upcoming event is a notification scenario, and **the Event Coordinator must then make the changes** (Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)).

> [!question]- Team decision required — post-confirmation venue loss automation
> Automation of the response to a post-confirmation venue loss (auto-release, auto-flag, auto-suggest alternatives) is for the team to propose ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)).

## Operating Hours

- Maintained by **Venue Staff** as part of venue information (Source: is212-discussions-2026-08-31, [#46](https://github.com/SinYang13/IS212-2026/discussions/46)).
- **All days are treated as equal for now** — no day-of-week variation, no weekend/public-holiday distinction, no holiday calendar required ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)).

## Booking Horizon

- **No booking horizon has been specified** (Source: is212-discussions-2026-08-31, [#57](https://github.com/SinYang13/IS212-2026/discussions/57)).
- **Team decision required:** propose one only if the solution requires it. Compare the parallel non-answers on event-request lead time ([#78](https://github.com/SinYang13/IS212-2026/discussions/78)) and equipment/technical-support notice period ([#25](https://github.com/SinYang13/IS212-2026/discussions/25), see [[technical-support]]).

## Open Points Summary

| # | Question | Customer position |
|---|---|---|
| 50 | Slot definition | Three slots, AM/PM/Night; hours listed but labels garbled; UX for team |
| 41 | Concurrent holds | One active hold or confirmed booking per venue + overlapping period |
| 39 | Hold duration / auto-release | Not specified — **team to propose** |
| 35 | Double-booking | Hard block, no override |
| 45 | Rejection with alternative | Informal suggestion; coordinator makes the change |
| 43 | Turnaround | Per-venue, recorded by Venue Staff |
| 46 | Operating hours | Venue Staff maintain; all days equal |
| 54 | Post-confirmation unavailability | Venue Staff update; coordinator reacts; automation **team to propose** |
| 57 | Booking horizon | Not specified — **team to propose** |

See also [[open-questions]], [[user-roles]], [[access-control]], [[reporting]].
