---
title: User Roles
type: concept
sources:
  - Customer Brief no. 1.md
  - Week 7 Customer Changes.md
  - IS212 2026 Discussions Oct 1.md
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

**Release 1 scope** ([[release-1-scope]]): all five briefed roles are 🎯 Release 1, load-bearing for core feature 1 (User authorisation and authentication) and for every named feature that assigns them actions. The **Event Coordinator Lead** (formerly the Event Operations Manager) carries the same 🎯 marker: Coordinator Assignment is core feature 5, and [#101](https://github.com/SinYang13/IS212-2026/discussions/101) settles that its actor "must be implemented as a separate system role". [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says the role "was already included as part of the requirement", so what [[week-7-customer-changes|Week 7 C5]] adds is the title and three capabilities, all 🎯 Release 1. The **Safety Officer** ([[week-7-customer-changes|Week 7 C6]]) is a new role, also 🎯 Release 1.

## Roles

The customer brief names exactly five roles and states that these "are the roles referred to throughout this customer briefing" ([[user-roles|brief §3]]). A sixth role — the **Event Operations Manager** — is not in that table but was confirmed on 2026-09-07 as a mandatory system role in its own right (Source: is212-discussions-2026-09-08, [#101](https://github.com/SinYang13/IS212-2026/discussions/101)); it is marked † below. The Week 7 change notice retitles it the **Event Coordinator Lead** ([[week-7-customer-changes|Week 7 C5]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) and adds one new internal role, the **Safety Officer** ([[week-7-customer-changes|Week 7 C6]]), marked ‡.

**Role count: seven** — the five briefed roles, the Event Coordinator Lead (formerly the Event Operations Manager), and the Safety Officer.

| Role | Internal / External | Main Responsibility | Key system actions |
|---|---|---|---|
| Event Organiser | **External** | Client representative who has an event to organise and communicates event requirements to ConnectSphere | Create/save draft and submit event requests, supply requirements, request changes, view confirmed arrangements, view/export attendee information for their own events |
| Event Coordinator | Internal | Assigned to coordinate the overall planning of an event; the main internal point of contact | Review and approve/reject/return requests, request clarification, search venues and raise venue booking requests, record equipment requirements, track readiness, confirm the event, cancel the event, set the registration period, mark completion |
| Event Coordinator Lead† (formerly Event Operations Manager) | Internal (by implication) | Oversees incoming event requests and assigns and reassigns Event Coordinators ([#101](https://github.com/SinYang13/IS212-2026/discussions/101); [[week-7-customer-changes\|Week 7 C5]]) | Identify newly submitted events in the unassigned queue, assign an Event Coordinator, reassign an event to a different coordinator, view basic event information, view the current assignments of all coordinators and active events ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| Safety Officer‡ | Internal | Reviews the operational safety of planned events ([[week-7-customer-changes\|Week 7 C6]]) | Perform the Operational Safety Check; approve it, reject the safety arrangement, or request changes — see [[operational-safety-check]] |
| Venue Staff | Internal | Responsible for venue information, availability, booking decisions, and physical venue preparation | Maintain the venue catalogue and operating hours, approve/reject venue booking requests and suggest alternatives, block venues, record setup/teardown/turnaround, prepare the venue |
| Technical Support Staff | Internal | Responsible for equipment, technical requirements, equipment reservations, and technical support for events | Maintain the equipment catalogue and operational status, check availability, reserve equipment, record defects on return, provide on-site support |
| Attendee | **External** | Participant who registers for and attends an event | Register for confirmed events where enabled, join a waiting list, withdraw, view registration status and open events |

† Not in the brief's table; mandatory per [#101](https://github.com/SinYang13/IS212-2026/discussions/101); retitled Event Coordinator Lead by [[week-7-customer-changes|Week 7 C5]] ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). ‡ Not in the brief's table; new in the Week 7 change notice (🎯 Release 1).

Detail on each role's domain lives in [[venue-booking]], [[equipment-management]], [[technical-support]], and [[attendee-registration]].

## The Event Coordinator Lead (formerly the Event Operations Manager)

[[week-7-customer-changes|Week 7 C5]] describes a new internal role "responsible for overseeing incoming event requests and assigning them to individual Event Coordinators". 🎯 Release 1. The customer has since said it is **not a new role**: it "was already included as part of the requirement" ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). Framing: the role was already a requirement; what is new is the title and three capabilities.

> [!success] Resolved — [#128](https://github.com/SinYang13/IS212-2026/discussions/128)
> "This role was already included as part of the requirement. The impact is now there is a change of the name of the title of the role as least on the display end, now the role is able to reassign, view basic event information and also the current assignments of all the coordinator assignments and active events." The Event Coordinator Lead **is** the Event Operations Manager of [#101](https://github.com/SinYang13/IS212-2026/discussions/101), retitled. The "separate system role" of [#101](https://github.com/SinYang13/IS212-2026/discussions/101) survives under the new name, and the earlier Lead-versus-manager question is closed. [#129](https://github.com/SinYang13/IS212-2026/discussions/129) asks formally whether the two are the same role and is unanswered on the board, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) already says so. "At least on the display end" leaves open whether the *stored* role name also changes (see Open Items). The EOM name stays below as the standing record.

| Fact | Source |
|---|---|
| Newly submitted requests are **no longer assigned directly** to a coordinator. They first enter an **unassigned queue** that the Lead can view | [[week-7-customer-changes\|Week 7 C5]] |
| **New capability — reassign.** The Lead reassigns events where necessary | [[week-7-customer-changes\|Week 7 C5]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| **New capability — view basic event information** of a queued request, to choose a suitable coordinator | [[week-7-customer-changes\|Week 7 C5]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| **New capability — view the current assignments of all coordinators and active events** | [[week-7-customer-changes\|Week 7 C5]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Event Coordinators can manage **only the events assigned to them** | [[week-7-customer-changes\|Week 7 C5]] |
| Relevant users are notified when an assignment or reassignment occurs — see [[notifications]] | [[week-7-customer-changes\|Week 7 C5]] |

**Duties that carry over unchanged:** the Lead's pre-existing duties remain as recorded under the standing record below — selection by an out-of-system SOP ([#93](https://github.com/SinYang13/IS212-2026/discussions/93)), reassignment assumed pre-approved ([#94](https://github.com/SinYang13/IS212-2026/discussions/94)), no accept or decline ([#95](https://github.com/SinYang13/IS212-2026/discussions/95)), and a worklist rather than a per-submission notification ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)). C5's "relevant users are notified on assignment and reassignment" does not conflict with [#117](https://github.com/SinYang13/IS212-2026/discussions/117): it says the Lead needs no notification per new *submission*, while C5 notifies relevant users on *assignment and reassignment*. They are different events.

**Supersession.** The oversight the Lead now has — viewing all coordinator assignments and active events — supersedes the SPM-99 AC3 prohibition on it ([[week-7-change-impact-review]]).

**"Under their supervision" is every coordinator.** The notice says the Lead views assignments "under their supervision"; [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says "all the coordinator assignments and active events". No coordinator-to-Lead link is needed in the model.

What the repo already supports (proposed, not applied): the unassigned queue is an [[event-request]] with status `Submitted` and no assigned coordinator, so it needs no new data in the model. Post-approval reassignment writes the live assignment on the [[event]]. No new role row is needed — it is the existing row retitled; only whether the stored name changes is open.

| # | Question | Status | Ref |
|---|---|---|---|
| Q1 | Does the Lead replace the Event Operations Manager, or do both exist? | **Answered** — same role, retitled | [#128](https://github.com/SinYang13/IS212-2026/discussions/128); [#129](https://github.com/SinYang13/IS212-2026/discussions/129) asks formally, unanswered |
| Q2 | Is "under their supervision" every coordinator or a subset per Lead? | **Answered** — all coordinators | [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Q3 | Is the outgoing coordinator on a reassignment a "relevant user" to notify? | **Open** — carried on [[notifications]] | [[week-7-customer-changes\|Week 7 C5]] |
| Q4 | May a Lead also act as an Event Coordinator? Holding several roles is allowed in general ([#56](https://github.com/SinYang13/IS212-2026/discussions/56)), but that answer's example was Venue Staff plus Attendee | **Open** | [[week-7-customer-changes\|Week 7 C5]]; [#56](https://github.com/SinYang13/IS212-2026/discussions/56) |

The Lead's views must rest on a legitimate relationship to the event and a need for it, not on the role alone ([#125](https://github.com/SinYang13/IS212-2026/discussions/125)). See [[access-control]].

## The Safety Officer

A new internal role from [[week-7-customer-changes|Week 7 C6]], "responsible for reviewing the operational safety of planned events". 🎯 Release 1. The Officer performs the **Operational Safety Check** and may approve, reject the safety arrangement, or request changes; the customer states the Officer "will always give a response". It is a new role and requirement **due to regulation** ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). The review draws on expected attendance, venue capacity and layout, emergency access, accessibility requirements, equipment placement, crowd movement and known venue restrictions. Process detail lives in [[operational-safety-check]]; the record it writes is [[safety-check]].

- The Officer sees an event only through a legitimate relationship to it and a need for the role, not by role alone ([#125](https://github.com/SinYang13/IS212-2026/discussions/125)). See [[access-control]].
- Reject versus request-changes, and what "the relevant earlier planning stage" means, are open on [[operational-safety-check]].

## The Event Operations Manager — standing record

This is the Event Coordinator Lead under its earlier title ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)); see the section above. The facts below stay as the standing record and apply to the Lead unchanged.

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
- The role is **internal** by implication — [#101](https://github.com/SinYang13/IS212-2026/discussions/101) does not say so in terms, but its only stated duties act on ConnectSphere staff assignments. It still has no briefed responsibility statement. Its visibility scope is now partly stated: basic event information and all coordinator assignments and active events ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)), within the legitimate-relationship rule of [#125](https://github.com/SinYang13/IS212-2026/discussions/125).
- [#117](https://github.com/SinYang13/IS212-2026/discussions/117) implies at least a minimum visibility: the manager must be able to see newly submitted event requests awaiting assignment. Whether visibility extends beyond that set is unstated. See [[access-control]] and [[notifications]].
- Assignment and reassignment were the only duties attributed to this role. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) adds viewing basic event information and viewing all coordinator assignments and active events. Every other decision remains with the roles in the briefed table — notably, cancellation stays with the assigned Event Coordinator alone (Source: is212-discussions-2026-08-31, [#68](https://github.com/SinYang13/IS212-2026/discussions/68)). See [[event-lifecycle]].

> [!question]- Team decision required — Event Operations Manager access-control implementation
> The detailed access-control implementation for the Event Operations Manager, and its visibility scope. [#101](https://github.com/SinYang13/IS212-2026/discussions/101) fixes that the role exists as a separate system role and sets one constraint — only authorised users may perform the assignment functions — but leaves the permission design open, and neither the brief nor the Q&A states what events or information the role can see beyond the unassigned submissions [#117](https://github.com/SinYang13/IS212-2026/discussions/117) requires it to find. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) now adds basic event information and all coordinator assignments and active events; the permission design stays the team's.

Note that [#93](https://github.com/SinYang13/IS212-2026/discussions/93) and [#94](https://github.com/SinYang13/IS212-2026/discussions/94) were answered on 2026-08-31 and do **not** appear in the earliest snapshot (Source: is212-discussions-2026-08-26); [#73](https://github.com/SinYang13/IS212-2026/discussions/73) was already answered on 2026-08-25 and is present in all three. [#101](https://github.com/SinYang13/IS212-2026/discussions/101) and [#117](https://github.com/SinYang13/IS212-2026/discussions/117) are new in the 2026-09-08 snapshot.

## Rules Governing Role Assignment

### A person may hold multiple roles

Confirmed without qualification — a Venue Staff member may also be an Attendee at another event (Source: is212-discussions-2026-08-31, [#56](https://github.com/SinYang13/IS212-2026/discussions/56)). Whether a Lead may also act as an Event Coordinator is a separate unanswered question (Q4 above). The system must therefore model **role as a many-to-many relationship with the user**, not as a single field, and must resolve permissions per event rather than per account. See [[access-control]].

### No administrator or system-configuration role

There is no admin role above the briefed stakeholders. The customer states that no system-configuration feature is specified and that **user administration is not defined as a separate business role**; if a solution requires user-administration permissions, the team may propose an approach alongside the existing access-control requirements (Source: is212-discussions-2026-08-31, [#52](https://github.com/SinYang13/IS212-2026/discussions/52)). The Event Operations Manager is not that role — [#101](https://github.com/SinYang13/IS212-2026/discussions/101) gives it coordinator-assignment authority, not user administration. The Event Coordinator Lead (the same role under its new title, [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) is not that role either: its duties ([[week-7-customer-changes|Week 7 C5]]) are assignment and oversight.

> [!question]- Team decision required — whether to add an admin capability
> Whether to introduce an administrative capability at all. The bootstrap argument that once forced this — nobody can create accounts, so the system cannot populate itself — is weaker since [#49](https://github.com/SinYang13/IS212-2026/discussions/49) confirmed that no onboarding feature is required for Release 1 and that **pre-loaded staff and client accounts may be used**. What survives is ongoing administration beyond the seed (new joiners, role changes) and account recovery, which is still unspecified ([#64](https://github.com/SinYang13/IS212-2026/discussions/64)). See [[access-control]].

### Staff are not scoped by venue, equipment type, or location

Access depends on role and responsibilities, but Venue Staff and Technical Support Staff are explicitly **not restricted by venue, equipment type, or location** (Source: is212-discussions-2026-08-31, [#66](https://github.com/SinYang13/IS212-2026/discussions/66)). Any Venue Staff member may act on any venue; any Technical Support Staff member may act on any equipment. This removes a whole class of territory/ownership modelling from scope.

Related: no separate staff self-declaration of availability is required, and there is no fixed roster update frequency or mandated assignment mechanism for Technical Support Staff (Source: is212-discussions-2026-08-31, [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89)). See [[technical-support]].

### No registration-only or check-in role

Attendance check-in "may be performed by an **authorized internal staff member involved in the event operations**. **No new registration-only role is required.** The team may assign this responsibility to an appropriate existing internal role as part of its design, subject to the access-control model" (Source: is212-discussions-2026-09-08, [#38](https://github.com/SinYang13/IS212-2026/discussions/38) follow-up). The original answer named only "the staff" as the actor; the follow-up rules out a further, check-in-only role but leaves **which** existing internal role holds the capability as a 🔶 team decision ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)). Note the constraint is *internal* and *involved in the event operations* — it does not licence giving check-in to an Attendee or an Event Organiser. See [[attendee-registration]] and [[access-control]].

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

- Since [[week-7-customer-changes|Week 7 C5]], new requests enter an **unassigned queue** and the Event Coordinator Lead (formerly the Event Operations Manager, [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) assigns from it. Event Coordinators manage **only events assigned to them**.
- A coordinator **can be assigned to two or more events concurrently** (Source: is212-discussions-2026-08-31, [#92](https://github.com/SinYang13/IS212-2026/discussions/92)) — no workload cap is defined.
- There is **no acceptance step**. Assignment takes effect immediately and a coordinator cannot reject or decline it in the system; any issue is handled outside the system (Source: is212-discussions-2026-08-31, [#95](https://github.com/SinYang13/IS212-2026/discussions/95)).
- Assignment and reassignment are both notification triggers. See [[notifications]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Whether the Event Operations Manager (now the Event Coordinator Lead) is a system role | **Closed — yes, a separate system role, mandatory** | [#101](https://github.com/SinYang13/IS212-2026/discussions/101) |
| Whether the Event Coordinator Lead replaces or coexists with the Event Operations Manager (Q1) | **Closed — same role, retitled.** [#129](https://github.com/SinYang13/IS212-2026/discussions/129) asks formally and is unanswered on the board | [#128](https://github.com/SinYang13/IS212-2026/discussions/128), [#129](https://github.com/SinYang13/IS212-2026/discussions/129) |
| Scope of "under their supervision" (Q2) | **Closed — all coordinators** | [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Whether a Lead may also be an Event Coordinator (Q4) | **Unanswered** | [[week-7-customer-changes\|Week 7 C5]] |
| Whether the stored role name changes to "Event Coordinator Lead", or only the displayed title ("at least on the display end") | 🔶 Team decision | [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Detailed access-control implementation and visibility scope for the Event Operations Manager | **Team to propose**, bounded by "only authorised users can perform these assignment functions" | [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| Which existing internal role performs attendance check-in | **Team to propose** — no new registration-only role is permitted | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |
| Whether any user-administration capability exists, and who holds it | **Unspecified — team to propose** | [#52](https://github.com/SinYang13/IS212-2026/discussions/52) |
| Whether a primary contact per client organisation is introduced | **Unspecified — team to propose** | [#65](https://github.com/SinYang13/IS212-2026/discussions/65) |
| Workload or concurrency limits on Event Coordinator assignment | None specified | [#92](https://github.com/SinYang13/IS212-2026/discussions/92) |
| How Technical Support Staff availability is maintained for assignment | Team to propose | [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89) |

See [[open-questions]] for the consolidated list.
