---
title: Venue Suitability
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
tags: [venue, suitability, search, filtering, capacity, room-layout, business-rules]
---

# Venue Suitability

Assessing whether a venue can satisfy an event's requirements. Distinct from the mechanics of holding and approving a booking, which are covered in [[venue-booking]].

## Search/Filtering vs Suitability Checking

The brief lists *Venue Search and Filtering* and *Venue Suitability Checking* as separate functionality areas without explaining the boundary. #83 asked whether filtering alone counts as the suitability check; the customer answered on 2026-08-31 that they are **two different functions** (Source: is212-discussions-2026-08-31, #83).

| | Search / Filtering | Suitability Checking |
|---|---|---|
| Purpose | **Narrow the list** of candidate venues | **Assess a selected or shortlisted venue** against current event requirements |
| Input | Criteria: date, time, capacity, accessibility, layout, facilities | The event's current requirements vs one venue's characteristics |
| Output | A list of potential venues | An indication of whether that venue can satisfy the event |
| Timing | Before selection | After a venue has been identified or selected |

> [!quote] Customer answer — #83
> Filtering is used to narrow the list of venues based on criteria such as date, time, capacity, accessibility, layout, and facilities. Suitability checking evaluates whether a selected or shortlisted venue can satisfy the current event requirements. (Source: is212-discussions-2026-08-31, #83)

The two are not redundant: filtering answers *"which venues might work?"*; suitability answers *"does this venue still work?"* — a question that must be re-askable long after the search is over, because requirements change (see [[change-impact-management]]).

> [!question]- Team decision required — how suitability is surfaced
> Presentation and implementation are explicitly left to the team (#83). The result "may be presented as an indication in the venue list or checked after selection" — i.e. an inline badge on search results, a check on selection, or both are all acceptable. The customer did not mandate a single UX.

## Suitability Factors

Drawn from the brief's list of factors on which venue suitability may depend (Source: customer-brief-no-1):

| Factor | Held on | Notes |
|---|---|---|
| Proposed date and time | Event | Evaluated against existing bookings, holds, blocks — see [[venue-booking]] |
| Maximum capacity | Venue | Compared to expected attendance |
| Supported room layouts | Venue | Must include the requested layout |
| Accessibility | Venue | Must meet the event's stated accessibility requirements |
| Available facilities | Venue | e.g. video-conferencing for hybrid events |
| Existing bookings | Venue calendar | Overlapping bookings/holds are a hard block (#35) |
| Operating hours | Venue | Maintained by Venue Staff; all days equal for now (#46) |
| Setup requirements | Event + venue | Preparation before the published start time |
| Turnaround time | Venue | Reset time after the event; recorded per venue (#43) |
| Venue unavailability | Venue | Maintenance, renovation, safety restrictions, internal activity |

## Capacity vs Expected Attendance

- The brief's own worked example: *"a venue should not normally be treated as suitable when expected attendance exceeds its capacity"* (Source: customer-brief-no-1).
- The word **"normally"** is the customer's; it was not tightened in either discussion snapshot. Whether over-capacity is a hard block or an overridable warning is therefore **unresolved**. Contrast [[venue-booking]] #35, where double-booking was explicitly confirmed as a hard block — no equivalent statement exists for capacity.
- Expected attendance and venue capacity are to be considered **throughout planning**, not only at initial selection (Source: customer-brief-no-1, *Event Capacity Management*).
- The brief's canonical impact example: raising attendance from 80 to 150 may make the already-selected venue unsuitable.
- Registration capacity is a related but separate limit — see [[attendee-registration]].

> [!question]- Team decision required — over-capacity: block or warn
> Whether exceeding capacity blocks a booking outright or raises an overridable warning, and who (if anyone) may override.

## Supported Room Layouts

- Event requirements may specify classroom, theatre, boardroom, banquet, exhibition, or another arrangement (Source: customer-brief-no-1).
- **Venue Staff record which layouts each venue supports** (Source: customer-brief-no-1, *Room Layout Management*).
- Layout is both a filter criterion and a suitability criterion (#83 lists it in both).
- Layout interacts with capacity: the same room seats different numbers under theatre vs banquet layout. The brief models capacity as a single venue attribute and does **not** state whether capacity varies by layout. This is a modelling gap for the team.

## Accessibility and Facilities

- Accessibility requirements are supplied by the Event Organiser at request time (Source: customer-brief-no-1) — see [[event-request-workflow]].
- Facilities requirements are frequently under-specified by organisers. The brief's example: an organiser requests a hybrid event without explaining what video-conferencing facilities are required; resolving this is the Event Coordinator's clarification duty (Step 4). The equivalent clarification path for equipment runs through the coordinator too — see [[technical-support]].

## Re-triggering Suitability

#83 asked explicitly whether the check should re-trigger on venue search, on coordinator submission, on updated event requirements, or all. The answer defined the two functions but did not enumerate trigger points, so the trigger set is inferred from the surrounding rules:

| Moment | Basis |
|---|---|
| On selecting/shortlisting a venue | #83 — suitability assesses a venue "after it has been identified or selected" |
| On submitting a venue booking request | Brief: the request carries "the relevant event timing and venue requirements for Venue Staff to review" |
| On a significant change to event requirements | #4 — changes to date/time, attendance, venue requirements are significant and "may invalidate confirmed arrangements"; #8 — changes may occur even after confirmation and require affected arrangements to be reviewed again |
| On a change to the venue itself | #54 — Venue Staff update availability; the coordinator must react |
| On reschedule / postponement with a new date | #16, #69 — arrangements must be reviewed again |

Significant changes requiring re-evaluation are handled under [[change-impact-management]]; outstanding arrangements roll up into [[event-readiness]].

> [!question]- Team decision required — re-evaluation triggers
> The exact trigger points and whether re-evaluation is automatic or coordinator-initiated. The customer said only that "the exact implementation is for you to propose" (#4) and gave no trigger list in #83.

## Snapshot Note

#83 does **not** appear in the Aug 26 snapshot (it was posted 2026-08-27 and answered 2026-08-31). The search/suitability distinction is therefore new information in [[is212-discussions-2026-08-31]] and supersedes any earlier assumption that filtering alone was sufficient.

See also [[venue-booking]], [[open-questions]], [[user-roles]], [[reporting]].
