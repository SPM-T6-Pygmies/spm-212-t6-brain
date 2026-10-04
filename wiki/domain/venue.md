---
title: Venue
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Sept 16.md"
  - "IS212 2026 Discussions Oct 1.md"
  - "Week 7 Customer Changes.md"
tags: [domain, venue]
---

# Venue

A physical event space owned and operated by [[connectsphere-event-services]], catalogued by Venue Staff and offered for booking against an [[event]] or [[session]]. All venues are located in Singapore ([#36](https://github.com/SinYang13/IS212-2026/discussions/36)). Process detail for search, filtering, suitability assessment and approval lives in [[venue-booking]] and [[venue-suitability]]; this page is the structural reference.

## Attributes

`Scope` is the Release 1 marker from [[release-1-scope]] for that attribute's functionality area.

| Field | Meaning | Status | Scope | Ref |
|---|---|---|---|---|
| Location | Facility/site; every venue is in Singapore | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]]; [#36](https://github.com/SinYang13/IS212-2026/discussions/36) |
| Capacity (per room layout) | Maximum attendance the venue can hold **in a given layout** — supplied per supported layout as venue master data, not estimated or calculated by the system. Held on the venue↔layout pairing in the deployed model (`venue_supported_layout.capacity`, migration D2 — see [[schema]]) | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]]; [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| Facilities | e.g. video-conferencing, AV fit-out | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]] |
| Accessibility | Accessibility features of the space | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]] |
| Supported room layouts | Which of classroom, theatre, boardroom, banquet, exhibition or another arrangement the venue supports, recorded by Venue Staff — each supported layout carries its own capacity | ✅ Specified | 🧩 Adjacent (features 8, 10) | [[event-planning-venue-booking-system\|brief §4]]; [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| Operating hours | Maintained by Venue Staff; no day-of-week variation | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]]; [#46](https://github.com/SinYang13/IS212-2026/discussions/46) |
| Operating hours vs the 15-minute grid | Whether operating hours now simply bound a day, and whether AM / PM / Night (7am–12pm, 1pm–6pm, 7pm–12am in [#50](https://github.com/SinYang13/IS212-2026/discussions/50)) survive as a display grouping or as operating-hour windows. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) is silent; the old 12pm–1pm and 6pm–7pm gaps have no stated meaning on a grid | 🔶 Team decision | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C1]]) | [#46](https://github.com/SinYang13/IS212-2026/discussions/46); [#50](https://github.com/SinYang13/IS212-2026/discussions/50) |
| Setup time | Per-venue, **configurable** time needed to set the venue up before an event. Included when checking availability and detecting conflicts. Example: 30 min setup before a 10:00 event starts occupancy at 9:30. Deployed as `venue.setup_time_minutes` (live again — see [[schema]]) | ✅ Specified | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C1]]) | [[event-planning-venue-booking-system\|brief §4]]; [#43](https://github.com/SinYang13/IS212-2026/discussions/43); [[week-7-customer-changes\|Week 7 C1]] |
| Turnaround time | Per-venue, **configurable** time needed to reset the venue after an event. Included in availability and conflict checks. Example: 45 min turnaround after a 12:00 end holds the venue to 12:45. Deployed as `venue.turnaround_time_minutes` | ✅ Specified | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C1]]) | [[event-planning-venue-booking-system\|brief §4]]; [#43](https://github.com/SinYang13/IS212-2026/discussions/43); [[week-7-customer-changes\|Week 7 C1]] |
| Setup / turnaround unit | The values are **minutes**, evidenced only by the worked example (10:00–12:00 + 30 + 45 ⇒ 9:30–12:45). The smallest unit of booking is 15 minutes ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)); whether setup/turnaround values must be multiples of 15 is not stated (30 and 45 are — inference only) | ⚠️ Partial | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C1]]) | [[week-7-customer-changes\|Week 7 C1]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Setup / turnaround when not configured | Whether a `null` (or unset) value means 0 or "not configured", and what the availability check does in that case | 🔶 Team decision | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C1]]) | [#43](https://github.com/SinYang13/IS212-2026/discussions/43); [[event-planning-venue-booking-system\|brief §4]] |
| Availability (per time window) | Available / tentatively held (unexpired) / confirmed / blocked / otherwise unavailable, on the 15-minute grid — see States | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]]; [[week-7-customer-changes\|Week 7 C1]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Venue-level capacity scalar | The deployed model **retains** `venue.capacity` alongside the new per-layout figure, so capacity now sits in two places with no stated precedence. Suitability reads the layout figure ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)); whether the scalar becomes derived (max across supported layouts) or is dropped is unresolved — see [[schema]] and the question below | 🔶 Team decision | 🧩 Adjacent (features 8, 10) | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| Temporary unavailability | Venue Staff may mark the venue **temporarily unavailable** (maintenance, equipment failure, renovation, safety concerns, other operational reasons), even where future events are already booked. Recorded as a period on [[venue-unavailability]], not as a venue status | ✅ Specified | 🎯 Release 1 ([[week-7-customer-changes\|Week 7 C2]]) | [[week-7-customer-changes\|Week 7 C2]] |
| Venue status (Active / Retired) | Whether the venue is still offered. A **retired** venue (permanent — unlike temporary unavailability) may still carry future bookings, which are not cancelled or auto-rescheduled but flagged for review ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). The customer presumes retirement exists without mandating a feature; the status names and who may set them are the team's. **Not in the deployed [[schema]]** | 🔶 Team decision | 🧩 Adjacent (features 10, 12) | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| Booking horizon | How far in advance a venue may be booked | 🔶 Team decision | 📋 Backlog | [#57](https://github.com/SinYang13/IS212-2026/discussions/57) |

> [!warning] Contradiction — [[week-7-customer-changes|Week 7 C1]] vs [#50](https://github.com/SinYang13/IS212-2026/discussions/50)/[#107](https://github.com/SinYang13/IS212-2026/discussions/107)/[#108](https://github.com/SinYang13/IS212-2026/discussions/108)/[#123](https://github.com/SinYang13/IS212-2026/discussions/123)
> The standing rules — three booking slots ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)), whole-slot consumption ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)), a slot as the smallest unit of setup and turnaround ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)), at most one adjacent slot per buffer with no stored length ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)) — are contradicted by [[week-7-customer-changes|Week 7 C1]]: each venue has a configurable setup and turnaround *time* (example 9:30–12:45), and [#128](https://github.com/SinYang13/IS212-2026/discussions/128) replaces the three slots with a **15-minute smallest unit of booking**. The later, explicitly Release 1 notice wins, so the retained `setup_time_minutes` / `turnaround_time_minutes` columns are live again and the slot model is history, kept under Superseded below. [#126](https://github.com/SinYang13/IS212-2026/discussions/126) (the 12nn–1pm gap) is unanswered, likely moot, unconfirmed.

> [!success] Resolved — [#112](https://github.com/SinYang13/IS212-2026/discussions/112)
> Capacity is no longer one number per venue. It **varies by room layout**, and the figure for each supported layout is provided as part of the venue attributes, so the system is not required to estimate or calculate it; the layout-specific figure is the one suitability evaluates against. This is a change to the object model, not a presentation detail: every rule that compares expected attendance to "capacity" must first resolve *which layout* the event requests. The representation is settled too: the venue↔layout pairing carries its own `capacity` column (migration D2, 2026-09-09 — see [[schema]]). What is still open is narrower — the retained venue-level scalar's fate, below.

> [!warning] Capacity threshold never tightened
> The brief's own worked rule is hedged: "a venue should **not normally** be treated as suitable when expected attendance exceeds its capacity" ([[event-planning-venue-booking-system|brief §4]]). The word "normally" was never tightened in either discussion snapshot, so whether over-capacity is a hard block or an overridable warning is unresolved. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) fixed *which* capacity figure the comparison uses, not what happens when it is exceeded. Contrast the Booking rule below, where double-booking *was* explicitly confirmed as a hard block. Full discussion at [[venue-suitability]].

### Booking unit

Venues are booked on a **15-minute grid**: the smallest unit of booking is 15 minutes, replacing the three slots ([[week-7-customer-changes|Week 7 C1]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)). A booking's occupancy is its event time plus the venue's setup time before and turnaround time after — see [[venue-booking]].

### Superseded — the three-slot model

Kept as history. Replaced by [[week-7-customer-changes|Week 7 C1]] and [#128](https://github.com/SinYang13/IS212-2026/discussions/128); none of the below is the current rule.

~~Venues are booked in three discrete daily slots; a full-day booking uses all three ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)). A slot is consumed whole: an event across two slots consumes both in full ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)). One slot is the smallest unit of setup and turnaround ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)); setup and turnaround each occupy the immediately adjacent slot, including across days ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)).~~

| ~~Slot (as named in the answer)~~ | ~~Hours (as listed)~~ |
|---|---|
| AM | 7am – 12pm |
| *(unlabelled)* | 1pm – 6pm |
| *(unlabelled)* | 7pm – 12am |

> [!warning] Source text for [#50](https://github.com/SinYang13/IS212-2026/discussions/50) is garbled (superseded history)
> The customer's answer reads verbatim: *"The venue will be booked based on 3 time slots (AM, PM, Night) if full day will be all 3 slots. AM (7am-12pm, 1pm-6pm, 7pm-12am)"*. Three time ranges are given but only the first is labelled "AM". The customary reading maps the remaining two ranges to PM and Night in the order the slots were named in the opening sentence, but **that mapping is inferred, not stated** — it is not recorded as fact in the table above. The unbooked gaps at 12pm–1pm and 6pm–7pm are also unexplained (break, changeover, transcription slip, or space for setup/turnaround is not stated). Do not hard-code the missing labels or gap purpose without confirming with the customer.
>
> [#123](https://github.com/SinYang13/IS212-2026/discussions/123) (2026-09-16) partly corroborates the *order*: the customer's own answer treats AM → PM → Night as the chronological sequence and Night → next-day AM as adjacent. That supports the inferred mapping of the unlabelled ranges to PM and Night by position. It still supplies no label for any range and no reason for the gaps, so the table above is deliberately unchanged.
>
> [[week-7-customer-changes|Week 7 C1]]'s example ends at 12:45pm, inside the 12pm–1pm gap, which first hinted that the gaps might host buffers (inference only). With the 15-minute grid ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) there is no slot gap to book across, so [#126](https://github.com/SinYang13/IS212-2026/discussions/126) (whether anything may be booked in the gap) is **likely moot, unconfirmed**; it is still unanswered.

## States

Availability status of a venue's time window on the 15-minute grid ([#128](https://github.com/SinYang13/IS212-2026/discussions/128); [[event-planning-venue-booking-system|brief §4]], *Venue Availability Calendar*). This is the status of a venue's **time**, not a lifecycle of the Venue record itself (the venue's own Active / Retired status is a separate attribute — see Attributes) — transitions between these are driven by the occupying [[booking]]; see [[booking]] for the tentative-hold lifecycle.

Available and Confirmed sit under the `🎯 Release 1` venue availability calendar and booking flow (features 9, 12–14). Blocked is `🎯 Release 1` as venue unavailability management ([[week-7-customer-changes|Week 7 C2]]). Tentatively held is `🎯 Release 1` ([[week-7-customer-changes|Week 7 C4]]): a hold carries an expiry and stops blocking when it passes. See [[release-1-scope]].

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Available | No hold, booking or block occupies the time | Default | [[event-planning-venue-booking-system\|brief §4]] |
| Tentatively held | A [[booking]] holds the time pending confirmation; the hold has an expiry date/time | Entry: hold placed. Exit: confirmed, released, or **expired** (the time is then Available, and the expired hold is never a confirmed booking) — see [[booking]] | [[event-planning-venue-booking-system\|brief §4]]; [#41](https://github.com/SinYang13/IS212-2026/discussions/41); [[week-7-customer-changes\|Week 7 C4]] |
| Confirmed | A [[booking]] has been approved for the time | Entry: booking confirmed. Exit: released on event completion, cancellation, or postponement without a new date | [[event-planning-venue-booking-system\|brief §4]]; [#69](https://github.com/SinYang13/IS212-2026/discussions/69) |
| Blocked | Venue Staff have blocked the time or period | Entry: Venue Staff block (maintenance, equipment failure, renovation, safety concerns, other operational reasons) — allowed even over booked dates, without cancelling those bookings; see [[venue-unavailability]]. Exit: Venue Staff unblock | [[event-planning-venue-booking-system\|brief §4]]; [[week-7-customer-changes\|Week 7 C2]] |
| Otherwise unavailable | Catch-all named in the brief but not further detailed | Not specified | [[event-planning-venue-booking-system\|brief §4]] |

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[booking]] | 1 : many | A venue accumulates many bookings/holds over time, but at most one active (unexpired) hold or confirmed booking may occupy the same venue for an overlapping period, judged over each booking's window including this venue's setup and turnaround | [#41](https://github.com/SinYang13/IS212-2026/discussions/41); [[week-7-customer-changes\|Week 7 C1]]; [[week-7-customer-changes\|Week 7 C4]] |
| [[event]] | many : many (via booking) | A venue hosts many events over time. One event may hold bookings at **several venues at the same time**, each checked independently ([[week-7-customer-changes\|Week 7 C3]]); a multi-session event may also use a venue per session (`📋 Backlog`). ~~Normally only one venue booking is confirmed per event/session~~ — retired | [[event-planning-venue-booking-system\|brief §4]]; [#74](https://github.com/SinYang13/IS212-2026/discussions/74); [[week-7-customer-changes\|Week 7 C3]] |
| [[session]] | 1 : many (via booking) | Each session of a multi-session event may book its own venue, independent of sibling sessions | [#74](https://github.com/SinYang13/IS212-2026/discussions/74) |
| [[venue-unavailability]] | 1 : many | A venue accumulates many unavailability periods over time; each period belongs to one venue and may overlap booked dates | [[week-7-customer-changes\|Week 7 C2]] |
| [[user-account]] (Venue Staff) | many : many | Venue Staff maintain venue master data; access to venue information is organisation-wide, not scoped to specific venues | [#66](https://github.com/SinYang13/IS212-2026/discussions/66) |

## Binding rules

- Venue capacity is held **per supported room layout** and is supplied as venue master data; the system neither estimates nor calculates it, and suitability compares expected attendance against the layout the event requests ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)). The deployed model stores that figure on the venue↔layout pairing; the separately retained `venue.capacity` scalar is **not** the figure suitability reads — see [[schema]].
- Each venue has a **configurable setup time and turnaround time**, included when checking availability and detecting conflicts; an event 10:00–12:00 with 30 min setup and 45 min turnaround occupies 9:30–12:45 ([[week-7-customer-changes|Week 7 C1]]). Bookings that become problematic when a venue's values change are **identified, not silently removed**.
- ~~Setup and turnaround each occupy at most one full slot, the one immediately adjacent to the booking, crossing midnight; no stored duration~~ — **superseded** by [[week-7-customer-changes|Week 7 C1]] ([#123](https://github.com/SinYang13/IS212-2026/discussions/123), [#108](https://github.com/SinYang13/IS212-2026/discussions/108)); the three-slot model ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107)) goes with it. [#43](https://github.com/SinYang13/IS212-2026/discussions/43)'s per-venue reading is restored.
- Marking a venue **temporarily unavailable** is allowed even where future events are booked; it never cancels them. Affected bookings are identified and the Coordinators informed ([[week-7-customer-changes|Week 7 C2]]). See [[venue-unavailability]].
- Retiring a venue does **not** cancel or auto-reschedule its future bookings: they are identified for review, the relevant Event Coordinators are notified, and the Coordinator and Venue Staff agree an alternative — possibly outside the system — after which the Coordinator updates the booking ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). See [[booking]] and [[venue-booking]].
- Updating a venue attribute does **not** automatically revert dependent bookings to Requested. Where the change affects an existing booking's suitability or availability, that booking is identified for review and the Event Coordinator notified; the resulting status transition is the team's to propose ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)) — see [[booking]].
- Operating hours are maintained by Venue Staff and have no day-of-week variation; there is no weekend/holiday distinction and no holiday calendar — "all days are equal for now" ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)).
- All venues are located in Singapore, and the system uses Singapore time throughout ([#36](https://github.com/SinYang13/IS212-2026/discussions/36)).
- Venue Staff access to venue information is **not** scoped by venue or location — it is organisation-wide, driven by role rather than assignment ([#66](https://github.com/SinYang13/IS212-2026/discussions/66)).
- There is no existing venue database to import — the venue catalogue starts fresh, and no data migration or system-integration requirement exists ([#12](https://github.com/SinYang13/IS212-2026/discussions/12)).
- **Filtering** (narrowing the candidate list by date, time, capacity, accessibility, layout, facilities) and **suitability checking** (assessing whether an already-shortlisted venue still satisfies the event's current requirements) are two distinct functions, not one — full treatment in [[venue-suitability]] ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)).
- A venue (or a period of it) may be blocked by Venue Staff for maintenance, renovation, safety restrictions, other internal activity, or other operational reasons ([[event-planning-venue-booking-system|brief §4]]).

## Open modelling questions

> [!question]- Team decision required — booking horizon
> No limit on how far in advance a venue may be booked has been specified. Propose an approach only if the solution requires one ([#57](https://github.com/SinYang13/IS212-2026/discussions/57)).

> [!question]- Team decision required — suitability-check presentation
> Whether the suitability result is shown as an inline indicator on the venue list, checked explicitly after selection, or both, is left to the team; the customer confirmed only that both are acceptable presentations ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)). Full discussion at [[venue-suitability]].

> [!question]- Team decision required — setup / turnaround values
> [[week-7-customer-changes|Week 7 C1]] makes the per-venue setup and turnaround time the requirement, so the deployed columns are read again. Still the team's, citing [#43](https://github.com/SinYang13/IS212-2026/discussions/43) and [[event-planning-venue-booking-system|brief §4]]: what a `null` value means (0, or "not configured" and so unbookable or unchecked); whether the unit is fixed at minutes (only the worked example shows it) and whether the values must be multiples of the 15-minute booking unit ([#128](https://github.com/SinYang13/IS212-2026/discussions/128) is silent); and what a buffer does at the edge of operating hours or against a Blocked period. Whether buffers show on the availability calendar and count in utilisation reports is open too — see [[venue-booking]].

> [!question]- Team decision required — venue retirement
> Whether Release 1 builds a retire action at all, the status vocabulary, who may retire a venue, and whether retirement is immediate or date-effective are all unspecified; the customer left "the exact workflow and status transitions" to the team ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). Whether a retired venue must disappear from search and booking requests is *implied* rather than stated. Retirement is permanent; temporary unavailability ([[venue-unavailability]]) is a separate, period-based record. Full discussion at [[venue-booking]].

> [!question]- Team decision required — fate of the retained venue-level capacity scalar
> How layout-specific capacity is *represented* is no longer open: Room Layout is a first-class object and the venue↔layout pairing carries its own `capacity` (migration D2 — see [[schema]]). What that migration did **not** do is remove `venue.capacity`, so the model now holds capacity twice with no precedence rule written down. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) fixes only that suitability reads the **layout-specific** figure; whether the scalar becomes a derived value (max across supported layouts), is dropped, or survives as an independent number — and which figure wins while both are writable and can disagree — is the team's to propose ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)). Either way the layout set stays Venue Staff-maintained (classroom, theatre, boardroom, banquet, exhibition, other) and serves as both a filter and a suitability criterion ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)) — see [[venue-suitability]].

See [[domain-model]] for the full object catalogue.
