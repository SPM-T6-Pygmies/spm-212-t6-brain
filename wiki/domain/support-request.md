---
title: Support Request
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, support, technical-support]
---

# Support Request

The record of an event's need for technical support and its staffing: what support is required, the clarification exchange around it, and which Technical Support Staff are assigned. This object is **largely left to the team** — the sources fix a handful of binding rules (below) but do not specify a request/assignment lifecycle, an assignment mechanism, or how staff availability is represented. Treat the sections below as a floor, not evidence of a more fully-specified object. Process detail lives in [[technical-support]]; this page is the structural reference.

> [!info] This object is `📋 Backlog`
> Technical support requests and technical staff assignment/rostering are both `📋 Backlog` in [[release-1-scope]] — neither is named in the Week 4 briefing's core 20. The Technical Support Staff **role** stays in scope via authentication (feature 1) and the equipment request/availability/reservation flow (features 15–17); this object — the support-request/assignment record itself — is not required for Week 12.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Support-needed flag | Whether the event requires technical support | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Support description | Free-text description of the technical support required | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Timing | Support may be needed before or during the event | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Clarification / question log | Questions, comments, or clarification needs Technical Support Staff record against the event | ✅ Specified | [#87](https://github.com/SinYang13/IS212-2026/discussions/87) |
| Response | The clarification response, coordinated via the Event Coordinator, retained with the event record | ✅ Specified | [#87](https://github.com/SinYang13/IS212-2026/discussions/87) |
| Assigned Technical Support Staff | The [[user-account]](s) allocated to support the event, according to availability and event requirements | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Assignment method | Whether assignment is system-assigned or manual | 🔶 Team decision | [#88](https://github.com/SinYang13/IS212-2026/discussions/88) |
| Staff availability representation | How Technical Support Staff availability is tracked and maintained | 🔶 Team decision | [#89](https://github.com/SinYang13/IS212-2026/discussions/89) |
| Roster update frequency | How often the support roster is updated | 🔶 Team decision | [#88](https://github.com/SinYang13/IS212-2026/discussions/88) |

## States

No source specifies a lifecycle or a named status set for a support request/assignment record — unlike [[event]] or [[equipment-item]]'s operational status, nothing here is confirmed as a formal state machine. This page does not propose one; the attributes above (need flag, clarification log, assignment) can be tracked without an explicit state field unless the team's design requires one.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | Recorded against the event; technical support is optional — not every event requires it | [[event-planning-venue-booking-system\|brief §4]]; [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| [[user-account]] | many : many | One or more Technical Support Staff may be assigned; the Event Coordinator remains the main liaison to the Organiser | [[event-planning-venue-booking-system\|brief §4]]; [#87](https://github.com/SinYang13/IS212-2026/discussions/87) |
| [[equipment-reservation]] | 0..many : 1 (loose) | Often accompanies an event's equipment reservations, but staffing and equipment are separate concerns tracked separately | [[event-planning-venue-booking-system\|brief §4]] |

## Binding rules

- Technical Support Staff record questions, comments, and clarification needs against the event ([#87](https://github.com/SinYang13/IS212-2026/discussions/87)).
- Where clarification from the Event Organiser is required, the Event Coordinator remains the main liaison and coordinates the response ([#87](https://github.com/SinYang13/IS212-2026/discussions/87)). ⚠️ Partial: the answer confirms the Coordinator's liaison role but does not state that Technical Support Staff are barred from contacting the Organiser directly — treat that as an inference, not a confirmed rule ([#87](https://github.com/SinYang13/IS212-2026/discussions/87)).
- The clarification and its response are retained with the event record ([#87](https://github.com/SinYang13/IS212-2026/discussions/87)).
- Technical support is one of the arrangements that may or may not be essential for a given event — some events may not require it at all ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)).
- No minimum notice period is specified for technical support requests; propose an approach if the solution requires one ([#25](https://github.com/SinYang13/IS212-2026/discussions/25)).

> [!warning] No named manager role for technical staff rostering
> Coordinator assignment has a named authority — the Event Operations Manager assigns and reassigns Event Coordinators ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#94](https://github.com/SinYang13/IS212-2026/discussions/94), [#95](https://github.com/SinYang13/IS212-2026/discussions/95)). No equivalent manager role is named for rostering Technical Support Staff to events: who performs the assignment (Event Operations Manager, Event Coordinator, or a TSS lead) is not stated, and [#88](https://github.com/SinYang13/IS212-2026/discussions/88) confirms only that there is "no hard requirement" either way for system-assigned vs. manual assignment. This gap is recorded consistently in [[technical-support]].

## Open modelling questions

> [!question]- Team decision required — staff availability representation
> "No separate staff self-declaration of availability is currently required. The system only needs to maintain sufficient availability information to support staff assignment. How availability is maintained can be proposed by students" ([#89](https://github.com/SinYang13/IS212-2026/discussions/89)).

> [!question]- Team decision required — roster update frequency
> No fixed roster update frequency is required; how often the support roster is refreshed is left to the team ([#88](https://github.com/SinYang13/IS212-2026/discussions/88)).

> [!question]- Team decision required — system-assigned vs. manual staff assignment
> "There is no hard requirement of system assign or manual for assigning the support staff" ([#88](https://github.com/SinYang13/IS212-2026/discussions/88)) — whether assignment is automated, manual, or a hybrid is left to the team, and no manager role is named to own the decision (see warning above).

See [[domain-model]] for the full object catalogue.
