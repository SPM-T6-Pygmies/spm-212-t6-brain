---
title: Venue
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, venue]
---

# Venue

A physical event space owned and operated by [[connectsphere-event-services]], catalogued by Venue Staff and offered for booking against an [[event]] or [[session]]. All venues are located in Singapore ([#36](https://github.com/SinYang13/IS212-2026/discussions/36)). Process detail for search, filtering, suitability assessment and approval lives in [[venue-booking]] and [[venue-suitability]]; this page is the structural reference.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Location | Facility/site; every venue is in Singapore | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; [#36](https://github.com/SinYang13/IS212-2026/discussions/36) |
| Capacity | Maximum attendance the venue can hold | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Facilities | e.g. video-conferencing, AV fit-out | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Accessibility | Accessibility features of the space | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Supported room layouts | Which of classroom, theatre, boardroom, banquet, exhibition or another arrangement the venue supports, recorded by Venue Staff | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Operating hours | Maintained by Venue Staff; no day-of-week variation | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; [#46](https://github.com/SinYang13/IS212-2026/discussions/46) |
| Setup time | Preparation time needed before the published start time | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Turnaround / reset time | Reset time needed after an event, before the venue is available again; recorded **per venue** | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; [#43](https://github.com/SinYang13/IS212-2026/discussions/43) |
| Availability (per slot) | Available / tentatively held / confirmed / blocked / otherwise unavailable — see States | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Room Layout as a modelled object | Whether "layout" is a first-class object (its own attributes, possibly capacity-per-layout) or just an enum value on Venue — never elaborated beyond its use as a filter term | 🔶 Team decision | [#83](https://github.com/SinYang13/IS212-2026/discussions/83) |
| Booking horizon | How far in advance a venue may be booked | 🔶 Team decision | [#57](https://github.com/SinYang13/IS212-2026/discussions/57) |

> [!warning] Capacity threshold never tightened
> The brief's own worked rule is hedged: "a venue should **not normally** be treated as suitable when expected attendance exceeds its capacity" ([[event-planning-venue-booking-system|brief §4]]). The word "normally" was never tightened in either discussion snapshot, so whether over-capacity is a hard block or an overridable warning is unresolved — contrast the Booking rule below, where double-booking *was* explicitly confirmed as a hard block. Full discussion at [[venue-suitability]].

### Booking slots

Venues are booked in three discrete daily slots; a full-day booking uses all three ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)).

| Slot (as named in the answer) | Hours (as listed) |
|---|---|
| AM | 7am – 12pm |
| *(unlabelled)* | 1pm – 6pm |
| *(unlabelled)* | 7pm – 12am |

> [!warning] Source text for [#50](https://github.com/SinYang13/IS212-2026/discussions/50) is garbled
> The customer's answer reads verbatim: *"The venue will be booked based on 3 time slots (AM, PM, Night) if full day will be all 3 slots. AM (7am-12pm, 1pm-6pm, 7pm-12am)"*. Three time ranges are given but only the first is labelled "AM". The customary reading maps the remaining two ranges to PM and Night in the order the slots were named in the opening sentence, but **that mapping is inferred, not stated** — it is not recorded as fact in the table above. The unbooked gaps at 12pm–1pm and 6pm–7pm are also unexplained (break, changeover, transcription slip, or space for setup/turnaround is not stated). Do not hard-code the missing labels or gap purpose without confirming with the customer.

## States

Per-slot availability status ([[event-planning-venue-booking-system|brief §4]], *Venue Availability Calendar*). This is the status of a venue's **slot**, not a lifecycle of the Venue record itself — transitions between these are driven by the occupying [[booking]]; see [[booking]] for the tentative-hold lifecycle.

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
| [[event]] | many : many (via booking) | A venue hosts many events over time; a multi-session event may use several venues, one per session | [[event-planning-venue-booking-system\|brief §4]]; [#74](https://github.com/SinYang13/IS212-2026/discussions/74) |
| [[session]] | 1 : many (via booking) | Each session of a multi-session event may book its own venue, independent of sibling sessions | [#74](https://github.com/SinYang13/IS212-2026/discussions/74) |
| [[user-account]] (Venue Staff) | many : many | Venue Staff maintain venue master data; access to venue information is organisation-wide, not scoped to specific venues | [#66](https://github.com/SinYang13/IS212-2026/discussions/66) |

## Binding rules

- Setup, teardown and turnaround requirements are recorded **per venue** — not a global constant and not (currently) variable by event type or size ([#43](https://github.com/SinYang13/IS212-2026/discussions/43)).
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

> [!question]- Team decision required — Room Layout as a modelled object
> Room Layout appears only as a filter/suitability criterion (classroom, theatre, boardroom, banquet, exhibition, other) and as something Venue Staff "record... supported by each venue." Whether it should be modelled as its own object (e.g. to support capacity that varies by layout) or remains a plain enum attribute on Venue is never addressed ([#83](https://github.com/SinYang13/IS212-2026/discussions/83)).

See [[domain-model]] for the full object catalogue.
