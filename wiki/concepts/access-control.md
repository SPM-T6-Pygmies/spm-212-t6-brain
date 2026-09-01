---
title: Access Control
type: concept
sources:
  - Customer Brief no. 1.md
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

Authentication and authorisation rules for the [[event-planning-venue-booking-system]]. The governing principle from the brief: "The system should only allow authorised users to access protected information. Access should depend on the user's **role and relationship to an event**" (Source: customer-brief-no-1, s8b). Roles themselves are defined in [[user-roles]].

Access control is two-dimensional throughout: **role** (what kind of user) combined with **relationship** (which events this specific user is connected to). Neither alone is sufficient.

## Authentication

| Aspect | Position | Source |
|---|---|---|
| Is authentication required? | **Yes** — secure authentication is required | #62, brief s4 |
| Which method? | **Not specified** — email/password, SSO and MFA were all asked about and none was chosen | #62 |
| Account recovery / locked accounts? | **Not specified in the current requirements** | #64 |
| External user onboarding? | **Not in the feature list** | #49, #53 |

> [!question]- Team decision required — authentication method
> The authentication method. The customer's full answer to "email/password, SSO, or MFA?" was "Secure authentication is required, but the authentication method is not specified. Please propose an appropriate approach." (Source: is212-discussions-2026-08-31, #62).

> [!question]- Team decision required — account recovery
> Account recovery and locked-account handling. Explicitly out of the stated requirements (Source: is212-discussions-2026-08-31, #64) — but a system with authentication and no recovery path is not usable in practice, so the team should decide whether to propose one or consciously defer it.

### Account provisioning gap

Two answers together leave the system with no defined way to create its own users:

- "How do external users (Event Organisers, Attendees) get accounts? Self-registration? Invitation by ConnectSphere?" → "The onboarding process feature is currently not in the list of features as yet." (Source: is212-discussions-2026-08-31, #49)
- "Do attendees need an account to register attendance?" → "The system supports Attendees as external users who can register for events and view their registration status. **External user onboarding features are not listed in the brief yet**" (Source: is212-discussions-2026-08-31, #53)

Combined with #52 — no admin role and no user-administration business role defined (see [[user-roles]]) — nothing in the specified scope creates accounts for either internal or external users.

> [!question]- Team decision required — user provisioning
> How users come to exist. Options: self-registration for external users, invitation issued by an Event Coordinator, pre-seeded internal accounts, or treating provisioning as out of scope with seeded data. #53 confirms Attendees are *users* of the system (they "register for events and view their registration status"), so an Attendee identity of some kind is required even if onboarding is not built.

## Protected Information

Clarified on 2026-08-31 (Source: is212-discussions-2026-08-31, #91). Protected information is information that should only be accessible to authorised users **based on their role and relationship to the event**. The customer's enumeration:

| Category | Examples |
|---|---|
| Internal planning information | Coordinator notes, review and clarification exchanges, outstanding actions, readiness state |
| Venue and equipment arrangements | Booking decisions, holds, reservations, availability blocks |
| Attendee information | Registration records and attendee personal data |
| Client-related information | Client organisation records and their event history |
| Other non-public event details | Anything not published to the general audience of an event |

Access follows the user's role and responsibility for the event.

> [!question]- Team decision required — field-level permission matrix
> The exact access-control design and **field-level permissions** are explicitly delegated to the solution team (Source: is212-discussions-2026-08-31, #91). This is the highest-leverage open item on this page — the customer has given the categories but not the matrix.

Note that #91 was asked on 2026-08-28 and answered on 2026-08-31; it does not appear in the earlier snapshot (Source: is212-discussions-2026-08-26).

## Client Organisation Visibility

The brief states only that "an Event Organiser should not normally be able to view events belonging to unrelated clients" (Source: customer-brief-no-1, s8b). The precise rule was left open in the earlier snapshot — #81 is listed as **unanswered** as of 2026-08-25 (Source: is212-discussions-2026-08-26) — and answered on 2026-08-31.

**The rule** (Source: is212-discussions-2026-08-31, #81):

| Relationship of the Event Organiser to the event | View | Edit / Update |
|---|---|---|
| Responsible for the event | ✅ | ✅ |
| Belongs to the **same client organisation**, not responsible for the event | ✅ | ❌ |
| Belongs to an **unrelated client organisation** | ❌ | ❌ |

Three consequences:

- Visibility inside a client organisation is **automatic and organisation-wide**. It is not granted per event by another Organiser, and it is not administered by a primary contact — no primary-contact role exists (Source: is212-discussions-2026-08-31, #65). See [[user-roles]].
- Edit rights are held by **exactly one** Event Organiser per event. Delegation is not supported; transferring responsibility requires a change of Event Organiser (Source: is212-discussions-2026-08-31, #61, #59).
- Cross-client isolation is a hard boundary, not a default. The brief's "not normally" is tightened by #81 to "must not access".

This rule scopes [[reporting]] as well: Event Organisers can view or export attendee information **for events they organise**, subject to access restrictions (Source: is212-discussions-2026-08-31, #32).

## Attendee Restrictions

- An Attendee **must not be able to view internal planning information** (Source: customer-brief-no-1, s8b) — that is, the internal-planning, venue/equipment arrangement, and client categories from #91.
- Attendees may view events they have registered for **and** events open for registration (Source: is212-discussions-2026-08-31, #10). Discoverability of open events is therefore intentional and is not a leak of protected information.
- Attendees see their own registration status; there is no stated visibility of other attendees' registrations. See [[attendee-registration]].

## Internal Staff Scope

Venue Staff and Technical Support Staff have access to the information needed for their responsibilities and are **not restricted by venue, equipment type, or location** (Source: is212-discussions-2026-08-31, #66). Internal staff scope is therefore functional, not territorial. See [[user-roles]].

## Role-Based Dashboards

The brief requires that "the system provides each internal or external role with an overview relevant to its responsibilities, such as events requiring attention, upcoming bookings, equipment requests, or attendee registrations" (Source: customer-brief-no-1, s4). Related requirements:

- The event calendar shows information that **depends on the user's role and access rights** (Source: customer-brief-no-1, s4).
- Information should not assume users understand ConnectSphere's internal processes; current status, confirmed arrangements, and outstanding actions must be clear to the appropriate user (Source: customer-brief-no-1, s7).

Indicative dashboard content per role:

| Role | Relevant overview |
|---|---|
| Event Organiser | Own events by status, own drafts, clarification requests awaiting response, confirmed arrangements, registration counts for own events |
| Event Coordinator | Assigned events requiring attention, pending venue/equipment decisions, incomplete arrangements, upcoming events — see [[event-readiness]] |
| Venue Staff | Incoming venue booking requests, upcoming bookings, blocked venues — see [[venue-booking]] |
| Technical Support Staff | Equipment requests to review, upcoming reservations, on-site support assignments — see [[technical-support]] |
| Attendee | Registered events, waiting-list positions, open events, registration status |

> [!question]- Team decision required — dashboard composition per role
> Dashboard composition per role. The brief gives examples ("such as"), not a specification, and the customer has consistently returned UI/UX questions to the team ("This is a UI/UX question, please propose and do the appropriate" — Source: is212-discussions-2026-08-31, #6, #27).

Since a person may hold multiple roles (Source: is212-discussions-2026-08-31, #56), the dashboard must resolve the **union** of that user's roles, and permission checks must be evaluated per event rather than per account.

## Auditability

Access control is supported by, but distinct from, the audit requirement: important actions are recorded so ConnectSphere can determine what was changed, who changed it, and when (Source: customer-brief-no-1, s8f). The technical mechanism is for the team to propose (Source: is212-discussions-2026-08-31, #14). Change history is itself protected — "users with **appropriate access** can review how important event information has changed over time" (Source: customer-brief-no-1, s4). See [[non-functional-requirements]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Authentication method | **Unspecified — team to propose** | #62 |
| Account recovery and locked-account handling | **Unspecified** | #64 |
| How accounts are created for internal and external users | **Not a listed feature — team to propose** | #49, #53, #52 |
| Field-level permission matrix | **Team to propose** | #91 |
| Whether a user-administration permission exists | **Team to propose** | #52 |
| Dashboard content per role | Team to propose | brief s4 |

See [[open-questions]] for the consolidated list.
