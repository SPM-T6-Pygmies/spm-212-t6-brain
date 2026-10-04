---
title: "Source: Week 7 Customer Changes"
type: source-summary
sources:
  - "Week 7 Customer Changes.md"
  - "Managing Changes – A Guide.md"
  - "GitHub Discussion 128 (live fetch 2026-10-04; see is212-discussions-2026-10-04-addendum)"
tags: [source, requirements, scope, release-1, change-notice, connectsphere, is212, authoritative]
---

# Source: Week 7 Customer Changes

The customer's **change notice** for Release 1. Six changes, all mandatory: "All of the suggested changes should be implemented in your submission. The customer does not anticipate any further changes for Release 1." It arrives after [[week-4-customer-briefing]] fixed the core 20 and after the team had started building the venue and assignment backlog, so it is the first source to *change* settled requirements rather than clarify them.

## Metadata

| Field | Value |
|---|---|
| Type | Customer change notice (mock) — "Week 7 Customer Update" |
| Course | IS212, AY 2026/27 Term 1 |
| Status | ✅ Authoritative; **binding for Release 1** |
| Supersedes | The slot model and the one-adjacent-slot buffer: [#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123), on time granularity — see C1 |
| Clarified by | [#128](https://github.com/SinYang13/IS212-2026/discussions/128), the customer's own gloss on all six changes — see [[is212-discussions-2026-10-04-addendum]]. It states the **15-minute** unit, that Release 1 is now **26 core** features, and that the Lead is the Operations Manager renamed |
| Depends on | [[week-4-customer-briefing]] (the core 20), [[customer-brief-no-1]] |
| Companion | `Managing Changes – A Guide.md`, the course's change-handling method — see [[is212-g9-section]] |
| Reviewed in | [[week-7-change-impact-review]] — ticket-level impact, proposed stories and migration 03 |

## The six changes

| # | Change | One-line effect | Pages |
|---|---|---|---|
| C1 | Venue setup and turnaround time | Each venue has a **configurable** setup time and turnaround time, included when checking availability and detecting conflicts. 10:00–12:00 + 30 min setup + 45 min turnaround occupies **9:30–12:45**. Bookings that become problematic are *identified*, not silently removed. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) adds that the smallest unit of booking is now **15 minutes**, replacing the three slots | [[venue-booking]], [[booking]], [[venue]] |
| C2 | Venue becomes unavailable after booking | Venue Staff may mark a venue temporarily unavailable (maintenance, equipment failure, renovation, safety, other) **even over booked dates**. The event is not cancelled; affected bookings are identified, Coordinators told alternatives are needed, original event information kept, and the Coordinator can search for and request a replacement venue | [[venue-booking]], [[venue-unavailability]], [[booking]] |
| C3 | Multiple venues for one event | One event may hold **more than one venue booking**. Each is checked independently for suitability, availability and conflicts; changing or cancelling one does not remove the others unless explicitly required | [[booking]], [[event]], [[venue-booking]] |
| C4 | Tentative venue holds expire | Every hold has an **expiry date/time**; on expiry the venue is available again, the Coordinator is informed before or at expiry, and an expired hold is never a confirmed booking | [[booking]], [[venue-booking]], [[notifications]] |
| C5 | Event Coordinator Lead role | The existing assignment role under a new title ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)), with more powers. New requests enter an **unassigned queue** the Lead sees; the Lead reviews basic event information, assigns and reassigns Coordinators and oversees all assignments and active events. Coordinators manage only their own events. Relevant users are notified on assignment and reassignment | [[user-roles]], [[event-request-workflow]], [[event-request]] |
| C6 | Operational feasibility check | New internal role, the **Safety Officer**. After venue and technical arrangements are confirmed, an Operational Safety Check must approve the event before it proceeds to preparation. Outcomes: approve, reject, or request changes; changes send the event back to the relevant earlier planning stage. The Officer "will always give a response" | [[operational-safety-check]], [[safety-check]], [[event-lifecycle]], [[event-readiness]] |

## What changes in the wiki's standing position

| Change | Standing position before | Now |
|---|---|---|
| C1 | Three slots (AM / PM / Night, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)); setup and turnaround **at most one adjacent slot** each ([#123](https://github.com/SinYang13/IS212-2026/discussions/123)); one slot the smallest unit ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)) | Per-venue configurable setup and turnaround; occupancy is `[start − setup, end + turnaround)` on a **15-minute** grid ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). **Contradiction** — see below |
| C2 | Venue Staff block slots; on a post-confirmation loss the Coordinator acts, possibly outside the system ([#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#122](https://github.com/SinYang13/IS212-2026/discussions/122)) | The replacement search and request are **in-system**; blocks may span booked dates; five stated reason categories |
| C3 | "At most one confirmed booking per event unless the event requires multiple venues" ([#109](https://github.com/SinYang13/IS212-2026/discussions/109)) | Multiple simultaneous bookings per event are a **requirement**, independently checked. The working rule is retired |
| C4 | Tentative holding `📋 Backlog`; expiry "for the team" ([#39](https://github.com/SinYang13/IS212-2026/discussions/39), [#41](https://github.com/SinYang13/IS212-2026/discussions/41)) | `🎯 Release 1`; **an expiry must exist** on every hold. How long, who sets it, and the warning lead time are still unstated |
| C5 | Event Operations Manager assigns and reassigns ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#94](https://github.com/SinYang13/IS212-2026/discussions/94), [#101](https://github.com/SinYang13/IS212-2026/discussions/101)) | The same role, **renamed the Lead** at least on the display end, which now also views basic event information and **all** coordinators' assignments and active events ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). The notice alone never mentioned the Manager; #128 settles it |
| C6 | Confirmation gated only on essential arrangements ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [#98](https://github.com/SinYang13/IS212-2026/discussions/98)) | A **mandatory safety gate** sits before preparation, and so before confirmation |

> [!warning] Contradiction — C1 vs [#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123)
> Read alone, the notice contradicted [#123](https://github.com/SinYang13/IS212-2026/discussions/123) (setup and turnaround "at most one full slot") and [#108](https://github.com/SinYang13/IS212-2026/discussions/108) (nothing smaller than a slot): its worked example, 9:30–12:45, touches neither neighbouring slot. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) removes the ambiguity — the customer says the three slots are replaced by a **15-minute smallest unit of booking**. So the whole slot model is superseded, not only the buffer rule: [#50](https://github.com/SinYang13/IS212-2026/discussions/50)'s AM / PM / Night booking unit and [#107](https://github.com/SinYang13/IS212-2026/discussions/107)'s two-slot rule go with it. The earlier team reading, "minutes on top of slots", was wrong and has been withdrawn. See [[venue-booking]].

> [!info] Two earlier inferences fall away
> The notice's example ends at 12:45pm, inside the 12pm–1pm gap between the old AM and PM slot hours, and this page first read that as a hint that the gaps host buffers. With slots gone there is no gap to host anything, and [#126](https://github.com/SinYang13/IS212-2026/discussions/126) (a booking between 12nn and 1pm) is **probably moot**, though unanswered. The same applies to the garbled slot labels in [#50](https://github.com/SinYang13/IS212-2026/discussions/50).

## What the notice leaves open

Each is carried on the page it affects and in [[open-questions]].

| Gap | Change | Page |
|---|---|---|
| ~~Does the Lead replace the Event Operations Manager or sit beside it?~~ **Answered by [#128](https://github.com/SinYang13/IS212-2026/discussions/128)**: the same role, renamed. Open only: is the stored role name changed, or just the displayed title? | C5 | [[user-roles]] |
| ~~Is "under their supervision" every coordinator or a subset per Lead?~~ **Answered by [#128](https://github.com/SinYang13/IS212-2026/discussions/128)**: "all the coordinator assignments and active events" | C5 | [[user-roles]] |
| Is the outgoing coordinator a "relevant user" to notify on reassignment? | C5 | [[notifications]] |
| Can a Lead also act as an Event Coordinator? | C5 | [[user-roles]] |
| How does a safety **reject** differ from **request changes**; is either terminal? | C6 | [[operational-safety-check]] |
| Which "relevant earlier planning stage" does a returned event go back to? | C6 | [[operational-safety-check]] |
| Does a significant change after approval require a fresh safety check? | C6 | [[operational-safety-check]] |
| Does the Organiser see the safety status or outcome? | C6 | [[operational-safety-check]] |
| ~~Does the notice supersede [#123](https://github.com/SinYang13/IS212-2026/discussions/123)/[#108](https://github.com/SinYang13/IS212-2026/discussions/108) outright?~~ **Answered by [#128](https://github.com/SinYang13/IS212-2026/discussions/128)**: yes, the 15-minute unit replaces the slots. Open: are setup and turnaround values also multiples of 15; do AM / PM / Night survive as a grouping; is [#126](https://github.com/SinYang13/IS212-2026/discussions/126) moot | C1 | [[venue-booking]] |
| Who sets a hold's expiry, can it be extended, and how long before expiry is the warning? | C4 | [[booking]] |

## The change-handling guide

`Managing Changes – A Guide.md` is course process, not domain content, so it has no page of its own; it is summarised under *Change management* in [[is212-g9-section]]. Its three scenarios (missed acceptance criterion → reopen; new requirement → new story; new requirement plus an incorrect original criterion → both) decided the ticket-by-ticket actions in [[week-7-change-impact-review]].

## Timing

The notice is undated. It is the Week 7 update. The 2026-10-01 export ([[is212-discussions-2026-10-01]]) carries no answer that touches it; the customer's clarification came two days later in [#128](https://github.com/SinYang13/IS212-2026/discussions/128), summarised in [[is212-discussions-2026-10-04-addendum]].
