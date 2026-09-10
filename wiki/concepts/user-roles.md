---
title: User Roles
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Sept 8.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - roles
  - stakeholders
  - permissions
  - connectsphere
---

# User Roles

**Objects:** [[user-account]] · [[client-organisation]]

The actors recognised by the [[event-planning-venue-booking-system]]. Roles determine both responsibility in [[event-request-workflow]] and visibility under [[access-control]]. [[connectsphere-event-services]] employs approximately 500 staff across the internal roles (Source: customer-brief-no-1).

**Release 1 scope** ([[release-1-scope]]): all five briefed roles are 🎯 Release 1, load-bearing for core feature 1 (User authorisation and authentication) and for every named feature that assigns them actions. The **Event Operations Manager** carries the same 🎯 marker: Coordinator Assignment is core feature 5, and [#101](https://github.com/SinYang13/IS212-2026/discussions/101) settles that its actor "must be implemented as a separate system role".

## Roles

The customer brief names exactly five roles and states that these "are the roles referred to throughout this customer briefing" ([[user-roles|brief §3]]). A sixth role — the **Event Operations Manager** — is not in that table but was confirmed on 2026-09-07 as a mandatory system role in its own right (Source: is212-discussions-2026-09-08, [#101](https://github.com/SinYang13/IS212-2026/discussions/101)); it is marked † below.

| Role | Internal / External | Main Responsibility | Key system actions |
|---|---|---|---|
| Event Organiser | **External** | Client representative who has an event to organise and communicates event requirements to ConnectSphere | Create/save draft and submit event requests, supply requirements, request changes, view confirmed arrangements, view/export attendee information for their own events |
| Event Coordinator | Internal | Assigned to coordinate the overall planning of an event; the main internal point of contact | Review and approve/reject/return requests, request clarification, search venues and raise venue booking requests, record equipment requirements, track readiness, confirm the event, cancel the event, set the registration period, mark completion |
| Event Operations Manager† | Internal (by implication) | Responsible for assigning and reassigning Event Coordinators ([#101](https://github.com/SinYang13/IS212-2026/discussions/101)) | Assign an Event Coordinator to a submitted event request, reassign an event to a different coordinator, identify newly submitted events awaiting assignment |
| Venue Staff | Internal | Responsible for venue information, availability, booking decisions, and physical venue preparation | Maintain the venue catalogue and operating hours, approve/reject venue booking requests and suggest alternatives, block venues, record setup/teardown/turnaround, prepare the venue |
| Technical Support Staff | Internal | Responsible for equipment, technical requirements, equipment reservations, and technical support for events | Maintain the equipment catalogue and operational status, check availability, reserve equipment, record defects on return, provide on-site support |
| Attendee | **External** | Participant who registers for and attends an event | Register for confirmed events where enabled, join a waiting list, withdraw, view registration status and open events |

Detail on each role's domain lives in [[venue-booking]], [[equipment-management]], [[technical-support]], and [[attendee-registration]].

## The Event Operations Manager

> [!success] Resolved — [#101](https://github.com/SinYang13/IS212-2026/discussions/101)
> The Event Operations Manager **must be implemented as a separate system role**: "the Event Operations Manager is the role responsible for assigning and reassigning Event Coordinators. It must be implemented as a separate system role" (Source: is212-discussions-2026-09-08, [#101](https://github.com/SinYang13/IS212-2026/discussions/101)). This closes the gap this page previously recorded — the role's absence from the brief's role table is an omission, not a denial, and the three modelling options once on the table (distinct role / permission on a senior Event Coordinator / out-of-system actor) are reduced to the first. What remains open is only the **detailed access-control implementation**, which the team may propose "as long as only authorised users can perform these assignment functions".

What the discussions establish:

| Fact | Source |
|---|---|
| The Event Operations Manager **must be implemented as a separate system role**; the detailed access-control implementation is left to the team, bounded by "only authorised users can perform these assignment functions" | is212-discussions-2026-09-08, [#101](https://github.com/SinYang13/IS212-2026/discussions/101) |
| After an event request is submitted, an **Event Operations Manager assigns an Event Coordinator** to the event | is212-discussions-2026-08-31, [#73](https://github.com/SinYang13/IS212-2026/discussions/73) |
| Choice of coordinator is **entirely up to the Event Operations Manager**, who follows an SOP maintained **outside the system** — no criteria need to be encoded | is212-discussions-2026-08-31, [#93](https://github.com/SinYang13/IS212-2026/discussions/93) |
| **Reassignment** of an event to a different coordinator is also performed by the Event Operations Manager; the request and approval process happens outside the system, and the system may assume any reassignment entered by the manager has already been approved | is212-discussions-2026-08-31, [#94](https://github.com/SinYang13/IS212-2026/discussions/94) |
| The manager **does not need a notification per new submission**, but must be able to **identify newly submitted events that require Event Coordinator assignment** — a worklist, not a push. Mechanism 🔶 team decision | is212-discussions-2026-09-08, [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |

Consequences:

- The brief's *Coordinator Assignment* feature has no named actor; [#73](https://github.com/SinYang13/IS212-2026/discussions/73)/[#94](https://github.com/SinYang13/IS212-2026/discussions/94) supply one and [#101](https://github.com/SinYang13/IS212-2026/discussions/101) makes it a system role. Step 3 of the 14-step process ("An Event Coordinator is assigned") is performed by a role the brief's role table omits.
- The role is **internal** by implication — [#101](https://github.com/SinYang13/IS212-2026/discussions/101) does not say so in terms, but its only stated duties act on ConnectSphere staff assignments. It still has no briefed responsibility statement and **no stated visibility scope**.
- [#117](https://github.com/SinYang13/IS212-2026/discussions/117) implies at least a minimum visibility: the manager must be able to see newly submitted event requests awaiting assignment. Whether visibility extends beyond that set is unstated. See [[access-control]] and [[notifications]].
- Only assignment and reassignment are attributed to this role. Every other decision remains with the roles in the briefed table — notably, cancellation stays with the assigned Event Coordinator alone (Source: is212-discussions-2026-08-31, [#68](https://github.com/SinYang13/IS212-2026/discussions/68)). See [[event-lifecycle]].

> [!question]- Team decision required — Event Operations Manager access-control implementation
> The detailed access-control implementation for the Event Operations Manager, and its visibility scope. [#101](https://github.com/SinYang13/IS212-2026/discussions/101) fixes that the role exists as a separate system role and sets one constraint — only authorised users may perform the assignment functions — but leaves the permission design open, and neither the brief nor the Q&A states what events or information the role can see beyond the unassigned submissions [#117](https://github.com/SinYang13/IS212-2026/discussions/117) requires it to find.

Note that [#93](https://github.com/SinYang13/IS212-2026/discussions/93) and [#94](https://github.com/SinYang13/IS212-2026/discussions/94) were answered on 2026-08-31 and do **not** appear in the earliest snapshot (Source: is212-discussions-2026-08-26); [#73](https://github.com/SinYang13/IS212-2026/discussions/73) was already answered on 2026-08-25 and is present in all three. [#101](https://github.com/SinYang13/IS212-2026/discussions/101) and [#117](https://github.com/SinYang13/IS212-2026/discussions/117) are new in the 2026-09-08 snapshot.

## Rules Governing Role Assignment

### A person may hold multiple roles

Confirmed without qualification — a Venue Staff member may also be an Attendee at another event (Source: is212-discussions-2026-08-31, [#56](https://github.com/SinYang13/IS212-2026/discussions/56)). The system must therefore model **role as a many-to-many relationship with the user**, not as a single field, and must resolve permissions per event rather than per account. See [[access-control]].

### No administrator or system-configuration role

There is no admin role above the briefed stakeholders. The customer states that no system-configuration feature is specified and that **user administration is not defined as a separate business role**; if a solution requires user-administration permissions, the team may propose an approach alongside the existing access-control requirements (Source: is212-discussions-2026-08-31, [#52](https://github.com/SinYang13/IS212-2026/discussions/52)). The Event Operations Manager is not that role — [#101](https://github.com/SinYang13/IS212-2026/discussions/101) gives it coordinator-assignment authority, not user administration.

> [!question]- Team decision required — whether to add an admin capability
> Whether to introduce an administrative capability at all. The bootstrap argument that once forced this — nobody can create accounts, so the system cannot populate itself — is weaker since [#49](https://github.com/SinYang13/IS212-2026/discussions/49) confirmed that no onboarding feature is required for Release 1 and that **pre-loaded staff and client accounts may be used**. What survives is ongoing administration beyond the seed (new joiners, role changes) and account recovery, which is still unspecified ([#64](https://github.com/SinYang13/IS212-2026/discussions/64)). See [[access-control]].

### Staff are not scoped by venue, equipment type, or location

Access depends on role and responsibilities, but Venue Staff and Technical Support Staff are explicitly **not restricted by venue, equipment type, or location** (Source: is212-discussions-2026-08-31, [#66](https://github.com/SinYang13/IS212-2026/discussions/66)). Any Venue Staff member may act on any venue; any Technical Support Staff member may act on any equipment. This removes a whole class of territory/ownership modelling from scope.

Related: no separate staff self-declaration of availability is required, and there is no fixed roster update frequency or mandated assignment mechanism for Technical Support Staff (Source: is212-discussions-2026-08-31, [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89)). See [[technical-support]].

### No registration-only or check-in role

Attendance check-in "may be performed by an **authorized internal staff member involved in the event operations**. **No new registration-only role is required.** The team may assign this responsibility to an appropriate existing internal role as part of its design, subject to the access-control model" (Source: is212-discussions-2026-09-08, [#38](https://github.com/SinYang13/IS212-2026/discussions/38) follow-up). The original answer named only "the staff" as the actor; the follow-up rules out a seventh, check-in-only role but leaves **which** existing internal role holds the capability as a 🔶 team decision ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)). Note the constraint is *internal* and *involved in the event operations* — it does not licence giving check-in to an Attendee or an Event Organiser. See [[attendee-registration]] and [[access-control]].

### No primary contact per client organisation

Multiple Event Organisers may belong to the same client organisation, but **no primary-contact or client-administrator role has been specified**; the customer invites a proposal if one is needed (Source: is212-discussions-2026-08-31, [#65](https://github.com/SinYang13/IS212-2026/discussions/65)). Combined with [#81](https://github.com/SinYang13/IS212-2026/discussions/81), this means intra-organisation access is a **fixed system rule**, not something a client-side administrator grants. See [[access-control]].

### Delegation is not supported; change of organiser is

| Question | Answer | Source |
|---|---|---|
| Can an Event Organiser delegate their role for a specific event to a colleague? | **No.** Delegation is not supported. | [#61](https://github.com/SinYang13/IS212-2026/discussions/61) |
| What is supported instead? | **Change of Event Organiser** — the responsible organiser is replaced, not shared | [#61](https://github.com/SinYang13/IS212-2026/discussions/61) |
| What happens to events when an Event Organiser's account is deactivated? | Use the change-of-Event-Organiser mechanism; events are not orphaned | [#59](https://github.com/SinYang13/IS212-2026/discussions/59) |

(Source: is212-discussions-2026-08-31, [#61](https://github.com/SinYang13/IS212-2026/discussions/61), [#59](https://github.com/SinYang13/IS212-2026/discussions/59))

Implication: responsibility for an event is held by exactly **one** Event Organiser at a time. There is no co-organiser, no temporary stand-in, and no per-event grant of edit rights. Colleagues in the same client organisation get view-only access automatically under [#81](https://github.com/SinYang13/IS212-2026/discussions/81) — see [[access-control]].

### Event Coordinator assignment mechanics

- A coordinator **can be assigned to two or more events concurrently** (Source: is212-discussions-2026-08-31, [#92](https://github.com/SinYang13/IS212-2026/discussions/92)) — no workload cap is defined.
- There is **no acceptance step**. Assignment takes effect immediately and a coordinator cannot reject or decline it in the system; any issue is handled outside the system (Source: is212-discussions-2026-08-31, [#95](https://github.com/SinYang13/IS212-2026/discussions/95)).
- Assignment and reassignment are both notification triggers. See [[notifications]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Whether the Event Operations Manager is a system role | **Closed — yes, a separate system role, mandatory** | [#101](https://github.com/SinYang13/IS212-2026/discussions/101) |
| Detailed access-control implementation and visibility scope for the Event Operations Manager | **Team to propose**, bounded by "only authorised users can perform these assignment functions" | [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| Which existing internal role performs attendance check-in | **Team to propose** — no new registration-only role is permitted | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |
| Whether any user-administration capability exists, and who holds it | **Unspecified — team to propose** | [#52](https://github.com/SinYang13/IS212-2026/discussions/52) |
| Whether a primary contact per client organisation is introduced | **Unspecified — team to propose** | [#65](https://github.com/SinYang13/IS212-2026/discussions/65) |
| Workload or concurrency limits on Event Coordinator assignment | None specified | [#92](https://github.com/SinYang13/IS212-2026/discussions/92) |
| How Technical Support Staff availability is maintained for assignment | Team to propose | [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89) |

See [[open-questions]] for the consolidated list.
