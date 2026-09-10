---
title: "Discussions Sept 8 — Critical Diffs"
type: review
sources:
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [review, diff, clarifications, github-discussions, ingestion, scrutiny]
---

# Discussions Sept 8 — Critical Diffs

An incremental-ingestion report on `raw/IS212 2026 Discussions Sept 8.md` against the previously ingested `raw/IS212 2026 Discussions Aug 31.md`. This page exists to answer one question: **of the 28 things that changed, which ones change what we had already decided?**

Seven do. The other 21 are additive — new facts on topics the wiki had recorded as open or unaddressed, absorbed into the relevant pages without contradiction. The full batch is summarised in [[is212-discussions-2026-09-08]]; only the load-bearing changes are argued here.

**Verdict:** no answer in this batch reverses a *customer* position. Every conflict is between a new answer and an **inference the team had made in the customer's silence** — which is the expected and healthy failure mode, but four of them had already reached the schema or the scope register, so the blast radius is real.

## Establishing the diff

The two snapshots were split per-discussion on the `#### [#N]` heading and compared block-by-block, rather than by whole-file diff, which is dominated by section-count churn.

| Change class | Count | Which |
|---|---|---|
| New discussions | 26 | [#96](https://github.com/SinYang13/IS212-2026/discussions/96)–[#121](https://github.com/SinYang13/IS212-2026/discussions/121) |
| New follow-up answer on an existing discussion | 2 | [#38](https://github.com/SinYang13/IS212-2026/discussions/38), [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| Status-flag flip, text byte-identical | 2 | [#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#55](https://github.com/SinYang13/IS212-2026/discussions/55) |
| Answer text edited or retracted | 0 | — |
| Discussions removed | 0 | — |

The Sept 8 file is a strict superset: every one of the 87 Aug 31 discussions is present with its text intact. The board now reports **0 unanswered**, down from 2 — but both of those were already resolved in substance on 2026-08-31 and [[is212-discussions-2026-08-31]] said so at the time. No new information.

## Severity summary

| # | Finding | What it hits | Severity |
|---|---|---|---|
| C1 | Event Operations Manager must be a real system role — the "gap" is closed | [[user-roles]], [[user-account]], [[open-questions]], [[schema]] | 🔴 Critical |
| C2 | Venue capacity varies by layout — it is not one number per venue | [[venue]], [[venue-suitability]], [[schema]] | 🔴 Critical |
| C3 | Request-level `Withdrawn` is now grounded and must be restored | [[event-request]], [[schema]] | 🔴 Critical |
| H1 | Organiser may not edit a submitted request at all | [[event-request-workflow]], [[change-impact-management]], [[access-control]] | 🟠 High |
| H2 | Change requests are multi-field and **partially** approvable | [[change-request]], [[schema]] | 🟠 High |
| H3 | Onboarding closed for staff and clients — pre-loaded accounts | [[access-control]], [[user-account]], [[release-1-scope]] | 🟠 High |
| M1 | Slot is the atomic unit of setup and turnaround | [[venue-booking]], [[venue]] | 🟡 Medium |
| M2 | Registration capacity is a hard cap, now with a citation | [[registration]], [[attendee-registration]] | 🟡 Medium |
| S1 | **Surfaced, not caused; storage now fixed.** The committed equipment availability rule was unrepresentable — `equipment_reservation` held no date columns. A nullable `return_date` was added 2026-09-09; what populates it is still open | [[schema]], [[equipment-reservation]] | 🔴 Critical |
| S2 | **Surfaced, not caused:** booking status after a venue change has no defined target state | [[booking]] | 🟡 Medium |
| S3 | **Found while migrating:** `booking` records no chosen room layout, so per-layout capacity can filter a venue search but cannot validate a booking | [[booking]], [[schema]] | 🟠 High |

S1–S3 are not diffs — no answer in this batch created them. They are pre-existing defects the batch made undeniable, either by removing the possibility that the customer would supply the missing piece (S1, S2) or by adding a structure whose dependency was then found missing (S3). S1 and S2 are argued under [Two answers that quietly add risk](#two-answers-that-quietly-add-risk); S3 under C2. **S1's storage half is now fixed** — see [[schema-migration-02]], which also lists every part that is not.

## Critical

### C1 — The Event Operations Manager is a system role, not a briefing gap

**[#101](https://github.com/SinYang13/IS212-2026/discussions/101):** *"the Event Operations Manager is the role responsible for assigning and reassigning Event Coordinators. It must be implemented as a separate system role. The team may propose the detailed access-control implementation, as long as only authorised users can perform these assignment functions."*

**What the wiki said.** [[user-roles]] carried an entire section headed `## Gap: The Event Operations Manager`, arguing the role "appears only in the Q&A discussions and is **absent from the customer brief's role table, feature list, and 14-step process**… a genuine omission that the team must resolve". [[user-account]] carried a matching `[!warning] The Event Operations Manager is a gap, not a contradiction`. Both pages listed *"Whether the Event Operations Manager is modelled as a system role"* as an open team decision.

**Why this is the batch's most consequential change.** The reasoning was correct — the brief really does enumerate five roles — but it had propagated into the scope register as a build blocker. [[open-questions]] carries this row, marked 🎯:

> **Who is authorised to assign a coordinator?** … Feature 1 enumerates exactly five authorisable roles; feature 5 says submitted events "can be assigned" without naming an actor… **Feature 5 is unbuildable without an answer.**

That blocker is now cleared, and cleared in the direction the wiki had listed as only one of three candidate options ("an Event Coordinator self-assigning, a sixth role, or a seeded manager account"). It is the sixth role.

**What is *not* settled.** [#101](https://github.com/SinYang13/IS212-2026/discussions/101) mandates the role's existence and one constraint on it — only authorised users may assign. It does **not** give the EOM a visibility scope, a dashboard, or any capability beyond assignment and reassignment. Do not read "separate system role" as a licence to grant it broad administrative rights; [#52](https://github.com/SinYang13/IS212-2026/discussions/52) still says there is no admin role, and [#93](https://github.com/SinYang13/IS212-2026/discussions/93) still puts the *choice* of coordinator in an SOP outside the system.

**Schema consequence: none — it was already right.** [[schema]] models roles as a `role` lookup table rather than a check constraint, and its seed block already inserts `'Event Operations Manager'` alongside the five briefed roles. The DDL had quietly taken the position that [#101](https://github.com/SinYang13/IS212-2026/discussions/101) has now confirmed, while the concept pages still called it a gap. The correction is entirely on the prose side — which is worth noting, because it means the wiki was internally inconsistent *before* this batch and the schema, not the concept pages, had it right.

### C2 — Venue capacity varies by room layout

**[#112](https://github.com/SinYang13/IS212-2026/discussions/112):** *"Venue capacity varies by room layout. The capacity for each supported layout will be provided as part of the venue attributes, so the system is not required to estimate or calculate the capacity. The relevant layout-specific capacity should be used when evaluating venue suitability."*

**What the wiki said.** [[venue]] modelled capacity as a single scalar — `| Capacity | Maximum attendance the venue can hold | ✅ Specified |` — and separately carried a `🔶 Team decision` row plus a foldable callout asking exactly this question:

> Whether it should be modelled as its own object (e.g. **to support capacity that varies by layout**) or remains a plain enum attribute on Venue is never addressed.

The customer has now answered the question the wiki had correctly identified and correctly deferred.

**Why it is critical rather than cosmetic.** This is a cardinality change in the object model, not a field edit. Capacity moves from an attribute *of a venue* to an attribute *of a (venue, layout) pair*. Everything downstream that compares expected attendance to "the venue's capacity" now has to say **which** capacity — including feature 11's suitability check, which is 🎯 Release 1, and feature 10's capacity filter. A venue with a 200-seat theatre layout and an 80-seat boardroom layout is suitable for a 150-person event only under one of them, and the booking has to record which.

**Two things the answer gives us for free.** The per-layout figures are **supplied as venue attributes** — the system does not estimate or compute them, which removes an entire class of derivation logic. And [#119](https://github.com/SinYang13/IS212-2026/discussions/119), landing in the same batch, keeps *expected attendance* as a single event-level value rather than pushing it into the venue-requirements structure, so only one side of the comparison became plural.

**Schema consequence — a real one, unlike C1. Now applied.** [[schema]] had `venue.capacity` as a single nullable integer and `venue_supported_layout` as a bare `(venue_id, room_layout_id)` junction, so the per-layout figure had nowhere to live. A `capacity` column was added to `venue_supported_layout` on 2026-09-09 ([[schema-migration-02]]). **Two things did not get resolved by that.** `venue.capacity` was retained rather than derived or dropped, so capacity now lives in two places with no stated precedence — treat the scalar as untrusted. And `booking` records no chosen room layout at all, so the system knows every venue's per-layout capacities but not which layout a given booking assumes; the new column can filter a search but cannot yet validate a booking.

> [!warning] The over-capacity hedge is still not resolved
> [#112](https://github.com/SinYang13/IS212-2026/discussions/112) says which capacity number to compare against. It does **not** say what happens when the comparison fails. The brief's *"a venue should **not normally** be treated as suitable when expected attendance exceeds its capacity"* keeps its hedge, and [[venue]]'s `[!warning] Capacity threshold never tightened` stands. Do not let C2 be mistaken for closing it — and note that M2 below hardens the *registration* ceiling only, which is a different rule on a different object.

### C3 — Request-level `Withdrawn` is grounded after all

**[#103](https://github.com/SinYang13/IS212-2026/discussions/103):** *"An Event Organiser may request the Event Coordinator, outside the system, to withdraw a submitted event request while it is under review. The Event Coordinator will process and record the withdrawal in the system. The withdrawn request should remain distinguishable from draft, submitted, approved, rejected, or other request states."*

**What the wiki said.** Both [[event-request]] and [[schema]] had explicitly *deleted* this value and left a note explaining why:

> Neither `In Progress` nor a request-level `Withdrawn` is grounded in any source; both appeared in earlier schema drafts and have been removed.

That deletion was correct on the evidence available — and [[current-schema-scrutiny]] had listed the invented `Withdrawn` as finding **C3**, a 🔴 Critical defect. The customer has now supplied the grounding that was missing. The `In Progress` half of the note remains correct and should survive.

**This was a schema change, not a prose fix — and it has been made.** `event_request_status_chk` enumerated exactly six values; `'Withdrawn'` was added as a seventh on 2026-09-09, along with the status table in [[schema]] and the state machine in [[event-request]] ([[schema-migration-02]]). Note this reverses a deliberate decision in [[schema-migration-01]], which had dropped `Withdrawn` as ungrounded — correctly, on the evidence then available.

**Read the answer precisely — it is narrower than "the Organiser can withdraw".** Three constraints are stated:

| Stated | Consequence |
|---|---|
| The Organiser **requests** withdrawal *outside the system* | There is no Organiser-facing withdraw action to build. Do not add one |
| The **Coordinator** processes and records it | The transition is Coordinator-actioned, like approve/reject/return |
| The withdrawn state must stay **distinguishable** from draft, submitted, approved, rejected | It is a distinct status value, not a soft-delete or an `is_active` flag |

The question asked about withdrawal *before* review; the answer describes it *while under review*. The exact set of source states from which withdrawal is reachable is therefore ambiguous — resolve it as a team decision rather than guessing an edge into the state machine.

## High

### H1 — The Organiser may not edit a submitted request at all

**[#102](https://github.com/SinYang13/IS212-2026/discussions/102):** *"After submission, the Event Organiser should not directly edit the submitted request. Any required changes, including non-significant changes, should be communicated to or requested through the Event Coordinator."*

No page asserted the opposite, so this is not a contradiction — but it is a **hard access-control rule that nothing in the wiki had stated**, and it is the kind of omission that produces a wrong build. The natural implementation of a draft-then-submit form leaves the Organiser's edit affordance in place after submission; this forbids it outright, for *every* field, significant or not.

The significant/non-significant distinction survives — but its job has narrowed. It no longer has anything to do with *who may edit*. It now governs only whether existing arrangements need revalidation once the Coordinator has made the change.

Together with C3, a consistent picture: **after submission, the Organiser has no direct write access to the request.** Changes and withdrawals alike route through the Coordinator, and the request-side conversation happens outside the system.

### H2 — Change requests span multiple fields and can be partially approved

**[#104](https://github.com/SinYang13/IS212-2026/discussions/104):** *"A single change request may cover several fields, and some changes may be accepted while others are not. Where a change is not accepted, the relevant reason or follow-up should be communicated to the Event Organiser."*

[[change-request]] had *"Fields affected"* and *"Significance flag"* as `⚠️ Partial`, citing [#4](https://github.com/SinYang13/IS212-2026/discussions/4). The new answer makes this structural rather than descriptive: **the approval outcome is per-field, not per-request.** A change request cannot carry one status; it carries a set of line outcomes plus a reason on each rejected line.

This is the same shape as [#90](https://github.com/SinYang13/IS212-2026/discussions/90)'s partial equipment reservation, where some requirement lines are fulfilled and others highlighted — worth modelling consistently rather than inventing a second partial-outcome pattern.

**Schema consequence — now applied.** [[schema]]'s `change_request` was flat: one `significance_flag boolean`, one `fields_affected text`, one `coordinator_decision text`. A single decision column cannot express "these two fields accepted, that one declined", and there was nowhere to store the per-field reason. A `change_request_item` child table was added on 2026-09-09, with a check constraint making `decision_reason` mandatory when a field is rejected — the customer's "communicate the reason" requirement enforced in the schema rather than left to the application ([[schema-migration-02]]). **One trap remains:** the flat columns were retained alongside the child table, and nothing enforces which wins.

Feature 19 (Event change requests) is 🎯 Release 1, so this lands inside the release, not the backlog — which makes D4 a Release 1 blocker rather than a modelling nicety.

### H3 — Onboarding is closed for staff and clients, but not for attendees

**[#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up** replaces the old, ambiguous *"the onboarding process feature is currently not in the list of features as yet"* with a per-class ruling: no self-registration or onboarding for ConnectSphere employees or Event Organisers in Release 1; **pre-loaded** staff and client accounts may be used. Attendee Registration remains a required Release 1 feature but *"refers to registering for an event, not creating a system account"*.

This closes [[user-account]]'s `[!question]- Team decision required — onboarding and self-registration` and [[access-control]]'s matching open row for two of the three user classes, and it converts a hand-wave into a concrete, testable Release 1 stance: **seed the accounts**.

> [!warning] The guest-attendee credential gap survives this answer — do not close it
> The same reply also states that attendees must be able to *"view appropriate event information, register, view their registration status, and withdraw"*. Pre-loaded accounts are offered for **staff and clients only**. An account-less attendee still holds a `registration_reference` and no credential, so *view status* and *withdraw* remain undeliverable. [[release-1-scope]]'s narrowed registration callout is still accurate and must stay. If anything this answer sharpens the problem, because it re-confirms all four verbs as required while removing seeded attendee accounts as an escape route.

## Medium

### M1 — The slot is the atomic unit of setup and turnaround

**[#108](https://github.com/SinYang13/IS212-2026/discussions/108):** one slot is the smallest unit for setup and turnaround; sub-slot durations are not required for Release 1. Paired with **[#107](https://github.com/SinYang13/IS212-2026/discussions/107)**: an event spanning two slots consumes **both in full**.

[[venue-booking]] had flagged this as unresolved in as many words — *"Whether per-venue turnaround adds on top of slot boundaries is not stated and should be resolved by the team."* It is now resolved, and it simplifies conflict detection considerably: occupancy is a set of whole slots, so overlap is set intersection rather than interval arithmetic over timestamps.

Per-venue turnaround ([#43](https://github.com/SinYang13/IS212-2026/discussions/43)) still exists as a recorded venue attribute; it is now quantised to whole slots when it feeds availability. Note this does **not** explain the unbooked 12pm–1pm and 6pm–7pm gaps in the slot definition, which remain unexplained on [[venue]].

### M2 — Registration capacity is a hard cap, and now has a citation

**[#99](https://github.com/SinYang13/IS212-2026/discussions/99):** *"VIPs may be added manually even when normal registration is full, but the overall event/venue capacity must still not be exceeded."*

[[registration]] already argued that a full event is refused outright in Release 1, but reached it by implementation inference — the brief's *"prevent **or appropriately handle**"* has only one available meaning once the waiting list is `📋 Backlog`. That inference now has a customer citation behind it, and it survives the strongest available counter-example: a VIP, added manually, by an Organiser, on a full event. Capacity still wins.

**Do not over-read it in two directions.** First, [#99](https://github.com/SinYang13/IS212-2026/discussions/99) does not disturb [#33](https://github.com/SinYang13/IS212-2026/discussions/33)'s routing — the question presupposed an Organiser adding a VIP directly, and the answer simply did not address who performs the act, so the VIP list still goes to the Coordinator. Second, this is the **registration** ceiling, a different rule on a different object from the venue-suitability *"not normally"* hedge discussed under C2.

## The 21 additive answers

Recorded here for completeness; none contradicts a standing position. Full detail in [[is212-discussions-2026-09-08]].

| Area | Answers |
|---|---|
| Scope narrowed | [#118](https://github.com/SinYang13/IS212-2026/discussions/118) no role-based dashboard · [#96](https://github.com/SinYang13/IS212-2026/discussions/96) no complete audit-history function · [#100](https://github.com/SinYang13/IS212-2026/discussions/100) categories not core, list non-exhaustive · [#98](https://github.com/SinYang13/IS212-2026/discussions/98) confirmation not a standalone feature but the transition must work · [#109](https://github.com/SinYang13/IS212-2026/discussions/109) simultaneous multi-venue holds not required |
| Events | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) Organiser told when a confirmed event un-confirms · [#106](https://github.com/SinYang13/IS212-2026/discussions/106) essentialness decided jointly with Technical Support Staff · [#119](https://github.com/SinYang13/IS212-2026/discussions/119) expected attendance stays event-level · [#121](https://github.com/SinYang13/IS212-2026/discussions/121) postponement without a new date confirmed |
| Venues | [#110](https://github.com/SinYang13/IS212-2026/discussions/110) Venue Staff suggest, Coordinator amends · [#111](https://github.com/SinYang13/IS212-2026/discussions/111) Organiser venue preference is non-binding · [#120](https://github.com/SinYang13/IS212-2026/discussions/120) venue attribute change flags bookings for review, does not auto-revert |
| Equipment | [#113](https://github.com/SinYang13/IS212-2026/discussions/113) no default return day · [#114](https://github.com/SinYang13/IS212-2026/discussions/114) requirements changeable after reservation |
| Registration | [#115](https://github.com/SinYang13/IS212-2026/discussions/115) registrations survive a reschedule · [#116](https://github.com/SinYang13/IS212-2026/discussions/116) no mandated waiting-list cut-off · [#97](https://github.com/SinYang13/IS212-2026/discussions/97) attendees not required to use the app · [#38](https://github.com/SinYang13/IS212-2026/discussions/38) check-in by an existing internal role, no new role |
| Notifications | [#117](https://github.com/SinYang13/IS212-2026/discussions/117) EOM not notified per submission, but must be able to find unassigned ones |
| No action | [#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#55](https://github.com/SinYang13/IS212-2026/discussions/55) — flag flips only, already recorded as resolved |

## Two answers that quietly add risk

Neither is a contradiction, so neither is graded above — but both put an unsettled input underneath a settled rule, which is the pattern that produces late surprises.

**[#113](https://github.com/SinYang13/IS212-2026/discussions/113) — no default equipment return day, and (at the time) nowhere to put one.** The D-1 / Return Day + 1 availability rule is one of the customer's few genuinely committed constraints, listed in [[index]]'s hard-rules callout. Its entire arithmetic pivots on the return day — and the customer has now said there is no default and the team must decide how the date is captured. A fixed rule now rests on an undefined input.

Checking this against [[schema]] turned up something sharper than [#113](https://github.com/SinYang13/IS212-2026/discussions/113) itself. **`equipment_reservation` had no date columns whatsoever** — not a return date, not a collection date, not a reservation period. Its full column list is `equipment_reservation_id`, `event_id`, `session_id`, `reviewed_by_user_account_id`, `status`, `created_at`, `updated_at`. The D-1 half of the rule is at least *derivable* by joining to the event's start date; the Return Day + 1 half was not derivable from anything the schema held, because nothing recorded when the equipment actually comes back.

So the committed availability rule was **unrepresentable**, and core feature 16 (Equipment availability checking, 🎯 Release 1) could not return a correct answer against that schema. This predates the Sept 8 batch — but [#113](https://github.com/SinYang13/IS212-2026/discussions/113) is what makes it undeniable, because it removes the one escape hatch left, namely a convention the customer might have supplied. Minimum fix: a nullable `return_date` on `equipment_reservation`, with the population mechanism recorded as the [#113](https://github.com/SinYang13/IS212-2026/discussions/113) team decision. **Applied 2026-09-09** ([[schema-migration-02]]) — so the rule is now representable, but the column is nullable with no default, and feature 16 still returns an undefined answer for any reservation that leaves it unset. Storage was the easy half. See [[equipment-reservation]], [[schema]].

**[#120](https://github.com/SinYang13/IS212-2026/discussions/120) — booking status after a venue change is undefined.** The answer is clear that a venue attribute update must *not* auto-revert bookings to `Requested`, and clear that affected bookings must be identified and the Coordinator notified. What it explicitly leaves open is the resulting status transition — so the booking state machine has an inbound event with no defined target state. See [[booking]].

## Related pages

- [[is212-discussions-2026-09-08]] — the source summary for this snapshot
- [[is212-discussions-2026-08-31]] — the superseded baseline this diff is taken against
- [[open-questions]] — consolidated register, updated for the items closed here
- [[release-1-scope]] — scope register, updated for the five narrowing answers
- [[current-schema-scrutiny]] — whose finding C3 (invented `Withdrawn`) is partly reversed by C3 above
