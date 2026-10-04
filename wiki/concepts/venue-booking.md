---
title: Venue Booking
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Sept 8.md
  - IS212 2026 Discussions Sept 16.md
  - IS212 2026 Discussions Oct 1.md
  - Week 7 Customer Changes.md
tags: [venue, booking, availability, scheduling, business-rules, connectsphere]
---

# Venue Booking

**Objects:** [[booking]] · [[venue]]

How [[connectsphere-event-services]] venues are catalogued, held, requested, approved, and protected from double-booking in the [[event-planning-venue-booking-system]]. Covers steps 6–7 of the planning process (see [[event-lifecycle]]). Assessment of whether a given venue *fits* an event is covered separately in [[venue-suitability]].

**Release 1 scope:** Venue catalogue, booking request, booking approval, and conflict detection (double-booking) are `🎯 Release 1` (core features 8, 12–14). Four capabilities were added by the customer's change notice and are `🎯 Release 1` on that basis (the customer's follow-up now calls Release 1 the "26 core", [#128](https://github.com/SinYang13/IS212-2026/discussions/128)): setup and turnaround time ([[week-7-customer-changes|Week 7 C1]]), venue unavailability management ([[week-7-customer-changes|Week 7 C2]]), multiple venues for one event ([[week-7-customer-changes|Week 7 C3]]) and tentative holding with mandatory expiry ([[week-7-customer-changes|Week 7 C4]]). Venue retirement stays `🧩 Adjacent`, on a thin footing — see [Venue Retirement](#venue-retirement). See [[release-1-scope]] for the full register and rationale, and [[week-7-customer-changes]] for the notice.

## Venue Catalogue

Venue Staff own venue master data — location, capacity, facilities, accessibility, supported room layouts, operating hours, and per-venue setup/teardown/turnaround time; see [[venue]] for the full attribute table (Source: customer-brief-no-1). Capacity drives suitability — see [[venue-suitability]] — and is held **per supported layout**, supplied rather than calculated (Source: is212-discussions-2026-09-08, [#112](https://github.com/SinYang13/IS212-2026/discussions/112)).

All venues are in Singapore and the system operates on Singapore time (Source: is212-discussions-2026-08-31, [#36](https://github.com/SinYang13/IS212-2026/discussions/36)) — see [[non-functional-requirements]].

## Booking Unit — the 15-Minute Grid

Bookings are made as a **start and end time on a 15-minute grid**. The customer's clarification of [[week-7-customer-changes|Week 7 C1]] says "the change of 3 slots to 15mins as the smallest unit of booking" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). This **replaces** the AM / PM / Night three-slot model: a slot is no longer a booking attribute, and a booking carries a start and end time at 15-minute granularity instead. Its effective occupancy is the event time plus the venue's setup and turnaround — see [Setup and Turnaround Time](#setup-and-turnaround-time).

The slot rules ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108), [#123](https://github.com/SinYang13/IS212-2026/discussions/123)) are all superseded; they are kept as history once, under [Superseded — the slot model](#superseded--the-slot-model). The surviving hours and gaps from [#50](https://github.com/SinYang13/IS212-2026/discussions/50) are on [[venue]], also marked superseded.

> [!question]- Team decision required — what the slot vocabulary becomes
> [#128](https://github.com/SinYang13/IS212-2026/discussions/128) does not say whether AM / PM / Night survive at all — as a **display grouping** on the availability calendar, or as **operating-hour windows** (7am–12pm, 1pm–6pm, 7pm–12am, from [#50](https://github.com/SinYang13/IS212-2026/discussions/50)) — or whether [#46](https://github.com/SinYang13/IS212-2026/discussions/46)'s operating hours now simply bound a day. Nor does it say whether setup and turnaround values must themselves be **multiples of 15 minutes**: the notice's 30 and 45 minutes are, but that is an inference from one example. The team proposes; deferred in [#50](https://github.com/SinYang13/IS212-2026/discussions/50) (slot selection UX) and [#46](https://github.com/SinYang13/IS212-2026/discussions/46) (operating hours).

## Booking Request and Approval

1. The Event Coordinator identifies candidate venues (search, filter, suitability check — see [[venue-suitability]]).
2. The Event Coordinator submits a **venue booking request** carrying the event timing and venue requirements (Source: customer-brief-no-1).
3. **Venue Staff** review and decide. They may approve, reject, or reject with a suggested alternative venue or arrangement (Source: customer-brief-no-1; is212-discussions-2026-08-31, [#45](https://github.com/SinYang13/IS212-2026/discussions/45)).

Rejection with an alternative is **not** a formal counter-offer object in the system. Venue Staff suggest; **the Event Coordinator then makes the changes** and re-requests (Source: is212-discussions-2026-08-31, [#45](https://github.com/SinYang13/IS212-2026/discussions/45)). This keeps the coordinator as the single point of control over event arrangements, consistent with [[event-request-workflow]].

There is correspondingly **no approve-with-amendments action**: Venue Staff cannot approve a request with details they have altered themselves. They may suggest an amended or alternative arrangement, but the Event Coordinator reviews and updates the booking request, and approval follows on the updated request (Source: is212-discussions-2026-09-08, [#110](https://github.com/SinYang13/IS212-2026/discussions/110)) — a restatement of [#45](https://github.com/SinYang13/IS212-2026/discussions/45), not a new rule.

### The Event Organiser's part

The Event Organiser may express venue-related preferences or requirements, and those discussions may happen outside the system entirely. Identifying suitable venue options against those requirements remains the **Event Coordinator's** responsibility (Source: is212-discussions-2026-09-08, [#111](https://github.com/SinYang13/IS212-2026/discussions/111)).

> [!question]- Team decision required — organiser-indicated preferred venue
> The team may, if it wishes, let the Event Organiser name a preferred venue in the system. It is optional, and it is a **preference only** — it must not be treated as a confirmed booking and must not override the normal venue suitability and approval process ([#111](https://github.com/SinYang13/IS212-2026/discussions/111)). Nothing in the sources requires it, so building it is a scope choice, not an obligation.

Booking request, approval, rejection, and change are all notification triggers (see [[notifications]]).

## Tentative Holds

A venue may be temporarily held while an event is still being finalised (Source: customer-brief-no-1). Tentative holding is `🎯 Release 1` ([[week-7-customer-changes|Week 7 C4]]; "not in the 20 core and now included in the 26 core", [#128](https://github.com/SinYang13/IS212-2026/discussions/128)). The hold/confirm/release/expire states and the one-active-hold-or-booking-per-venue rule are defined on [[booking]] — see States and Binding rules.

**Who holds, and what the notice settles** ([[week-7-customer-changes|Week 7 C4]]):

- **Venue Staff** place holds — "Venue Staff have explained that they sometimes temporarily hold a venue while an Event Coordinator is finalising". This answers the earlier question of who the actor is. The Coordinator-side flow (finalise, then confirm or release) is unchanged.
- **Every hold must have an expiry date/time.** If the required action is not completed before the deadline the hold **expires** and the venue becomes available to other requests.
- The **Event Coordinator is informed before or when** the hold expires.
- **An expired hold is never treated as a confirmed booking.** It needs its own terminal state, distinct from Released (a person gave the hold up) — proposed as `Expired` on [[booking]].

This answers [#39](https://github.com/SinYang13/IS212-2026/discussions/39) and [#41](https://github.com/SinYang13/IS212-2026/discussions/41) in part: they deferred expiry to the team, and the notice now says one **must exist**.

> [!question]- Team decision required — hold duration, ownership, extension and warning lead time
> The notice fixes that an expiry exists, not its parameters ([[week-7-customer-changes|Week 7 C4]]). Still for the team, citing [#39](https://github.com/SinYang13/IS212-2026/discussions/39) and [#41](https://github.com/SinYang13/IS212-2026/discussions/41): how long a hold lasts or whether there is a default; **who sets** the expiry (the Venue Staff member placing it, or a system default); whether it can be **extended**, and by whom; and **how long before expiry** the Coordinator is warned ("before or when" gives no lead time — do not invent one). A before-expiry warning is time-triggered, so it needs a scheduled job rather than an event hook — see [[notifications]]. The brief's pain point still applies to the *absence* of an expiry: "cancelled events may continue to occupy venue or equipment reservations".

Competing holds are impossible by rule: the first **unexpired** hold wins the venue and period until it is confirmed, released or expires. Multiple coordinators cannot shortlist the same period in parallel.

**Shortlisting is not booking several venues.** Two separate points:

- A single Coordinator may identify and consider **several candidate venues** for one event, but placing more than one of them on tentative hold at the same time while shortlisting is **not required for Release 1** (Source: is212-discussions-2026-09-08, [#109](https://github.com/SinYang13/IS212-2026/discussions/109)). This still stands.
- An event that genuinely needs **several venues at once** (the notice's example: an auditorium plus three breakout rooms) may hold several venue bookings — see [Multiple Venues for One Event](#multiple-venues-for-one-event). [#109](https://github.com/SinYang13/IS212-2026/discussions/109)'s working rule that only one booking is normally confirmed per event/session is **retired** by [[week-7-customer-changes|Week 7 C3]].

## Conflict Detection and Double-Booking

The system must **identify and block** incompatible or overlapping bookings (Source: is212-discussions-2026-08-31, [#35](https://github.com/SinYang13/IS212-2026/discussions/35)).

- This is a **hard block**, not a warning.
- The question explicitly offered a soft-warning-with-override option (override by Venue Staff for VIP organisations); the customer did **not** take it.
- No override or exception path for privileged clients is authorised. Any such path would need fresh clarification.

Overlap detection must account for setup and turnaround time, not only the published event start and end times ([[event-planning-venue-booking-system|brief §4]]). Under [[week-7-customer-changes|Week 7 C1]] each venue's **configured setup and turnaround time** are "included when checking venue availability and detecting booking conflicts". A booking's effective occupancy is therefore its event time plus the venue's setup before and turnaround after — see [Setup and Turnaround Time](#setup-and-turnaround-time). A hold that has expired no longer blocks; an unexpired hold blocks like a confirmed booking ([#41](https://github.com/SinYang13/IS212-2026/discussions/41), [[week-7-customer-changes|Week 7 C4]]). Conflicts are judged **per booking**, so two bookings of the *same event* at different venues do not conflict with each other ([[week-7-customer-changes|Week 7 C3]]). See [[venue-suitability]] and [[event-readiness]].

## Setup and Turnaround Time

`🎯 Release 1` ([[week-7-customer-changes|Week 7 C1]]).

**What the customer says** ([[week-7-customer-changes|Week 7 C1]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)):

- **Each venue has a configurable setup time and turnaround time.** They are set per venue, which agrees with [#43](https://github.com/SinYang13/IS212-2026/discussions/43).
- Both must be **included when checking availability and detecting conflicts**.
- Worked example: an event 10:00–12:00, with 30 minutes of setup and 45 minutes of turnaround, **occupies 9:30–12:45**.
- The smallest unit of booking is **15 minutes**, not a three-way slot ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). Setup and turnaround time "was not part of the 20 core" at Week 4 "but now it is included in the release 1".
- Existing bookings that become problematic under the rule are **identified**, not silently removed. A change to a venue's buffers is one such cause — see [[change-impact-management]].

> [!warning] Contradiction — [[week-7-customer-changes|Week 7 C1]] vs [#50](https://github.com/SinYang13/IS212-2026/discussions/50)/[#107](https://github.com/SinYang13/IS212-2026/discussions/107)/[#108](https://github.com/SinYang13/IS212-2026/discussions/108)/[#123](https://github.com/SinYang13/IS212-2026/discussions/123)
> The standing rules booked in three slots ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)), consumed whole slots ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)), made one slot the smallest unit of setup and turnaround ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)) and fixed each buffer at at most one adjacent slot ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)). [[week-7-customer-changes|Week 7 C1]] gives per-venue configurable times, an example (9:30–12:45) that reaches no neighbouring slot, and [#128](https://github.com/SinYang13/IS212-2026/discussions/128) states that the three slots become a 15-minute smallest unit. They cannot both hold. This wiki follows the **later, explicitly Release 1** notice and its clarification; the old rules are kept below, marked superseded. See [[week-7-customer-changes]].

| Position | Status |
|---|---|
| Per-venue configurable setup and turnaround time, counted in availability and conflict checks | ✅ Specified ([[week-7-customer-changes\|Week 7 C1]]) |
| The smallest unit of booking is 15 minutes, replacing the three slots | ✅ Specified ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| Occupancy is `[start − setup, end + turnaround)` on the 15-minute grid. The interval notation is the team's; the arithmetic is the customer's example | ✅ Specified ([[week-7-customer-changes\|Week 7 C1]]) |
| The setup/turnaround unit is **minutes**, evidenced only by the example; whether the values must be multiples of 15 is not stated (30 and 45 are — inference only) | ⚠️ Partial |
| A venue with no setup/turnaround value configured counts as 0 | 🔶 Team decision ([#43](https://github.com/SinYang13/IS212-2026/discussions/43); [[event-planning-venue-booking-system\|brief §4]]) |
| AM / PM / Night survive as a display grouping or operating-hour windows; operating hours bound a day | 🔶 Team decision ([#50](https://github.com/SinYang13/IS212-2026/discussions/50); [#46](https://github.com/SinYang13/IS212-2026/discussions/46)) |

> [!question]- Team decision required — padded window mechanics
> What survives of the retired buffer-slot questions, with the slot model gone:
> - **Operating-hour edges.** What a buffer does when setup would start before opening, or turnaround would run past closing (including past midnight): refuse the booking, or waive the buffer. Operating hours are maintained per venue ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)).
> - **[#126](https://github.com/SinYang13/IS212-2026/discussions/126).** A team member asked what happens to a booking made between 12nn and 1pm. It is **likely moot, unconfirmed**: there is no slot gap to book across on a 15-minute grid. It is still unanswered, and whether 12nn–1pm stays bookable depends on the question above.
> - **Visibility.** Whether buffer time shows as occupied on the availability calendar ([[event-planning-venue-booking-system|brief §4]]) and counts towards utilisation in [[reporting]].
> - **Stored or derived.** Whether the padded window is stored on the booking or derived from the venue's current values. A stored (snapshotted) window means a later buffer change flags existing bookings instead of rewriting them; the schema review proposes this ([[week-7-change-impact-review]]).

Related notes:

- Insufficient turnaround between consecutive events is a named problem in the current manual process (Source: customer-brief-no-1).
- Extending or moving an event may invalidate the buffer before or after it — a [[change-impact-management]] trigger.
- The deployed `venue.setup_time_minutes` / `turnaround_time_minutes` columns are live again — see [[venue]] and [[schema]]. The deployed `booking_slot` table (AM / PM / Night) no longer matches the booking unit; see [[schema]].

### Superseded — the slot model

Kept as history. None of this is the current rule; all of it is replaced by [[week-7-customer-changes|Week 7 C1]] and [#128](https://github.com/SinYang13/IS212-2026/discussions/128).

- ~~Bookings are made against AM / PM / Night slots; a full-day booking is all three; partial blocking is by individual slots~~ ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)). Replaced by a start and end time on a 15-minute grid.
- ~~The slot is indivisible: an event across two slots consumes both slots in full, with no residue~~ ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)).
- ~~Each of setup and turnaround needs at most one full slot; multi-slot buffers and buffers spanning several days are not required~~ ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)).
- ~~The buffer is the slot immediately adjacent to the event's booking, including across days; the user never selects the buffer slot~~ ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)):

| Event slot | ~~Setup occupied~~ | ~~Turnaround occupied~~ |
|---|---|---|
| AM | ~~Night of the previous day~~ | ~~PM, same day~~ |
| PM | ~~AM, same day~~ | ~~Night, same day~~ |
| Night | ~~PM, same day~~ | ~~AM of the following day~~ |

- ~~One slot is the smallest unit of setup and turnaround; sub-slot durations are not required for Release 1~~ ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)).
- ~~The venue's setup/turnaround values are at most a whether-required indicator, not a length~~ — reversed: the per-venue values are the requirement again.
- The "worked consequence" that a buffer took a whole slot (so back-to-back events in adjacent slots were impossible), and the question of neighbouring events sharing a buffer slot, are **moot**.
- The unexplained 12pm–1pm and 6pm–7pm gaps in [#50](https://github.com/SinYang13/IS212-2026/discussions/50)'s hours, and the garbled slot labels, are history too — see [[venue]]. The notice's 12:45pm example end sat inside the old 12nn–1pm gap, which first suggested gaps might host buffers; with the grid it no longer matters.

## Multiple Venues for One Event

`🎯 Release 1` ([[week-7-customer-changes|Week 7 C3]]). The customer calls it "a new requirement as it was not mandated previously" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)).

- One event may have **more than one venue booking at the same time** — the notice's example is an auditorium plus three breakout rooms.
- Each venue booking is checked **independently** for suitability, availability and conflicts, while staying associated with the same event.
- Changing or cancelling one venue arrangement must **not automatically remove the others** unless explicitly required.
- This **retires** the wiki's working rule that at most one booking is normally confirmed per event/session (from [#109](https://github.com/SinYang13/IS212-2026/discussions/109)). Event to booking was already 1 : N, so there is no cardinality change; see [[booking]] and [[event]].
- **Sessions are not pulled in.** The notice describes simultaneous spaces under one event, not sessions; per-session booking stays `📋 Backlog` ([[session]]).

> [!question]- Team decision required — attendance and purpose per venue booking
> Two things the notice does not say, proposed here rather than assumed: whether each venue is judged against the event's **whole expected attendance** or a **portion** of it (see [[venue-suitability]]); and whether each booking carries its own **purpose label** (e.g. "Breakout A"). Neither was put to the customer.

## Venue Unavailability and Blocking

`🎯 Release 1` ([[week-7-customer-changes|Week 7 C2]]); "not in the 20 core and now included in the 26 core" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). The record itself is [[venue-unavailability]].

- Venue Staff can block venues for maintenance, renovation, safety issues, internal activities, or other operational reasons (Source: customer-brief-no-1).
- ~~Blocking was at slot granularity, enabling partial-day unavailability~~ ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)) — superseded with the slot model. Partial-day unavailability presumably runs on the same 15-minute grid; [[week-7-customer-changes|Week 7 C2]] and [#128](https://github.com/SinYang13/IS212-2026/discussions/128) do not say so, so the granularity of a period is open (see [[venue-unavailability]]).
- [[week-7-customer-changes|Week 7 C2]] lets Venue Staff mark a venue **temporarily unavailable** for maintenance, equipment failure, renovation, safety concerns or other operational reasons, **even when future events are already booked**.
- **The existing event is not automatically cancelled.** The affected bookings are **identified** and the relevant Event Coordinators **informed that alternative arrangements are required**.
- The system **preserves the original event information** while the Coordinator **searches for and requests a replacement venue**. This is in-system. [#122](https://github.com/SinYang13/IS212-2026/discussions/122) had said an alternative might be agreed outside the system and the Coordinator then updates the booking; C2 extends that, it does not void it. A replacement is an **ordinary booking request** (search, suitability, Venue Staff approval) linked to the original.
- Earlier answer, still the baseline: when a venue becomes unavailable **after** confirmation, Venue Staff update availability, the change is a notification scenario, and **the Event Coordinator must then make the changes** (Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)).
- An ordinary **venue-attribute update** — as distinct from blocking the venue — does **not** automatically revert existing bookings to Requested. Where the change affects a booking's suitability or availability, that booking is identified for review and the Event Coordinator notified; the resulting status transition is for the team to propose (Source: is212-discussions-2026-09-08, [#120](https://github.com/SinYang13/IS212-2026/discussions/120)). See [[booking]] and [[change-impact-management]].

> [!question]- Team decision required — post-confirmation venue loss automation and destination state
> Automation of the response (auto-release, auto-flag, auto-suggest alternatives) is for the team to propose ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)). So is the **destination** of a flagged booking and what status the original booking ends in once a replacement is confirmed ([#120](https://github.com/SinYang13/IS212-2026/discussions/120), [#122](https://github.com/SinYang13/IS212-2026/discussions/122)). The schema review *proposes* a review flag on the booking (`review_reason` / `review_flagged_at`) rather than a new status, and a link from the replacement to the original — a team proposal, not a customer statement ([[week-7-change-impact-review]]).

## Venue Retirement

A venue may be retired while bookings against it are still in the future. The customer's rule (Source: is212-discussions-2026-09-16, [#122](https://github.com/SinYang13/IS212-2026/discussions/122)):

- Future bookings are **not cancelled** and **not automatically rescheduled**.
- The affected bookings are **identified for review**, and the relevant Event Coordinators are **notified**.
- The Event Coordinator, **together with Venue Staff**, decides on an alternative venue or arrangement. That coordination **may take place outside the system**.
- Once an alternative is agreed, the **Event Coordinator updates the venue arrangement in the system** and reconsiders suitability, availability and booking requirements where applicable — the ordinary [[venue-suitability]] and conflict checks, re-run on the replacement.

**Retirement is not unavailability.** Retirement ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)) is permanent; unavailability ([[week-7-customer-changes|Week 7 C2]]) is temporary and is recorded as a period on [[venue-unavailability]]. How retirement is represented is unresolved — a venue status or an open-ended unavailability row.

This is now one pattern with five triggers: an attribute update ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)), unavailability ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)), retirement ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)), a **change to a venue's setup or turnaround time** ([[week-7-customer-changes|Week 7 C1]]), and **venue unavailability over booked dates** ([[week-7-customer-changes|Week 7 C2]]). In each, affected bookings are flagged, the Coordinator notified, the Coordinator acts, and nothing changes automatically. The system's part is **detection and notification**; the resolution is human. See [[booking]], [[change-impact-management]], [[notifications]].

**Scope.** The customer answers as if retirement exists but has not asked for a retire feature; [[week-4-customer-briefing]]'s core 20 do not name it. It is recorded as `🧩 Adjacent` — a retired venue must stop being offered by venue search (feature 10) and booking requests (feature 12) — and the minimal implementation is a status on the venue, not a workflow. See [[release-1-scope]] and [[venue]].

> [!question]- Team decision required — retirement workflow and status transitions
> The customer explicitly leaves "the exact workflow and status transitions" to the team ([#122](https://github.com/SinYang13/IS212-2026/discussions/122)). Open: who may retire a venue (Venue Staff is the natural owner of venue master data, unconfirmed); whether retiring is immediate or effective from a date; whether new bookings are refused from the moment of retirement; which state, if any, the affected bookings enter — the same unresolved destination as [#120](https://github.com/SinYang13/IS212-2026/discussions/120) — and how a booking that has been reviewed and kept, or replaced, is marked as resolved.

## Operating Hours

- Maintained by **Venue Staff** as part of venue information (Source: is212-discussions-2026-08-31, [#46](https://github.com/SinYang13/IS212-2026/discussions/46)).
- **All days are treated as equal for now** — no day-of-week variation, no weekend/public-holiday distinction, no holiday calendar required ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)).

## Booking Horizon

- **No booking horizon has been specified** (Source: is212-discussions-2026-08-31, [#57](https://github.com/SinYang13/IS212-2026/discussions/57)).
- **Team decision required:** propose one only if the solution requires it. Compare the parallel non-answers on event-request lead time ([#78](https://github.com/SinYang13/IS212-2026/discussions/78)) and equipment/technical-support notice period ([#25](https://github.com/SinYang13/IS212-2026/discussions/25), see [[technical-support]]).

## Open Points Summary

| # | Question | Customer position |
|---|---|---|
| [#50](https://github.com/SinYang13/IS212-2026/discussions/50) | Slot definition | ~~Three slots, AM/PM/Night; hours listed but labels garbled; UX for team~~ — **superseded**: smallest unit of booking is 15 minutes ([[week-7-customer-changes\|Week 7 C1]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)). Whether AM/PM/Night survive as display or operating windows is open |
| [#41](https://github.com/SinYang13/IS212-2026/discussions/41) | Concurrent holds | One active hold or confirmed booking per venue + overlapping period; "active" now means unexpired ([[week-7-customer-changes\|Week 7 C4]]) |
| [#39](https://github.com/SinYang13/IS212-2026/discussions/39) | Hold duration / auto-release | An expiry **must exist** ([[week-7-customer-changes\|Week 7 C4]]); duration, who sets it, extension and warning lead time — **team to propose** |
| [#35](https://github.com/SinYang13/IS212-2026/discussions/35) | Double-booking | Hard block, no override |
| [#45](https://github.com/SinYang13/IS212-2026/discussions/45) | Rejection with alternative | Informal suggestion; coordinator makes the change |
| [#43](https://github.com/SinYang13/IS212-2026/discussions/43) | Turnaround | Per-venue, recorded by Venue Staff — live again under [[week-7-customer-changes\|Week 7 C1]] |
| [#46](https://github.com/SinYang13/IS212-2026/discussions/46) | Operating hours | Venue Staff maintain; all days equal |
| [#54](https://github.com/SinYang13/IS212-2026/discussions/54) | Post-confirmation unavailability | Venue Staff update; coordinator reacts; extended by [[week-7-customer-changes\|Week 7 C2]]; automation **team to propose** |
| [#57](https://github.com/SinYang13/IS212-2026/discussions/57) | Booking horizon | Not specified — **team to propose** |
| [#107](https://github.com/SinYang13/IS212-2026/discussions/107) | Event spanning two slots | ~~Consumes both slots in full~~ — **superseded** with the slot model ([[week-7-customer-changes\|Week 7 C1]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| [#108](https://github.com/SinYang13/IS212-2026/discussions/108) | Smallest setup/turnaround unit | ~~One slot; sub-slot durations not required~~ — **superseded** ([[week-7-customer-changes\|Week 7 C1]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| [#123](https://github.com/SinYang13/IS212-2026/discussions/123) | Setup/turnaround size and placement | ~~At most one full adjacent slot each~~ — **superseded** ([[week-7-customer-changes\|Week 7 C1]]): per-venue configurable time; example 9:30–12:45 |
| [#109](https://github.com/SinYang13/IS212-2026/discussions/109) | Multiple candidate venues | Considering several is fine; simultaneous holds while shortlisting not required for Release 1 — still stands. The "one confirmed booking per event/session" working rule is **retired** by [[week-7-customer-changes\|Week 7 C3]] |
| [#110](https://github.com/SinYang13/IS212-2026/discussions/110) | Approve with amended details | No — Venue Staff suggest, coordinator updates and resubmits (restates [#45](https://github.com/SinYang13/IS212-2026/discussions/45)) |
| [#111](https://github.com/SinYang13/IS212-2026/discussions/111) | Organiser venue preference | Organiser may state preferences, possibly outside the system; coordinator identifies options; an in-system preferred venue is optional and non-binding |
| [#112](https://github.com/SinYang13/IS212-2026/discussions/112) | Capacity by layout | Varies by layout; per-layout figures supplied as venue attributes, not calculated — see [[venue-suitability]] |
| [#120](https://github.com/SinYang13/IS212-2026/discussions/120) | Venue attribute change vs booking status | No auto-revert to Requested; affected booking flagged for review and coordinator notified; transition **team to propose** |
| [#122](https://github.com/SinYang13/IS212-2026/discussions/122) | Future bookings when a venue is retired | Not cancelled, not auto-rescheduled; identified for review, Coordinators notified; Coordinator + Venue Staff agree an alternative (possibly outside the system), Coordinator updates the booking; workflow and transitions **team to propose** |
| [#126](https://github.com/SinYang13/IS212-2026/discussions/126) | Booking inside the 12nn–1pm gap | **Unanswered**; likely moot on the 15-minute grid, unconfirmed ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| [[week-7-customer-changes\|Week 7 C1]] | Venue setup and turnaround time | Configurable per venue, counted in availability and conflicts; 10:00–12:00 + 30 + 45 ⇒ 9:30–12:45; smallest unit of booking is 15 minutes ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)); multiples-of-15 and null handling open |
| [[week-7-customer-changes\|Week 7 C2]] | Venue unavailable after booking | Venue Staff mark it; events not cancelled; bookings identified; in-system replacement search and request |
| [[week-7-customer-changes\|Week 7 C3]] | Multiple venues for one event | Several simultaneous bookings, each checked independently; changing one leaves the others |
| [[week-7-customer-changes\|Week 7 C4]] | Tentative holds expire | Every hold has an expiry; expired hold frees the venue and is never a confirmed booking; Coordinator informed before or at expiry |

See also [[open-questions]], [[user-roles]], [[access-control]], [[reporting]].
