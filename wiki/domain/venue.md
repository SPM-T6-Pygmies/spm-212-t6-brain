---
title: Venue
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Sept 16.md"
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
| Setup / turnaround buffer rule | Setup needs **at most one full slot** before the event and turnaround **at most one full slot** after, each the **immediately adjacent** slot, crossing midnight where needed (AM setup → previous Night; Night turnaround → next AM). No multi-slot or multi-day buffers, no user selection of the buffer slot. A rule about neighbouring slots, not a stored duration — see Booking slots below | ✅ Specified | 🧩 Adjacent (feature 14) | [[event-planning-venue-booking-system\|brief §4]]; [#108](https://github.com/SinYang13/IS212-2026/discussions/108); [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| Setup / turnaround requirement (venue-level) | Whether bookings at this venue consume a setup and/or turnaround slot. [#43](https://github.com/SinYang13/IS212-2026/discussions/43) made setup, teardown and turnaround per-venue values; [#123](https://github.com/SinYang13/IS212-2026/discussions/123) fixed their size at one slot but never says whether the requirement is per venue, per event, or universal. The deployed `setup_time_minutes` / `turnaround_time_minutes` columns describe a duration the customer no longer needs — see [[schema]] | ⚠️ Partial | 🧩 Adjacent (feature 14) | [#43](https://github.com/SinYang13/IS212-2026/discussions/43); [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| Availability (per slot) | Available / tentatively held / confirmed / blocked / otherwise unavailable — see States | ✅ Specified | 🎯 Release 1 | [[event-planning-venue-booking-system\|brief §4]] |
| Venue-level capacity scalar | The deployed model **retains** `venue.capacity` alongside the new per-layout figure, so capacity now sits in two places with no stated precedence. Suitability reads the layout figure ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)); whether the scalar becomes derived (max across supported layouts) or is dropped is unresolved — see [[schema]] and the question below | 🔶 Team decision | 🧩 Adjacent (features 8, 10) | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| Venue status (Active / Retired) | Whether the venue is still offered. A **retired** venue may still carry future bookings, which are not cancelled or auto-rescheduled but flagged for review ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). The customer presumes retirement exists without mandating a feature; the status names and who may set them are the team's. **Not in the deployed [[schema]]** | 🔶 Team decision | 🧩 Adjacent (features 10, 12) | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| Booking horizon | How far in advance a venue may be booked | 🔶 Team decision | 📋 Backlog | [#57](https://github.com/SinYang13/IS212-2026/discussions/57) |

> [!success] Resolved — [#112](https://github.com/SinYang13/IS212-2026/discussions/112)
> Capacity is no longer one number per venue. It **varies by room layout**, and the figure for each supported layout is provided as part of the venue attributes, so the system is not required to estimate or calculate it; the layout-specific figure is the one suitability evaluates against. This is a change to the object model, not a presentation detail: every rule that compares expected attendance to "capacity" must first resolve *which layout* the event requests. The representation is settled too: the venue↔layout pairing carries its own `capacity` column (migration D2, 2026-09-09 — see [[schema]]). What is still open is narrower — the retained venue-level scalar's fate, below.

> [!warning] Capacity threshold never tightened
> The brief's own worked rule is hedged: "a venue should **not normally** be treated as suitable when expected attendance exceeds its capacity" ([[event-planning-venue-booking-system|brief §4]]). The word "normally" was never tightened in either discussion snapshot, so whether over-capacity is a hard block or an overridable warning is unresolved. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) fixed *which* capacity figure the comparison uses, not what happens when it is exceeded. Contrast the Booking rule below, where double-booking *was* explicitly confirmed as a hard block. Full discussion at [[venue-suitability]].

### Booking slots

Venues are booked in three discrete daily slots; a full-day booking uses all three ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)). A slot is consumed whole: an event that runs across two slots consumes **both slots in full** ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)), and one slot is also the smallest unit of setup and turnaround time ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)). Setup and turnaround each occupy **the immediately adjacent slot**, including across days — the slot order is AM → PM → Night, then the next day's AM ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)) — so a single-slot event with both buffers occupies three slots on this venue's calendar.

| Slot (as named in the answer) | Hours (as listed) |
|---|---|
| AM | 7am – 12pm |
| *(unlabelled)* | 1pm – 6pm |
| *(unlabelled)* | 7pm – 12am |

> [!warning] Source text for [#50](https://github.com/SinYang13/IS212-2026/discussions/50) is garbled
> The customer's answer reads verbatim: *"The venue will be booked based on 3 time slots (AM, PM, Night) if full day will be all 3 slots. AM (7am-12pm, 1pm-6pm, 7pm-12am)"*. Three time ranges are given but only the first is labelled "AM". The customary reading maps the remaining two ranges to PM and Night in the order the slots were named in the opening sentence, but **that mapping is inferred, not stated** — it is not recorded as fact in the table above. The unbooked gaps at 12pm–1pm and 6pm–7pm are also unexplained (break, changeover, transcription slip, or space for setup/turnaround is not stated). Do not hard-code the missing labels or gap purpose without confirming with the customer.
>
> [#123](https://github.com/SinYang13/IS212-2026/discussions/123) (2026-09-16) partly corroborates the *order*: the customer's own answer treats AM → PM → Night as the chronological sequence and Night → next-day AM as adjacent. That supports the inferred mapping of the unlabelled ranges to PM and Night by position. It still supplies no label for any range and no reason for the gaps, so the table above is deliberately unchanged.

## States

Per-slot availability status ([[event-planning-venue-booking-system|brief §4]], *Venue Availability Calendar*). This is the status of a venue's **slot**, not a lifecycle of the Venue record itself (the venue's own Active / Retired status is a separate attribute — see Attributes) — transitions between these are driven by the occupying [[booking]]; see [[booking]] for the tentative-hold lifecycle.

Release 1 scope differs by state: Available and Confirmed sit under the `🎯 Release 1` venue availability calendar and booking flow (features 9, 12–14); Blocked is `🧩 Adjacent`, pulled in by feature 9's unavailability language; Tentatively held is `📋 Backlog` — tentative holding is not among the core 20. See [[release-1-scope]].

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Available | No hold, booking or block occupies the slot | Default | [[event-planning-venue-booking-system\|brief §4]] |
| Tentatively held | A [[booking]] holds the slot pending confirmation | Entry: hold placed. Exit: confirmed or released — see [[booking]] | [[event-planning-venue-booking-system\|brief §4]]; [#41](https://github.com/SinYang13/IS212-2026/discussions/41) |
| Confirmed | A [[booking]] has been approved for the slot | Entry: booking confirmed. Exit: slot released on event completion, cancellation, or postponement without a new date | [[event-planning-venue-booking-system\|brief §4]]; [#69](https://github.com/SinYang13/IS212-2026/discussions/69) |
| Blocked | Venue Staff have blocked the slot | Entry: Venue Staff block (maintenance, renovation, safety restrictions, other internal activity, other operational reasons). Exit: Venue Staff unblock | [[event-planning-venue-booking-system\|brief §4]] |
| Otherwise unavailable | Catch-all named in the brief but not further detailed | Not specified | [[event-planning-venue-booking-system\|brief §4]] |

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[booking]] | 1 : many | A venue accumulates many bookings/holds over time, but at most one active hold or confirmed booking may occupy the same venue for an overlapping period | [#41](https://github.com/SinYang13/IS212-2026/discussions/41) |
| [[event]] | many : many (via booking) | A venue hosts many events over time; a multi-session event may use several venues, one per session. Normally only one venue booking is confirmed for the same event/session, unless the event genuinely requires multiple venues | [[event-planning-venue-booking-system\|brief §4]]; [#74](https://github.com/SinYang13/IS212-2026/discussions/74); [#109](https://github.com/SinYang13/IS212-2026/discussions/109) |
| [[session]] | 1 : many (via booking) | Each session of a multi-session event may book its own venue, independent of sibling sessions | [#74](https://github.com/SinYang13/IS212-2026/discussions/74) |
| [[user-account]] (Venue Staff) | many : many | Venue Staff maintain venue master data; access to venue information is organisation-wide, not scoped to specific venues | [#66](https://github.com/SinYang13/IS212-2026/discussions/66) |

## Binding rules

- Venue capacity is held **per supported room layout** and is supplied as venue master data; the system neither estimates nor calculates it, and suitability compares expected attendance against the layout the event requests ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)). The deployed model stores that figure on the venue↔layout pairing; the separately retained `venue.capacity` scalar is **not** the figure suitability reads — see [[schema]].
- Setup and turnaround each occupy **at most one full slot, the one immediately adjacent to the booking**, crossing midnight where needed; multi-slot buffers are not required and the user does not choose the buffer slot ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)). This narrows [#43](https://github.com/SinYang13/IS212-2026/discussions/43)'s per-venue setup/teardown/turnaround *duration* and [#108](https://github.com/SinYang13/IS212-2026/discussions/108)'s quantise-to-slots rule into a single adjacency rule; whether the *requirement* stays per venue is open (see Attributes and below).
- Retiring a venue does **not** cancel or auto-reschedule its future bookings: they are identified for review, the relevant Event Coordinators are notified, and the Coordinator and Venue Staff agree an alternative — possibly outside the system — after which the Coordinator updates the booking ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). See [[booking]] and [[venue-booking]].
- Updating a venue attribute does **not** automatically revert dependent bookings to Requested. Where the change affects an existing booking's suitability or availability, that booking is identified for review and the Event Coordinator notified; the resulting status transition is the team's to propose ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)) — see [[booking]].
- Operating hours are maintained by Venue Staff and have no day-of-week variation; there is no weekend/holiday distinction and no holiday calendar — "all days are equal for now" ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)).
- All venues are located in Singapore, and the system uses Singapore time throughout ([#36](https://github.com/SinYang13/IS212-2026/discussions/36)).
- Venue Staff access to venue information is **not** scoped by venue or location — it is organisation-wide, driven by role rather than assignment ([#66](https://github.com/SinYang13/IS212-2026/discussions/66)).
- There is no existing venue database to import — the venue catalogue starts fresh, and no data migration or system-integration requirement exists ([#12](https://github.com/SinYang13/IS212-2026/discussions/12)).
- **Filtering** (narrowing the candidate list by date, time, capacity, accessibility, layout, facilities) and **suitability checking** (assessing whether an already-shortlisted venue still satisfies the event's current requirements) are two distinct functions, not one — full treatment in [[venue-suitability]] ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)).
- A venue (or one of its slots) may be blocked by Venue Staff for maintenance, renovation, safety restrictions, other internal activity, or other operational reasons ([[event-planning-venue-booking-system|brief §4]]).

## Open modelling questions

> [!question]- Team decision required — booking horizon
> No limit on how far in advance a venue may be booked has been specified. Propose an approach only if the solution requires one ([#57](https://github.com/SinYang13/IS212-2026/discussions/57)).

> [!question]- Team decision required — suitability-check presentation
> Whether the suitability result is shown as an inline indicator on the venue list, checked explicitly after selection, or both, is left to the team; the customer confirmed only that both are acceptable presentations ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)). Full discussion at [[venue-suitability]].

> [!question]- Team decision required — the setup / turnaround attributes
> [#123](https://github.com/SinYang13/IS212-2026/discussions/123) makes the buffer a fixed adjacent-slot rule. The deployed `venue.setup_time_minutes` and `venue.turnaround_time_minutes` ([[schema]]) hold a duration in minutes that nothing now reads. Options are the team's: drop them; replace them with per-venue booleans (`requires_setup`, `requires_turnaround`) if the requirement genuinely varies by venue ([#43](https://github.com/SinYang13/IS212-2026/discussions/43)); or move the requirement to the event/booking if it varies by event. The customer's phrase "an event requiring setup" leaves that unsettled.

> [!question]- Team decision required — venue retirement
> Whether Release 1 builds a retire action at all, the status vocabulary, who may retire a venue, and whether retirement is immediate or date-effective are all unspecified; the customer left "the exact workflow and status transitions" to the team ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). Whether a retired venue must disappear from search and booking requests is *implied* rather than stated. Full discussion at [[venue-booking]].

> [!question]- Team decision required — fate of the retained venue-level capacity scalar
> How layout-specific capacity is *represented* is no longer open: Room Layout is a first-class object and the venue↔layout pairing carries its own `capacity` (migration D2 — see [[schema]]). What that migration did **not** do is remove `venue.capacity`, so the model now holds capacity twice with no precedence rule written down. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) fixes only that suitability reads the **layout-specific** figure; whether the scalar becomes a derived value (max across supported layouts), is dropped, or survives as an independent number — and which figure wins while both are writable and can disagree — is the team's to propose ([#112](https://github.com/SinYang13/IS212-2026/discussions/112)). Either way the layout set stays Venue Staff-maintained (classroom, theatre, boardroom, banquet, exhibition, other) and serves as both a filter and a suitability criterion ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)) — see [[venue-suitability]].

See [[domain-model]] for the full object catalogue.
