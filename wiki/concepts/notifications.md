---
title: Notifications
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Sept 8.md
  - IS212 2026 Discussions Sept 16.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - notifications
  - reminders
  - communication
  - connectsphere
---

# Notifications

**Objects:** [[notification]] · [[event]]

How the [[event-planning-venue-booking-system]] keeps users informed when events or arrangements relevant to them change. The brief's stated purpose: "Users should be kept informed when important events or arrangements relevant to them change" ([[notifications|brief §6]]). Notifications directly address several of the brief's named pain points — last-minute changes not reaching affected staff or attendees, Technical Support Staff receiving insufficient notice, and Attendees receiving outdated event information ([[connectsphere-event-services|brief §2]]).

**Release 1 scope** ([[release-1-scope]]): the notification system described below — scenarios, channels, delivery timing — is 🎯 Release 1, core feature 20. The Reminders section is `📋 Backlog` — not named in the core 20; feature 20 lists event-driven triggers only, and the confirmed incomplete-arrangements reminder ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)) remains a documented requirement, not a built one, in Release 1.

## Notification Scenarios

The brief lists the following as examples of situations that may generate a notification ([[notifications|brief §6]]). Grouped by concept:

| Scenario                                                           | Primary recipients                            | Related page                                |
| ------------------------------------------------------------------ | --------------------------------------------- | ------------------------------------------- |
| A new event request is submitted | Event Operations Manager — worklist, not per submission ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)) | [[event-request-workflow]] |
| An Event Coordinator is assigned                                   | Assigned coordinator, Event Organiser         | [[user-roles]]                              |
| Additional information or amendment is requested                   | Event Organiser                               | [[event-request-workflow]]                  |
| An event request is approved, rejected, or returned                | Event Organiser                               | [[event-lifecycle]]                         |
| A venue booking is requested, approved, rejected, or changed       | Venue Staff, Event Coordinator                | [[venue-booking]]                           |
| Requested equipment is confirmed or found to be unavailable        | Event Coordinator                             | [[equipment-management]]                    |
| An event becomes confirmed                                         | Event Organiser, internal staff               | [[event-lifecycle]]                         |
| The Event Organiser requests a significant change                  | Event Coordinator                             | [[change-impact-management]]                |
| An event is rescheduled, postponed, or cancelled | All affected parties incl. Attendees; Event Organiser is customer-stated for de-confirmation ([#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115)) | [[change-impact-management]] |
| Registration opens or closes                                       | Attendees                                     | [[attendee-registration]]                   |
| Registration reaches capacity or a waiting-list status changes     | Event Coordinator, Attendees                  | [[attendee-registration]]                   |
| A registered Attendee withdraws where that affects availability    | Event Coordinator, waitlisted Attendees       | [[attendee-registration]]                   |
| An upcoming event still has incomplete arrangements                | Event Coordinator                             | [[event-readiness]]                         |
| A venue or equipment availability change affects an upcoming event | Event Coordinator                             | [[venue-booking]], [[equipment-management]] |

The list is illustrative ("Examples of situations that **may** generate a notification"), not exhaustive. Recipients above are inferred from role responsibilities in [[user-roles]] — the brief does not specify a recipient matrix. See [[notification]] for the canonical trigger-scenario table with full citations.

A **proposed** recipient matrix now exists, resolving each row above to exactly one recipient role per card — see [[notification]] "Recipient matrix". Where a scenario above lists two audiences (coordinator assignment, venue booking, reschedule/cancellation), the matrix splits it into separate cards, on the reasoning that a notification to an external Attendee and one to the client contact are bounded by different access-control rules ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)) and are not one piece of work.

Three rows are no longer inference. The 2026-09-07 answers name a recipient outright for de-confirmation of a confirmed event and for a reschedule reaching attendees, and settle that the first row is a **worklist rather than a push** (Source: is212-discussions-2026-09-08, [#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115), [#117](https://github.com/SinYang13/IS212-2026/discussions/117)).

> [!question]- Team decision required — recipient matrix
> The recipient matrix — which roles receive which notification, and whether recipients are derived from relationship to the event or configured. The brief lists triggers only. A proposal is now recorded in [[notification]] "Recipient matrix"; what remains open is the customer's confirmation of it, not its absence. Rows 1, 9-Organiser and 9-Attendees are now customer-stated ([#117](https://github.com/SinYang13/IS212-2026/discussions/117), [#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115)) and can be dropped from the confirmation conversation.

## New Submissions: A Worklist, Not a Push

The Event Operations Manager "**does not need to be notified of every new submission**, but they **should be able to identify newly submitted events that require Event Coordinator assignment**. The team may propose an appropriate mechanism." (Source: is212-discussions-2026-09-08, [#117](https://github.com/SinYang13/IS212-2026/discussions/117)).

This is a real design constraint, not merely a relaxation. The obligation it creates is a **queryable state** — the set of submitted-but-unassigned event requests must be reachable by the manager on demand — which is a dashboard/worklist requirement rather than a notification one. Notifying per submission would satisfy the customer but is explicitly not required; a worklist alone satisfies the customer and a per-submission push alone does *not*, because a manager who missed a message would have no way back to the list. See [[access-control]] (Role-Based Dashboards), [[event-request-workflow]], and [[user-roles]] for the role itself, which [#101](https://github.com/SinYang13/IS212-2026/discussions/101) confirms is a mandatory separate system role.

## The System Is Not the Only Channel

Three 2026-09-07 answers all permit an obligation to inform to be discharged outside the application (Source: is212-discussions-2026-09-08):

| Obligation | Channel permitted | Source |
|---|---|---|
| A confirmed event is changed such that it is no longer considered confirmed → the Event Organiser **should be informed** | "through the system or another appropriate communication channel"; the exact mechanism may be proposed by the team | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |
| Attendees affected by a reschedule → informed of the new date/time and allowed to withdraw | "either through this system or through another appropriate communication channel" | [#115](https://github.com/SinYang13/IS212-2026/discussions/115) |
| Attendee access to event information generally | Through the system, but the application need not be "the only source"; updates "may also be communicated outside the system through other appropriate channels" | [#97](https://github.com/SinYang13/IS212-2026/discussions/97) |

Two things this does and does not mean:

- **It caps the obligation, not the design.** Where §6 names a trigger, the in-system notification is still the natural implementation; these answers stop the team owing a guaranteed in-system delivery for cases like a de-confirmed event, where an out-of-band phone call to the client contact is the customer's own working practice.
- **It does not relax [[access-control]].** Content is bounded by [#91](https://github.com/SinYang13/IS212-2026/discussions/91) on whichever channel carries it; an external channel is not a route around protected information.

[#105](https://github.com/SinYang13/IS212-2026/discussions/105) is also the first answer to name a notification recipient outright rather than leave it to inference — the Event Organiser, for the de-confirmation case. The event-state side of [#105](https://github.com/SinYang13/IS212-2026/discussions/105) is covered in [[event-lifecycle]] and [[change-impact-management]].

## Channels

**Both in-app and email**, configurable **per notification type** (Source: is212-discussions-2026-08-31, [#37](https://github.com/SinYang13/IS212-2026/discussions/37)).

The question asked whether configuration was per user or mass-configured; the answer was neither exactly — "We should allow both channels but it should also be configurable for the **type of notifications**." Configuration is therefore keyed on notification type rather than on the individual user.

| Aspect | Position |
|---|---|
| Channels supported | In-app **and** email |
| Configuration axis | Per notification **type** |
| Per-user preference | Not stated in [#37](https://github.com/SinYang13/IS212-2026/discussions/37); the brief separately lists "communication preferences" as profile data ([[event-planning-venue-booking-system\|brief §4]]) |

Email is explicitly a **notification-channel requirement, not an integration requirement**: "No integration with existing external applications or services is required for the current scope. However, the notification requirements may include sending email notifications. This should be treated as a notification-channel requirement rather than a general integration requirement." (Source: is212-discussions-2026-08-31, [#79](https://github.com/SinYang13/IS212-2026/discussions/79)).

> [!question]- Team decision required — per-user vs per-type preferences
> Whether per-user preferences (from the User Profile Management feature) combine with the per-type channel configuration, and how conflicts resolve. [#37](https://github.com/SinYang13/IS212-2026/discussions/37) defines type-level configurability; the brief's profile "communication preferences" implies a user-level axis that has not been reconciled with it.

## Delivery Timing: Batch vs Real-Time

This answer evolved across the two snapshots and is the most consequential timing decision on the page.

| Snapshot | State of [#55](https://github.com/SinYang13/IS212-2026/discussions/55) |
|---|---|
| 2026-08-25 (Source: is212-discussions-2026-08-26) | Listed as **unanswered / re-opened by a follow-up**. Previous answer: "We will put it as a batch but the frequency should be configurable". Follow-up from JameszLau asking what "batch" means was outstanding. |
| 2026-08-31 (Source: is212-discussions-2026-08-31) | Still listed as unanswered, but the customer **did respond** in a follow-up dated 2026-08-31. |

**The 2026-08-31 clarification, verbatim in substance** (Source: is212-discussions-2026-08-31, [#55](https://github.com/SinYang13/IS212-2026/discussions/55)):

- Notifications **do not need to be delivered immediately in real time**.
- They **may be grouped and sent at configurable intervals**, and the exact frequency **should be configurable**.
- **If real-time delivery is technically simpler, that is also acceptable.**
- The underlying requirement: **there is no hard requirement for notifications to be received within a specific time limit.**

This is not a contradiction of the earlier "batch" answer — it is a relaxation that explains it. The operative constraint is the absence of a delivery-time SLA, not a mandate to build a queue. A team may implement real-time delivery and still satisfy [#55](https://github.com/SinYang13/IS212-2026/discussions/55).

> [!question]- Team decision required — batched vs immediate delivery
> Whether to implement batched delivery with a configurable interval, or immediate delivery. [#55](https://github.com/SinYang13/IS212-2026/discussions/55) explicitly permits either. The only firm requirement is that **no delivery-time guarantee is owed**. If batching is chosen, the interval must be configurable.

Because there is no delivery-time requirement, notification latency should **not** be treated as a performance target. The brief's performance requirement covers "common operations such as viewing an event, searching available venues, opening a venue calendar, checking equipment availability, or submitting a registration" — notification delivery is not among them ([[non-functional-requirements|brief §8a]]). See [[non-functional-requirements]].

## Reminders

The brief's Reminder System feature: the system "may remind relevant users about upcoming events, incomplete arrangements, registration deadlines, preparation activities, or other time-sensitive matters" ([[event-planning-venue-booking-system|brief §4]]).

### Incomplete arrangements

Confirmed as required: "**Yes**, reminders for incomplete arrangements. The frequency would be **by batch** and should also be **configurable**." (Source: is212-discussions-2026-08-31, [#47](https://github.com/SinYang13/IS212-2026/discussions/47)).

This answers a direct question about whether the system should "nag" the coordinator about incomplete arrangements or pending approvals. It aligns with the brief's "An upcoming event still has incomplete arrangements" notification scenario and with the Outstanding Action Tracking feature. See [[event-readiness]].

### Reminder timing

> [!question]- Team decision required — reminder timing
> How far in advance and how often reminders fire. Asked with concrete options ("1 week before event, 1 day before?") and returned in full: "**The timing and configurability are for you to propose.**" (Source: is212-discussions-2026-08-31, [#40](https://github.com/SinYang13/IS212-2026/discussions/40)).

Note the interaction between [#40](https://github.com/SinYang13/IS212-2026/discussions/40) and [#47](https://github.com/SinYang13/IS212-2026/discussions/47)/[#55](https://github.com/SinYang13/IS212-2026/discussions/55): the customer requires that *frequency* be configurable for batched notifications and incomplete-arrangement reminders, but leaves *lead time* and the broader reminder configurability model entirely to the team. These are compatible — [#47](https://github.com/SinYang13/IS212-2026/discussions/47) fixes that reminders exist and are configurable; [#40](https://github.com/SinYang13/IS212-2026/discussions/40) leaves the parameter values and the configuration surface open.

## Availability Changes Affecting Upcoming Events

An availability change that affects an upcoming event is a notification scenario ([[notifications|brief §6]]; is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)).

The customer's answer to what happens when a confirmed venue becomes unavailable (Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)):

1. **Venue Staff are responsible for updating venue availability** when operational circumstances change.
2. An availability change affecting an upcoming event **is a notification scenario**.
3. The **Event Coordinator** then makes the necessary changes to the event.
4. "Please propose an appropriate automation mechanisms if required."

Responsibility is therefore split: Venue Staff record the fact, the system surfaces it, the Event Coordinator resolves it. The system is not stated to auto-cancel or auto-rebook. The same pattern applies to equipment becoming damaged or unavailable — Technical Support Staff maintain equipment status (Source: is212-discussions-2026-08-31, [#9](https://github.com/SinYang13/IS212-2026/discussions/9)) and the resulting impact is reviewed by the coordinator. See [[change-impact-management]], [[venue-booking]], and [[equipment-management]].

**Venue retirement is a further trigger of the same shape.** When a venue is retired, the Event Coordinators of its future bookings are notified and the bookings are identified for review; nothing is cancelled or rescheduled automatically (Source: is212-discussions-2026-09-16, [#122](https://github.com/SinYang13/IS212-2026/discussions/122)). It fits row 14 of the trigger table — a venue availability change affecting an upcoming event — rather than being a fifteenth scenario, and reuses SPM-71's recipient (the assigned Event Coordinator). What the notification carries and how the affected set is computed are the team's to propose.

> [!question]- Team decision required — availability-change automation
> The automation mechanism linking an availability change to the affected events — how affected upcoming events are detected, and whether anything beyond notification happens automatically ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)).

## Design Constraints

- The system must be desktop- and mobile-friendly; internal staff may need access while preparing venues or equipment ([[non-functional-requirements|brief §7]]). This is relevant to in-app notification design.
- Offline access is **not** a requirement (Source: is212-discussions-2026-08-31, [#31](https://github.com/SinYang13/IS212-2026/discussions/31)).
- Notification content is subject to [[access-control]] — a notification must not disclose protected information to a recipient not entitled to see it (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
- Notification volume interacts with the role-based dashboard, which surfaces the same "events requiring attention" information in-app ([[event-planning-venue-booking-system|brief §4]]).

## Open Items

| Item | Status | Reference |
|---|---|---|
| Batch interval vs real-time delivery | **Either acceptable — team to choose; no delivery-time requirement** | [#55](https://github.com/SinYang13/IS212-2026/discussions/55) |
| Reminder lead time and configurability model | **Team to propose** | [#40](https://github.com/SinYang13/IS212-2026/discussions/40) |
| Recipient matrix per notification type | **Proposed by the team** — see [[notification]] "Recipient matrix"; awaiting customer confirmation | [[notifications\|brief §6]] |
| Reconciliation of per-type channel config with per-user communication preferences | Not specified | [#37](https://github.com/SinYang13/IS212-2026/discussions/37), [[event-planning-venue-booking-system\|brief §4]] |
| Automation mechanism for availability-change impact | **Team to propose if required** | [#54](https://github.com/SinYang13/IS212-2026/discussions/54) |
| Mechanism by which the Event Operations Manager finds newly submitted events awaiting assignment | **Team to propose** — a per-submission notification is explicitly not required | [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| Notification mechanism when a confirmed event is de-confirmed | **Team to propose**; the Event Organiser must be informed, by system or other channel | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |
| Which notifications, if any, are delivered outside the system | **Team to propose** — [#97](https://github.com/SinYang13/IS212-2026/discussions/97), [#105](https://github.com/SinYang13/IS212-2026/discussions/105) and [#115](https://github.com/SinYang13/IS212-2026/discussions/115) all permit an external channel | [#97](https://github.com/SinYang13/IS212-2026/discussions/97), [#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115) |

See [[open-questions]] for the consolidated list.
