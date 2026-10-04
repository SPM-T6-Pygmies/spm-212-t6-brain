---
title: Access Control
type: concept
sources:
  - Customer Brief no. 1.md
  - Week 7 Customer Changes.md
  - IS212 2026 Discussions Oct 1.md
  - IS212 2026 Discussions Sept 8.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - security
  - authentication
  - authorisation
  - permissions
  - connectsphere
---

# Access Control

**Objects:** [[user-account]] · [[event]] · [[client-organisation]]

Authentication and authorisation rules for the [[event-planning-venue-booking-system]]. The governing principle from the brief: "The system should only allow authorised users to access protected information. Access should depend on the user's **role and relationship to an event**" ([[non-functional-requirements|brief §8b]]). Roles themselves are defined in [[user-roles]].

Access control is two-dimensional throughout: **role** (what kind of user) combined with **relationship** (which events this specific user is connected to). Neither alone is sufficient.

**Release 1 scope** ([[release-1-scope]]): authentication and the access rules below are 🎯 Release 1 (core feature 1 — User authorisation and authentication), and the auditability rule cited in that section is likewise 🎯 as a binding non-functional requirement. Client Organisation Visibility is `🧩 Adjacent`, pulled in because feature 1 scopes access by "relationship to an event"; Role-Based Dashboards are `📋 Backlog` — not named in the core 20. The per-event relationship-and-need rule for decision outcomes ([#125](https://github.com/SinYang13/IS212-2026/discussions/125)) applies feature 1's "relationship to an event" scoping, so it is 🎯 Release 1. The two roles Week 7 adds ([[week-7-customer-changes|Week 7 C5]], [[week-7-customer-changes|Week 7 C6]]) are 🎯 Release 1 and fall under it.

## Authentication

| Aspect | Position | Source |
|---|---|---|
| Is authentication required? | **Yes.** Feature 1 asks that users "securely access the system", with access depending on role; [#49](https://github.com/SinYang13/IS212-2026/discussions/49) restates it flatly — "Authentication is required". Neither source says every role holds a credential | [#62](https://github.com/SinYang13/IS212-2026/discussions/62), [#49](https://github.com/SinYang13/IS212-2026/discussions/49), [[event-planning-venue-booking-system\|brief §4]] |
| …must an **Attendee** authenticate? | **Still never stated.** [#49](https://github.com/SinYang13/IS212-2026/discussions/49)'s bare "Authentication is required" is not an answer to this: the same reply distinguishes account creation from event registration ("Attendee Registration refers to event registration, not account creation") and offers pre-loaded accounts to staff and clients only — consistent with the account-less attendee path, not a reversal of it. [#53](https://github.com/SinYang13/IS212-2026/discussions/53) likewise calls Attendees external users who "can register for events and view their registration status" without requiring an account. The registration path is built account-less ([[spm-24-implementation-decisions]]). How a guest reaches *view status* and *withdraw* is still open — see below and [[release-1-scope]] | [#53](https://github.com/SinYang13/IS212-2026/discussions/53), [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| Which method? | **Not specified** — email/password, SSO and MFA were all asked about and none was chosen | [#62](https://github.com/SinYang13/IS212-2026/discussions/62) |
| Account recovery / locked accounts? | **Not specified in the current requirements** | [#64](https://github.com/SinYang13/IS212-2026/discussions/64) |
| Account onboarding / self-registration? | **Not required for Release 1**, for internal staff or Event Organisers. Pre-loaded staff accounts (with roles and access controls) and pre-loaded client accounts may be used instead | [#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#53](https://github.com/SinYang13/IS212-2026/discussions/53) |

> [!question]- Team decision required — authentication method
> The authentication method. The customer's full answer to "email/password, SSO, or MFA?" was "Secure authentication is required, but the authentication method is not specified. Please propose an appropriate approach." (Source: is212-discussions-2026-08-31, [#62](https://github.com/SinYang13/IS212-2026/discussions/62)).

> [!question]- Team decision required — account recovery
> Account recovery and locked-account handling. Explicitly out of the stated requirements (Source: is212-discussions-2026-08-31, [#64](https://github.com/SinYang13/IS212-2026/discussions/64)) — but a system with authentication and no recovery path is not usable in practice, so the team should decide whether to propose one or consciously defer it.

### Account provisioning

This page previously recorded a provisioning **gap**: [#49](https://github.com/SinYang13/IS212-2026/discussions/49) ("The onboarding process feature is currently not in the list of features as yet") and [#53](https://github.com/SinYang13/IS212-2026/discussions/53) ("External user onboarding features are not listed in the brief yet"), combined with [#52](https://github.com/SinYang13/IS212-2026/discussions/52)'s absence of any user-administration role, left nothing in scope that could create a user. The 2026-09-07 follow-up to [#49](https://github.com/SinYang13/IS212-2026/discussions/49) closes that gap for internal staff and Event Organisers.

> [!success] Resolved — [#49](https://github.com/SinYang13/IS212-2026/discussions/49)
> Onboarding and self-registration are **not a Release 1 requirement**, and **pre-loaded accounts are the sanctioned substitute**. ConnectSphere employees: "No separate self-registration/onboarding feature is required for Release 1. The team may use pre-loaded staff accounts with the appropriate roles and access controls." Event Organisers: "account onboarding or self-registration is not required for Release 1. Pre-loaded client accounts may be used for testing." Attendees: "Attendee Registration is still a required Release 1 feature, but this refers to registering for an **event**, not creating a system account. Where registration is enabled, attendees should be able to view appropriate event information, register, view their registration status, and withdraw." (Source: is212-discussions-2026-09-08, [#49](https://github.com/SinYang13/IS212-2026/discussions/49)). Seeded data is therefore a compliant answer to provisioning, not a workaround — the seeded set must include the Event Operations Manager role, which [#101](https://github.com/SinYang13/IS212-2026/discussions/101) makes mandatory (see [[user-roles]]).

Week 7 adds one new role to the seeded set, the Safety Officer ([[week-7-customer-changes|Week 7 C6]]). The Event Coordinator Lead is the Event Operations Manager retitled ([[week-7-customer-changes|Week 7 C5]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128)), so it is the existing seeded role under a new title; whether the stored role name changes too is 🔶 team decision ([#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#128](https://github.com/SinYang13/IS212-2026/discussions/128)). See [[user-roles]].

One thing [#49](https://github.com/SinYang13/IS212-2026/discussions/49) does **not** close: pre-loaded accounts are offered for staff and clients only, and the answer explicitly separates event registration from account creation for Attendees.

> [!warning] Guest attendees have no credential behind their four confirmed actions
> [#49](https://github.com/SinYang13/IS212-2026/discussions/49) confirms that where registration is enabled an attendee must be able to **view appropriate event information, register, view their registration status, and withdraw**. Two of the four are served without a login in the current build: *register* creates a record identified by a name-and-email pair ([[registration]]) and *view event information* is a public list of events already open for registration ([[spm-24-implementation-decisions]]). **View status** and **withdraw** are not. A guest holds only a `registration_reference` UUID and no credential, and nothing yet delivers that reference to them. [#49](https://github.com/SinYang13/IS212-2026/discussions/49) offers pre-loaded accounts to ConnectSphere employees and Event Organisers, not to Attendees, so it does not supply the missing mechanism. A magic-link or reference-plus-email lookup would; so would seeded attendee accounts. Record the route chosen — see [[release-1-scope]] and [[registration]].

## Protected Information

Clarified on 2026-08-31 (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)). Protected information is information that should only be accessible to authorised users **based on their role and relationship to the event**. The customer's enumeration:

| Category | Examples |
|---|---|
| Internal planning information | Coordinator notes, review and clarification exchanges, outstanding actions, readiness state |
| Venue and equipment arrangements | Booking decisions, holds, reservations, availability blocks |
| Attendee information | Registration records and attendee personal data |
| Client-related information | Client organisation records and their event history |
| Other non-public event details | Anything not published to the general audience of an event |

Access follows the user's role and responsibility for the event.

> [!question]- Team decision required — field-level permission matrix
> The exact access-control design and **field-level permissions** are explicitly delegated to the solution team (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)). This is the highest-leverage open item on this page — the customer has given the categories but not the matrix.

Note that [#91](https://github.com/SinYang13/IS212-2026/discussions/91) was asked on 2026-08-28 and answered on 2026-08-31; it does not appear in the earlier snapshot (Source: is212-discussions-2026-08-26).

## Client Organisation Visibility

The brief states only that "an Event Organiser should not normally be able to view events belonging to unrelated clients" ([[non-functional-requirements|brief §8b]]). The precise rule was left open in the earlier snapshot — [#81](https://github.com/SinYang13/IS212-2026/discussions/81) is listed as **unanswered** as of 2026-08-25 (Source: is212-discussions-2026-08-26) — and answered on 2026-08-31.

**The rule** (Source: is212-discussions-2026-08-31, [#81](https://github.com/SinYang13/IS212-2026/discussions/81)):

| Relationship of the Event Organiser to the event | View | Edit / Update |
|---|---|---|
| Responsible for the event | ✅ | ✅ **while `Draft` only** — see below |
| Belongs to the **same client organisation**, not responsible for the event | ✅ | ❌ |
| Belongs to an **unrelated client organisation** | ❌ | ❌ |

Four consequences:

- Visibility inside a client organisation is **automatic and organisation-wide**. It is not granted per event by another Organiser, and it is not administered by a primary contact — no primary-contact role exists (Source: is212-discussions-2026-08-31, [#65](https://github.com/SinYang13/IS212-2026/discussions/65)). See [[user-roles]].
- Edit rights are held by **exactly one** Event Organiser per event. Delegation is not supported; transferring responsibility requires a change of Event Organiser (Source: is212-discussions-2026-08-31, [#61](https://github.com/SinYang13/IS212-2026/discussions/61), [#59](https://github.com/SinYang13/IS212-2026/discussions/59)).
- Edit rights exist **only before submission**. Once an [[event-request]] is submitted, the responsible Event Organiser loses direct write access to it entirely — including to non-significant fields — and every change is communicated to or requested through the [[user-roles|Event Coordinator]] (Source: is212-discussions-2026-09-08, [#102](https://github.com/SinYang13/IS212-2026/discussions/102)). The `✅` in the table above is therefore scoped to `Draft`. The significant/non-significant distinction survives but no longer governs *who may edit* — it governs only whether existing arrangements need revalidation. See [[change-impact-management]].
- Cross-client isolation is a hard boundary, not a default. The brief's "not normally" is tightened by [#81](https://github.com/SinYang13/IS212-2026/discussions/81) to "must not access".

This rule scopes [[reporting]] as well: Event Organisers can view or export attendee information **for events they organise**, subject to access restrictions (Source: is212-discussions-2026-08-31, [#32](https://github.com/SinYang13/IS212-2026/discussions/32)).

## Outcome Visibility — Legitimate Relationship and Need

Asked whether only the assigned Coordinator sees an event request's approve/reject outcome, or any non-Attendee role, the customer answered on 2026-09-28 (Source: is212-discussions-2026-10-01, [#125](https://github.com/SinYang13/IS212-2026/discussions/125)):

| Who may see the outcome | Position |
|---|---|
| Users with a **legitimate relationship to the event** who **need it for their role** — "such as the responsible Event Organiser and relevant authorised internal users" | ✅ Visible |
| Every non-Attendee role, **automatically, because of the role alone** | ❌ Not visible |

- This is a relationship-and-need rule, not a role list. It matches the per-event resolution this page already requires: role **and** relationship, neither alone ([[non-functional-requirements|brief §8b]]). It names no roles, so which internal roles qualify for a given event stays a team design call within the field-level matrix above.
- It applies to the decision record on [[event-request]].
- It constrains the Week 7 roles. The Event Coordinator Lead's oversight of assignments and active events ([[week-7-customer-changes|Week 7 C5]]) and the Safety Officer's view of events under check ([[week-7-customer-changes|Week 7 C6]]) must each rest on a legitimate relationship to the event and a need for the role. [#128](https://github.com/SinYang13/IS212-2026/discussions/128) says the Lead views "all the coordinator assignments and active events", so the customer has stated the Lead's oversight scope as every coordinator. Which relationship qualifies the Safety Officer to see a given event is not stated.
- Event Coordinators manage **only** events assigned to them ([[week-7-customer-changes|Week 7 C5]]).

## Attendee Restrictions

- An Attendee **must not be able to view internal planning information** ([[non-functional-requirements|brief §8b]]) — that is, the internal-planning, venue/equipment arrangement, and client categories from [#91](https://github.com/SinYang13/IS212-2026/discussions/91).
- Attendees may view events they have registered for **and** events open for registration (Source: is212-discussions-2026-08-31, [#10](https://github.com/SinYang13/IS212-2026/discussions/10)). Discoverability of open events is therefore intentional and is not a leak of protected information.
- Attendees see their own registration status; there is no stated visibility of other attendees' registrations. See [[attendee-registration]].
- Attendees "should be able to access **appropriate** event information through the system, but it is not a requirement that they MUST use the application as the only source of event information. Information and updates may also be communicated outside the system through other appropriate channels" (Source: is212-discussions-2026-09-08, [#97](https://github.com/SinYang13/IS212-2026/discussions/97)). This is a *floor* on the attendee view, not a ceiling on the channel: the system must expose the appropriate subset, but out-of-system communication is a sanctioned parallel path. It does not relax the internal-planning restriction above, and it does not licence pushing protected information down an unbounded external channel. See [[notifications]].

## Internal Staff Scope

Venue Staff and Technical Support Staff have access to the information needed for their responsibilities and are **not restricted by venue, equipment type, or location** (Source: is212-discussions-2026-08-31, [#66](https://github.com/SinYang13/IS212-2026/discussions/66)). Internal staff scope is therefore functional, not territorial. See [[user-roles]].

Two 2026-09-07 answers add internal-scope constraints:

| Constraint | Position | Source |
|---|---|---|
| Coordinator assignment authority | The Event Operations Manager is a **separate system role** and **only authorised users** may perform the assignment and reassignment functions. The detailed access-control implementation is 🔶 team decision; the visibility scope is unstated except that the role must be able to identify newly submitted events awaiting assignment. Since Week 7 the role is titled **Event Coordinator Lead** (the same role, [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) and can also reassign, view basic event information, and view the current assignments of all coordinators and active events; Event Coordinators manage only their own events — see [[user-roles]] | [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#117](https://github.com/SinYang13/IS212-2026/discussions/117), [[week-7-customer-changes\|Week 7 C5]], [#128](https://github.com/SinYang13/IS212-2026/discussions/128) |
| Operational safety review authority | The Safety Officer records the Operational Safety Check outcome. Visibility is bounded by the legitimate-relationship-and-need rule above; the detailed permission design is unstated — see [[operational-safety-check]] | [[week-7-customer-changes\|Week 7 C6]], [#125](https://github.com/SinYang13/IS212-2026/discussions/125) |
| Attendance check-in authority | Check-in "may be performed by an **authorized internal staff member involved in the event operations**"; **no new registration-only role is required**, and which existing internal role holds the capability is 🔶 team decision, "subject to the access-control model" | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |

## Role-Based Dashboards

The brief requires that "the system provides each internal or external role with an overview relevant to its responsibilities, such as events requiring attention, upcoming bookings, equipment requests, or attendee registrations" ([[event-planning-venue-booking-system|brief §4]]). Related requirements:

- The event calendar shows information that **depends on the user's role and access rights** ([[event-planning-venue-booking-system|brief §4]]).
- Information should not assume users understand ConnectSphere's internal processes; current status, confirmed arrangements, and outstanding actions must be clear to the appropriate user ([[non-functional-requirements|brief §7]]).

Indicative dashboard content per role:

| Role | Relevant overview |
|---|---|
| Event Organiser | Own events by status, own drafts, clarification requests awaiting response, confirmed arrangements, registration counts for own events |
| Event Coordinator Lead (formerly Event Operations Manager) | Unassigned queue; the current assignments of all coordinators and active events ([[week-7-customer-changes\|Week 7 C5]]; [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |
| Event Coordinator | Assigned events requiring attention, pending venue/equipment decisions, incomplete arrangements, upcoming events — see [[event-readiness]] |
| Venue Staff | Incoming venue booking requests, upcoming bookings, blocked venues — see [[venue-booking]] |
| Technical Support Staff | Equipment requests to review, upcoming reservations, on-site support assignments — see [[technical-support]] |
| Attendee | Registered events, waiting-list positions, open events, registration status |

> [!question]- Team decision required — dashboard composition per role
> Dashboard composition per role. The brief gives examples ("such as"), not a specification, and the customer has consistently returned UI/UX questions to the team ("This is a UI/UX question, please propose and do the appropriate" — Source: is212-discussions-2026-08-31, [#6](https://github.com/SinYang13/IS212-2026/discussions/6), [#27](https://github.com/SinYang13/IS212-2026/discussions/27)).

Since a person may hold multiple roles (Source: is212-discussions-2026-08-31, [#56](https://github.com/SinYang13/IS212-2026/discussions/56)), the dashboard must resolve the **union** of that user's roles, and permission checks must be evaluated per event rather than per account.

## Auditability

Access control is supported by, but distinct from, the audit requirement: important actions are recorded so ConnectSphere can determine what was changed, who changed it, and when ([[non-functional-requirements|brief §8f]]). The technical mechanism is for the team to propose (Source: is212-discussions-2026-08-31, [#14](https://github.com/SinYang13/IS212-2026/discussions/14)). Change history is itself protected — "users with **appropriate access** can review how important event information has changed over time" ([[event-planning-venue-booking-system|brief §4]]). See [[non-functional-requirements]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Authentication method | **Unspecified — team to propose** | [#62](https://github.com/SinYang13/IS212-2026/discussions/62) |
| Account recovery and locked-account handling | **Unspecified** | [#64](https://github.com/SinYang13/IS212-2026/discussions/64) |
| How accounts are created for internal staff and Event Organisers | **Closed — not required for Release 1; pre-loaded staff and client accounts may be used** | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| How an account-less Attendee is identified when viewing status or withdrawing | **Open — team to propose.** [#49](https://github.com/SinYang13/IS212-2026/discussions/49) confirms both actions are required but offers pre-loaded accounts to staff and clients only | [#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#53](https://github.com/SinYang13/IS212-2026/discussions/53) |
| Field-level permission matrix | **Team to propose** | [#91](https://github.com/SinYang13/IS212-2026/discussions/91) |
| Which relationship qualifies the Safety Officer to see an event | **Team to propose**, within [#125](https://github.com/SinYang13/IS212-2026/discussions/125)'s relationship-and-need rule | [[week-7-customer-changes\|Week 7 C6]], [#125](https://github.com/SinYang13/IS212-2026/discussions/125) |
| Access-control implementation for the Event Operations Manager (now the Event Coordinator Lead) | **Team to propose** — the role itself is settled and mandatory; its visibility now includes basic event information and all coordinators' assignments and active events ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)) | [#101](https://github.com/SinYang13/IS212-2026/discussions/101), [#117](https://github.com/SinYang13/IS212-2026/discussions/117) |
| Which existing internal role may perform attendance check-in | **Team to propose** — no new registration-only role is permitted | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |
| Whether a user-administration permission exists | **Team to propose** — narrower since [#49](https://github.com/SinYang13/IS212-2026/discussions/49): seeding covers the initial population, so only ongoing administration is at stake | [#52](https://github.com/SinYang13/IS212-2026/discussions/52) |
| Dashboard content per role | Team to propose | [[event-planning-venue-booking-system\|brief §4]] |

See [[open-questions]] for the consolidated list.
