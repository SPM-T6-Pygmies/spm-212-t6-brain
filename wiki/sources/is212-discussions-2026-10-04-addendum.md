---
title: "Source: IS212 Discussions — addendum #127–#129 (live fetch, 2026-10-04)"
type: source-summary
sources:
  - "GitHub Discussions #127, #128, #129 — SinYang13/IS212-2026, fetched live with gh on 2026-10-04; not yet in raw/"
tags: [source, clarifications, q-and-a, github-discussions, is212, authoritative, week-7]
---

# Source: IS212 Discussions — addendum #127–#129

Three discussions posted after the 2026-10-01 export ([[is212-discussions-2026-10-01]]). They were read straight from the board, not from a `raw/` file, because no later export exists. The board held **129 discussions** when fetched; no earlier discussion had been edited or newly answered since 2026-09-28. **Re-export the board to `raw/` to make this page checkable against a snapshot.**

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Fetched | 2026-10-04 (live, via `gh`) |
| New discussions | [#127](https://github.com/SinYang13/IS212-2026/discussions/127), [#128](https://github.com/SinYang13/IS212-2026/discussions/128), [#129](https://github.com/SinYang13/IS212-2026/discussions/129) |
| Answered | [#128](https://github.com/SinYang13/IS212-2026/discussions/128) only |
| Unanswered | [#127](https://github.com/SinYang13/IS212-2026/discussions/127), [#129](https://github.com/SinYang13/IS212-2026/discussions/129), plus [#126](https://github.com/SinYang13/IS212-2026/discussions/126) from the previous export |

## [#128](https://github.com/SinYang13/IS212-2026/discussions/128) — "Week 7 Changes clarification" (General)

Posted 2026-10-02 by `josephsungls`, the customer persona, as the customer's own gloss on [[week-7-customer-changes]]. The board's answer, from `SinYang13` on 2026-10-03, reads "Answered by prof in post", so **the post itself is the answer**. It is the most important item since the notice, because it settles or reverses several readings the wiki had made.

| Change | What #128 says | Effect on the wiki |
|---|---|---|
| C1 Setup and turnaround | "was not part of the 20 core in release 1 at wk 4 but now it is included in the release 1. One major change from what was clarified is the change of **3 slots to 15mins as the smallest unit of booking**" | **The AM / PM / Night slot model is replaced.** The smallest unit of booking is 15 minutes. This overturns the reading that slots stay the booking unit with minutes on top, and supersedes [#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123) on time granularity. [#126](https://github.com/SinYang13/IS212-2026/discussions/126), which asks about the 12nn–1pm gap between slots, is probably moot but remains unanswered |
| C2 Venue unavailable | "was not in the 20 core and now included in the **26 core**" | Release 1 is now **26 core features**: the original 20 plus these six changes |
| C3 Multiple venues | "a new requirement as it was not mandated previously" | Confirms C3 is new scope, not a clarification of [#109](https://github.com/SinYang13/IS212-2026/discussions/109) |
| C4 Holds expire | "was not in the 20 core and now included in the 26 core" | Confirms tentative holding is now in Release 1 |
| C5 Event Coordinator Lead | "This role **was already included** as part of the requirement. The impact is now there is a change of the name of the title of the role at least on the display end, now the role is able to **reassign, view basic event information and also the current assignments of all the coordinator assignments and active events**" | **The Lead is the Event Operations Manager ([#101](https://github.com/SinYang13/IS212-2026/discussions/101)), renamed** — at least in the displayed title — and widened. This answers the question of whether the Lead replaces the Manager, and answers Q2: the oversight view covers **all** coordinators' assignments and active events |
| C6 Operational feasibility | "a new role and requirement **due to regulation**" | A new role and a new requirement. The reason is regulation, which supports treating the gate as mandatory for every event, though the post does not say so |

### What #128 leaves open

- **Is setup and turnaround also in 15-minute units?** The notice's example values (30 and 45 minutes) are multiples of 15, which suggests but does not state it.
- **What survives of AM / PM / Night?** Nothing says whether the three names remain as a display grouping, or whether the 7am–12pm, 1pm–6pm and 7pm–12am hours survive as operating windows.
- **Is the stored role name changed, or only the displayed title?** "At least on the display end" leaves it open.
- **Q3 and Q4 are untouched:** whether the outgoing coordinator is a "relevant user" on reassignment, and whether a Lead may also act as a Coordinator.
- **Q5–Q8 on the safety check, and Q10 on hold expiry,** are not addressed.

## [#129](https://github.com/SinYang13/IS212-2026/discussions/129) — "Events Operation Manager the same as Event Coordinator Lead?" (Users) — *unanswered*

Posted 2026-10-04 by `kchoongg`: is the Event Operations Manager, "mentioned in other discussion posts", the same role as the Event Coordinator Lead in the Week 7 changes? It has no answer on the board, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) (posted two days earlier) already says the role "was already included" and was renamed, so the substance is answered. Treat the question as settled in the wiki and the discussion as awaiting a direct reply.

## [#127](https://github.com/SinYang13/IS212-2026/discussions/127) — "Do we need a separate Open / Closed registration status for events?" (Registration & Attendees) — *unanswered*

Posted 2026-10-01 by `jolenechia2024`: should registration have its own open/closed status, or close automatically once the registration end date passes or the event is full or cancelled? It is unrelated to Week 7. It bears on the registration period and capacity rules already recorded in [[attendee-registration]] and [[registration]] ([#30](https://github.com/SinYang13/IS212-2026/discussions/30), [#99](https://github.com/SinYang13/IS212-2026/discussions/99)). Until it is answered, the wiki's position is unchanged: registration has an enabled flag and optional dates, and refuses a full event.

## Still open

Three discussions are unanswered on the board: [#126](https://github.com/SinYang13/IS212-2026/discussions/126), [#127](https://github.com/SinYang13/IS212-2026/discussions/127) and [#129](https://github.com/SinYang13/IS212-2026/discussions/129). See [[open-questions]].
