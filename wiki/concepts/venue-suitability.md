---
title: Venue Suitability
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Sept 8.md
tags: [venue, suitability, search, filtering, capacity, room-layout, business-rules]
---

# Venue Suitability

**Objects:** [[venue]] · [[event]]

Assessing whether a venue can satisfy an event's requirements. Distinct from the mechanics of holding and approving a booking, which are covered in [[venue-booking]].

**Release 1 scope:** Venue suitability checking and venue search/filtering are both `🎯 Release 1` (core features 10–11) — see [[release-1-scope]] for why the customer treats them as genuinely separate functions. Room layout management (Supported Room Layouts, below) is `🧩 Adjacent`, pulled in by feature 8's catalogue attribute and feature 10's filter criterion, not a standalone layout-management feature. The ongoing, throughout-planning capacity tracking described under Event Capacity Management is `📋 Backlog` — only the point-in-time capacity check against expected attendance (feature 11) is required.

## Search/Filtering vs Suitability Checking

The brief lists *Venue Search and Filtering* and *Venue Suitability Checking* as separate functionality areas without explaining the boundary. [#83](https://github.com/SinYang13/IS212-2026/discussions/83) asked whether filtering alone counts as the suitability check; the customer answered on 2026-08-31 that they are **two different functions** (Source: is212-discussions-2026-08-31, [#83](https://github.com/SinYang13/IS212-2026/discussions/83)).

| | Search / Filtering | Suitability Checking |
|---|---|---|
| Purpose | **Narrow the list** of candidate venues | **Assess a selected or shortlisted venue** against current event requirements |
| Input | Criteria: date, time, capacity, accessibility, layout, facilities | The event's current requirements vs one venue's characteristics |
| Output | A list of potential venues | An indication of whether that venue can satisfy the event |
| Timing | Before selection | After a venue has been identified or selected |

> [!quote] Customer answer — [#83](https://github.com/SinYang13/IS212-2026/discussions/83)
> Filtering is used to narrow the list of venues based on criteria such as date, time, capacity, accessibility, layout, and facilities. Suitability checking evaluates whether a selected or shortlisted venue can satisfy the current event requirements. (Source: is212-discussions-2026-08-31, [#83](https://github.com/SinYang13/IS212-2026/discussions/83))

The two are not redundant: filtering answers *"which venues might work?"*; suitability answers *"does this venue still work?"* — a question that must be re-askable long after the search is over, because requirements change (see [[change-impact-management]]).

> [!question]- Team decision required — how suitability is surfaced
> Presentation and implementation are explicitly left to the team ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)). The result "may be presented as an indication in the venue list or checked after selection" — i.e. an inline badge on search results, a check on selection, or both are all acceptable. The customer did not mandate a single UX.

## Suitability Factors

Drawn from the brief's list of factors on which venue suitability may depend (Source: customer-brief-no-1):

| Factor | Held on | Notes |
|---|---|---|
| Proposed date and time | Event | Evaluated against existing bookings, holds, blocks — see [[venue-booking]] |
| Capacity for the requested layout | Venue | Capacity varies by layout; the figure for the layout the event requests is the one compared to expected attendance ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)) |
| Supported room layouts | Venue | Must include the requested layout — which also selects the capacity figure used above ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)) |
| Accessibility | Venue | Must meet the event's stated accessibility requirements |
| Available facilities | Venue | e.g. video-conferencing for hybrid events |
| Existing bookings | Venue calendar | Overlapping bookings/holds are a hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)) |
| Operating hours | Venue | Maintained by Venue Staff; all days equal for now ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)) |
| Setup requirements | Event + venue | Preparation before the published start time; one slot is the smallest supported unit ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)) |
| Turnaround time | Venue | Reset time after the event; recorded per venue ([#43](https://github.com/SinYang13/IS212-2026/discussions/43)), in whole booking slots ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)) |
| Venue unavailability | Venue | Maintenance, renovation, safety restrictions, internal activity |

## Capacity vs Expected Attendance

- **Which capacity figure applies is settled.** Venue capacity varies by room layout, and the capacity for each supported layout is provided as part of the venue attributes — the system is **not** required to estimate or calculate it. Suitability compares expected attendance against the **requested layout's** capacity, not against any venue-wide number, so the requested layout must be resolved before the comparison can be made (Source: is212-discussions-2026-09-08, [#112](https://github.com/SinYang13/IS212-2026/discussions/112)). The deployed model now holds that figure: the venue↔layout pairing carries its own `capacity` (migration D2, 2026-09-09 — see [[schema]]). See [[venue]] for the object-model consequence: capacity is no longer one number per venue.
- **A venue-level scalar still exists alongside it.** D2 added the per-layout column but left `venue.capacity` in place, so two capacity fields coexist with no stated precedence. Suitability reads the layout one ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)); whether the scalar becomes derived or is dropped is an open `🔶 Team decision` on [[venue]]. Any rule, report or query written against a bare `venue.capacity` is reading the wrong field.
- **Expected attendance is an event-level requirement**, held once and not duplicated inside the venue-requirements structure — but it is still what suitability, capacity and booking decisions are evaluated against (Source: is212-discussions-2026-09-08, [#119](https://github.com/SinYang13/IS212-2026/discussions/119)). The exact data model is the team's to propose, provided the figure stays consistent and is not maintained in two conflicting places ([#119](https://github.com/SinYang13/IS212-2026/discussions/119)).
- The brief's own worked example: *"a venue should not normally be treated as suitable when expected attendance exceeds its capacity"* (Source: customer-brief-no-1).
- The word **"normally"** is the customer's; it was not tightened in either discussion snapshot. Whether over-capacity is a hard block or an overridable warning is therefore **unresolved** — [#112](https://github.com/SinYang13/IS212-2026/discussions/112) fixed *which* capacity figure to compare against, not what happens when it is exceeded. Contrast [[venue-booking]] [#35](https://github.com/SinYang13/IS212-2026/discussions/35), where double-booking was explicitly confirmed as a hard block — no equivalent statement exists for capacity.
- Expected attendance and venue capacity are to be considered **throughout planning**, not only at initial selection (Source: customer-brief-no-1, *Event Capacity Management*).
- The brief's canonical impact example: raising attendance from 80 to 150 may make the already-selected venue unsuitable.
- Registration capacity is a related but separate limit — see [[attendee-registration]].

> [!question]- Team decision required — over-capacity: block or warn
> Whether exceeding capacity blocks a booking outright or raises an overridable warning, and who (if anyone) may override.

## Supported Room Layouts

- Event requirements may specify classroom, theatre, boardroom, banquet, exhibition, or another arrangement (Source: customer-brief-no-1).
- **Venue Staff record which layouts each venue supports** (Source: customer-brief-no-1, *Room Layout Management*).
- Layout is both a filter criterion and a suitability criterion ([#83](https://github.com/SinYang13/IS212-2026/discussions/83) lists it in both).
- **Layout determines capacity.** The same room seats different numbers under theatre vs banquet layout, and the customer has confirmed that capacity varies accordingly. The per-layout figures are supplied as venue attributes; the system does not derive them (Source: is212-discussions-2026-09-08, [#112](https://github.com/SinYang13/IS212-2026/discussions/112)). The brief's single-capacity reading is superseded.
- The **representation** is no longer open: Room Layout is a first-class object and each venue↔layout pairing carries its own capacity (migration D2 — see [[schema]]). What remains a `🔶 Team decision` on [[venue]] is the fate of the retained venue-level scalar ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)).
- **Which layout a given booking assumes is not recorded anywhere.** The requested layout survives only as free text on the event / event request; no [[booking]] column references a layout. The comparison [#112](https://github.com/SinYang13/IS212-2026/discussions/112) mandates therefore has a supplied left-hand side (expected attendance) and a stored right-hand side (per-layout capacity) with nothing joining them — see [[booking]].

## Accessibility and Facilities

- Accessibility requirements are supplied by the Event Organiser at request time (Source: customer-brief-no-1) — see [[event-request-workflow]].
- The Event Organiser may also express **venue-related preferences**, and may do so outside the system entirely. Identifying suitable venue options against those requirements is the Event Coordinator's job, and an organiser-stated preference — if the team chooses to capture one in the system at all — is a preference only: it is not a confirmed booking and does not override the suitability or approval process (Source: is212-discussions-2026-09-08, [#111](https://github.com/SinYang13/IS212-2026/discussions/111)). See [[venue-booking]].
- Facilities requirements are frequently under-specified by organisers. The brief's example: an organiser requests a hybrid event without explaining what video-conferencing facilities are required; resolving this is the Event Coordinator's clarification duty (Step 4). The equivalent clarification path for equipment runs through the coordinator too — see [[technical-support]].

## Re-triggering Suitability

[#83](https://github.com/SinYang13/IS212-2026/discussions/83) asked explicitly whether the check should re-trigger on venue search, on coordinator submission, on updated event requirements, or all. The answer defined the two functions but did not enumerate trigger points, so the trigger set is inferred from the surrounding rules:

| Moment | Basis |
|---|---|
| On selecting/shortlisting a venue | [#83](https://github.com/SinYang13/IS212-2026/discussions/83) — suitability assesses a venue "after it has been identified or selected" |
| On submitting a venue booking request | Brief: the request carries "the relevant event timing and venue requirements for Venue Staff to review" |
| On a significant change to event requirements | [#4](https://github.com/SinYang13/IS212-2026/discussions/4) — changes to date/time, attendance, venue requirements are significant and "may invalidate confirmed arrangements"; [#8](https://github.com/SinYang13/IS212-2026/discussions/8) — changes may occur even after confirmation and require affected arrangements to be reviewed again |
| On a change to the venue itself | [#54](https://github.com/SinYang13/IS212-2026/discussions/54) — Venue Staff update availability; the coordinator must react. [#120](https://github.com/SinYang13/IS212-2026/discussions/120) — an attribute update that affects a booking's suitability or availability flags that booking for review and notifies the coordinator, but does **not** revert its status |
| On reschedule / postponement with a new date | [#16](https://github.com/SinYang13/IS212-2026/discussions/16), [#69](https://github.com/SinYang13/IS212-2026/discussions/69) — arrangements must be reviewed again |

Significant changes requiring re-evaluation are handled under [[change-impact-management]]; outstanding arrangements roll up into [[event-readiness]].

> [!question]- Team decision required — re-evaluation triggers
> The exact trigger points and whether re-evaluation is automatic or coordinator-initiated. The customer said only that "the exact implementation is for you to propose" ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)) and gave no trigger list in [#83](https://github.com/SinYang13/IS212-2026/discussions/83).

## Snapshot Note

[#83](https://github.com/SinYang13/IS212-2026/discussions/83) does **not** appear in the Aug 26 snapshot (it was posted 2026-08-27 and answered 2026-08-31). The search/suitability distinction is therefore new information in [[is212-discussions-2026-08-31]] and supersedes any earlier assumption that filtering alone was sufficient.

[#112](https://github.com/SinYang13/IS212-2026/discussions/112) and [#119](https://github.com/SinYang13/IS212-2026/discussions/119) are newer still (answered 2026-09-07). Between them they supersede the capacity model this page previously described as an open modelling gap: capacity is per-layout and supplied, and expected attendance is a single event-level figure used in the comparison. The physical model caught up on 2026-09-09 — migration D2 gave the venue↔layout pairing its own `capacity` column ([[schema]]), so the per-layout figure is stored, not merely specified. Any earlier page or diagram treating `Venue.capacity` as the *only* capacity is stale; the scalar itself still exists in the deployed schema, but it is not what suitability reads.

See also [[venue-booking]], [[open-questions]], [[user-roles]], [[reporting]].
