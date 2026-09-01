---
title: User Account
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, user-account, roles]
---

# User Account

The record of a person who interacts with the system — ConnectSphere internal staff or an external client-side / attendee user — carrying identity, contact and role information. Every other domain object attributes ownership, assignment or authorship to a User Account. Process detail lives in [[user-roles]] and [[access-control]]; this page is the structural reference.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Name | Full name | ✅ Specified | brief §4 |
| Organisation | External users' organisation (client organisation, for Event Organisers) | ✅ Specified | brief §4 |
| Contact details | Means of reaching the user | ✅ Specified | brief §4 |
| Communication preferences | Preferred channel/format for notifications | ✅ Specified | brief §4 |
| Department / responsibilities | Internal-user-only field | ✅ Specified | brief §4 |
| Role(s) | One or more roles held by the account; a person may hold multiple roles at once | ✅ Specified | #56 |
| Availability | Factor used to determine whether a Technical Support Staff member can be allocated to support an event; no self-declaration mechanism or update frequency is specified | ⚠️ Partial | brief §4; #88, #89 |
| Credentials | Authentication credentials; method not specified | 🔶 Team decision | #62 |

The many-to-many treatment of Role(s) (one account, several roles, resolved per event rather than per account) is this wiki's structural reading of #56 and is elaborated in [[user-roles]] and [[access-control]].

## Role capability matrix

Established capabilities per role, drawn from brief §3 (main responsibility), brief §4 (feature descriptions) and the discussion answers below. This is not an exhaustive permission matrix — see [[access-control]] for the field-level question, which remains open.

| Role | Established capabilities | Ref |
|---|---|---|
| Event Organiser | Create/save draft event requests and submit them; request changes to submitted or confirmed events; view confirmed arrangements; exchange comments on own events; view/export attendee information for own events; view (but not edit) other events belonging to the same client organisation | brief §3, §4; #32, #81 |
| Event Coordinator | Review, approve, reject or return event requests; request clarification/amendment; search venues and submit venue booking requests; record equipment and technical-support requirements; set and extend the registration period; track readiness; confirm the event; cancel the event at any stage; mark completion; may be assigned to 2+ events concurrently; has no acceptance/decline step for an assignment | brief §3, §4, §5; #30, #42, #68, #71, #92, #95 |
| Event Operations Manager* | Assign an Event Coordinator to a submitted event request; select the coordinator per its own out-of-system SOP; reassign an event to a different coordinator (system assumes the reassignment is pre-approved) | #73, #93, #94 |
| Venue Staff | Maintain the venue catalogue, operating hours and supported layouts; approve/reject venue booking requests and suggest alternatives; block venues for maintenance, renovation, safety or other operational reasons; record setup/turnaround; prepare the physical venue; access is organisation-wide, not scoped to specific venues | brief §3, §4; #66 |
| Technical Support Staff | Maintain the equipment catalogue and operational status; check equipment availability; reserve equipment; provide on-site technical support; access is organisation-wide, not scoped to equipment type or location | brief §3, §4; #9, #66 |
| Attendee | Register for confirmed events where registration is enabled; join a waiting list; withdraw a registration; view own registration status and events open for registration; no limit is specified on the number of events registered for; must not view internal planning information | brief §3, §4, §8b; #10, #26, #48 |

\* Not in the brief's role table — see the gap below.

> [!warning] The Event Operations Manager is a gap, not a contradiction
> This role appears only in the discussions (#73, #93, #94, #95), assigning and reassigning Event Coordinators. It is absent from brief §3's five-role table, from brief §4's feature list, and from the brief §5 14-step process. Nothing in the brief denies the role's existence — the brief itself notes there may be special cases not yet described — so this is treated as an omission for the team to resolve, not a conflict between sources. Consistent with [[user-roles]], which analyses the gap in full.

## States

No account-level lifecycle (e.g. active / suspended / deactivated) is specified in either source. #59 asks what happens to an Event Organiser's events "if their account is deactivated" — the question presupposes deactivation as a concept, but the answer addresses only event ownership ("We can use the change of event organiser") and does not confirm that account deactivation itself is a modelled state. Whether a User Account carries a status field, and what would gate or trigger it, is left to the team (🔶 #59).

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[client-organisation]] | many : 1 (Event Organisers only) | An Event Organiser account belongs to exactly one client organisation; internal roles and Attendees carry no client-organisation link | brief §4 |
| [[event]] | many : many | An account may be the owning Event Organiser or the assigned Event Coordinator on many events; an event has exactly one of each | brief §2, §4 |
| [[event-request]] | 1 : many | An Event Organiser account authors many event requests | brief §2 |
| [[booking]] | many : many | An Event Coordinator account requests, and a Venue Staff account decides, many venue bookings | brief §4 |
| [[equipment-reservation]] | many : many | A Technical Support Staff account reviews and reserves many equipment reservations | #19 |
| [[support-request]] | many : many | A Technical Support Staff account may be assigned to many support requests | brief §4; #87 |
| [[change-request]] | 1 : many | An Event Organiser account requests many changes | brief §4 |
| [[registration]] | 1 : many | An Attendee account holds many registrations | brief §4 |
| [[audit-record]] | 1 : many | Actions performed by this account, recorded for accountability | brief §8f |

## Binding rules

- A person may hold multiple roles at once (#56); permission checks must therefore resolve per event, not per account — see [[access-control]].
- Venue Staff and Technical Support Staff access is governed by role and responsibility, and is explicitly **not** scoped by venue, equipment type, or location (#66).
- Delegation of the Event Organiser role for a specific event is **not** supported; only a change of Event Organiser is supported, both for handing off a single event and for reassigning the events of a deactivated account (#61, #59).
- An Event Organiser belonging to the same client organisation as an event's owner gets **view-only** access to that event and cannot edit or update it; an Event Organiser must not access events belonging to unrelated client organisations (#81).
- An Event Coordinator may be assigned to two or more events concurrently; no workload cap is specified (#92).
- Coordinator selection for a given event is entirely up to the Event Operations Manager, following an SOP maintained outside the system — no selection criteria need to be encoded (#93).
- Reassignment of an event to a different coordinator is performed only by the Event Operations Manager; the system may assume any reassignment entered has already been approved (#94).
- There is no acceptance step for a coordinator assignment or reassignment — it takes effect immediately and cannot be declined in the system; issues are handled outside the system (#95).

## Open modelling questions

> [!question]- Team decision required — authentication method
> Secure authentication is required, but the method (email/password, SSO, MFA) is not specified: "Secure authentication is required, but the authentication method is not specified. Please propose an appropriate approach." (#62).

> [!question]- Team decision required — account recovery and lockout
> Account recovery and locked-account handling are not specified in the current requirements (#64).

> [!question]- Team decision required — onboarding and self-registration
> How external users (Event Organisers, Attendees) obtain accounts is unresolved: "The onboarding process feature is currently not in the list of features as yet" (#49). Attendees are confirmed as system users who register and view registration status, but external-user onboarding is likewise not listed (#53).

> [!question]- Team decision required — admin role and system configuration
> Whether an administrative role above the five briefed stakeholders exists. The customer states no system-configuration feature is specified and user administration is not defined as a separate business role, inviting a proposal only if the solution requires it (#52).

> [!question]- Team decision required — field-level access-control design
> The exact access-control design and field-level permissions are explicitly delegated to the team; the customer gives only the categories of protected information (internal planning, venue/equipment arrangements, attendee information, client-related information, other non-public details), not a matrix (#91).

See [[domain-model]] for the full object catalogue.
