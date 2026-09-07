---
title: Event Lifecycle
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - event-status
  - state-machine
  - business-rules
  - connectsphere
---

# Event Lifecycle

**Objects:** [[event]]

The set of statuses an event passes through in the [[event-planning-venue-booking-system]], and the rules governing transitions between them. The lifecycle is the backbone that [[event-request-workflow]], [[event-readiness]], and [[change-impact-management]] all attach to.

**Release 1 scope:** Per [[release-1-scope]], event status management is 🎯 Release 1 (core feature 6); event confirmation and cancellation management are 🧩 Adjacent — confirmation because `confirmed` is a feature 6 status and a feature 20 notification trigger, cancellation because `cancelled` is likewise a feature 6 status and feature 20 trigger.

## Statuses

The brief lists the following as an example set — "Events progress through suitable statuses so that users can understand their current stage, for example draft, submitted, under review, approved, planning, awaiting arrangements, confirmed, completed, cancelled, or rejected" (Source: customer-brief-no-1). The full status table, entry/exit rules, and state diagram are maintained on [[event]] — see States.

The team has since fixed the status set, splitting the brief's single list across the two records it spans. Each value has exactly one owner:

| Record | Statuses | Page |
|---|---|---|
| [[event-request]] | Draft, Submitted, Under Review, Approved, Rejected, Returned | [[event-request]] |
| [[event]] | Planning, Blocked, Confirmed, Completed, Cancelled | [[event]] |

`Returned` is [[event-request-workflow|brief §5]]'s return-for-amendment outcome ("An event request is approved, rejected, or returned"). `Blocked` is the team's name for the brief's *awaiting arrangements* — retained as a distinct state rather than collapsed into a readiness flag on *planning*, because [#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s confirmation gate needs somewhere to live. [[schema]] enforces both sets.

> [!question]- Team decision required — permitted transitions between the fixed statuses
> The status *set* is now settled, but the customer never fixed the state machine over it. The transitions drawn on [[event]] and [[event-request]] remain this wiki's proposal — in particular the backward `Confirmed → Planning` edge for major post-confirmation change, which the customer confirmed only as "arrangements may require... to be reviewed again" ([#70](https://github.com/SinYang13/IS212-2026/discussions/70)).

## Approved vs Confirmed

The single most load-bearing distinction in the lifecycle, clarified in full on 2026-08-31 (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)).

| Dimension | Approved | Confirmed |
|---|---|---|
| What it asserts | The request contains sufficient information for planning to proceed | The essential arrangements for that event are complete |
| Commitment | **None** to venue, equipment, technical support, registration setup, or final delivery | ConnectSphere commits to the arrangements; the Event Organiser can view them |
| Position in flow | Step 5 of the 14-step process | Step 10 of the 14-step process |
| Precondition | Coordinator judges information adequate after review and clarification | All arrangements identified as essential for that event are complete |

Key consequences of [#80](https://github.com/SinYang13/IS212-2026/discussions/80):

- Approval is an **information-sufficiency gate**, not a resource commitment. Nothing is reserved by approving.
- Confirmation is a **readiness gate**. See [[event-readiness]] for what counts as essential.
- An event **may** be confirmed while non-essential follow-up actions remain outstanding, provided every essential arrangement is complete ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), answer 4).
- The system **should block confirmation** when an arrangement identified as required for that event is still incomplete ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), answer 5).

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

This is narrower than the brief, which says "an authorised user can cancel an event" (Source: customer-brief-no-1). The Q&A answer scopes that to the assigned coordinator alone — Event Organisers cannot cancel directly and must route a cancellation through their coordinator. Related venue and equipment reservations must no longer remain committed to a cancelled event (Source: customer-brief-no-1); see [[venue-booking]] and [[equipment-management]].

## Postponement vs Cancellation

There is no separate *postponed* status in the customer's answer. Postponement is resolved into two existing paths (Source: is212-discussions-2026-08-31, [#69](https://github.com/SinYang13/IS212-2026/discussions/69)):

| Case | Treatment | Effect on arrangements |
|---|---|---|
| Postponed **with** a new date/time | Treat as a **rescheduling request** | Review affected arrangements — see [[change-impact-management]] |
| Postponed **with no** new date known | Release current bookings, **retain the event record** until a new date is provided | Venue and equipment released; event not cancelled |
| Cancelled | Terminal; only the assigned coordinator may set it | Reservations released, event closed |

The distinguishing property of postponement is that the event record survives — the client relationship and requirements are preserved for a later date, whereas cancellation ends the event.

> [!question]- Team decision required — status for "postponed, date unknown"
> The customer did not name a status for "postponed, date unknown". The team must decide whether this is a distinct status, a flag on an existing status, or a return to `Planning` (or `Blocked`) with dates cleared. Note that the settled status set above contains no such value, so today the case has no representation.

## Rejection and Resubmission

Rejected or returned requests must retain an appropriate record of the decision (Source: customer-brief-no-1, step 5).

> [!question]- Team decision required — resubmission after rejection
> Whether a rejected event can be resubmitted, or whether rejection is final, was explicitly **not specified**. The customer's full answer was "Please propose an appropriate approach if needed." (Source: is212-discussions-2026-08-31, [#67](https://github.com/SinYang13/IS212-2026/discussions/67)). Options for the team: rejection is terminal and a new request must be created; rejection returns the request to draft for amendment and resubmission; or a distinct *returned for amendment* outcome is separated from a terminal *rejected*.

## Re-entrant Transitions

The lifecycle is not strictly forward-only:

- A confirmed event may need to move back to planning when a major change occurs (Source: is212-discussions-2026-08-31, [#70](https://github.com/SinYang13/IS212-2026/discussions/70)) — see [[change-impact-management]].
- Rescheduling via the calendar must still follow the relevant event-change process and must not bypass required checks or approvals (Source: is212-discussions-2026-08-31, [#3](https://github.com/SinYang13/IS212-2026/discussions/3)).
- There is **no change cut-off period** before the event date, so backward transitions remain possible up to delivery (Source: is212-discussions-2026-08-31, [#8](https://github.com/SinYang13/IS212-2026/discussions/8)).

## Cross-Cutting Rules

- Status changes are notification triggers: submission, coordinator assignment, approval/rejection/return, confirmation, reschedule, postponement, cancellation (Source: customer-brief-no-1, s6). See [[notifications]].
- Significant status actions are captured in activity history — what changed, who changed it, when (Source: customer-brief-no-1, s8f).
- Visibility of a status and its underlying detail depends on role and relationship to the event. See [[access-control]] and [[user-roles]].
- The current status, confirmed arrangements, and outstanding actions must be easy for the appropriate user to understand without knowledge of ConnectSphere's internal processes (Source: customer-brief-no-1, s7).

## Open Items

| Item | Status | Reference |
|---|---|---|
| Exact status set | **Settled** — split one-per-record across [[event-request]] and [[event]]; enforced by [[schema]] | [[event-planning-venue-booking-system\|brief §4]], [[event-request-workflow\|brief §5]] |
| Permitted transitions between those statuses | Team to propose | [#70](https://github.com/SinYang13/IS212-2026/discussions/70), brief |
| Rejected event resubmission | **Unspecified — team to propose** | [#67](https://github.com/SinYang13/IS212-2026/discussions/67) |
| Status representation for indefinite postponement | Team to propose | [#69](https://github.com/SinYang13/IS212-2026/discussions/69) |
| Whether auto-completion at +7 days is mandatory or optional ("may") | Team to interpret | [#71](https://github.com/SinYang13/IS212-2026/discussions/71) |

See [[open-questions]] for the consolidated list.
