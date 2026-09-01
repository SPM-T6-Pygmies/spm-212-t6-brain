---
title: Technical Support
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
tags: [technical-support, staffing, rostering, equipment, roles, business-rules]
---

# Technical Support

The Technical Support Staff role: reviewing equipment and technical requirements, reserving resources, and being assigned to support events on-site. The equipment inventory and availability rules themselves are in [[equipment-management]].

## Responsibilities

Technical Support Staff are internal users responsible for equipment, technical requirements, equipment reservations, and technical support for events (Source: customer-brief-no-1). Concretely:

| Responsibility | Source |
|---|---|
| Maintain records of available equipment | customer-brief-no-1 |
| Maintain equipment operational status (damaged, under maintenance, unavailable) | #9, #23 |
| Determine whether sufficient suitable equipment can be reserved for a date and time | customer-brief-no-1 |
| Reserve available and suitable equipment against an event | #19, #90 |
| Record questions, comments, or clarification needs against an event | #87 |
| Log defects, damage, or missing items on equipment return | #86 |
| Be assigned to support an event on-site | customer-brief-no-1, #88, #89 |
| Update preparation information as work progresses (Step 9, Step 13) | customer-brief-no-1 |

Access is **not** scoped by venue, equipment type, or location — Technical Support Staff have access to the information needed for their responsibilities organisation-wide (Source: is212-discussions-2026-08-31, #66). See [[access-control]] and [[user-roles]].

## Equipment Requirement Review Flow

Step 8 of the planning process (Source: customer-brief-no-1). The flow is deliberately **lighter than venue booking**:

1. The **Event Coordinator records the equipment requirement** for the event, including quantities and relevant technical requirements.
2. **Technical Support Staff review** whether the requested equipment is suitable and available.
3. Available equipment **can then be reserved** for the event.

> [!info] No formal approval workflow for equipment
> There is **no formal request → approval workflow** for equipment equivalent to the venue booking approval in [[venue-booking]]. The customer answered #19 directly: the coordinator records the requirement, TSS review suitability and availability, and available equipment is reserved. (Source: is212-discussions-2026-08-31, #19)

Practical consequence: there is no "approved"/"rejected" decision object for equipment. The outcome is expressed as reservation state plus flagged unavailable items. Requested equipment being confirmed or found unavailable is still a [[notifications]] trigger (Source: customer-brief-no-1).

Reservation may be **partial** — suitable available items are reserved even while others are unavailable, with the unavailable items highlighted or alternatives suggested; the Event Coordinator then judges the impact on overall readiness (#90). See [[event-readiness]].

## Clarification of Insufficient Information

When an event's technical requirements are incomplete or unclear (the brief's example: a hybrid event requested without specifying video-conferencing needs), the flow is (Source: is212-discussions-2026-08-31, #87):

1. Technical Support Staff **record questions, comments, or clarification needs against the event**.
2. Where clarification from the **Event Organiser** is required, the **Event Coordinator remains the main liaison** and coordinates the response.
3. The clarification and its response are **retained with the event record**.

Technical Support Staff therefore do **not** contact the Event Organiser directly. The coordinator's role as sole external liaison, established in the brief and in [[event-request-workflow]], holds for technical clarifications too. This mirrors [[venue-booking]] #45, where Venue Staff suggest an alternative but the coordinator makes the change.

Comments are retained with the event record; whether they are threaded or flat is a UI/UX decision (#15).

## Technical Support Requests

- Event Coordinators can indicate when an event requires technical support **before or during** the event and describe the support required (Source: customer-brief-no-1).
- Technical support is one of the arrangements that may be **essential** for a given event — and if essential and incomplete, the event must not be confirmable (Source: is212-discussions-2026-08-31, #80). There is no fixed list of essential arrangements; it depends on the event (see [[event-readiness]]).
- Insufficient notice to Technical Support Staff about upcoming equipment or support requirements is a named pain point in the current manual process (Source: customer-brief-no-1) — yet no minimum notice period was set (see below).

## Staff Assignment and Rostering

| Question | Customer position | Source |
|---|---|---|
| Can staff self-declare availability for rostering? | **No.** No separate staff self-declaration of availability is currently required. The system only needs to maintain **sufficient availability information to support staff assignment**. | #89 |
| How is availability maintained? | **Team decision required** — "How availability is maintained can be proposed by students." | #89 |
| How often is the roster updated? | **No fixed roster update frequency is required.** | #88 |
| Who does the rostering — system-assigned or manual? | **No hard requirement** either way. | #88 |
| Minimum notice period for equipment / technical support requests | **None specified** — team to propose if the solution requires one. | #25 |

> [!question]- Team decision required — staff availability, assignment and cadence
> #88 and #89 together leave the entire staff-assignment mechanism open — availability representation, assignment method (manual vs automated), and update cadence. The only fixed constraint is that *enough* availability information must exist to make an assignment possible. A minimal design (coordinator or TSS lead picks a name, no availability calendar) satisfies the letter of both answers.

Contrast the coordinator side, where assignment **is** specified: an Event Operations Manager assigns and reassigns Event Coordinators, with no acceptance step and no in-system approval (#73, #94, #95). No equivalent manager role was named for Technical Support Staff. Whether the Event Operations Manager, the Event Coordinator, or a TSS lead performs technical staffing is **not stated** and is an [[open-questions]] item.

A person may hold multiple roles (#56), so a Technical Support Staff member can also be, for example, an Attendee at another event.

## Notice Period

- **No minimum notice period has been specified** for equipment and technical support requests (Source: is212-discussions-2026-08-31, #25).
- **Team decision required:** propose an appropriate approach if the solution requires one.
- This sits alongside two other unset time bounds: no minimum event-request lead time (#78) and no venue booking horizon (#57, see [[venue-booking]]). If the team introduces one lead-time rule, the three should be designed consistently.

## Open Points Summary

| # | Question | Customer position |
|---|---|---|
| 19 | Formal equipment approval workflow? | No — record requirement, TSS review, reserve |
| 87 | Clarifying insufficient technical info | TSS record against event; Event Coordinator liaises with organiser; retain with record |
| 88 | Roster frequency / who rosters | No fixed frequency; no hard requirement system vs manual |
| 89 | Staff self-declared availability | Not required; availability representation **team to propose** |
| 25 | Minimum notice period | None specified — **team to propose** |
| 90 | Partial fulfilment | TSS reserve what's available, flag the rest; coordinator judges readiness |
| 66 | Scope of staff access | Organisation-wide, not scoped by venue/equipment type/location |

See also [[equipment-management]], [[event-readiness]], [[user-roles]], [[event-lifecycle]].
