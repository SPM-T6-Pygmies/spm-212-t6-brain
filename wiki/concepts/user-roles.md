---
title: User Roles
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - roles
  - stakeholders
  - permissions
  - connectsphere
---

# User Roles

The actors recognised by the [[event-planning-venue-booking-system]]. Roles determine both responsibility in [[event-request-workflow]] and visibility under [[access-control]]. [[connectsphere-event-services]] employs approximately 500 staff across the internal roles (Source: customer-brief-no-1).

## Briefed Roles

The customer brief names exactly five roles and states that these "are the roles referred to throughout this customer briefing" (Source: customer-brief-no-1, s3).

| Role | Internal / External | Main Responsibility | Key system actions |
|---|---|---|---|
| Event Organiser | **External** | Client representative who has an event to organise and communicates event requirements to ConnectSphere | Create/save draft and submit event requests, supply requirements, request changes, view confirmed arrangements, view/export attendee information for their own events |
| Event Coordinator | Internal | Assigned to coordinate the overall planning of an event; the main internal point of contact | Review and approve/reject/return requests, request clarification, search venues and raise venue booking requests, record equipment requirements, track readiness, confirm the event, cancel the event, set the registration period, mark completion |
| Venue Staff | Internal | Responsible for venue information, availability, booking decisions, and physical venue preparation | Maintain the venue catalogue and operating hours, approve/reject venue booking requests and suggest alternatives, block venues, record setup/teardown/turnaround, prepare the venue |
| Technical Support Staff | Internal | Responsible for equipment, technical requirements, equipment reservations, and technical support for events | Maintain the equipment catalogue and operational status, check availability, reserve equipment, record defects on return, provide on-site support |
| Attendee | **External** | Participant who registers for and attends an event | Register for confirmed events where enabled, join a waiting list, withdraw, view registration status and open events |

Detail on each role's domain lives in [[venue-booking]], [[equipment-management]], [[technical-support]], and [[attendee-registration]].

## Gap: The Event Operations Manager

> [!warning] Gap between the brief and the clarifications
> A sixth role — the **Event Operations Manager** — appears only in the Q&A discussions and is **absent from the customer brief's role table, feature list, and 14-step process**. This is not a contradiction: nothing in the brief denies the role's existence, and the brief itself says "there may be special cases and business rules that have not been described in this initial briefing" (Source: customer-brief-no-1). It is a genuine omission that the team must resolve, because a role with no entry in the briefed access-control model now holds the coordinator-assignment authority.

What the discussions establish:

| Fact | Source |
|---|---|
| After an event request is submitted, an **Event Operations Manager assigns an Event Coordinator** to the event | is212-discussions-2026-08-31, #73 |
| Choice of coordinator is **entirely up to the Event Operations Manager**, who follows an SOP maintained **outside the system** — no criteria need to be encoded | is212-discussions-2026-08-31, #93 |
| **Reassignment** of an event to a different coordinator is also performed by the Event Operations Manager; the request and approval process happens outside the system, and the system may assume any reassignment entered by the manager has already been approved | is212-discussions-2026-08-31, #94 |

Consequences:

- The brief's *Coordinator Assignment* feature has no named actor; #73/#94 supply one. Step 3 of the 14-step process ("An Event Coordinator is assigned") is therefore performed by a role not in the brief's role table.
- The Event Operations Manager is **internal** by implication but has no briefed responsibility statement, no dashboard definition, and no stated visibility scope.
- Only assignment and reassignment are attributed to this role. Every other decision remains with the roles in the briefed table — notably, cancellation stays with the assigned Event Coordinator alone (Source: is212-discussions-2026-08-31, #68). See [[event-lifecycle]].

> [!question]- Team decision required — how to model the Event Operations Manager
> Whether to model the Event Operations Manager as a distinct sixth role, as a permission held by a senior Event Coordinator, or as an out-of-system actor whose decisions are simply recorded. The team must also define its visibility scope, since neither the brief nor the Q&A specifies what events or information the role can see.

Note that #93 and #94 were answered on 2026-08-31 and do **not** appear in the earlier snapshot (Source: is212-discussions-2026-08-26); #73 was already answered on 2026-08-25 and is present in both.

## Rules Governing Role Assignment

### A person may hold multiple roles

Confirmed without qualification — a Venue Staff member may also be an Attendee at another event (Source: is212-discussions-2026-08-31, #56). The system must therefore model **role as a many-to-many relationship with the user**, not as a single field, and must resolve permissions per event rather than per account. See [[access-control]].

### No administrator or system-configuration role

There is no admin role above the five briefed stakeholders. The customer states that no system-configuration feature is specified and that **user administration is not defined as a separate business role**; if a solution requires user-administration permissions, the team may propose an approach alongside the existing access-control requirements (Source: is212-discussions-2026-08-31, #52).

> [!question]- Team decision required — whether to add an admin capability
> Whether to introduce an administrative capability at all. This interacts directly with #49 (no onboarding feature specified) and #64 (no account recovery specified) — if nobody can create or restore accounts, the system has no way to bootstrap its own users. See [[access-control]].

### Staff are not scoped by venue, equipment type, or location

Access depends on role and responsibilities, but Venue Staff and Technical Support Staff are explicitly **not restricted by venue, equipment type, or location** (Source: is212-discussions-2026-08-31, #66). Any Venue Staff member may act on any venue; any Technical Support Staff member may act on any equipment. This removes a whole class of territory/ownership modelling from scope.

Related: no separate staff self-declaration of availability is required, and there is no fixed roster update frequency or mandated assignment mechanism for Technical Support Staff (Source: is212-discussions-2026-08-31, #88, #89). See [[technical-support]].

### No primary contact per client organisation

Multiple Event Organisers may belong to the same client organisation, but **no primary-contact or client-administrator role has been specified**; the customer invites a proposal if one is needed (Source: is212-discussions-2026-08-31, #65). Combined with #81, this means intra-organisation access is a **fixed system rule**, not something a client-side administrator grants. See [[access-control]].

### Delegation is not supported; change of organiser is

| Question | Answer | Source |
|---|---|---|
| Can an Event Organiser delegate their role for a specific event to a colleague? | **No.** Delegation is not supported. | #61 |
| What is supported instead? | **Change of Event Organiser** — the responsible organiser is replaced, not shared | #61 |
| What happens to events when an Event Organiser's account is deactivated? | Use the change-of-Event-Organiser mechanism; events are not orphaned | #59 |

(Source: is212-discussions-2026-08-31, #61, #59)

Implication: responsibility for an event is held by exactly **one** Event Organiser at a time. There is no co-organiser, no temporary stand-in, and no per-event grant of edit rights. Colleagues in the same client organisation get view-only access automatically under #81 — see [[access-control]].

### Event Coordinator assignment mechanics

- A coordinator **can be assigned to two or more events concurrently** (Source: is212-discussions-2026-08-31, #92) — no workload cap is defined.
- There is **no acceptance step**. Assignment takes effect immediately and a coordinator cannot reject or decline it in the system; any issue is handled outside the system (Source: is212-discussions-2026-08-31, #95).
- Assignment and reassignment are both notification triggers. See [[notifications]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Whether the Event Operations Manager is modelled as a system role, and its visibility scope | **Gap — team to propose** | #73, #93, #94 |
| Whether any user-administration capability exists, and who holds it | **Unspecified — team to propose** | #52 |
| Whether a primary contact per client organisation is introduced | **Unspecified — team to propose** | #65 |
| Workload or concurrency limits on Event Coordinator assignment | None specified | #92 |
| How Technical Support Staff availability is maintained for assignment | Team to propose | #88, #89 |

See [[open-questions]] for the consolidated list.
