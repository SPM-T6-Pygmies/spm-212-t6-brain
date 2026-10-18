---
title: Event Lifecycle
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Sept 8.md
  - Week 7 Customer Changes.md
tags:
  - event-status
  - state-machine
  - business-rules
  - connectsphere
---

# Event Lifecycle

**Objects:** [[event]]

The set of statuses an event passes through in the [[event-planning-venue-booking-system]], and the rules governing transitions between them. The lifecycle is the backbone that [[event-request-workflow]], [[event-readiness]], and [[change-impact-management]] all attach to.

**Release 1 scope:** Per [[release-1-scope]], event status management is 🎯 Release 1 (core feature 6); event confirmation and cancellation management are 🧩 Adjacent — confirmation because `confirmed` is a feature 6 status and a feature 20 notification trigger, cancellation because `cancelled` is likewise a feature 6 status and feature 20 trigger. The **Operational Safety Check** that now sits before preparation and confirmation is 🎯 Release 1, made mandatory by [[week-7-customer-changes|Week 7 C6]] — see [[operational-safety-check]].

## Statuses

The brief lists the following as an example set — "Events progress through suitable statuses so that users can understand their current stage, for example draft, submitted, under review, approved, planning, awaiting arrangements, confirmed, completed, cancelled, or rejected" (Source: customer-brief-no-1). The full status table, entry/exit rules, and state diagram are maintained on [[event]] — see States.

The team has since fixed the status set, splitting the brief's single list across the two records it spans. Each value has exactly one owner:

| Record | Statuses | Page |
|---|---|---|
| [[event-request]] | Draft, Submitted, Under Review, Approved, Rejected, Returned, Withdrawn | [[event-request]] |
| [[event]] | Planning, Blocked, Confirmed, Completed, Cancelled | [[event]] |

> [!info] Proposed additions are not part of the settled set
> The two tables above are the deployed set. [[week-7-customer-changes|Week 7 C6]] puts an Operational Safety Check between Step 8 and Step 9, and the team's proposal adds two **event** statuses to carry it: `Safety Review` and `Preparation` (🔶, [[week-7-customer-changes|Week 7 C6]], [#98](https://github.com/SinYang13/IS212-2026/discussions/98), [#80](https://github.com/SinYang13/IS212-2026/discussions/80)). They are not applied to [[schema]]. The alternative is to keep `Planning` and derive the gate from [[safety-check]] rows alone, which leaves Step 9 without a status. The proposed transitions are drawn on [[event]].

`Returned` is [[event-request-workflow|brief §5]]'s return-for-amendment outcome ("An event request is approved, rejected, or returned"). `Blocked` is the team's name for the brief's *awaiting arrangements* — retained as a distinct state rather than collapsed into a readiness flag on *planning*, because [#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s confirmation gate needs somewhere to live.

`Withdrawn` is the newest value and the only one not derived from the brief's example list: the Event Coordinator records a withdrawal that the Organiser asked for outside the system, and the withdrawn request must "remain distinguishable from draft, submitted, approved, rejected, or other request states" ([#103](https://github.com/SinYang13/IS212-2026/discussions/103)). [[schema]] enforces both sets, `event_request`'s having been widened to admit `Withdrawn` on 2026-09-09 (migration D3); the deployed DDL and this page no longer diverge. See [[event-request]] for the transition rules.

> [!question]- Team decision required — permitted transitions between the fixed statuses
> The status *set* is now settled, but the customer never fixed the state machine over it. The transitions drawn on [[event]] and [[event-request]] remain this wiki's proposal — in particular the backward `Confirmed → Planning` edge for major post-confirmation change, which the customer confirmed only as "arrangements may require... to be reviewed again" ([#70](https://github.com/SinYang13/IS212-2026/discussions/70)). The safety loop adds more: the customer fixes only that a change request sends the event back to "the relevant earlier planning stage" ([[week-7-customer-changes|Week 7 C6]]); every edge into and out of the proposed `Safety Review` and `Preparation` statuses is the team's.

## Approved vs Confirmed

The single most load-bearing distinction in the lifecycle, clarified in full on 2026-08-31 (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)).

| Dimension | Approved | Confirmed |
|---|---|---|
| What it asserts | The request contains sufficient information for planning to proceed | The essential arrangements for that event are complete |
| Commitment | **None** to venue, equipment, technical support, registration setup, or final delivery | ConnectSphere commits to the arrangements; the Event Organiser can view them |
| Position in flow | Step 5 of the 14-step process | Step 10 of the 14-step process |
| Precondition | Coordinator judges information adequate after review and clarification | All arrangements identified as essential for that event are complete, **and** the Safety Officer has approved the event ([[week-7-customer-changes\|Week 7 C6]]) |

Key consequences of [#80](https://github.com/SinYang13/IS212-2026/discussions/80):

- Approval is an **information-sufficiency gate**, not a resource commitment. Nothing is reserved by approving.
- Confirmation is a **readiness gate**. See [[event-readiness]] for what counts as essential.
- An event **may** be confirmed while non-essential follow-up actions remain outstanding, provided every essential arrangement is complete ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), answer 4).
- The system **should block confirmation** when an arrangement identified as required for that event is still incomplete ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), answer 5).
- Confirmation needs **no standalone feature**, but the Release 1 workflow must let an Event Coordinator confirm an event once its essential arrangements are complete, must take the event to a `Confirmed` status, and must reflect that state to relevant users. The UI or mechanism is for the team ([#98](https://github.com/SinYang13/IS212-2026/discussions/98)). Status progression alone, with no confirming act, does not satisfy this.
- **A second, separate gate sits in front of confirmation.** After the venue and technical arrangements are confirmed (through Step 8), a Safety Officer must approve the event before it can "proceed to preparation" (Step 9). Preparation precedes confirmation (Step 10), so an event without safety approval cannot be confirmed either ([[week-7-customer-changes|Week 7 C6]]). SPM-50 "Confirm an event" must therefore refuse when the latest [[safety-check]] is not `Approved`; the rule is the customer's, the mechanism the team's ([#98](https://github.com/SinYang13/IS212-2026/discussions/98)). It is independent of the essential-arrangement gate and applies to every event — see [[operational-safety-check]] and [[event-readiness]].
- "Venue and technical arrangements confirmed" means venue bookings `Confirmed` and equipment reserved, **not** the event's own `Confirmed` status — the event is confirmed only after the check (🔶 reading, [[week-7-customer-changes|Week 7 C6]]).
- Losing confirmation is itself an event the Organiser must hear about: if a confirmed event changes such that it is no longer considered confirmed, the Event Organiser is informed, by the system or another appropriate channel ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)).

## Completion

- After the event has taken place, the Event Coordinator can mark it as completed.
- If it remains open, the system **may automatically mark it completed 7 days after the event end date** (Source: is212-discussions-2026-08-31, [#71](https://github.com/SinYang13/IS212-2026/discussions/71)).
- At completion, attendance information, operational notes, or issues may be recorded and the event closed (Source: customer-brief-no-1, step 14).
- Completed and cancelled events remain available as historical records; there is no retention policy and archival is out of scope (Source: is212-discussions-2026-08-31, [#20](https://github.com/SinYang13/IS212-2026/discussions/20)). See [[non-functional-requirements]].

## Cancellation

| Question | Answer |
|---|---|
| Who may cancel? | **Only the Event Coordinator in charge of that event** |
| At what stage? | **Any stage** |

(Source: is212-discussions-2026-08-31, [#68](https://github.com/SinYang13/IS212-2026/discussions/68))

This is narrower than the brief, which says "an authorised user can cancel an event" (Source: customer-brief-no-1). The Q&A answer scopes that to the assigned coordinator alone — Event Organisers cannot cancel directly and must route a cancellation through their coordinator. Related venue and equipment reservations must no longer remain committed to a cancelled event (Source: customer-brief-no-1); see [[venue-booking]] and [[equipment-management]]. The Safety Officer is not a canceller: a safety **reject** returns the event to planning and cannot cancel it, since only the assigned coordinator may ([[week-7-customer-changes|Week 7 C6]], [#68](https://github.com/SinYang13/IS212-2026/discussions/68); what a reject does otherwise is open, Q5).

## Postponement vs Cancellation

There is no separate *postponed* status in the customer's answer. Postponement is resolved into two existing paths (Source: is212-discussions-2026-08-31, [#69](https://github.com/SinYang13/IS212-2026/discussions/69)):

| Case | Treatment | Effect on arrangements |
|---|---|---|
| Postponed **with** a new date/time | Treat as a **rescheduling request** | Review affected arrangements — see [[change-impact-management]] |
| Postponed **with no** new date known | Permitted — an event may be postponed even when the new date is not yet known ([#121](https://github.com/SinYang13/IS212-2026/discussions/121)). Release current bookings, **retain the event record** until a new date is provided | Venue and equipment released; event not cancelled |
| Cancelled | Terminal; only the assigned coordinator may set it | Reservations released, event closed |

The distinguishing property of postponement is that the event record survives — the client relationship and requirements are preserved for a later date, whereas cancellation ends the event.

> [!question]- Team decision required — status for "postponed, date unknown"
> The customer did not name a status for "postponed, date unknown", but has now confirmed outright that the case **occurs** — an event may be postponed with no new date known ([#121](https://github.com/SinYang13/IS212-2026/discussions/121)), which upgrades this from a reading of [#69](https://github.com/SinYang13/IS212-2026/discussions/69) to a stated fact. The team must therefore decide whether it is a distinct status, a flag on an existing status, or a return to `Planning` (or `Blocked`) with dates cleared. The settled status set above still contains no such value, so today a confirmed customer scenario has no representation.
>
> **Team decision (2026-10-18):** postponement follows the reschedule workflow — an Organiser change request to the date/time — and gets no flow or card of its own (see SPM-54's cancellation note).

## Rejection and Resubmission

Rejected or returned requests must retain an appropriate record of the decision (Source: customer-brief-no-1, step 5).

> [!question]- Team decision required — resubmission after rejection
> Whether a rejected event can be resubmitted, or whether rejection is final, was explicitly **not specified**. The customer's full answer was "Please propose an appropriate approach if needed." (Source: is212-discussions-2026-08-31, [#67](https://github.com/SinYang13/IS212-2026/discussions/67)). Options for the team: rejection is terminal and a new request must be created; rejection returns the request to draft for amendment and resubmission; or a distinct *returned for amendment* outcome is separated from a terminal *rejected*.

## Withdrawal of a Submitted Request

A third exit from review, alongside rejection and return. The Event Organiser asks the Event Coordinator — **outside the system** — to withdraw a submitted request while it is under review; the coordinator then processes and records the withdrawal, and the withdrawn request stays distinguishable from draft, submitted, approved and rejected requests (Source: is212-discussions-2026-09-08, [#103](https://github.com/SinYang13/IS212-2026/discussions/103)).

Consequences for the lifecycle:

- Withdrawal is a **coordinator action**, not an Organiser self-service one. No Organiser-facing withdraw control is required for a request.
- It applies to the [[event-request]] only. An approved request has already become an [[event]], whose exit is cancellation and whose only permitted canceller is the assigned coordinator ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)).
- Whether a withdrawn request may later be revived or resubmitted is unspecified, exactly as with rejection ([#67](https://github.com/SinYang13/IS212-2026/discussions/67)).

## Re-entrant Transitions

The lifecycle is not strictly forward-only:

- A confirmed event may need to move back to planning when a major change occurs (Source: is212-discussions-2026-08-31, [#70](https://github.com/SinYang13/IS212-2026/discussions/70)) — see [[change-impact-management]].
- **A safety outcome can send the event back.** A `Rejected` or `Changes Requested` check returns the event to "the relevant earlier planning stage"; the affected venue or technical arrangements may need review again, and once they are confirmed again the event goes back to the Officer for a new round ([[week-7-customer-changes|Week 7 C6]]). Which stage is "relevant" — always `Planning`, or dependent on the arrangement — is open (Q6). See [[operational-safety-check]].
- **A significant change after safety approval may need a fresh check** — equipment placement, attendance and venue are all safety factors. Not stated by the customer; OPEN (Q7, [[week-7-customer-changes|Week 7 C6]], [#4](https://github.com/SinYang13/IS212-2026/discussions/4)).
- Rescheduling via the calendar must still follow the relevant event-change process and must not bypass required checks or approvals (Source: is212-discussions-2026-08-31, [#3](https://github.com/SinYang13/IS212-2026/discussions/3)).
- There is **no change cut-off period** before the event date, so backward transitions remain possible up to delivery (Source: is212-discussions-2026-08-31, [#8](https://github.com/SinYang13/IS212-2026/discussions/8)).

## Cross-Cutting Rules

- Status changes are notification triggers: submission, coordinator assignment, approval/rejection/return, confirmation, reschedule, postponement, cancellation ([[notifications|brief §6]]). Add to these the loss of confirmation — an Organiser must be told when a confirmed event is no longer confirmed (Source: is212-discussions-2026-09-08, [#105](https://github.com/SinYang13/IS212-2026/discussions/105)). A withdrawn safety approval would be one more cause of that loss (🔶 proposed, not stated by the customer; depends on Q7). See [[notifications]].
- Significant status actions are captured in activity history — what changed, who changed it, when ([[non-functional-requirements|brief §8f]]). The customer's illustrative list of materially significant actions is submission, approval/rejection, coordinator assignment/reassignment, important event information changes, venue booking decisions, equipment reservation changes, confirmation, rescheduling and cancellation — with the exact set for the team, and no complete audit-history function required in Release 1 (Source: is212-discussions-2026-09-08, [#96](https://github.com/SinYang13/IS212-2026/discussions/96)). See [[change-impact-management]].
- Visibility of a status and its underlying detail depends on role and relationship to the event. See [[access-control]] and [[user-roles]].
- The current status, confirmed arrangements, and outstanding actions must be easy for the appropriate user to understand without knowledge of ConnectSphere's internal processes ([[non-functional-requirements|brief §7]]).

## Open Items

| Item | Status | Reference |
|---|---|---|
| Exact status set | **Settled** — split one-per-record across [[event-request]] and [[event]], and the deployed [[schema]] now matches: its `event_request_status_chk` was widened on 2026-09-09 (migration D3) for the request-level `Withdrawn` that [#103](https://github.com/SinYang13/IS212-2026/discussions/103) grounds | [[event-planning-venue-booking-system\|brief §4]], [[event-request-workflow\|brief §5]], [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| Whether a withdrawn request may be revived or resubmitted | **Unspecified — team to propose** | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| Permitted transitions between those statuses | Team to propose | [#70](https://github.com/SinYang13/IS212-2026/discussions/70), brief |
| Rejected event resubmission | **Unspecified — team to propose** | [#67](https://github.com/SinYang13/IS212-2026/discussions/67) |
| Status representation for indefinite postponement | Team to propose — the case itself is now confirmed to occur, so it needs a representation | [#69](https://github.com/SinYang13/IS212-2026/discussions/69), [#121](https://github.com/SinYang13/IS212-2026/discussions/121) |
| Mechanism for performing event confirmation | Team to propose; the confirming act itself is required in Release 1 | [#98](https://github.com/SinYang13/IS212-2026/discussions/98) |
| Channel for telling the Organiser a confirmed event is no longer confirmed | Team to propose; informing them is required | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |
| Whether auto-completion at +7 days is mandatory or optional ("may") | Team to interpret | [#71](https://github.com/SinYang13/IS212-2026/discussions/71) |
| Event statuses for the safety gate: add `Safety Review` and `Preparation`, or derive the gate from [[safety-check]] rows alone | 🔶 Team decision — the gate itself is the customer's rule | [[week-7-customer-changes\|Week 7 C6]], [#98](https://github.com/SinYang13/IS212-2026/discussions/98), [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| How a safety **reject** differs from **request changes**, and whether either is terminal (Q5) | **Unspecified** | [[week-7-customer-changes\|Week 7 C6]], [#68](https://github.com/SinYang13/IS212-2026/discussions/68) |
| Which "relevant earlier planning stage" a returned event goes back to (Q6) | **Unspecified** | [[week-7-customer-changes\|Week 7 C6]] |
| Whether a significant change after safety approval requires a fresh check (Q7), and whether approval then lapses | **Unspecified**; recommended reading is yes | [[week-7-customer-changes\|Week 7 C6]], [#4](https://github.com/SinYang13/IS212-2026/discussions/4) |
| Whether the Event Organiser sees the safety status or outcome (Q8) | **Unspecified** — bounded by the legitimate-relationship rule | [[week-7-customer-changes\|Week 7 C6]], [#125](https://github.com/SinYang13/IS212-2026/discussions/125) |

See [[open-questions]] for the consolidated list.
