---
title: Venue Unavailability
type: domain-object
sources:
  - "Week 7 Customer Changes.md"
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Sept 16.md"
  - "IS212 2026 Discussions Oct 1.md"
tags: [domain, venue, unavailability, release-1, week-7]
---

# Venue Unavailability

A period during which a [[venue]] is **temporarily** not available for events, recorded by Venue Staff for maintenance, equipment failure, renovation, safety concerns or another operational reason. It can be recorded **even when future events are already booked**: those bookings are identified, not cancelled ([[week-7-customer-changes|Week 7 C2]]). Process detail — how affected bookings are handled and replaced — lives in [[venue-booking]] and [[change-impact-management]]; this page is the structural reference.

`🎯 Release 1` as venue unavailability management ([[week-7-customer-changes|Week 7 C2]]); the customer's follow-up says it "was not in the 20 core and now included in the 26 core" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). It extends the Blocked state already on [[venue]] from the brief ([[event-planning-venue-booking-system|brief §4]]).

> [!warning] Not in the deployed schema
> [[schema]] has **no table** for venue blocks or unavailability today: `venue` carries no status and `booking_slot` rows (AM / PM / Night) belong to bookings and are themselves superseded by the 15-minute grid, so nothing stores "this venue is out of use from X to Y". A `venue_unavailability` table is only *proposed* — as part of migration 03 in [[week-7-change-impact-review]], which is not applied. The attribute table below describes the requirement and a proposed shape, not a deployed one.

## Attributes

`Scope` is the Release 1 marker from [[release-1-scope]].

| Field | Meaning | Status | Scope | Ref |
|---|---|---|---|---|
| Venue | The [[venue]] that is unavailable. One venue per record | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |
| Period | The time span the venue is out of use. The notice says only that the unavailability is **temporary** and may overlap booked dates | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |
| Period granularity | Blocking used to be by slot ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), superseded). On the 15-minute booking grid ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) a period is presumably a start/end time range on the same grid, which the review proposes as a timestamp range; no source states the unavailability granularity | 🔶 Team decision | 🎯 Release 1 | [#50](https://github.com/SinYang13/IS212-2026/discussions/50); [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Reason category | Exactly one of the five stated reasons: **maintenance**, **equipment failure**, **renovation**, **safety concerns**, **other operational reasons**. The brief's earlier list also named "internal activities" ([[event-planning-venue-booking-system\|brief §4]]); C2's five do not, so internal activity would fall under *other* — a team reading | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]]; [[event-planning-venue-booking-system\|brief §4]] |
| Recorded by | The [[user-account]] (Venue Staff) who marked the venue unavailable | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |
| Recorded at | When the unavailability was entered. Not stated for this record; follows from the auditability requirement (what changed, who, when) | ⚠️ Partial | 🎯 Release 1 | [[non-functional-requirements\|brief §8f]] |
| Lifted at | When Venue Staff end the unavailability early. Venue Staff can unblock a blocked venue; whether "lifted" and the period's end are separate facts is not stated | ⚠️ Partial | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]]; [[week-7-customer-changes\|Week 7 C2]] |
| Affected bookings | The bookings whose occupancy window overlaps the period. **Derived, not stored.** They are identified, and the relevant Event Coordinators informed that alternative arrangements are required | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |
| Original event information | Preserved while the Coordinator searches for and requests a replacement; the event is **not** automatically cancelled | ✅ Specified | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |
| Free-text reason note | An explanatory note, proposed for use under *other*. Not mentioned by the customer | ⚠️ Partial | 🎯 Release 1 | [[week-7-customer-changes\|Week 7 C2]] |

## States

The customer does not name states; these follow from "temporarily" and from Venue Staff's ability to unblock. They are this wiki's reading, not customer labels.

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| In force | The venue is unavailable for the period; new requests cannot use it and overlapping bookings are flagged | Entry: Venue Staff record it. Exit: the period ends, or Venue Staff lift it | [[week-7-customer-changes\|Week 7 C2]] |
| Lifted | Venue Staff ended the unavailability before its period ran out | Terminal. The venue is available again; bookings already flagged are not reverted automatically | [[event-planning-venue-booking-system\|brief §4]] |

Once the period has simply elapsed the record is history. Whether that needs a state of its own is the team's.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[venue]] | many : 1 | A venue accumulates many unavailability periods over time; each record belongs to one venue | [[week-7-customer-changes\|Week 7 C2]] |
| [[booking]] | many : many (derived, not stored) | **Clash detection.** A booking whose padded occupancy window overlaps an in-force period is identified for review and its Event Coordinator informed; it is not cancelled. A new request into an in-force period is not available | [[week-7-customer-changes\|Week 7 C2]] |
| [[user-account]] (Venue Staff) | many : 1 | The Venue Staff member who recorded it | [[week-7-customer-changes\|Week 7 C2]] |
| [[event]] | many : many (via booking) | An affected event keeps its record and original information; the Coordinator searches for and requests a replacement venue | [[week-7-customer-changes\|Week 7 C2]] |
| [[notification]] | 1 : many | Recording one informs the relevant Event Coordinators that alternative arrangements are required — see [[notifications]] | [[week-7-customer-changes\|Week 7 C2]]; [[notifications\|brief §6]] |

## Binding rules

- **Venue Staff** mark a venue temporarily unavailable, for maintenance, equipment failure, renovation, safety concerns or other operational reasons, even when future events are already booked ([[week-7-customer-changes|Week 7 C2]]).
- **The existing event is never automatically cancelled.** Affected bookings are identified and the relevant Event Coordinators informed that alternative arrangements are required ([[week-7-customer-changes|Week 7 C2]]).
- **The original event information is preserved**, and the Coordinator can **search for and request a replacement venue** in the system ([[week-7-customer-changes|Week 7 C2]]). This extends [#122](https://github.com/SinYang13/IS212-2026/discussions/122), which let the alternative be agreed outside the system and the Coordinator then update the booking; it does not void it.
- A **replacement** is an ordinary booking request — venue search, suitability and Venue Staff approval apply as usual — linked to the booking it replaces. What status the **original** booking ends in is open: see below.
- Clash detection runs over each booking's padded window, including the venue's setup and turnaround time ([[week-7-customer-changes|Week 7 C1]]).
- Where an event has several venue bookings ([[week-7-customer-changes|Week 7 C3]]), only the bookings that overlap the period are flagged; the event's others are untouched.

## Unavailability is not retirement

| | Unavailability | Retirement |
|---|---|---|
| Duration | Temporary; ends or is lifted | Permanent |
| Source | [[week-7-customer-changes\|Week 7 C2]] | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| Representation | A period record on this page | Unresolved: a venue status (`retired_at`), or an open-ended unavailability row. Not in [[schema]] (gap E2) |
| Affected bookings | Identified; Coordinator informed; replacement requested in-system | Identified; Coordinator informed; alternative agreed with Venue Staff, possibly outside the system |
| Release scope | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C2]]) | 🧩 Adjacent (features 10, 12) |

Creating this record does **not** close the retirement gap: the two stay distinct, and the representation of retirement is still the team's to decide ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)).

## Open modelling questions

> [!question]- Team decision required — destination of a flagged booking and the original's end state
> This is the fifth instance of the flag-for-review pattern after [#120](https://github.com/SinYang13/IS212-2026/discussions/120), [#54](https://github.com/SinYang13/IS212-2026/discussions/54) and [#122](https://github.com/SinYang13/IS212-2026/discussions/122) (the buffer change under [[week-7-customer-changes|Week 7 C1]] is the fourth). The notice says affected bookings are "identified" but not what state a flagged booking enters, nor what status the **original** ends in once a replacement is confirmed (Released, or a distinct value) — still a team decision ([#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#120](https://github.com/SinYang13/IS212-2026/discussions/120), [#122](https://github.com/SinYang13/IS212-2026/discussions/122)). The review *proposes* a review flag on the booking (`review_reason` / `review_flagged_at`) and a replacement link, not a new status. That is a team proposal, not customer fact ([[week-7-change-impact-review]]).

> [!question]- Team decision required — period granularity and enforcement
> Slot-granular blocking was the earlier reading ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), superseded); a timestamp range on the 15-minute grid ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) is what [[week-7-customer-changes|Week 7 C1]]'s minute-level buffers need. Enforcement is split across two tables, so a database exclusion constraint cannot hold it; the review proposes a trigger that refuses new bookings into an in-force period. Neither is customer-specified.

See [[venue-booking]], [[booking]], [[change-impact-management]] and [[domain-model]] for the full object catalogue.
