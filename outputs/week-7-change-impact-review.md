---
title: "Week 7 Change-Impact Review — Tickets, Stories and Schema"
type: review
sources:
  - "Week 7 Customer Changes.md"
  - "Managing Changes – A Guide.md"
  - "IS212 2026 Discussions Oct 1.md"
  - "IS212 2026 Discussions Sept 16.md"
  - "GitHub Discussions 127 to 129 (live fetch 2026-10-04; summarised in is212-discussions-2026-10-04-addendum)"
tags: [review, change-management, linear, backlog, schema, release-1, week-7]
---

# Week 7 Change-Impact Review — Tickets, Stories and Schema

`raw/Week 7 Customer Changes.md` adds six changes to Release 1. They are binding: "All of the suggested changes should be implemented in your submission. The customer does not anticipate any further changes for Release 1." This review applies `raw/Managing Changes – A Guide.md` to the `SPM - IS212` Linear board (checked 2026-10-04) and to the deployed physical model in [[schema]]. For each change it records:

- the classification
- the backlog action on every affected ticket
- the acceptance-criteria impact
- the estimate
- the design and schema delta
- the tests

**This review edits no ticket or schema.** No Linear ticket was edited or created and no DDL was applied. Week 7 and the Oct 1 export were ingested into the standing wiki afterwards, on 2026-10-04 (see [[#Follow-ups]]).

## Revision — [#128](https://github.com/SinYang13/IS212-2026/discussions/128) and board movement (2026-10-04, later the same day)

Two things landed after this review's first draft, and both overturn parts of it. Everything below the revision has been corrected to match; this section records what moved and why.

**1. The customer clarified the Week 7 changes in [#128](https://github.com/SinYang13/IS212-2026/discussions/128)** (posted 2026-10-02 by the customer persona, answered "by prof in post" on 2026-10-03; not in the Oct 1 export, so the first draft missed it). See [[is212-discussions-2026-10-04-addendum]].

| Item | First draft | Now |
|---|---|---|
| **C1 time model** | "Minutes on top of slots": AM / PM / Night stays the unit a Coordinator selects, conflicts run over padded windows | **The three slots are replaced.** [#128](https://github.com/SinYang13/IS212-2026/discussions/128): "the change of 3 slots to **15mins as the smallest unit of booking**". Setup and turnaround pad a booking on that grid. [#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123) are all superseded. [#126](https://github.com/SinYang13/IS212-2026/discussions/126) is probably moot |
| **C5 Lead vs Operations Manager** | Unresolved (Q1); recommended the Lead supersede the Manager; four Done tickets "reopen, conditional" | **Settled: the Lead is the Manager, renamed** — "was already included as part of the requirement … a change of the name of the title of the role at least on the display end". Q1 closed. The four conditional reopens are cancelled: they become **relabels** |
| **C5 oversight scope** | Q2: all coordinators or a subset | **All.** The Lead views "the current assignments of all the coordinator assignments and active events". Q2 closed; no supervision link |
| **Release 1 size** | Six mandatory changes | The customer now says **"the 26 core"**: the original 20 plus these six. C2 and C4 are named as newly in the 26 |
| **C6** | A new role and gate | Adds that it is "a new role and requirement **due to regulation**", which supports a gate on every event |
| **New unanswered** | [#126](https://github.com/SinYang13/IS212-2026/discussions/126) | Plus [#127](https://github.com/SinYang13/IS212-2026/discussions/127) (a separate Open / Closed registration status?) and [#129](https://github.com/SinYang13/IS212-2026/discussions/129) (are the Manager and the Lead the same role? — answered in substance by [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |

**2. The `SPM - IS212` board moved while this was written.** The first draft read tickets earlier on 2026-10-04; re-reading at the end of the day shows:

| Ticket | First draft | Board now |
|---|---|---|
| **SPM-46** Submit a venue booking request | In Review; AC1 keeps slot selection | Still In Review, but its ACs were **rewritten to the 15-minute grid**: start and end times, touching allowed, "any overlap, even by 15 minutes, is blocked". PRs connectsphere#75 and connectsphere#80 ("book and search venues by 15-minute start and end times") carry it. **Not yet padded** — AC4 compares bare start and end, so setup and turnaround are still to add |
| **SPM-150** date & slot filter (sub of SPM-44) | Not Done | **Done** — implemented on 15-minute start and end times, though its title still says "slot" |
| **SPM-50** Confirm an event | In Review; "fix the AC before the PR merges" | **Done.** It merged without the safety gate. See the corrected C6 section |
| **SPM-41** Specify equipment requirements | In Progress | **Done** |
| **SPM-57** Notify a coordinator of their assignment | In Progress; ask Q3 before PR connectsphere#66 lands | **Done.** The window to ask first has closed; Q3 now decides whether a *new* story is needed |
| **SPM-58, 59, 60** organiser notifications | Todo / Done | **Done** |
| **SPM-104** Record room layout on a booking | In Progress | **Done** |
| **SPM-45** Check venue suitability | Todo | Todo, now split into sub-tasks SPM-246 to 249 |

**Net effect on the numbers:** reopens 4 → **0**; modify-in-place 27 → **24** (SPM-41, 50 and 57 are now Done); new stories 15 cards / 47 points → **15 cards / 45 points** (W7-06 withdrawn, W7-08 smaller, W7-18 added); customer questions still open 10 → **8** (Q1, Q2 closed; Q9 reduced to three residuals).

The remainder of the document has been corrected in place.

## Headline

| | Count |
|---|---|
| Changes | 6, all now in the customer's "26 core" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). C1 contradicts settled answers ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108), [#123](https://github.com/SinYang13/IS212-2026/discussions/123)); C5 is the existing assignment role renamed, with new capabilities, and so contradicts nothing except SPM-99 AC3's ban on oversight |
| Existing tickets reviewed | 53, re-read against the board at the end of 2026-10-04 |
| — modify in place (not Done) | 24 (was 27: SPM-41, 50 and 57 reached Done) |
| — reopen, Scenario 3 | **0** (was 4: the Lead is the Manager renamed, so SPM-130, 29, 16, 98 are relabels) |
| — Done, keep Done + new story, Scenario 2 | 1 (SPM-44 → W7-01) |
| — Done, keep Done; relabel or no change | 12 plus SPM-41, 50, 57. Relabel only: SPM-78, 130, 29, 16, 98 |
| — open, no change needed | 7 (SPM-18, 51, 58, 66, 104, 222, 226) |
| — cancelled/duplicate, leave closed | 6 (SPM-30, 56, 95, 96, 97, 99) |
| New stories proposed | 15 cards, 45 points (W7-01 … W7-18; W7-06 withdrawn; W7-10/11 reserved) |
| Tickets to delete | **0**. Nothing is removed; the guide and [[release-1-scope]] both keep superseded work on the board |
| Schema changes | 1 extension, 2 new tables, column and constraint changes on `booking` and `event`, 1 role seed (a rename) and 1 insert; `booking_slot` becomes obsolete (W8); closes E1 and E3 on [[schema]] (E2 stays open) |

**The three things that matter most:**

1. **C1 replaces the whole time model, not only the buffer rule.** [#128](https://github.com/SinYang13/IS212-2026/discussions/128) swaps the three AM / PM / Night slots for a **15-minute smallest unit of booking**, with per-venue setup and turnaround padding the booking on that grid. Ten tickets carry the slot or adjacent-slot reading in an AC, a Resolved note or a 2026-09-21 comment, and the code has already started to move (SPM-46's ACs, PR connectsphere#80). The double-booking block that was never deployed (E3) now has to be an interval exclusion, not a slot index.
2. **C6 inserts a gate before confirmation, and SPM-50 merged without it.** "Confirm an event" must refuse until the Safety Officer approves. SPM-50 is now Done, so the gate arrives as a new story (W7-18), not an edit before merge.
3. **The Lead is the Operations Manager renamed** ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)), so Q1 and Q2 are closed and the four conditional reopens are cancelled. What still gates work is Q3 (is the outgoing coordinator a "relevant user"?) and Q5 (safety reject versus request changes).

## Method

The guide's three scenarios set the action on each ticket:

| Situation | Guide's action | Applied here as |
|---|---|---|
| Ticket not Done | Update the backlog item; update its ACs; re-estimate only if size materially changes | **Modify** |
| Done, and the change is a *new* requirement | Keep it Done; add a new story (Scenario 2) | **Keep + new story** |
| Done, and an original AC is now *incorrect* | Reopen; estimate only the remaining work (Scenario 3) | **Reopen** |

"Not Done" includes In Review and In Progress: SPM-46 and SPM-49 are still open, so they are amended rather than reopened. (SPM-41, 50, 57 and 104 were open when this review was drafted and are now Done.) Estimates for new stories are **proposals for planning poker**, sized against the board's existing calibration (SPM-46 = 5, SPM-34 = 2). They are not customer facts.

New story cards follow the user-story standard: abstract role, 3–7 binary ACs, and no number the customer did not state. Where Week 7 leaves a parameter open, the card carries a question instead of a guess.

## Change register (guide step 1)

| C | Change | Kind | Contradicts | Release-scope effect |
|---|---|---|---|---|
| C1 | Venue setup and turnaround time | **Correction** of an existing requirement, and a **replacement** of the slot model | [#50](https://github.com/SinYang13/IS212-2026/discussions/50) (AM / PM / Night), [#107](https://github.com/SinYang13/IS212-2026/discussions/107) (two-slot rule), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) (no sub-slot durations), [#123](https://github.com/SinYang13/IS212-2026/discussions/123) (one adjacent slot). Agrees with [#43](https://github.com/SinYang13/IS212-2026/discussions/43) and [[event-planning-venue-booking-system\|brief §4]] | `🧩 Adjacent` → mandatory |
| C2 | Venue becomes unavailable after booking | **New** (extends [#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#120](https://github.com/SinYang13/IS212-2026/discussions/120), [#122](https://github.com/SinYang13/IS212-2026/discussions/122)) | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) let the alternative be agreed outside the system; C2 requires an in-system replacement request | `🧩 Adjacent` → mandatory |
| C3 | Multiple venues for one event | **Clarification** of [#109](https://github.com/SinYang13/IS212-2026/discussions/109)'s "unless the event requires multiple venues" | The wiki's "at most one *confirmed* booking per event" working rule | New, at event level (sessions stay `📋 Backlog`) |
| C4 | Tentative venue holds expire | **New**; answers [#39](https://github.com/SinYang13/IS212-2026/discussions/39)/[#41](https://github.com/SinYang13/IS212-2026/discussions/41)'s deferred expiry | Tentative holding's `📋 Backlog` marker | `📋 Backlog` → mandatory |
| C5 | Event Coordinator Lead role | **Rename plus new capabilities**: [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says the role "was already included" | Nothing settled, except SPM-99 AC3, which forbids the cross-coordinator oversight C5 requires. [#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93), [#94](https://github.com/SinYang13/IS212-2026/discussions/94), [#101](https://github.com/SinYang13/IS212-2026/discussions/101) carry over to the Lead | Coordinator assignment stays `🎯`; the oversight and the title are new |
| C6 | Operational Safety Check | **New role and workflow gate**, "due to regulation" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) | [#80](https://github.com/SinYang13/IS212-2026/discussions/80)/[#98](https://github.com/SinYang13/IS212-2026/discussions/98): confirmation gated only on essential arrangements | New |

> [!warning] Contradiction — C1 vs [#50](https://github.com/SinYang13/IS212-2026/discussions/50), [#107](https://github.com/SinYang13/IS212-2026/discussions/107), [#108](https://github.com/SinYang13/IS212-2026/discussions/108) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123)
> Under [#123](https://github.com/SinYang13/IS212-2026/discussions/123), a 10:00–12:00 AM event occupies the previous Night (setup), AM, and PM (turnaround). Under Week 7 it occupies 9:30–12:45, which reaches neither neighbouring slot. The first draft of this review reconciled the two as "minutes on top of slots". [#128](https://github.com/SinYang13/IS212-2026/discussions/128) rules that out: the customer says the three slots are replaced by a **15-minute smallest unit of booking**. So the AM / PM / Night booking unit ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)) and the two-slot rule ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)) go too. [#126](https://github.com/SinYang13/IS212-2026/discussions/126) (a booking between 12nn and 1pm) is probably moot, but is still unanswered.

---

## C1 — Venue setup and turnaround time

**Change.** Each venue has a configurable setup time and turnaround time. Both count when checking availability and detecting conflicts. Existing bookings that become problematic are *identified*, not silently removed. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) adds the unit: the smallest unit of booking is **15 minutes**, replacing the three slots.

### Backlog and acceptance criteria

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-23** Configure venue turnaround and setup buffer | Backlog · 2 | **Modify; re-estimate 2 → 3** | AC1–AC2 are right again (per venue, [#43](https://github.com/SinYang13/IS212-2026/discussions/43)). **Delete AC3** ("quantised up to whole slots", [#108](https://github.com/SinYang13/IS212-2026/discussions/108)). Add: values are whole minutes ≥ 0 (whether they must be multiples of 15 is Q11); changing a value runs the conflict check over existing live bookings at that venue and flags each clash for review (→ SPM-105), never moves or removes it. Add the `Release 1` label and drop the `🧩 Adjacent` framing. The estimate rises because the padding engine that justified 5 → 2 on 2026-09-08 is back |
| **SPM-46** Submit a venue booking request | **In Review** · 5 | **Modify; the grid is done, the padding is not** | **AC1 and AC4 were already rewritten on 2026-10-04** to start and end times on the 15-minute grid, with touching allowed and any overlap blocked. What is missing is the padding: AC4 compares a request's bare start and end with another booking's, so setup and turnaround never enter. Add: the request's padded window `[start − setup, end + turnaround)` is checked against every live booking's padded window at that venue. Also add the C3 rule (another booking of the *same* event at a different venue is not a clash). PRs connectsphere#75 and connectsphere#80 are in flight: if they merge first, record the padded check as a known gap on the card rather than reopen, because the grid was the customer's change and the padding is additive |
| **SPM-19** Automatic availability update on confirmation | Backlog · 5 | **Modify** | AC3 is still correct. AC1–AC2 say "slot(s)": restate as the booking's occupancy window. Add: a Tentative Hold that has not expired blocks like a Confirmed booking; an expired one does not (C4). Retire the 2026-09-21 comment's buffer-slot questions (a)/(b) |
| **SPM-43** View venue availability calendar | Todo · 5 | **Modify** (largest rework; sub-issues SPM-152 "slot status derivation", SPM-153 "blocked-slot reason", SPM-154, SPM-155) | The calendar is drawn on the 15-minute grid, not three slots, and a period is shown occupied if *any* padded window overlaps it, so setup and turnaround time is visible before a Coordinator submits. Add a "held (expires …)" state (C4) and a reason category on blocked time (C2). SPM-152 and SPM-153 are named for slots and need re-scoping. Re-estimate after the team reviews them |
| **SPM-44** Search and filter venues | **Done** | **Keep Done + new story W7-01** (Scenario 2) | AC2 filters on open time; SPM-150 already implements it on 15-minute start and end times (PR connectsphere#80, Done 2026-10-04), so its "slot" wording is stale but its behaviour is right. Padded availability is a new requirement. Counter-reading: [[event-planning-venue-booking-system\|brief §4]] always mentioned setup time, so a reopen is defensible. This review prefers Scenario 2. Hygiene: relabel SPM-150 |
| **SPM-42** Maintain the venue catalogue | **Done** | **Keep Done** | No AC invalidated; buffer configuration is SPM-23 by design. Its operating-hours AC still stands; whether operating hours now bound a day on the 15-minute grid is Q12. The 2026-09-21 comment ("don't build fields on `setup_time_minutes`") is **reversed**: those columns are now the requirement |
| **SPM-54** Reschedule an event | Backlog · 8 | **Modify** | **Delete the Resolved note** ("overlap is set intersection, not interval arithmetic"); it is now wrong. AC2: padded window, every venue booking of the event (C3) |
| **SPM-20** Cancel venue booking | Backlog · 3 | **Modify** | AC2: frees the booking's padded window, not "slot(s)" |
| **SPM-21** Indicate venue unavailable | Backlog · 3 | Modify (see C2) | AC1 blocks "slots": a period instead. AC3: a block clashes with a booking's padded window, not only its published time |
| **SPM-105** Flag affected bookings for review | Backlog · 3 | **Modify** (central; see C2) | Add setup/turnaround to AC1's trigger list |
| **SPM-224** Book a venue per session | Backlog | **Modify; stays `📋 Backlog`** | AC4 and AC5 restate [#108](https://github.com/SinYang13/IS212-2026/discussions/108)/[#123](https://github.com/SinYang13/IS212-2026/discussions/123): replace with "a session's occupancy is its own time plus the venue's setup and turnaround" |
| **SPM-218** Place a tentative hold on a venue slot | Backlog | Modify (see C4) | Title and ACs name a "slot": a time window instead |
| **SPM-63** Notify venue staff of a cancelled booking | Backlog · 2 | Modify (wording) | AC2/AC3 "slot(s)" → the released window |
| **SPM-209** Review venue usage over a period | Backlog (`📋`) | Modify (wording) | Utilisation is measured in time, not slots; it stays `📋 Backlog` |

The comments posted on 2026-09-21 under [[linear-ticket-audit-sept-16-batch]] (SPM-19, 20, 21, 23, 42, 43, 44, 46, 54) all assume [#123](https://github.com/SinYang13/IS212-2026/discussions/123). **Post one superseding comment on each** pointing here, so nobody builds from the older one.

### New story

**W7-01 · Search venues by padded availability** · Venues · Release 1 · proposed 3
> As an Event Coordinator, I want venue search to exclude venues whose setup or turnaround time would clash during my event's time, so that I don't shortlist a venue that will be refused when I submit.
> - Given a venue whose existing booking's padded window overlaps my event's padded window, then it is not listed as available
> - The padded window counts both my event's buffers at that venue and the other booking's buffers at the same venue
> - A venue's own setup/turnaround values are used, not a system-wide default ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))
> - Venues with time marked unavailable (C2) or an unexpired hold (C4) overlapping the window are excluded
>
> **Open question (Q11):** are setup and turnaround values multiples of 15? The example's 30 and 45 are, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) does not say.

### Schema

- **E1 reversed.** `venue.setup_time_minutes` and `turnaround_time_minutes` are live again. Keep them; whether `null` means 0 or "not configured" is a 🔶 team decision.
- **The slot model goes (W8).** `booking_slot` and its slot enum no longer describe the booking unit. A booking carries `starts_at` and `ends_at`, constrained to the 15-minute grid. **First check what the `connectsphere` repo has already migrated** (PR connectsphere#80, "book and search venues by 15-minute start and end times"): the code may be ahead of [[schema]], and migration 03 should be written against what is deployed.
- **E3 resolved by changing the shape of the rule, not the index.** Booking gains a snapshotted `occupancy tstzrange` = `[starts_at − setup, ends_at + turnaround)`. The live-booking block becomes a GiST **exclusion constraint** on `(venue_id, occupancy)`, which a slot index could never express.
- **"Identified, not removed" needs a flag, not a status.** `booking.review_reason` and `review_flagged_at` record the flag. Occupancy is snapshotted at request time, so a later buffer change never rewrites an existing row: a detection query compares the *new* padded windows and flags the clashes. The migration backfills existing rows with unpadded occupancy so the constraint can be created, then flags any clash the padding reveals.

### Tests

- **Unit:** padded-window computation (the 9:30–12:45 example as a fixture); overlap is half-open, so back-to-back windows don't clash; a start or end off the 15-minute grid is refused.
- **Integration:** an exclusion-constraint violation is refused at the database; a buffer change flags rather than mutates.
- **Regression:** SPM-44 search, SPM-46 submit.
- **E2E:** change a venue's turnaround and see the flagged booking on the calendar.

---

## C2 — Venue becomes unavailable after booking

**Change.** Venue Staff mark a venue temporarily unavailable, for maintenance, equipment failure, renovation, safety or another reason, even over booked dates. The event is not cancelled. Affected bookings are identified, Coordinators informed, original event information preserved, and the Coordinator can search for and request a replacement venue.

### Backlog and acceptance criteria

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-21** Indicate venue unavailable | Backlog · 3 | **Modify; re-estimate 3 → 5; add `Release 1` label** | AC1: a period on a venue, not only slots, with a reason category from the customer's five (maintenance, equipment failure, renovation, safety, other); free text only under "other". Add: allowed even when bookings exist in the period; existing bookings in the period are flagged (→ SPM-105), not cancelled |
| **SPM-105** Flag affected bookings for review | Backlog · 3 | **Modify; re-estimate 3 → 5; add `Release 1` label** | The scope note ("narrower than [#54](https://github.com/SinYang13/IS212-2026/discussions/54)") is now wrong: widen the trigger to unavailability (C2) and buffer changes (C1). Add: the event and its original booking record remain unchanged and readable. AC5: the event's *other* venue bookings are left alone (C3). The open target-status question now has four triggers; resolve it as a flag (see Schema) |
| **SPM-71** Notify the coordinator of an availability change | Backlog · 5 | **Modify; add `Release 1` label** | AC2 names the reason category. Add: from the notification the Coordinator can start a replacement request (W7-02). AC1/AC3 already match ("informed", no auto-cancel) |
| **SPM-20** Cancel venue booking | Backlog · 3 | Modify | Its open question is **answered**: Venue Staff get no cancellation trigger; unavailability flags instead |
| **SPM-63** Notify venue staff of a cancelled booking | Backlog · 2 | Modify | Close the same open question |
| **SPM-55** Cancel an event | Backlog · 8 | Modify | Note that venue unavailability never cancels the event; only AC1's actor can ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)) |
| **SPM-44** / **SPM-46** | Done / In Review | Covered by W7-01 / W7-02 | Search excludes unavailable venues; a replacement request is an ordinary request plus a link |

### New story

**W7-02 · Request a replacement venue** · Venues · Release 1 · proposed 5
> As the assigned Event Coordinator, I want to request a replacement venue for a booking affected by venue unavailability, so that the event can still go ahead without being re-entered from scratch.
> - Given a booking flagged as affected, when I start a replacement, then venue search opens with the event's existing date, time, attendance and layout requirements prefilled
> - The replacement is submitted as an ordinary booking request (SPM-46) and goes through Venue Staff review (SPM-22)
> - The replacement is linked to the booking it replaces, and the original booking and event information remain readable throughout
> - The event is not cancelled at any point in this flow
> - Given the replacement is confirmed, then the original booking no longer blocks the unavailable venue
>
> **Open question:** what status does the original booking end in (`Released`, or a distinct value)? This is the [#54](https://github.com/SinYang13/IS212-2026/discussions/54)/[#120](https://github.com/SinYang13/IS212-2026/discussions/120)/[#122](https://github.com/SinYang13/IS212-2026/discussions/122) question, still a 🔶 team decision.

### Schema

- **New table `venue_unavailability`.** It is period-based (`tstzrange`) with `reason_category` constrained to the customer's five values. This closes the gap [[schema]] never covered: there is no table for blocks today, so SPM-21's slot blocks and C2's periods converge on one model, and the calendar derives "blocked" from it.
- **`booking.replaces_booking_id`** is a self-reference linking a replacement to its original.
- **`booking.review_reason`** (shared with C1) is the flag. This answers the open target-status question without inventing a status: the booking keeps its status and gains a flag.
- **The unavailability-vs-booking clash spans two tables**, so an exclusion constraint can't hold it. A trigger refuses new bookings into an unavailable period; existing ones are flagged by the same detection query as C1.
- **E2 (no venue status for retirement)** is **not** closed by this table. Retirement is permanent, so still decide between `venue.retired_at` and an open-ended unavailability row.

### Tests

- **Unit:** reason-category validation; affected-booking detection over padded windows.
- **Integration:** the trigger refuses a booking into an unavailable period; marking unavailable flags but does not mutate status.
- **E2E:** Venue Staff mark unavailable → Coordinator notified → replacement requested → approved → original no longer blocks.
- **Regression:** SPM-46, SPM-43.

---

## C3 — Multiple venues for one event

**Change.** One event may have several venue bookings at the same time. Each is checked independently for suitability, availability and conflict, and cancelling one does not remove the others.

### Backlog and acceptance criteria

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-46** Submit a venue booking request | In Review · 5 | **Modify** | Add: an event may hold more than one booking; each is checked independently; another booking of the *same* event at a different venue is not a conflict |
| **SPM-45** Check venue suitability | Todo · 5 | **Modify** | AC1 runs per booking against that booking's layout. New open question: is each venue judged against the event's whole attendance or a portion of it? |
| **SPM-20** Cancel venue booking | Backlog · 3 | **Modify** | Add: cancelling one booking leaves the event's other bookings untouched |
| **SPM-55** Cancel an event | Backlog · 8 | **Modify** | AC2 "its venue booking" → *all* of its venue bookings and active holds (C4) |
| **SPM-54** Reschedule an event | Backlog · 8 | Modify | AC2 re-checks every booking |
| **SPM-19**, **SPM-22**, **SPM-62** | Backlog | Modify (wording) | Decided and notified per booking; the notification names which venue |
| **SPM-104** Record the room layout on a booking | In Progress · 5 | **No change** | Already per booking |
| **SPM-224** / **SPM-222** Sessions | Backlog | **Keep `📋 Backlog`** | C3 is simultaneous spaces under *one* event, not sessions. Do not pull sessions in to deliver C3 |

### New story

**W7-03 · Book several venues for one event** · Venues · Release 1 · proposed 3
> As an Event Coordinator, I want to book more than one venue for the same event, so that a large event can use a main hall and breakout rooms at the same time.
> - An event can have more than one live venue booking at once
> - Each booking is checked for suitability, availability and conflicts independently of the event's other bookings
> - The event page lists all of its venue bookings with each one's status
> - Given one booking is cancelled or rejected, then the event's other bookings are unchanged
> - The event's venue arrangement counts as complete for confirmation only when every live booking is Confirmed and none is flagged for review
>
> **Open question:** does each booking carry its own purpose label (e.g. "Breakout A")? Not stated; propose rather than assume.

### Schema

- **No cardinality change.** Event to booking is already 1:N, with no unique constraint on `booking.event_id`.
- **`booking.starts_at` / `ends_at`** (from C1) let a breakout room's times differ from the event's.
- **`event_essential_arrangement`'s PK `(event_id, arrangement_type)`** allows one `venue` row per event. Keep the table, and define venue readiness as *derived* across all live bookings (the last AC above) rather than adding `booking_id` to the PK.
- **[[booking]]'s Relationships** — "at most one normally confirmed per event" — is a wiki inference, now superseded. Fix it at ingest.

### Tests

- **Unit:** readiness derivation across N bookings.
- **Integration:** two bookings of one event at different venues do not conflict; two at the same venue with overlapping windows do.
- **Regression:** SPM-50 readiness, SPM-55.

---

## C4 — Tentative venue holds expire

**Change.** Venue Staff sometimes hold a venue while a Coordinator finalises. Every hold has an expiry date/time. On expiry the venue becomes available, the Coordinator is informed before or at expiry, and an expired hold is never treated as confirmed.

### Backlog and acceptance criteria

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-198** Tentative Venue Holding (epic) | Backlog | **Modify: re-scope `📋 Backlog` → Release 1** | Remove "do not pull into a Release 1 sprint", add the `Release 1` label, and remove the 21-point estimate (epics are not estimated). Children: SPM-218, SPM-219, W7-04, W7-05 |
| **SPM-218** Place a tentative hold | Backlog · unestimated | **Modify; estimate 3** | **Actor answered:** Week 7 says *Venue Staff* "temporarily hold a venue while an Event Coordinator is finalising", so replace the unnamed role. Add: every hold has an expiry date/time when placed. AC2: "active" means unexpired. The buffer open question becomes: a hold reserves its padded window |
| **SPM-219** Confirm or release a hold | Backlog · unestimated | **Modify; estimate 2** | Its open question ("no expiry AC … because no number exists") is answered by C4 requiring an expiry; W7-04 is the third card it anticipated. Add: an expired hold cannot be confirmed |
| **SPM-43** Calendar | Todo · 5 | Modify | Held state shows its expiry; expired holds show as available |
| **SPM-46**, **SPM-19** | | Modify (see C1) | Block on unexpired holds only |
| **SPM-55** Cancel an event | Backlog · 8 | Modify | Releases the event's active holds |

### New stories

**W7-04 · Expire an unconfirmed hold** · Venues · Release 1 · proposed 3
> As Venue Staff, I want a tentative hold to lapse at its expiry time, so that a venue is not reserved indefinitely for an event that never firms up.
> - Given a hold reaches its expiry date/time unconfirmed, then it is marked expired and no longer blocks the venue
> - An expired hold is never shown or counted as a confirmed booking
> - Once a hold expires, another request or hold may take the same time
> - The expired hold remains on record with its expiry time
>
> **Open questions:** who sets the expiry (Venue Staff placing it, or a team-proposed default)? Can it be extended? Week 7 states neither. [#39](https://github.com/SinYang13/IS212-2026/discussions/39) left the duration to the team, so any default is a 🔶 team decision citing [#39](https://github.com/SinYang13/IS212-2026/discussions/39).

**W7-05 · Warn the coordinator of hold expiry** · Notifications (parent SPM-47) · Release 1 · proposed 2
> As the assigned Event Coordinator, I want to be told before and when a hold on my event's venue expires, so that I can finalise in time or arrange an alternative.
> - Given an unconfirmed hold on my event, then I am notified before it expires
> - Given it expires, then I am notified that it has expired and the venue is no longer held
> - Each notification names the venue, the held time and the expiry time
> - I receive each notification once per hold
>
> **Open question:** how long before expiry? Week 7 says "before or when" and gives no lead time; do not invent one. This is the first *time-triggered* notification on the board ("Reminder system" is `📋 Backlog`), so it needs a scheduled job, not an event hook.

### Schema

- **`booking_status_chk` gains `'Expired'`.** "Not treated as a confirmed booking" needs a terminal value distinct from `Released`, which means a person gave it up.
- **`booking_hold_expiry_chk`:** a `Tentative Hold` must have `hold_expires_at`. The column exists today but nothing reads or requires it.
- **`booking.hold_expiry_notified_at`** makes the before-expiry warning idempotent.
- **A scheduled job** (Supabase `pg_cron` or a scheduled edge function) flips due holds to `Expired`. The booking path must also treat `hold_expires_at < now()` as expired, so a late sweep never blocks a legitimate request.

### Tests

- **Unit:** the expiry predicate at, before and after the boundary instant.
- **Integration:** the sweep is idempotent; the exclusion constraint ignores expired rows; a late sweep never blocks.
- **E2E:** hold → warning → expiry → another Coordinator books the time.
- **Regression:** SPM-219 confirm path.

---

## C5 — Event Coordinator Lead

**Change.** New submissions enter an **unassigned queue** visible to the Event Coordinator Lead. The Lead reviews basic event information, assigns a suitable coordinator and reassigns where necessary. Coordinators manage only their own events. The Lead views all coordinator assignments and active events under their supervision. Relevant users are notified on assignment and reassignment.

**What already exists.** The connectsphere repo already has the queue and manual assignment, under the Event Operations Manager: `/staff/ops` with Unassigned and Assigned tabs, the `operations_assign_event_coordinator` function, and `assign-event-coordinator.ts`. A submitted request is `Submitted` with no coordinator until someone assigns one. So the customer's "should no longer be assigned directly" is **already true**. What changes is the title, plus two new capabilities (oversight and post-approval reassignment).

> [!success] Resolved — [#128](https://github.com/SinYang13/IS212-2026/discussions/128): the Lead is the Operations Manager, renamed
> The first draft left this open (Q1) because the notice never mentions the Manager. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) answers it: the role "was already included as part of the requirement. The impact is now there is a change of the name of the title of the role at least on the display end, now the role is able to reassign, view basic event information and also the current assignments of all the coordinator assignments and active events." So [#101](https://github.com/SinYang13/IS212-2026/discussions/101)'s separate system role survives under a new name, and Q2 ("under their supervision") means **all** coordinators. [#129](https://github.com/SinYang13/IS212-2026/discussions/129) asks the same question and is unanswered on the board, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) predates it by two days. **Still open:** "at least on the display end" leaves it unsaid whether the stored role name changes too (Q13).

### Backlog and acceptance criteria

The four tickets the first draft marked "reopen, conditional" are **relabels**. Their acceptance criteria name a role by its old title; the behaviour is unchanged and no criterion was missed or incorrect, so Scenario 3 does not apply.

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-130** Assignment of event requests | **Done** · 3 | **Keep Done; relabel** | AC2/AC3 name the "Event Operations Manager" as the authorisation rule. Rename to the Lead in the AC text and in the UI label. The role check and the `/staff/ops` gate are unchanged. No re-estimate |
| **SPM-29** Viewing of all event requests | **Done** · 2 | **Keep Done; relabel** | Actor in AC1/AC2. Cross-request oversight goes to W7-08, not here |
| **SPM-16** Clear access-denied response | **Done** · 2 | **Keep Done; relabel** | AC4's table row "`/staff/ops/*` → contact your Event Operations Manager" gets the new title. The new `/staff/safety/*` row is new work (W7-12) |
| **SPM-98** Seed pre-loaded accounts | **Done** | **Keep Done; relabel** | AC1 names the Manager and says "six" roles. The Lead is the existing seeded account, retitled; the Safety Officer seed goes on W7-12. The role count is now seven |
| **SPM-78** EOM landing view | Done | **Keep Done; relabel** | No AC is invalidated beyond naming; its out-of-scope note ("coordinator workload… event pipelines") is superseded by W7-08 |
| **SPM-57** Notify a coordinator of assignment | **Done** · 2 | **Keep Done** | It reached Done after the first draft. Its AC4 ("…I do not [get notified]") deliberately skips the outgoing coordinator (SPM-172). Whether "relevant users" includes that coordinator is **Q3**: if yes, it is a new story (Scenario 2), not a reopen |
| **SPM-58** Notify the organiser of a coordinator | **Done** · 2 | No change | AC3 already covers reassignment |
| **SPM-143** Refuse reassignment once Approved | Backlog (bug) | **Modify** | Still valid: the *request's* assignment is frozen at approval. Item 3's "Operations Manager" → Lead. Its out-of-scope pointer ("reassigning on the `event` … is its own card") points at a card that doesn't exist; W7-09 is that card |
| **SPM-121**, **SPM-32** | Done | **Keep Done** | "Coordinators only manage events assigned to them" is already AC1/AC7 and AC4/AC5. Wording-only references to the Operations Manager |
| **SPM-31**, **SPM-34**, **SPM-13**, **SPM-15** | Done | **Keep Done** | Unaffected; new roles' nav entries go on W7-12. If a Lead may also act as a Coordinator (Q4), SPM-15's descoped multi-role AC3 returns |
| **SPM-95/96/97/99/30/56** | Cancelled / Duplicate | **Leave closed** | Don't reopen cancelled cards. Reuse SPM-95 AC1/2/4/5 and SPM-97's ACs in W7-07. **SPM-99 AC3's ban on cross-coordinator oversight is superseded by [#128](https://github.com/SinYang13/IS212-2026/discussions/128).** SPM-56 stays cancelled ([#117](https://github.com/SinYang13/IS212-2026/discussions/117): discoverability, not notification) |

### New stories

**W7-06 · Event Coordinator Lead role** — **withdrawn.** The Lead is the existing role renamed, so there is no new role to build. The rename is carried by the relabels above. The number is not reused.

**W7-07 · Review an unassigned request** · Events · Release 1 · proposed 2
> As an Event Coordinator Lead, I want to open an unassigned request and read its basic event information, so that I can choose a suitable coordinator.
> - The unassigned queue lists submitted requests with no coordinator, reachable on demand, not by notification ([#117](https://github.com/SinYang13/IS212-2026/discussions/117))
> - Each row shows the client organisation, the event and its requested date(s)
> - Opening a row shows the request's basic event information, read-only
> - A withdrawn request does not appear ([#103](https://github.com/SinYang13/IS212-2026/discussions/103))
>
> Mostly built: `operations_view_event_request` exists. The work is mainly the title and a test.

**W7-08 · Oversee coordinator assignments** · Events · Release 1 · proposed **3** (was 5)
> As an Event Coordinator Lead, I want to see every coordinator's assignments and the active events, so that I can spot when work needs to move.
> - Lists each coordinator with the requests and active events assigned to them
> - Covers both event requests and approved events (Planning through Confirmed), not requests only
> - An event appears under its current coordinator and moves when reassigned
>
> The "subset per Lead" acceptance criterion is dropped: [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says **all**, so there is no supervision scope to filter on, and the estimate falls. Outcome visibility must still respect [#125](https://github.com/SinYang13/IS212-2026/discussions/125)'s "legitimate relationship" rule.

**W7-09 · Reassign an active event's coordinator** · Events · Release 1 · proposed 3
> As an Event Coordinator Lead, I want to reassign the coordinator of an approved event, so that cover can change after planning has started.
> - Can reassign an event in Planning, Blocked or Confirmed to a different coordinator; there is no acceptance step ([#95](https://github.com/SinYang13/IS212-2026/discussions/95), [#94](https://github.com/SinYang13/IS212-2026/discussions/94))
> - The previous coordinator loses access to the event; the new one gains it
> - The newly assigned coordinator and the Organiser are notified (SPM-57, SPM-58)
> - The reassignment is recorded with who did it and when
>
> **Open question (Q3):** is the outgoing coordinator also a "relevant user"?

### Schema

- **Role seed:** a **rename**, not an insert: `update role set role_name = 'Event Coordinator Lead' where role_name = 'Event Operations Manager'`. That keeps the `role_id`, so existing grants and code checks hold. If the customer's "display end" means the label only, change the displayed title and leave `role_name` alone (Q13).
- **Supervision:** nothing to add. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says the Lead views all coordinators, so no `supervised_by` column or junction table is needed.
- **No queue DDL.** `event_request.status = 'Submitted' and assigned_coordinator_user_account_id is null` *is* the queue. Post-approval reassignment writes `event.assigned_coordinator_user_account_id`, which already exists. Fix [[schema]]'s comment on `event_request` lines 163–167, which names the Manager.
- **RLS:** [[schema]] still ships with RLS enabled and no policies. The Lead's oversight read is the first policy that crosses coordinators; write it explicitly.

### Tests

- **Integration:** a non-Lead's assign attempt is refused at the RPC as well as the UI; a reassigned event's former coordinator gets not-found ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
- **Regression:** SPM-130, SPM-121, SPM-32, SPM-16 AC1–AC3, SPM-57.
- **E2E:** submit → Lead's queue → assign → coordinator sees it → Lead reassigns → visibility moves.

---

## C6 — Operational Safety Check

**Change.** Once venue and technical arrangements are confirmed, a Safety Officer reviews the event before it proceeds to preparation. The review covers attendance, venue capacity and layout, emergency access, accessibility, equipment placement, crowd movement and venue restrictions. The outcome is approve, reject, or request changes. Changes send the event back to the relevant earlier planning stage, where arrangements may need re-review. The event cannot proceed until the check is approved, and the Officer always responds.

**Where it sits.** The brief's workflow runs Step 8 Technical requirements → **Step 9 Event preparation** → Step 10 Event confirmation ([[event-request-workflow]]). The check sits between Steps 8 and 9, and therefore **before** SPM-50's confirmation. "Venue and technical arrangements confirmed" means bookings `Confirmed` and equipment reserved, not the event's `Confirmed` status.

### Backlog and acceptance criteria

| Ticket | Status | Action | What changes |
|---|---|---|---|
| **SPM-50** Confirm an event | **Done** · 3 (was In Review) | **Keep Done + new story W7-18** (Scenario 2) | The first draft said "modify before merging PR connectsphere#55". It merged on 2026-10-04 without the gate. Its ACs are not *incorrect*: they say confirmation is blocked while an essential arrangement is incomplete, and that is still true. The safety gate is an additional requirement, so it is a new story (W7-18). **Counter-reading:** AC2 now reads as if essentials alone decide confirmation, so a reopen is defensible; it would cost about the same, and the guide prefers Scenario 2 when the original criteria still hold |
| **SPM-144** Decide arrangement essentiality | Backlog | **Modify** | Record the decision that the safety check is a **mandatory gate for every event**, not an `arrangement_type` that could be marked non-essential. Week 7 says the event "cannot proceed … until the Safety Officer has approved it", and [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says the check is "due to regulation", which points the same way without stating it |
| **SPM-53** Revalidate after a significant change | Backlog · 8 | **Modify** | Add: a significant change after safety approval returns the event for a fresh check (pending Q7). Changes requested by the Safety Officer reuse this card's flag-for-review mechanism. At 8 points, split rather than grow it (W7-15) |
| **SPM-49** Update event planning information | Todo · 5 | **Modify** | Add: an edit to attendance, venue or equipment invalidates an existing safety approval (pending Q7) |
| **SPM-41** Specify equipment requirements | **Done** · 5 (was In Progress) | **Keep Done** | Reached Done after the first draft. Its AC1 status list ("Planning, Blocked or Confirmed") and AC8/AC11 (a change to reserved equipment affects equipment placement, a safety factor) only matter if the proposed statuses are adopted or Q7 answers yes; either becomes a new story, not a reopen |
| **SPM-48** Track my event's status | Backlog · 3 | Modify | New statuses need organiser-readable labels (AC4). Should the Organiser see the safety outcome at all? Q8 |
| **SPM-111** Notify organiser that confirmation lapsed | Backlog · 2 | Modify | AC3's list of causes gains "safety approval withdrawn" |
| **SPM-66**, **SPM-51**, **SPM-18**, **SPM-226** | Backlog | No change | Downstream or out of Release 1; SPM-226 stays `📋 Backlog` |

### New stories

**W7-12 · Safety Officer role** · Users · Release 1 · proposed 2
> As ConnectSphere, I want a Safety Officer role, so that only authorised staff can decide an operational safety check.
> - Only Safety Officers can record a safety check outcome; others are refused (SPM-16)
> - The role has its own navigation, its own workspace and an access-denied contact row
> - At least one seeded Safety Officer account exists
> - The Safety Officer sees only events that have reached the safety check, not every event ([#125](https://github.com/SinYang13/IS212-2026/discussions/125))

**W7-13 · See events awaiting a safety check** · Events · Release 1 · proposed 3
> As a Safety Officer, I want a list of events whose venue and technical arrangements are confirmed but not yet safety-checked, so that I know what to review.
> - An event enters the list when all its live venue bookings are Confirmed and its equipment requirements are reserved
> - Each row shows the event, its date, expected attendance and venue(s)
> - An event leaves the list once a check outcome is recorded, and re-enters if it is sent back for a fresh check
> - Events still in planning do not appear
>
> **Open question:** what counts as "technical arrangements confirmed" when an event has no equipment lines? Propose; don't assume.

**W7-14 · Conduct an operational safety check** · Events · Release 1 · proposed 5
> As a Safety Officer, I want to review an event's operational safety and record approve, reject or request changes, so that unsafe arrangements are caught before preparation starts.
> - Shows the inputs the customer listed: expected attendance, each venue's capacity for its chosen layout, accessibility requirements, equipment lines and any known venue restrictions
> - Can record exactly one of: Approved, Rejected, Changes requested
> - Given Rejected or Changes requested, the outcome records which arrangement(s) are affected
> - Each outcome is attributed and timestamped, and earlier checks on the same event stay readable
>
> **Open question (Q5):** how does Reject differ from Request changes? Is reject terminal for the arrangement, or for the event? [#68](https://github.com/SinYang13/IS212-2026/discussions/68) bars anyone but the Coordinator from cancelling, so reject cannot cancel.
>
> Emergency access and crowd movement have no data in the model. Treat them as the Officer's judgement recorded in notes, not fields, unless the customer says otherwise.

**W7-15 · Return an event after a safety outcome** · Events · Release 1 · proposed 5
> As the assigned Event Coordinator, I want an event sent back by the Safety Officer to show me what must change, so that I can rework it and resubmit it for a check.
> - Given Changes requested or Rejected, the event returns to planning and cannot proceed to preparation or confirmation
> - The arrangements the Officer named are flagged for re-review by their owning role (reuses SPM-53)
> - Once the flagged arrangements are confirmed again, the event re-enters the safety check list
> - The event cannot be confirmed until its latest check is Approved (SPM-50)
>
> **Open question (Q6):** "the relevant earlier planning stage" — is that always planning, or does it depend on which arrangement is affected?

**W7-16 · Notify the Safety Officer of an event ready for check** · Notifications (SPM-47) · Release 1 · proposed 2

**W7-17 · Notify the coordinator of the safety outcome** · Notifications (SPM-47) · Release 1 · proposed 2

**W7-18 · Block confirmation until the safety check is approved** · Events · Release 1 · proposed 2
> As the assigned Event Coordinator, I want confirmation refused until the event's safety check is Approved, so that an event cannot be confirmed while its safety is unreviewed.
> - Given the event's latest safety check is not Approved, when I try to confirm, then confirmation is blocked and the pending, rejected or changes-requested check is named
> - Given the latest check is Approved and every essential arrangement is complete, then confirmation proceeds as before (SPM-50)
> - The refusal is made by the system, not only hidden in the UI
> - An event with no safety check at all is treated as not approved
>
> Replaces the "modify SPM-50 before merge" action of the first draft. It depends on W7-14 for the check itself.

W7-16 and W7-17 follow the SPM-57 pattern: name the event, link to it, mark as read. W7-10 and W7-11 are reserved for any reassignment-notification split once Q3 is answered.

### Schema

- **New table `safety_check`:** one row per review round (1:N per event), outcome `Pending | Approved | Rejected | Changes Requested`. A partial unique index allows at most one `Pending` row per event. The gate reads "the latest row is Approved".
- **`event_status_chk` gains `'Safety Review'` and `'Preparation'`**, so that "cannot proceed to preparation until approved" becomes a refused transition, not a convention, and SPM-48 can show it. Transitions:
  - Planning → Safety Review (arrangements confirmed)
  - Safety Review → Preparation (Approved)
  - Safety Review → Planning (Rejected / Changes requested)
  - Preparation → Confirmed (SPM-50)

  🔶 Team decision. The alternative is to keep `Planning` and derive the gate from `safety_check` alone, but Step 9 then has no status, and C6 names it.
- **Role seed `'Safety Officer'`.**
- **`audit_record`** needs no DDL, since it is polymorphic. `notification.trigger_scenario` is free text, so the new triggers need none either.

### Tests

- **Unit:** the state machine, including every refused edge (no Planning → Preparation; no Safety Review → Confirmed).
- **Integration:** a second `Pending` check is refused; SPM-50 confirm is refused without an Approved check.
- **E2E:** request → … → arrangements confirmed → Safety Officer requests changes → coordinator fixes → re-check → approve → confirm.
- **Regression:** SPM-50, SPM-41 status gate, SPM-49.

---

## Consolidated actions

### Create (15 cards)

| ID | Title | Project | Pts | Change |
|---|---|---|---|---|
| W7-01 | Search venues by padded availability | Venues | 3 | C1 |
| W7-02 | Request a replacement venue | Venues | 5 | C2 |
| W7-03 | Book several venues for one event | Venues | 3 | C3 |
| W7-04 | Expire an unconfirmed hold | Venues | 3 | C4 |
| W7-05 | Warn the coordinator of hold expiry | Notifications | 2 | C4 |
| ~~W7-06~~ | ~~Event Coordinator Lead role~~ — **withdrawn**: the Lead is the Manager renamed | — | — | C5 |
| W7-07 | Review an unassigned request | Events | 2 | C5 |
| W7-08 | Oversee coordinator assignments | Events | 3 (was 5) | C5 |
| W7-09 | Reassign an active event's coordinator | Events | 3 | C5 |
| W7-10, W7-11 | *Reserved*: reassignment notifications, if Q3 widens SPM-57 | Notifications | — | C5 |
| W7-12 | Safety Officer role | Users | 2 | C6 |
| W7-13 | See events awaiting a safety check | Events | 3 | C6 |
| W7-14 | Conduct an operational safety check | Events | 5 | C6 |
| W7-15 | Return an event after a safety outcome | Events | 5 | C6 |
| W7-16 | Notify the Safety Officer of an event ready for check | Notifications | 2 | C6 |
| W7-17 | Notify the coordinator of the safety outcome | Notifications | 2 | C6 |
| W7-18 | Block confirmation until the safety check is approved | Events | 2 | C6 (replaces the SPM-50 edit) |

New work: **45 points** across 15 cards, before the open questions are answered. Add a **spike** for the migration itself (backfill, grid constraint, exclusion constraint, flagging query). It is the riskiest single item and should be pulled forward. It must start by reading what `connectsphere` has already migrated.

### Reopen (Scenario 3)

**None.** The first draft listed SPM-130, SPM-29, SPM-16 and SPM-98 conditional on Q1. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) closed Q1 in favour of a rename, so they are relabels (see C5).

### Modify in place (24)

| Area | Tickets |
|---|---|
| Venue time model (C1) | SPM-19, 20, 21, 23, 43, 46, 54, 63, 105, 224 |
| Unavailability (C2) | SPM-21, 55, 71, 105 |
| Multi-venue (C3) | SPM-19, 20, 22, 45, 46, 54, 55, 62 |
| Holds (C4) | SPM-198, 218, 219, 43, 46, 55 |
| Lead (C5) | SPM-143 |
| Safety (C6) | SPM-48, 49, 53, 111, 144 |

Many tickets appear under more than one change; edit each once, carrying every change. **Do these first:** SPM-46 is In Review now (the padding is the missing piece), SPM-49 is In Progress, and SPM-43 and SPM-45 are in the current cycle. SPM-41, 50 and 57 left this list when they reached Done.

**Labels:** add `Release 1` to SPM-21, 23, 71, 104, 105, 198, 218 and 219. The customer now calls the set "the 26 core", so consider a label for the six Week 7 areas.

### Keep Done; relabel only

SPM-78, 130, 29, 16 and 98 (the Lead's new title), and SPM-150 (its title says "slot"). Hygiene: SPM-44 is Done while its sub-tasks were still In Review last week; they are Done now.

### Keep Done, no change

SPM-42, SPM-44 (+ W7-01), SPM-31, SPM-32, SPM-34, SPM-121, SPM-13, SPM-15, SPM-41, SPM-57, SPM-58. SPM-50 stays Done and gains W7-18.

### Remove

None. Superseded content is struck or annotated, not deleted: SPM-23 AC3, SPM-54's Resolved note, SPM-224 AC4–5, SPM-99 AC3, and the nine 2026-09-21 comments.

---

## Schema delta (proposed migration 03, not applied)

Written against [[schema]]'s conventions. Items marked 🔶 are team decisions that the SQL must not silently settle.

```sql
create extension if not exists btree_gist;

-- C1 + C3: booking carries its own time on the 15-minute grid (#128) and a snapshotted padded occupancy (#43, Week 7 C1).
-- CHECK FIRST what connectsphere PR connectsphere#80 has already migrated; write this against the deployed tables, not this sketch.
alter table booking
  add column starts_at            timestamptz,
  add column ends_at              timestamptz,
  add column occupancy            tstzrange,   -- [starts_at - setup, ends_at + turnaround)
  add column review_reason        text,        -- C1/C2: flagged, never silently removed
  add column review_flagged_at    timestamptz,
  add column replaces_booking_id  bigint references booking (booking_id) on delete set null,  -- C2
  add column hold_expiry_notified_at timestamptz,                                             -- C4
  add constraint booking_time_chk   check (starts_at is null or ends_at is null or ends_at > starts_at),
  -- 15-minute smallest unit of booking (#128): whole quarter-hours, no seconds
  add constraint booking_grid_chk   check (
    (starts_at is null or (extract(minute from starts_at)::int % 15 = 0 and extract(second from starts_at) = 0)) and
    (ends_at   is null or (extract(minute from ends_at)::int   % 15 = 0 and extract(second from ends_at)   = 0))),
  add constraint booking_review_chk check ((review_reason is null) = (review_flagged_at is null));

-- C4: expired is its own terminal value; a hold must carry its expiry
alter table booking drop constraint booking_status_chk;
alter table booking add constraint booking_status_chk
  check (status in ('Requested','Tentative Hold','Confirmed','Rejected','Released','Cancelled','Expired'));
alter table booking add constraint booking_hold_expiry_chk
  check (status <> 'Tentative Hold' or hold_expires_at is not null);

-- E3 closed: hard double-booking block over padded windows (#35, #41)
-- backfill occupancy for existing rows from published times FIRST, then:
alter table booking add constraint booking_no_double_booking_excl
  exclude using gist (venue_id with =, occupancy with &&)
  where (status in ('Tentative Hold', 'Confirmed'));
-- then run the padded-window clash query and set review_reason on each hit

-- C2: one model for slot blocks and period unavailability
create table venue_unavailability (
  venue_unavailability_id     bigint generated always as identity primary key,
  venue_id                    bigint not null references venue (venue_id) on delete cascade,
  period                      tstzrange not null,
  reason_category             text not null,
  reason_note                 text,
  recorded_by_user_account_id bigint not null references user_account (user_account_id) on delete restrict,
  recorded_at                 timestamptz not null default now(),
  lifted_at                   timestamptz,
  constraint venue_unavailability_reason_chk
    check (reason_category in ('Maintenance','Equipment failure','Renovation','Safety','Other'))
);
create index venue_unavailability_venue_period_idx on venue_unavailability using gist (venue_id, period);
-- + trigger: refuse a live booking whose occupancy overlaps an unlifted period

-- C6: safety check rounds + the two new event statuses (team decision)
create table safety_check (
  safety_check_id             bigint generated always as identity primary key,
  event_id                    bigint not null references event (event_id) on delete cascade,
  outcome                     text not null default 'Pending',
  requested_at                timestamptz not null default now(),
  decided_by_user_account_id  bigint references user_account (user_account_id) on delete restrict,
  decided_at                  timestamptz,
  affected_arrangements       text,
  notes                       text,
  constraint safety_check_outcome_chk
    check (outcome in ('Pending','Approved','Rejected','Changes Requested')),
  constraint safety_check_decided_chk
    check ((outcome = 'Pending') = (decided_at is null)),
  constraint safety_check_decider_chk
    check (outcome = 'Pending' or decided_by_user_account_id is not null)
);
create unique index safety_check_one_pending_uidx on safety_check (event_id) where outcome = 'Pending';

alter table event drop constraint event_status_chk;
alter table event add constraint event_status_chk
  check (status in ('Planning','Safety Review','Preparation','Blocked','Confirmed','Completed','Cancelled'));

-- C5 / C6: roles. The Lead is the Operations Manager renamed (#128): update, do not insert.
-- If 'display end' means the label only (Q13), skip this update and change the UI label instead.
update role set role_name = 'Event Coordinator Lead' where role_name = 'Event Operations Manager';
insert into role (role_name) values ('Safety Officer');
-- C5: no supervision column. #128 says the Lead views all coordinators.
-- W8: booking_slot no longer describes the booking unit; drop it or keep it as a display grouping once Q12 is answered.
```

| Change | Table / object | Resolves |
|---|---|---|
| C1 | `booking.starts_at/ends_at` on the 15-minute grid, `occupancy`, `booking_no_double_booking_excl`, `btree_gist` | E1 (columns live again), E3 (padded block), W8 (slots obsolete) |
| C1, C2 | `booking.review_reason/review_flagged_at` | The open target-status question on [[booking]] (flag, not status) |
| C2 | `venue_unavailability` + trigger, `booking.replaces_booking_id` | No block table existed; SPM-21's slot blocks converge here |
| C3 | none (already 1:N); venue readiness derived across bookings | The `event_essential_arrangement` single-venue-row limit, by rule rather than DDL |
| C4 | `'Expired'`, `booking_hold_expiry_chk`, `hold_expiry_notified_at`, scheduled job | The unused `hold_expires_at` |
| C5 | role rename; first cross-coordinator RLS policy | — |
| C6 | `safety_check`, `event_status_chk` + 2 values, role insert | — |

**Not resolved by this migration:**

- E2: venue retirement status.
- `venue.capacity` vs per-layout capacity (SPM-106).
- RLS policies generally.
- Whether `null` setup/turnaround means 0, and whether setup and turnaround values must be multiples of 15 (Q11).
- The fate of `booking_slot` and the slot enum (W8, Q12).

**Other design artefacts** the guide lists:

- **C4 container diagram:** add the scheduled job (hold expiry, expiry warnings).
- **Class diagram:** `VenueUnavailability`, `SafetyCheck`, and `Booking.occupancy`.
- **Sequence diagrams** for W7-02 (replacement), W7-04/05 (expiry) and W7-14/15 (safety round-trip).
- [[domain-model]]'s ER diagrams, and the event state diagram on [[event]].

## Wiki-schema impact (CLAUDE.md)

`CLAUDE.md` is this wiki's own schema, and three of its rules no longer fit the source set:

1. **Release-scope markers.** `🎯 Release 1` is defined as "named in the Week 4 briefing's core-20 table". Week 7's six changes are mandatory but are not in that table, so today they could only be marked `🧩 Adjacent`, which misstates them. **Proposed, then applied 2026-10-04:** widen `🎯` to "named in the Week 4 core-20 table **or a later customer change notice**", and cite the change (e.g. `Week 7 C4`).
2. **Citation linking.** There is no convention for citing Week 7. **Proposed, then applied 2026-10-04:** "Week 7 C*N*", linked to [[week-7-customer-changes]], with a row added to the citation table beside `brief §N`.
3. **Roles.** [[user-roles]] now counts seven: five briefed, the Lead (the Operations Manager renamed, [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) and the Safety Officer. The `[!important]` hard-rules list in [[index]] states the Manager rule from [#101](https://github.com/SinYang13/IS212-2026/discussions/101) and the slot rules from [#50](https://github.com/SinYang13/IS212-2026/discussions/50)/[#123](https://github.com/SinYang13/IS212-2026/discussions/123); both were updated on 2026-10-04.

## Questions for the customer

[#128](https://github.com/SinYang13/IS212-2026/discussions/128) answered Q1, Q2 and most of Q9. What remains:

| Q | Question | Blocks |
|---|---|---|
| ~~Q1~~ | ~~Does the Lead replace the Event Operations Manager?~~ **Answered ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)): the same role, renamed** | — |
| ~~Q2~~ | ~~Does "under their supervision" mean all coordinators or a subset?~~ **Answered: all** | — |
| Q3 | On reassignment, is the outgoing coordinator a "relevant user" to notify? | W7-10/11; a possible new story on the now-Done SPM-57 |
| Q4 | Can a Lead also act as an Event Coordinator? | SPM-15 descoped AC3 |
| Q5 | How does a safety **Reject** differ from **Request changes**, and is either terminal? | W7-14, W7-15 |
| Q6 | Which "earlier planning stage" does the event return to? | W7-15; state machine |
| Q7 | Does a significant change after safety approval require a fresh check? | SPM-49, SPM-53 |
| Q8 | Does the Organiser see the safety status or outcome? | SPM-48 |
| ~~Q9~~ | ~~Does [[week-7-customer-changes\|Week 7 C1]] supersede [#123](https://github.com/SinYang13/IS212-2026/discussions/123)/[#108](https://github.com/SinYang13/IS212-2026/discussions/108)?~~ **Answered ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)): yes, the 15-minute unit replaces the slots.** [#126](https://github.com/SinYang13/IS212-2026/discussions/126) is probably moot but unanswered | — |
| Q10 | Who sets a hold's expiry, can it be extended, and how long before expiry is the warning? | W7-04, W7-05 |
| Q11 | Are setup and turnaround values also multiples of 15 minutes? The example's 30 and 45 are | SPM-23, W7-01 |
| Q12 | Do AM / PM / Night survive as a display grouping or as operating windows, and do operating hours ([#46](https://github.com/SinYang13/IS212-2026/discussions/46)) bound a day? | SPM-42, SPM-43, `booking_slot` |
| Q13 | "Change of the name of the title … at least on the display end": does the stored role name change, or only the label? | Role seed; SPM-98 |

Q10 has a team-decision fallback (🔶, citing [#39](https://github.com/SinYang13/IS212-2026/discussions/39)), and Q11 and Q12 can be worked around by treating setup and turnaround as whole minutes and keeping the slot names as labels, so none needs to block a sprint. **Ask Q3 and Q5 first**: they gate cards already in the current cycle. Unrelated to Week 7 but new on the board: [#127](https://github.com/SinYang13/IS212-2026/discussions/127) (a separate Open / Closed registration status) and [#129](https://github.com/SinYang13/IS212-2026/discussions/129) (the Manager-versus-Lead question, answered in substance by [#128](https://github.com/SinYang13/IS212-2026/discussions/128)).

## Follow-ups

- **Not done here:**
  - Linear edits and ticket creation. The 15 cards, 24 edits and 6 relabels above are ready to apply. **Check the board again before applying**: it moved three times while this review was written.
  - Migration 03. Read `connectsphere`'s own migrations first (PR connectsphere#80 already stores 15-minute start and end times).
- **Full ingest of `raw/Week 7 Customer Changes.md`, `raw/IS212 2026 Discussions Oct 1.md` and [#128](https://github.com/SinYang13/IS212-2026/discussions/128)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129) — done 2026-10-04:**
  - source summaries: [[week-7-customer-changes]], [[is212-discussions-2026-10-01]] and [[is212-discussions-2026-10-04-addendum]]
  - the slot model marked superseded on [[venue-booking]], [[venue]] and [[booking]], with a contradiction callout on each
  - [[release-1-scope]] markers for C1–C6, after widening `🎯` in `CLAUDE.md` (see Wiki-schema impact, item 1 — applied)
  - the Lead recorded on [[user-roles]] and [[user-account]] as the Operations Manager renamed; the Safety Officer added; new pages [[operational-safety-check]], [[safety-check]] and [[venue-unavailability]]
  - the event state machine on [[event]] and [[event-lifecycle]]
  - E1–E3 on [[schema]], which also gains W1–W8; the DDL itself is unchanged
- **Export the board to `raw/` again.** [[is212-discussions-2026-10-04-addendum]] reads [#127](https://github.com/SinYang13/IS212-2026/discussions/127)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129) from the live board, because no export contains them. Until a newer export exists those citations cannot be checked against a snapshot.
- **[[open-questions]]:** [#39](https://github.com/SinYang13/IS212-2026/discussions/39)'s expiry and SPM-218's actor are answered; the booking target status is answered as a flag (team proposal); Q1, Q2 and most of Q9 are closed by [#128](https://github.com/SinYang13/IS212-2026/discussions/128).
