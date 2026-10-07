---
title: Event Readiness
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Sept 8.md
  - Week 7 Customer Changes.md
tags:
  - readiness
  - confirmation-gate
  - outstanding-actions
  - dashboards
  - connectsphere
---

# Event Readiness

**Objects:** [[event]] · [[booking]] · [[equipment-reservation]] · [[support-request]] · [[registration]]

Whether an event's arrangements are sufficiently complete for the Event Coordinator to confirm it, and how the system surfaces what is still outstanding. Readiness is the gate between *planning* and *confirmed* in the [[event-lifecycle]].

**Release 1 scope:** Per [[release-1-scope]], event readiness tracking — the confirmation gate this page describes — is 🧩 Adjacent, inherited from the [#80](https://github.com/SinYang13/IS212-2026/discussions/80) blocking rule and feature 6's requirement that status changes stay "consistent with the actions performed"; only the gate itself is required, not a readiness dashboard. Outstanding action tracking, the list-and-chase surface below, is 📋 Backlog. The **safety gate** described under "Two gates" is separate and 🎯 Release 1, made mandatory by [[week-7-customer-changes|Week 7 C6]] — see [[operational-safety-check]].

## The Core Rule

Confirmation is permitted when **every arrangement identified as essential for that particular event** is complete. It is blocked otherwise (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)).

The gate is not optional for Release 1. Event Confirmation needs no standalone feature, but the Release 1 workflow **must** let an Event Coordinator confirm an event once its required/essential arrangements are complete, must take the event to a `Confirmed` status, and must reflect that confirmed state to relevant users; only the UI or mechanism is left open (Source: is212-discussions-2026-09-08, [#98](https://github.com/SinYang13/IS212-2026/discussions/98)). Status progression without a confirming act does not satisfy this.

| # | Question asked ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) | Customer answer |
|---|---|---|
| 2 | Which arrangements are essential before confirmation? | Those required for that particular event to proceed — may include venue, equipment, technical support, programme, registration setup, or other event-specific arrangements. **There is no single fixed list that applies to every event.** |
| 3 | Do required arrangements depend on the particular event? | **Yes.** Some events may not require equipment, technical support, or attendee registration at all. |
| 4 | Can an event be confirmed with non-essential actions outstanding? | **Yes**, provided all arrangements considered essential for that event are complete. Non-essential follow-up actions may remain outstanding. |
| 5 | Should the system prevent confirmation when a required arrangement is incomplete? | **Yes.** If an arrangement has been identified as required/essential for that event and is still incomplete, the event should not be confirmable. |

## Two Gates, Not One

The essential-arrangement gate above ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [#106](https://github.com/SinYang13/IS212-2026/discussions/106)) is **separate from, and in addition to**, the Operational Safety Check. After the venue and technical arrangements are confirmed, the Safety Officer must approve the event before it proceeds to preparation, and so before confirmation ([[week-7-customer-changes|Week 7 C6]]).

| | Essential-arrangement gate | Safety gate |
|---|---|---|
| Source | [#80](https://github.com/SinYang13/IS212-2026/discussions/80), [#98](https://github.com/SinYang13/IS212-2026/discussions/98), [#106](https://github.com/SinYang13/IS212-2026/discussions/106) | [[week-7-customer-changes\|Week 7 C6]] |
| Applies to | The arrangements essential to **that** event; per-event | **Every** event (🔶 team reading; the check is "due to regulation" per [#128](https://github.com/SinYang13/IS212-2026/discussions/128), which strengthens but does not state it) |
| Decided by | Event Coordinator (with Technical Support Staff for equipment) | Safety Officer |
| Can the Coordinator waive it? | Yes — by judging an arrangement non-essential | No. The check is **not an arrangement type** the Coordinator may mark non-essential (🔶, [[week-7-customer-changes\|Week 7 C6]], [#80](https://github.com/SinYang13/IS212-2026/discussions/80)) |
| Blocks | Confirmation | Preparation (Step 9), and therefore confirmation (Step 10) |

Confirmation requires both. The Coordinator must be able to see which gate is blocking: an incomplete essential arrangement, or a safety check that is `Pending`, `Rejected` or `Changes Requested` ([[non-functional-requirements|brief §7]]). The mechanism is the team's ([#98](https://github.com/SinYang13/IS212-2026/discussions/98)).

## Essential vs Non-Essential

There is no global checklist. Essentiality is **per-event and derived from the event's actual needs**.

| Arrangement | Essential when… | Not applicable when… |
|---|---|---|
| Venue | Always, in practice — an event needs a place. Where an event has **several** venue bookings ([[week-7-customer-changes\|Week 7 C3]]), venue readiness is 🔶 *derived across all live bookings*: complete only when every live booking is `Confirmed` and none is flagged for review ([[week-7-customer-changes\|Week 7 C3]], [#80](https://github.com/SinYang13/IS212-2026/discussions/80)) | — |
| Equipment | The Organiser requested equipment | No equipment requested |
| Technical support | The event requires support before or during delivery | No technical requirements |
| Programme / agenda | The event's operations depend on the schedule | Simple single-session events may not need a detailed programme |
| Registration setup | Registration is enabled for the event | No attendee registration required |
| Other event-specific arrangements | Identified during planning | — |

(Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80); customer-brief-no-1)

### Who decides essentiality

[#80](https://github.com/SinYang13/IS212-2026/discussions/80) established *that* essentiality is per-event; it did not say who judges it. For equipment the customer has now named the decider, and it is a **joint** judgement rather than the coordinator's alone (Source: is212-discussions-2026-09-08, [#106](https://github.com/SinYang13/IS212-2026/discussions/106)):

| Question | Answer | Ref |
|---|---|---|
| Who decides whether an incomplete equipment requirement is essential? | The **Event Coordinator, together with the relevant Technical Support Staff** | [#106](https://github.com/SinYang13/IS212-2026/discussions/106) |
| What follows if it is judged essential? | The event **must not be confirmed** until the requirement is resolved | [#106](https://github.com/SinYang13/IS212-2026/discussions/106) |
| Who performs the confirmation itself? | Unchanged — whichever role the system already authorises to confirm, i.e. the assigned Event Coordinator | [#106](https://github.com/SinYang13/IS212-2026/discussions/106), [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |

Two things follow. The essentiality judgement and the confirmation act are **separate decisions with different participants**: Technical Support Staff have a say in the first and none in the second. And [#90](https://github.com/SinYang13/IS212-2026/discussions/90)'s division of labour — which read as the coordinator judging a shortfall alone — is narrowed accordingly for equipment. The customer says nothing about who is consulted on venue, programme or registration essentiality, so no equivalent joint rule may be assumed there.

> [!question]- Team decision required — how essentiality is determined and recorded
> Because there is no fixed list, the team must design **how essentiality is determined and recorded**. Candidate approaches: derive required arrangements automatically from the request's declared requirements (equipment requested → equipment arrangement becomes essential); let the Event Coordinator mark each arrangement as essential or optional during planning; or a hybrid where the system proposes a derived set the coordinator can adjust. This is the mechanism the confirmation block in [#80](https://github.com/SinYang13/IS212-2026/discussions/80) answer 5 depends on, so it cannot be left implicit.

## Blocking Confirmation

The customer explicitly asked for a **hard gate**, not a warning: an event with an incomplete essential arrangement "should not be confirmable" (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)). This mirrors the treatment of double-booking, which the customer also wants blocked rather than merely flagged (Source: is212-discussions-2026-08-31, [#35](https://github.com/SinYang13/IS212-2026/discussions/35)) — see [[venue-booking]].

Design implications:

- Readiness must be **computable at confirmation time** from the state of the constituent arrangements.
- The Event Coordinator must be able to see *which* arrangement is blocking, not just that confirmation is unavailable — the brief requires outstanding actions to be easy for the appropriate user to understand ([[non-functional-requirements|brief §7]]).
- The Coordinator must also see whether the **safety check** is blocking — see Two gates, above.
- For multi-session events, readiness aggregates across sessions, since sessions may have different venues, equipment, and attendance limits (Source: is212-discussions-2026-08-31, [#74](https://github.com/SinYang13/IS212-2026/discussions/74)). See [[event-request-workflow]].

## Partial Fulfilment Feeding Readiness

Equipment does not have to be all-or-nothing before a reservation is made (Source: is212-discussions-2026-08-31, [#90](https://github.com/SinYang13/IS212-2026/discussions/90)):

> [!quote] Customer answer — [#90](https://github.com/SinYang13/IS212-2026/discussions/90)
> Equipment that is available and suitable **may be reserved even if some other requested equipment is unavailable**. Technical Support Staff should identify which requirements can be fulfilled and highlight unavailable items or suggest alternatives. **The Event Coordinator then determines whether the outstanding requirement affects overall event readiness.**

This establishes a three-part division of labour:

| Actor | Responsibility |
|---|---|
| Technical Support Staff | Reserve what is available; highlight what is not; suggest alternatives; **take part in the essentiality judgement** with the coordinator ([#106](https://github.com/SinYang13/IS212-2026/discussions/106)) |
| System | Represent partial fulfilment as a first-class state, not a binary reserved/unreserved flag |
| Event Coordinator | **Judge**, together with the relevant Technical Support Staff, whether the shortfall blocks readiness for that event; confirm the event ([#106](https://github.com/SinYang13/IS212-2026/discussions/106)) |

The readiness judgement is therefore **not fully automatable**. The system computes and presents; the coordinator decides, in consultation with Technical Support Staff where equipment is at stake. An equipment requirement partially met may be acceptable for one event and blocking for another. See [[equipment-management]] and [[technical-support]].

## Outstanding Action Tracking

The system should identify planning matters that are incomplete or require action before an event can be confirmed or delivered (Source: customer-brief-no-1). This addresses two problems named in the brief's pain-point list: "It is difficult to identify which upcoming events still have incomplete arrangements" and "Technical Support Staff may receive insufficient notice about upcoming equipment or support requirements."

Sources of outstanding actions:

- Essential arrangements not yet complete (blocking).
- Non-essential follow-ups (non-blocking, but still tracked — [#80](https://github.com/SinYang13/IS212-2026/discussions/80) answer 4).
- Unfulfilled or partially fulfilled equipment requirements ([#90](https://github.com/SinYang13/IS212-2026/discussions/90)).
- Pending venue booking decisions and pending clarifications from the Event Organiser.
- Arrangements invalidated by a change and awaiting revalidation — see [[change-impact-management]].

Outstanding arrangements are also a **reminder trigger**: the customer confirmed reminders for incomplete arrangements, delivered in batch at a configurable frequency (Source: is212-discussions-2026-08-31, [#47](https://github.com/SinYang13/IS212-2026/discussions/47)). "An upcoming event still has incomplete arrangements" is a listed notification scenario ([[notifications|brief §6]]). See [[notifications]].

## Role-Based Dashboards

Each role gets an overview relevant to its responsibilities — events requiring attention, upcoming bookings, equipment requests, attendee registrations (Source: customer-brief-no-1).

| Role | Readiness view should surface |
|---|---|
| Event Coordinator | Own events by readiness; blocking arrangements; pending clarifications; events approaching delivery with gaps |
| Venue Staff | Pending venue booking requests; upcoming setup/turnaround obligations; venues with availability changes affecting booked events |
| Technical Support Staff | Pending equipment reviews; unfulfilled requirements; upcoming support assignments; equipment awaiting post-event inspection |
| Event Organiser | Status of own events; confirmed arrangements; information requested from them |
| Attendee | Registrations and registration status only — no internal planning information |

Dashboard content is constrained by [[access-control]]: an Attendee must not see internal planning information, and an Event Organiser must not see unrelated clients' events ([[non-functional-requirements|brief §8b]]). Protected information includes internal planning information, venue and equipment arrangements, attendee information, and client-related information (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)). See [[user-roles]].

The specific dashboard layout and presentation are UI/UX decisions the customer has repeatedly delegated to the team (Source: is212-discussions-2026-08-31, [#6](https://github.com/SinYang13/IS212-2026/discussions/6), [#10](https://github.com/SinYang13/IS212-2026/discussions/10), [#27](https://github.com/SinYang13/IS212-2026/discussions/27)).

## Readiness After Confirmation

Readiness is not a one-time check. A confirmed event can lose readiness:

- A booked venue may become unavailable due to an operational issue (Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)).
- Reserved equipment may become damaged or go into maintenance (Source: customer-brief-no-1).
- A significant change may invalidate arrangements already made (Source: is212-discussions-2026-08-31, [#4](https://github.com/SinYang13/IS212-2026/discussions/4)).
- A safety approval may be withdrawn by a significant change to attendance, venue or equipment (equipment placement is a safety factor). Not stated by the customer; 🔶 proposed, depends on Q7 ([[week-7-customer-changes|Week 7 C6]], [#4](https://github.com/SinYang13/IS212-2026/discussions/4)). See [[operational-safety-check]].
- Equipment requirements may be changed after reservation, in which case the affected arrangements are reviewed and updated and Technical Support Staff re-check suitability and availability (Source: is212-discussions-2026-09-08, [#114](https://github.com/SinYang13/IS212-2026/discussions/114)).

In these cases the event may need to move back to planning — see [[change-impact-management]] and [[event-lifecycle]]. Whichever route is taken, losing confirmation is not a silent internal state change: the Event Organiser **must be informed** that the confirmed event is no longer confirmed, by the system or another appropriate channel, with the mechanism left to the team (Source: is212-discussions-2026-09-08, [#105](https://github.com/SinYang13/IS212-2026/discussions/105)). See [[notifications]].

## Open Items

| Item | Status | Ref |
|---|---|---|
| How essential vs non-essential is determined and recorded per event | **Unspecified — team to propose** (highest-value design decision on this page). The *decider* is now fixed for equipment: coordinator plus relevant Technical Support Staff | [#80](https://github.com/SinYang13/IS212-2026/discussions/80), [#106](https://github.com/SinYang13/IS212-2026/discussions/106) |
| UI or mechanism for performing event confirmation | Team to propose; the confirming act and the resulting `Confirmed` status are required in Release 1 | [#98](https://github.com/SinYang13/IS212-2026/discussions/98) |
| Channel for informing the Organiser that a confirmed event is no longer confirmed | Team to propose; informing them is required | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |
| Whether the safety gate is carried by new event statuses (`Safety Review`, `Preparation`) or derived from [[safety-check]] rows | 🔶 Team decision — the gate itself is the customer's rule | [[week-7-customer-changes\|Week 7 C6]], [#98](https://github.com/SinYang13/IS212-2026/discussions/98) |
| What "technical arrangements confirmed" means for an event with no equipment lines | 🔶 Team decision — propose, do not assume | [[week-7-customer-changes\|Week 7 C6]], [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| How partial equipment fulfilment is represented in readiness state | Team to propose. **Decided 2026-10-07:** no partial reservation within a line, so each line is either reserved in full or unfulfilled; partial fulfilment exists only across lines (SPM-274) | [#90](https://github.com/SinYang13/IS212-2026/discussions/90) |
| Reminder timing and configurability for incomplete arrangements | **Unspecified — team to propose** | [#40](https://github.com/SinYang13/IS212-2026/discussions/40), [#47](https://github.com/SinYang13/IS212-2026/discussions/47) |
| Dashboard content and layout per role | UI/UX — team to propose | [#27](https://github.com/SinYang13/IS212-2026/discussions/27) |
| Field-level access control within readiness views | Team to propose | [#91](https://github.com/SinYang13/IS212-2026/discussions/91) |

See [[open-questions]].
