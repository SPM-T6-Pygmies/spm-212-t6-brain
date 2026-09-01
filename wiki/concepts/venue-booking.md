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

How [[connectsphere-event-services]] venues are catalogued, held, requested, approved, and protected from double-booking in the [[event-planning-venue-booking-system]]. Covers steps 6–7 of the planning process (see [[event-lifecycle]]). Assessment of whether a given venue *fits* an event is covered separately in [[venue-suitability]].

## Venue Catalogue

Venue Staff own venue master data (Source: customer-brief-no-1).

| Attribute | Notes |
|---|---|
| Location | Facility/site; all venues are owned and operated by ConnectSphere in Singapore |
| Capacity | Maximum attendance; drives suitability (see [[venue-suitability]]) |
| Facilities | e.g. video-conferencing, AV fit-out |
| Accessibility | Accessibility features of the space |
| Supported room layouts | classroom, theatre, boardroom, banquet, exhibition, other |
| Operating hours | Maintained by Venue Staff |
| Setup / teardown / turnaround time | Recorded **per venue** (#43) |

All venues are in Singapore and the system operates on Singapore time (Source: is212-discussions-2026-08-31, #36) — see [[non-functional-requirements]].

## Three-Slot Booking Model

Venues are booked in **three fixed daily time slots — AM, PM, Night**. A full-day booking is all three slots. Partial blocking (e.g. mornings only during a renovation) is expressed by blocking individual slots (Source: is212-discussions-2026-08-31, #50).

| Slot | Hours (as read) |
|---|---|
| AM | 7am – 12pm |
| PM | 1pm – 6pm |
| Night | 7pm – 12am |

> ⚠️ **Source ambiguity — slot labels.** The customer's answer to #50 reads verbatim: *"The venue will be booked based on 3 time slots (AM, PM, Night) if full day will be all 3 slots. AM (7am-12pm, 1pm-6pm, 7pm-12am)"*. Three time ranges are listed but only the **first** is labelled ("AM"). The table above applies the obvious positional mapping (AM / PM / Night in the order the slots were named), but this mapping is **inferred, not stated**. Note also the unbooked gaps at 12pm–1pm and 6pm–7pm, whose purpose (break, changeover, or a transcription slip) is not explained; they may or may not be intended to absorb setup and turnaround. Both points should be confirmed with the customer before the slot model is hard-coded.

**Team decision required:** the exact UX and implementation of slot selection are left open (#50) — including whether slots are stored as an enum or as concrete start/end timestamps, and how a multi-day or [[event-lifecycle|multi-session]] event maps onto slots.

## Booking Request and Approval

1. The Event Coordinator identifies candidate venues (search, filter, suitability check — see [[venue-suitability]]).
2. The Event Coordinator submits a **venue booking request** carrying the event timing and venue requirements (Source: customer-brief-no-1).
3. **Venue Staff** review and decide. They may approve, reject, or reject with a suggested alternative venue or arrangement (Source: customer-brief-no-1; is212-discussions-2026-08-31, #45).

Rejection with an alternative is **not** a formal counter-offer object in the system. Venue Staff suggest; **the Event Coordinator then makes the changes** and re-requests (Source: is212-discussions-2026-08-31, #45). This keeps the coordinator as the single point of control over event arrangements, consistent with [[event-request-workflow]].

Booking request, approval, rejection, and change are all notification triggers (see [[notifications]]).

## Tentative Holds

A venue may be temporarily held while an event is still being finalised (Source: customer-brief-no-1).

| Rule | Status |
|---|---|
| Only **one** active tentative hold *or* confirmed booking may exist for the same venue and overlapping time period | Specified (#41) |
| A tentative hold may later be confirmed or released | Specified (#41) |
| Hold duration / expiry period | **Not specified** (#39, #41) |
| Auto-release mechanism | **Not specified** (#39, #41) |
| Whether hold expiry is configurable | **Not specified** (#39) |

> **Team decision required:** expiry period and release mechanism for tentative holds are explicitly for the team to propose (Source: is212-discussions-2026-08-31, #39 and #41). This matters because a hold that never expires is functionally a confirmed booking and will silently block the venue — a stated pain point in the brief ("cancelled events may continue to occupy venue or equipment reservations").

Competing holds are therefore impossible by rule: the first hold wins the slot until it is confirmed or released. Multiple coordinators cannot shortlist the same slot in parallel.

## Conflict Detection and Double-Booking

The system must **identify and block** incompatible or overlapping bookings (Source: is212-discussions-2026-08-31, #35).

- This is a **hard block**, not a warning.
- The question explicitly offered a soft-warning-with-override option (override by Venue Staff for VIP organisations); the customer did **not** take it.
- No override or exception path for privileged clients is authorised. Any such path would need fresh clarification.

Overlap detection must account for setup and turnaround time, not only the published event start and end times (Source: customer-brief-no-1). See [[venue-suitability]] and [[event-readiness]].

## Setup and Turnaround Time

- Venue Staff record **setup, teardown and turnaround requirements per venue** (Source: is212-discussions-2026-08-31, #43).
- It is a per-venue attribute, not a global constant and not (currently) variable by event type or size.
- Insufficient turnaround between consecutive events is a named problem in the current manual process (Source: customer-brief-no-1).
- Extending an event may invalidate the turnaround before the next booking — a [[change-impact-management]] trigger.

Note the tension with the fixed three-slot model: if slots are indivisible, per-venue turnaround may be partly absorbed by the 12pm–1pm and 6pm–7pm gaps. Whether per-venue turnaround adds *on top of* slot boundaries is not stated and should be resolved by the team.

## Venue Unavailability and Blocking

- Venue Staff can block venues for maintenance, renovation, safety issues, internal activities, or other operational reasons (Source: customer-brief-no-1).
- Blocking is at slot granularity, enabling partial-day unavailability (#50).
- When a venue becomes unavailable **after** a booking is confirmed, Venue Staff are responsible for updating availability. The availability change affecting an upcoming event is a notification scenario, and **the Event Coordinator must then make the changes** (Source: is212-discussions-2026-08-31, #54).

> **Team decision required:** automation of the response to a post-confirmation venue loss (auto-release, auto-flag, auto-suggest alternatives) is for the team to propose (#54).

## Operating Hours

- Maintained by **Venue Staff** as part of venue information (Source: is212-discussions-2026-08-31, #46).
- **All days are treated as equal for now** — no day-of-week variation, no weekend/public-holiday distinction, no holiday calendar required (#46).

## Booking Horizon

- **No booking horizon has been specified** (Source: is212-discussions-2026-08-31, #57).
- **Team decision required:** propose one only if the solution requires it. Compare the parallel non-answers on event-request lead time (#78) and equipment/technical-support notice period (#25, see [[technical-support]]).

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
