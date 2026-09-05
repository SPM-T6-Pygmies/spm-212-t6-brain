---
title: Event Readiness
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
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

**Release 1 scope:** Per [[release-1-scope]], event readiness tracking — the confirmation gate this page describes — is 🧩 Adjacent, inherited from the [#80](https://github.com/SinYang13/IS212-2026/discussions/80) blocking rule and feature 6's requirement that status changes stay "consistent with the actions performed"; only the gate itself is required, not a readiness dashboard. Outstanding action tracking, the list-and-chase surface below, is 📋 Backlog.

## The Core Rule

Confirmation is permitted when **every arrangement identified as essential for that particular event** is complete. It is blocked otherwise (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)).

| # | Question asked ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) | Customer answer |
|---|---|---|
| 2 | Which arrangements are essential before confirmation? | Those required for that particular event to proceed — may include venue, equipment, technical support, programme, registration setup, or other event-specific arrangements. **There is no single fixed list that applies to every event.** |
| 3 | Do required arrangements depend on the particular event? | **Yes.** Some events may not require equipment, technical support, or attendee registration at all. |
| 4 | Can an event be confirmed with non-essential actions outstanding? | **Yes**, provided all arrangements considered essential for that event are complete. Non-essential follow-up actions may remain outstanding. |
| 5 | Should the system prevent confirmation when a required arrangement is incomplete? | **Yes.** If an arrangement has been identified as required/essential for that event and is still incomplete, the event should not be confirmable. |

## Essential vs Non-Essential

There is no global checklist. Essentiality is **per-event and derived from the event's actual needs**.

| Arrangement | Essential when… | Not applicable when… |
|---|---|---|
| Venue | Always, in practice — an event needs a place | — |
| Equipment | The Organiser requested equipment | No equipment requested |
| Technical support | The event requires support before or during delivery | No technical requirements |
| Programme / agenda | The event's operations depend on the schedule | Simple single-session events may not need a detailed programme |
| Registration setup | Registration is enabled for the event | No attendee registration required |
| Other event-specific arrangements | Identified during planning | — |

(Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80); customer-brief-no-1)

> [!question]- Team decision required — how essentiality is determined and recorded
> Because there is no fixed list, the team must design **how essentiality is determined and recorded**. Candidate approaches: derive required arrangements automatically from the request's declared requirements (equipment requested → equipment arrangement becomes essential); let the Event Coordinator mark each arrangement as essential or optional during planning; or a hybrid where the system proposes a derived set the coordinator can adjust. This is the mechanism the confirmation block in [#80](https://github.com/SinYang13/IS212-2026/discussions/80) answer 5 depends on, so it cannot be left implicit.

## Blocking Confirmation

The customer explicitly asked for a **hard gate**, not a warning: an event with an incomplete essential arrangement "should not be confirmable" (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)). This mirrors the treatment of double-booking, which the customer also wants blocked rather than merely flagged (Source: is212-discussions-2026-08-31, [#35](https://github.com/SinYang13/IS212-2026/discussions/35)) — see [[venue-booking]].

Design implications:

- Readiness must be **computable at confirmation time** from the state of the constituent arrangements.
- The Event Coordinator must be able to see *which* arrangement is blocking, not just that confirmation is unavailable — the brief requires outstanding actions to be easy for the appropriate user to understand (Source: customer-brief-no-1, s7).
- For multi-session events, readiness aggregates across sessions, since sessions may have different venues, equipment, and attendance limits (Source: is212-discussions-2026-08-31, [#74](https://github.com/SinYang13/IS212-2026/discussions/74)). See [[event-request-workflow]].

## Partial Fulfilment Feeding Readiness

Equipment does not have to be all-or-nothing before a reservation is made (Source: is212-discussions-2026-08-31, [#90](https://github.com/SinYang13/IS212-2026/discussions/90)):

> [!quote] Customer answer — [#90](https://github.com/SinYang13/IS212-2026/discussions/90)
> Equipment that is available and suitable **may be reserved even if some other requested equipment is unavailable**. Technical Support Staff should identify which requirements can be fulfilled and highlight unavailable items or suggest alternatives. **The Event Coordinator then determines whether the outstanding requirement affects overall event readiness.**

This establishes a three-part division of labour:

| Actor | Responsibility |
|---|---|
| Technical Support Staff | Reserve what is available; highlight what is not; suggest alternatives |
| System | Represent partial fulfilment as a first-class state, not a binary reserved/unreserved flag |
| Event Coordinator | **Judge** whether the shortfall blocks readiness for that event |

The readiness judgement is therefore **not fully automatable**. The system computes and presents; the coordinator decides. An equipment requirement partially met may be acceptable for one event and blocking for another. See [[equipment-management]] and [[technical-support]].

## Outstanding Action Tracking

The system should identify planning matters that are incomplete or require action before an event can be confirmed or delivered (Source: customer-brief-no-1). This addresses two problems named in the brief's pain-point list: "It is difficult to identify which upcoming events still have incomplete arrangements" and "Technical Support Staff may receive insufficient notice about upcoming equipment or support requirements."

Sources of outstanding actions:

- Essential arrangements not yet complete (blocking).
- Non-essential follow-ups (non-blocking, but still tracked — [#80](https://github.com/SinYang13/IS212-2026/discussions/80) answer 4).
- Unfulfilled or partially fulfilled equipment requirements ([#90](https://github.com/SinYang13/IS212-2026/discussions/90)).
- Pending venue booking decisions and pending clarifications from the Event Organiser.
- Arrangements invalidated by a change and awaiting revalidation — see [[change-impact-management]].

Outstanding arrangements are also a **reminder trigger**: the customer confirmed reminders for incomplete arrangements, delivered in batch at a configurable frequency (Source: is212-discussions-2026-08-31, [#47](https://github.com/SinYang13/IS212-2026/discussions/47)). "An upcoming event still has incomplete arrangements" is a listed notification scenario (Source: customer-brief-no-1, s6). See [[notifications]].

## Role-Based Dashboards

Each role gets an overview relevant to its responsibilities — events requiring attention, upcoming bookings, equipment requests, attendee registrations (Source: customer-brief-no-1).

| Role | Readiness view should surface |
|---|---|
| Event Coordinator | Own events by readiness; blocking arrangements; pending clarifications; events approaching delivery with gaps |
| Venue Staff | Pending venue booking requests; upcoming setup/turnaround obligations; venues with availability changes affecting booked events |
| Technical Support Staff | Pending equipment reviews; unfulfilled requirements; upcoming support assignments; equipment awaiting post-event inspection |
| Event Organiser | Status of own events; confirmed arrangements; information requested from them |
| Attendee | Registrations and registration status only — no internal planning information |

Dashboard content is constrained by [[access-control]]: an Attendee must not see internal planning information, and an Event Organiser must not see unrelated clients' events (Source: customer-brief-no-1, s8b). Protected information includes internal planning information, venue and equipment arrangements, attendee information, and client-related information (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)). See [[user-roles]].

The specific dashboard layout and presentation are UI/UX decisions the customer has repeatedly delegated to the team (Source: is212-discussions-2026-08-31, [#6](https://github.com/SinYang13/IS212-2026/discussions/6), [#10](https://github.com/SinYang13/IS212-2026/discussions/10), [#27](https://github.com/SinYang13/IS212-2026/discussions/27)).

## Readiness After Confirmation

Readiness is not a one-time check. A confirmed event can lose readiness:

- A booked venue may become unavailable due to an operational issue (Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54)).
- Reserved equipment may become damaged or go into maintenance (Source: customer-brief-no-1).
- A significant change may invalidate arrangements already made (Source: is212-discussions-2026-08-31, [#4](https://github.com/SinYang13/IS212-2026/discussions/4)).

In these cases the event may need to move back to planning — see [[change-impact-management]] and [[event-lifecycle]].

## Open Items

| Item | Status | Ref |
|---|---|---|
| How essential vs non-essential is determined and recorded per event | **Unspecified — team to propose** (highest-value design decision on this page) | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| How partial equipment fulfilment is represented in readiness state | Team to propose | [#90](https://github.com/SinYang13/IS212-2026/discussions/90) |
| Reminder timing and configurability for incomplete arrangements | **Unspecified — team to propose** | [#40](https://github.com/SinYang13/IS212-2026/discussions/40), [#47](https://github.com/SinYang13/IS212-2026/discussions/47) |
| Dashboard content and layout per role | UI/UX — team to propose | [#27](https://github.com/SinYang13/IS212-2026/discussions/27) |
| Field-level access control within readiness views | Team to propose | [#91](https://github.com/SinYang13/IS212-2026/discussions/91) |

See [[open-questions]].
