---
title: Notification
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, notification, reminder]
---

# Notification

A message sent to a user when something relevant to them changes: either **event-driven** (fired by one of fourteen named trigger scenarios, [[notifications|brief §6]]) or **time-driven** (a Reminder, [[event-planning-venue-booking-system|brief §4]] — see below). Delivered in-app and/or by email, with channel and delivery timing configurable per notification type. Process detail and the evolution of the channel/timing answers live in [[notifications]] (the concept-level narrative — not to be confused with this page, the structural reference for the object itself).

**Release 1 scope** ([[release-1-scope]]): the event-driven Notification object described here — trigger scenarios, channels, delivery timing — is 🎯 Release 1, core feature 20. The Reminders (time-driven) section below is `📋 Backlog` — not named in the core 20; the confirmed incomplete-arrangements reminder ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)) remains a documented requirement, not a built one, in Release 1.

## Attributes

| Field            | Meaning                                                                                                                                                     | Status      | Ref                         |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | --------------------------- |
| Trigger scenario | Which of the fourteen §6 scenarios (or which reminder condition) generated the notification                                                                 | ✅ Specified | [[notifications\|brief §6]] |
| Recipient        | The [[user-account]] the notification is addressed to                                                                                                       | ⚠️ Partial  | [[notifications\|brief §6]] |
| Channel          | In-app, email, or both                                                                                                                                      | ✅ Specified | [#37](https://github.com/SinYang13/IS212-2026/discussions/37)                         |
| Related object   | The [[event]] (or the [[event-request]], [[booking]], [[equipment-reservation]], [[registration]], [[change-request]] beneath it) the notification concerns | ✅ Specified | [[notifications\|brief §6]] |
| Message content  | Description of what changed, generated per trigger scenario                                                                                                 | ⚠️ Partial  | [[notifications\|brief §6]] |
| Delivery timing  | Real-time or batched at a configurable interval; no hard delivery-time SLA either way                                                                       | ✅ Specified | [#55](https://github.com/SinYang13/IS212-2026/discussions/55)                         |

Recipient is marked ⚠️ Partial: §6 names situations that "may generate a notification" but never names *who* receives each one. It stays ⚠️ Partial and does not become ✅ Specified now that a Recipient matrix exists below — the matrix is the team's proposal, and a proposal the customer has not confirmed is not a customer statement. Three individual rows are now customer-stated ([#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115), [#117](https://github.com/SinYang13/IS212-2026/discussions/117)), which does not make the field as a whole specified. Message content is likewise ⚠️ Partial: the brief establishes that a notification exists per scenario, not its wording or template.

## Trigger scenarios

The brief's full list of situations that "may generate a notification" ([[notifications|brief §6]]) — illustrative examples, not stated to be exhaustive. All fourteen, in source order:

| # | Trigger ([[notifications\|brief §6]]) | Likely audience | Ref |
|---|---|---|---|
| 1 | A new event request is submitted | Event Operations Manager — but **not per submission**; a worklist of unassigned submissions satisfies the requirement | [[notifications\|brief §6]]; [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| 2 | An Event Coordinator is assigned | Assigned Event Coordinator; Event Organiser | [[notifications\|brief §6]] |
| 3 | Additional information or amendment is requested | Event Organiser | [[notifications\|brief §6]] |
| 4 | An event request is approved, rejected, or returned | Event Organiser | [[notifications\|brief §6]] |
| 5 | A venue booking is requested, approved, rejected, or changed | Venue Staff; Event Coordinator | [[notifications\|brief §6]] |
| 6 | Requested equipment is confirmed or found to be unavailable | Event Coordinator | [[notifications\|brief §6]] |
| 7 | An event becomes confirmed | Event Organiser; assigned Event Coordinator | [[notifications\|brief §6]] |
| 8 | The Event Organiser requests a significant change | Event Coordinator | [[notifications\|brief §6]] |
| 9 | An event is rescheduled, postponed, or cancelled | Event Organiser; Event Coordinator; registered Attendees, where applicable. Delivery may be in-system **or** through another appropriate channel | [[notifications\|brief §6]]; [#105](https://github.com/SinYang13/IS212-2026/discussions/105), [#115](https://github.com/SinYang13/IS212-2026/discussions/115) |
| 10 | Registration opens or closes | Attendees; Event Organiser | [[notifications\|brief §6]] |
| 11 | Registration reaches capacity or a waiting-list status changes | Attendees, incl. waitlisted; Event Coordinator | [[notifications\|brief §6]] |
| 12 | A registered Attendee withdraws where that affects availability | Event Coordinator; next waitlisted Attendee | [[notifications\|brief §6]] |
| 13 | An upcoming event still has incomplete arrangements | Assigned Event Coordinator | [[notifications\|brief §6]]; [#47](https://github.com/SinYang13/IS212-2026/discussions/47) |
| 14 | A venue or equipment availability change affects an upcoming event | Assigned Event Coordinator | [[notifications\|brief §6]]; [#54](https://github.com/SinYang13/IS212-2026/discussions/54) |

> [!info] Audience column is this wiki's inference, not customer text
> Brief §6 names the trigger only, in every one of the fourteen rows. No recipient matrix is given. The "Likely audience" values above are inferred from the role responsibilities described in [[connectsphere-event-services|brief §2]], §3 and §5 (e.g. the Event Coordinator is "the main internal point of contact," Venue Staff review booking requests) — they are a structural proposal, not a specified data model. See Open modelling questions.

Rows 13 and 14 carry a second citation because they are also separately confirmed in Q&A, not only listed in §6: [#47](https://github.com/SinYang13/IS212-2026/discussions/47) confirms incomplete-arrangement reminders exist (see Reminders, below — note row 13 in §6 is the *notification* framing of the same underlying condition that [#47](https://github.com/SinYang13/IS212-2026/discussions/47)'s *reminder* answers), and [#54](https://github.com/SinYang13/IS212-2026/discussions/54) confirms that a venue availability change affecting an upcoming event "is a notification scenario," with the Event Coordinator responsible for acting on it.

## Recipient matrix

The decision §6 never made, now proposed. Each row is one backlog card: one trigger, one recipient role, one independently testable behaviour. This is the structural output of the "Likely audience" reasoning above — that column is the inference, this table is what the team committed to build from it.

| § | Trigger | Recipient | Card | Scope |
|---|---|---|---|---|
| 1 | New event request submitted — as a **worklist**, not a push per submission | Event Operations Manager | SPM-56 | 🎯 Release 1 |
| 2 | Coordinator assigned | Assigned Event Coordinator | SPM-57 | 🎯 Release 1 |
| 2 | Coordinator assigned | Event Organiser | SPM-58 | 🎯 Release 1 |
| 3 | Clarification or amendment requested | Event Organiser | SPM-59 | 🎯 Release 1 |
| 4 | Request approved, rejected, or returned | Event Organiser | SPM-60 | 🎯 Release 1 |
| 5 | Venue booking requested | Venue Staff | SPM-61 | 🎯 Release 1 |
| 5 | Venue booking approved or rejected | Requesting Event Coordinator | SPM-62 | 🎯 Release 1 |
| 5 | Confirmed booking cancelled | Venue Staff | SPM-63 | 🎯 Release 1 |
| 6 | Equipment requirement recorded or changed | Technical Support Staff | SPM-64 | 🎯 Release 1 |
| 6 | Equipment confirmed or unavailable | Assigned Event Coordinator | SPM-65 | 🎯 Release 1 |
| 7 | Event confirmed | Event Organiser | SPM-66 | 🎯 Release 1 |
| 8 | Significant change requested | Assigned Event Coordinator | SPM-67 | 🎯 Release 1 |
| 9 | Event rescheduled, postponed, or cancelled | Event Organiser | SPM-68 | 🎯 Release 1 |
| 9 | Event rescheduled, postponed, or cancelled | Registered Attendees | SPM-69 | 🎯 Release 1 |
| 10 | Registration opens or closes | Attendees | SPM-70 | 🎯 Release 1 |
| 14 | Venue or equipment availability change affects an upcoming event | Assigned Event Coordinator | SPM-71 | 🧩 Adjacent — core feature 9 |
| 11 | Registration reaches capacity | Assigned Event Coordinator | SPM-72 | 🧩 Adjacent — core feature 18 |
| 11 | Waiting-list status changes | Waitlisted Attendee | SPM-73 | 📋 Backlog |
| 12 | Registered Attendee withdraws, affecting availability | Assigned Event Coordinator | SPM-74 | 📋 Backlog |
| 13 | Upcoming event still has incomplete arrangements | Assigned Event Coordinator | SPM-75 | 📋 Backlog |

> [!info] Trigger 1 is a worklist, not a per-submission notification
> "The Event Operations Manager **does not need to be notified of every new submission**, but they **should be able to identify newly submitted events that require Event Coordinator assignment**. The team may propose an appropriate mechanism." (Source: is212-discussions-2026-09-08, [#117](https://github.com/SinYang13/IS212-2026/discussions/117)). SPM-56 is therefore satisfied by a **pull surface** — a queue of unassigned submissions on the manager's overview (see [[access-control]], Role-Based Dashboards) — rather than by one message per submitted request. The card stays 🎯 Release 1 because the *capability* is required; only the push mechanism is optional. The mechanism itself is 🔶 team decision ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)).

Four structural decisions the split forced, each a team proposal rather than a customer statement:

- **A trigger with two audiences is two cards, not one.** §6 rows 2, 5 and 9 each reach materially different recipients — row 9 reaches both the client contact and an unbounded set of external Attendees, whose notification is bounded by [[access-control]] in a way the Organiser's is not ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
- **No self-notification.** A role is not notified of an action it performed itself. This is why event confirmation (row 7) is carded for the Event Organiser only, though the "Likely audience" column infers the Coordinator too.
- **Row 12 collapses without a waiting list.** Its qualifier, "where that affects availability", presupposes promotion from a queue. With the waiting list `📋 Backlog`, a Release 1 withdrawal simply frees the place — a state change with no audience.
- **Some recipients are now customer-stated, not inferred.** Row 9's Event Organiser is fixed by [#105](https://github.com/SinYang13/IS212-2026/discussions/105) (SPM-68) and its registered Attendees by [#115](https://github.com/SinYang13/IS212-2026/discussions/115) (SPM-69). Row 1's recipient is fixed by [#117](https://github.com/SinYang13/IS212-2026/discussions/117), though as a worklist rather than a push. The Recipient attribute stays ⚠️ Partial overall — three rows of twenty are not a matrix.

## Reminders (time-driven)

Distinct from the event-driven notifications above: the brief's Reminder System feature is that the system "may remind relevant users about upcoming events, incomplete arrangements, registration deadlines, preparation activities, or other time-sensitive matters" ([[event-planning-venue-booking-system|brief §4]]).

| Reminder type | Confirmed | Frequency / lead time | Ref |
|---|---|---|---|
| Incomplete arrangements / pending approval (nags the Coordinator) | ✅ Specified — required. "Yes, reminders for incomplete arrangements." | Batched, configurable frequency | [#47](https://github.com/SinYang13/IS212-2026/discussions/47) |
| Upcoming events | ✅ Specified as a candidate reminder category | Not separately confirmed | [[event-planning-venue-booking-system\|brief §4]] |
| Registration deadlines | ✅ Specified as a candidate reminder category | Not separately confirmed | [[event-planning-venue-booking-system\|brief §4]] |
| Preparation activities | ✅ Specified as a candidate reminder category | Not separately confirmed | [[event-planning-venue-booking-system\|brief §4]] |
| Lead time (how far in advance / how often reminders fire) | 🔶 Team decision — "The timing and configurability are for you to propose." | N/A | [#40](https://github.com/SinYang13/IS212-2026/discussions/40) |

Of the four reminder categories [[event-planning-venue-booking-system|brief §4]] lists, only the incomplete-arrangements variant received a direct Q&A confirmation ([#47](https://github.com/SinYang13/IS212-2026/discussions/47), "Yes... The frequency would be by batch and should also be configurable"); the other three are stated in the brief's feature description without a corresponding Q&A answer confirming scope or behaviour.

## States

Neither source specifies a notification/reminder status field. Given delivery timing is itself either real-time or batched ([#55](https://github.com/SinYang13/IS212-2026/discussions/55)), a minimal lifecycle is structurally implied by that requirement rather than customer-specified — this diagram is this wiki's structural proposal, not a confirmed state machine:

```mermaid
stateDiagram-v2
    [*] --> Generated
    Generated --> Sent : real-time delivery
    Generated --> Queued : batched delivery configured for this type
    Queued --> Sent : batch interval elapses
    Sent --> [*]
```

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Generated | A trigger scenario (event-driven) or reminder condition (time-driven) has fired | Entry: one of the fourteen §6 scenarios, or a reminder condition above | [[notifications\|brief §6]] |
| Queued | Held pending the next configurable batch interval | Entry: batched delivery is configured for this notification type. Exit: interval elapses | [#55](https://github.com/SinYang13/IS212-2026/discussions/55) |
| Sent | Delivered to the recipient via in-app and/or email | Entry: immediate (real-time path), or batch interval elapsed | [#55](https://github.com/SinYang13/IS212-2026/discussions/55); [#37](https://github.com/SinYang13/IS212-2026/discussions/37) |

Whether a sent notification is further tracked as read/unread, acknowledged, or dismissed is not addressed by either source and is not modelled here.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[user-account]] | many : 1 | Every notification is addressed to a recipient account | [[notifications\|brief §6]] |
| [[event]] | many : 0..1 | Most triggers concern a specific event, directly or via a related record beneath it | [[notifications\|brief §6]] |
| [[event-request]] | many : 0..1 | Submission, assignment, amendment-request, and approve/reject/return triggers | [[notifications\|brief §6]] |
| [[booking]] | many : 0..1 | Venue booking requested/approved/rejected/changed trigger | [[notifications\|brief §6]] |
| [[equipment-reservation]] | many : 0..1 | Equipment confirmed/unavailable trigger | [[notifications\|brief §6]] |
| [[registration]] | many : 0..1 | Registration opens/closes, capacity/waiting-list, and withdrawal triggers | [[notifications\|brief §6]] |
| [[change-request]] | many : 0..1 | Significant-change-requested trigger | [[notifications\|brief §6]] |

## Binding rules

- Both **in-app and email** channels are supported, configurable **per notification type** — not per user and not mass-configured across all types ([#37](https://github.com/SinYang13/IS212-2026/discussions/37)).
- Delivery is **batch**: notifications need not be delivered in real time, may be grouped and sent at a configurable interval, and real-time delivery is also acceptable if technically simpler. The underlying requirement is that **there is no hard delivery-time SLA** — this is a floor requirement (either implementation satisfies it), not a mandate to build a queueing system ([#55](https://github.com/SinYang13/IS212-2026/discussions/55)).
- Reminders for incomplete arrangements and pending approvals are required, batched, and configurable in frequency ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)).
- A venue or equipment availability change affecting an upcoming event is itself a notification scenario, not merely an internal record update ([#54](https://github.com/SinYang13/IS212-2026/discussions/54); [[notifications|brief §6]]).
- **The system need not be the sole channel.** Three 2026-09-07 answers permit an obligation to inform to be discharged "through the system or another appropriate communication channel": de-confirmation of a confirmed event ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)), a reschedule reaching registered attendees ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)), and attendee event information generally ([#97](https://github.com/SinYang13/IS212-2026/discussions/97)). This bounds what the notification feature is *obliged* to deliver; it does not license omitting the in-system path where §6 lists the trigger. Notification content remains subject to [[access-control]] on whichever channel is used ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
- If a confirmed event is changed such that it is **no longer considered confirmed**, the Event Organiser **must be informed**; the exact notification mechanism is 🔶 team decision ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)). This fixes the recipient for that case rather than leaving it to the inferred audience column. See [[event-lifecycle]].
- The Event Operations Manager **does not require a notification per new submission**, but must be able to identify newly submitted events awaiting Event Coordinator assignment; the mechanism is 🔶 team decision ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)).
- Attendees affected by a reschedule should be informed of the new date/time and allowed to withdraw, by system or other channel ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)) — see [[attendee-registration]].
- Email is a **notification-channel requirement**, not a general system-integration requirement: "No integration with existing external applications or services is required for the current scope. However, the notification requirements may include sending email notifications. This should be treated as a notification-channel requirement rather than a general integration requirement." ([#79](https://github.com/SinYang13/IS212-2026/discussions/79), point 3).
- 🔶 Reminder lead time and its configurability model are left to the team to propose ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)).

## Open modelling questions

> [!question]- Team decision required — recipient matrix per trigger scenario
> Which role(s) receive each of the fourteen §6 trigger scenarios, and whether recipients are derived automatically from relationship to the event or configured separately. §6 lists triggers only; no recipient matrix is given. **A proposal now exists** — see Recipient matrix, above, and the twenty cards behind it. It remains open until the customer confirms it: the open part is no longer "what should the matrix be" but "is this matrix right", which is a shorter conversation and a specific one to have at the next Q&A.

> [!question]- Team decision required — per-user vs per-type channel preference
> [#37](https://github.com/SinYang13/IS212-2026/discussions/37) fixes channel configurability at the notification-**type** level. The brief separately lists "communication preferences" as user-profile data ([[event-planning-venue-booking-system|brief §4]]). Whether and how a per-user preference interacts with, or overrides, the per-type configuration is not reconciled by either source.

> [!question]- Team decision required — reminder lead time and configurability
> How far in advance and how often reminders fire, and the configuration surface for that timing, are left to the team: "The timing and configurability are for you to propose." ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)).

> [!question]- Team decision required — unassigned-submission worklist mechanism
> How the Event Operations Manager identifies newly submitted events requiring Event Coordinator assignment, given that a notification per submission is explicitly not required ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)). Candidates: a filtered queue on the manager's dashboard, a digest batched under [#55](https://github.com/SinYang13/IS212-2026/discussions/55)'s configurable interval, or a status filter on the event-request list. This is a **dashboard/worklist** decision as much as a notification one — see [[access-control]], Role-Based Dashboards, and [[event-request-workflow]].

> [!question]- Team decision required — availability-change automation mechanism
> The mechanism linking a venue or equipment availability change to the affected upcoming event(s) — how affected events are detected, and whether anything beyond raising the notification happens automatically — is left to the team to propose "if required" ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)).

See [[domain-model]] for the full object catalogue.
