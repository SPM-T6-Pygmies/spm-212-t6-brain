---
title: Non-Functional Requirements
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - nfr
  - quality-attributes
  - constraints
  - architecture
  - connectsphere
---

# Non-Functional Requirements

**Objects:** [[event]] · [[booking]] · [[audit-record]] · [[user-account]]

Section 8 of the brief states seven quality attributes for the [[event-planning-venue-booking-system]] (Source: customer-brief-no-1). Every one is stated qualitatively — the customer set **no numeric targets** anywhere. The Q&A rounds narrowed the operating envelope (timezone, language, retention, integration) rather than adding thresholds, and repeatedly returned mechanism decisions to the team.

## Summary

| # | Attribute | Customer position | Team obligation |
|---|---|---|---|
| a | Performance | "Reasonable time" for common operations; no figure given | Propose and justify targets |
| b | Security | Role- **and relationship**-based access to protected information | Design access model; see [[access-control]] |
| c | Usability | Usable without extensive training by all user groups | Design for non-expert external users |
| d | Reliability & consistency | No contradictory information across related arrangements | Enforce via [[change-impact-management]] |
| e | Scalability | Support 3 years of anticipated growth | Size against ~500 staff + external users |
| f | Auditability | Record **what** changed, **who** changed it, **when** | Mechanism is the team's choice ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)) |
| g | Maintainability | New functionality and business rules addable without a rebuild | Modular design, externalised business rules |

## a. Performance

- Common operations must "complete within a reasonable time": viewing an event, searching available venues, opening a venue calendar, checking equipment availability, submitting a registration (Source: customer-brief-no-1, s8a).
- No response-time figure, throughput target, or percentile was given. The named operations are read-heavy and calendar/availability-oriented — the performance profile is dominated by [[venue-booking]] availability queries and [[equipment-management]] availability checks.
- Related: notifications carry **no** delivery-time requirement. Batch delivery at configurable intervals is acceptable, and "there is no hard requirement for notifications to be received within a specific time limit" (Source: is212-discussions-2026-08-31, [#55](https://github.com/SinYang13/IS212-2026/discussions/55)). See [[notifications]].

> [!question]- Team decision required — performance acceptance criteria
> Define measurable performance acceptance criteria (e.g. p95 latency per named operation) since the customer supplied none.

## b. Security

- Only authorised users may access protected information. Access depends on the user's **role and relationship to an event** — not role alone (Source: customer-brief-no-1, s8b).
- Stated examples: an Event Organiser should not normally view events of unrelated clients; an Attendee should not view internal planning information.
- Protected information was defined on 2026-08-31 as internal planning information, venue and equipment arrangements, attendee information, client-related information, and other non-public event details. **The exact access-control design and field-level permissions are for the team to propose** (Source: is212-discussions-2026-08-31, [#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
- Secure authentication is required, but **the authentication method is not specified** — email/password, SSO, or MFA are all open (Source: is212-discussions-2026-08-31, [#62](https://github.com/SinYang13/IS212-2026/discussions/62)).
- Account recovery and locked-account handling are **not specified in the current requirements** (Source: is212-discussions-2026-08-31, [#64](https://github.com/SinYang13/IS212-2026/discussions/64)).

Full role and permission detail is in [[access-control]] and [[user-roles]].

## c. Usability

- All user groups must complete their common activities "without extensive training or guidance" (Source: customer-brief-no-1, s8c).
- Information must not assume the user understands ConnectSphere's internal processes; current status, confirmed arrangements, and outstanding actions must be easy to understand (Source: customer-brief-no-1, s7).
- The system must be both **desktop- and mobile-friendly**: external users on personal computers or mobile devices, internal staff while physically preparing event spaces or equipment (Source: customer-brief-no-1, s7).

Two constraints that shrink the usability scope:

| Constraint | Position | Reference |
|---|---|---|
| Offline access | **Not specified as a requirement for now**, despite the mobile/on-site use case | [#31](https://github.com/SinYang13/IS212-2026/discussions/31) |
| Multiple languages | Not currently specified; **assume English** | [#28](https://github.com/SinYang13/IS212-2026/discussions/28) |

(Source: is212-discussions-2026-08-31)

> [!warning] Tension — offline access vs on-site use
> Internal staff are explicitly expected to use the system "while preparing event spaces or equipment" (s7) and a team asked directly about poor venue connectivity, yet offline access was declined ([#31](https://github.com/SinYang13/IS212-2026/discussions/31)). The mobile experience must therefore assume connectivity.

## d. Reliability and Consistency

- The system must maintain consistent information when related arrangements change. Users must not see contradictory information about event dates, venue bookings, equipment availability, or registration status (Source: customer-brief-no-1, s8d).
- This is the quality attribute behind most of the functional pain in the brief's background section: duplicate spreadsheets, stale venue requirements, double-booked venues and equipment, attendees receiving outdated dates.
- Concrete consistency rules already fixed by the customer:

| Rule | Reference |
|---|---|
| Overlapping or incompatible venue bookings must be **blocked**, not merely warned | [#35](https://github.com/SinYang13/IS212-2026/discussions/35) |
| Only one active tentative hold **or** confirmed booking per venue and overlapping period | [#41](https://github.com/SinYang13/IS212-2026/discussions/41) |
| A piece of equipment cannot serve two events on the same day; available again only from Return Day +1 | [#5](https://github.com/SinYang13/IS212-2026/discussions/5) |
| Transfer and preparation time must be considered when determining pre-event equipment availability | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) |
| An event must not be confirmable while an arrangement identified as essential is incomplete | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) |
| Cancelled events must not continue to hold venue or equipment reservations | brief, s4 |

(Source: is212-discussions-2026-08-31; customer-brief-no-1)

Significant changes must trigger re-validation of affected arrangements; ordinary edits need not (Source: is212-discussions-2026-08-31, [#4](https://github.com/SinYang13/IS212-2026/discussions/4)). See [[change-impact-management]] and [[event-lifecycle]].

## e. Scalability

- The solution must support ConnectSphere's anticipated growth over the **next three years**, across events, users, client organisations, venues, equipment, and registrations (Source: customer-brief-no-1, s8e).
- Asked directly how many concurrent users to support and whether microservices are needed, the customer declined to give a number: the system should support three years of expected growth, and **"the architecture, including whether microservices are appropriate, is for you to propose. Do also consider the nature of business and staff strength."** (Source: is212-discussions-2026-08-31, [#7](https://github.com/SinYang13/IS212-2026/discussions/7)).
- The sizing anchors the customer did provide:

| Anchor | Value | Reference |
|---|---|---|
| Internal staff | ~500 (Event Coordinators, Venue Staff, Technical Support Staff) | brief, s1 |
| External users | Unbounded — Event Organisers per client organisation, plus Attendees per event | brief, s3 |
| Geography | Singapore only; all owned venues are in Singapore | [#36](https://github.com/SinYang13/IS212-2026/discussions/36) |
| Growth horizon | 3 years | brief, s8e |

- **Timezone: Singapore only.** Because all venues ConnectSphere owns are in Singapore, the system uses the Singapore timezone, including for organisers based outside Singapore (Source: is212-discussions-2026-08-31, [#36](https://github.com/SinYang13/IS212-2026/discussions/36)). No multi-timezone rendering, no per-user timezone preference.
- ~500 staff is a working population, not a concurrency figure. Concurrency assumptions must be derived and stated by the team.

> [!question]- Team decision required — concurrency and architecture
> Concurrency assumptions and the architectural style (monolith, modular monolith, microservices) are entirely the team's to propose and defend ([#7](https://github.com/SinYang13/IS212-2026/discussions/7)).

## f. Auditability

- Important actions must be recorded. Where appropriate, ConnectSphere must be able to determine **what was changed, who changed it, and when** (Source: customer-brief-no-1, s8f).
- This maps to two functional areas in the brief: **Activity History** (significant actions, actor, timestamp) and **Change History** (how important event information changed over time, visible to users with appropriate access). See [[reporting]].
- Asked whether the audit log must be tamper-proof (append-only, no edits or deletes) or whether a regular database table suffices, the customer refused to constrain the implementation: the requirement is the what/who/when record, and **"the technical mechanism used to achieve this is for you to propose"** (Source: is212-discussions-2026-08-31, [#14](https://github.com/SinYang13/IS212-2026/discussions/14)).
- A driving pain point from the brief: "It is difficult to determine who changed important event information and when the change occurred" (Source: customer-brief-no-1, s2).
- Retention interacts with audit: there is **no retention policy**, so audit records are not bounded by one either (see below).

## g. Maintainability

- ConnectSphere expects its processes to evolve. The system must be designed so future functionality and business rules can be added **without requiring the entire system to be rebuilt** (Source: customer-brief-no-1, s8g).
- The brief reinforces this in its closing note: the customer will continue discussing the system with stakeholders and "there may be special cases and business rules that have not been described in this initial briefing".
- The volume of unresolved business rules in [[open-questions]] is itself the maintainability argument: lead times, hold durations, notice periods, reminder timings, registration limits and booking horizons are all unset, so they should be **configurable rather than hard-coded**.
- Already-confirmed configurability requirements:

| Item | Requirement | Reference |
|---|---|---|
| Notification frequency | Batch frequency must be configurable | [#55](https://github.com/SinYang13/IS212-2026/discussions/55), [#47](https://github.com/SinYang13/IS212-2026/discussions/47) |
| Notification channels | In-app and email, configurable per notification type | [#37](https://github.com/SinYang13/IS212-2026/discussions/37) |
| Turnaround / setup / teardown | Recorded **per venue** by Venue Staff, not a global constant | [#43](https://github.com/SinYang13/IS212-2026/discussions/43) |

(Source: is212-discussions-2026-08-31)

## Scope Constraints and Exclusions

These answers bound the non-functional surface. Each removes work — and each is a documented assumption that could change.

| Area | Position | Reference |
|---|---|---|
| Timezone | Singapore timezone only; all owned venues are in Singapore | [#36](https://github.com/SinYang13/IS212-2026/discussions/36) |
| Offline access | Not required | [#31](https://github.com/SinYang13/IS212-2026/discussions/31) |
| Internationalisation | English only; multi-language not specified | [#28](https://github.com/SinYang13/IS212-2026/discussions/28) |
| Data retention / archival | **No retention policy.** Completed and cancelled events may remain available as historical records. Archival and housekeeping are outside current functional scope | [#20](https://github.com/SinYang13/IS212-2026/discussions/20) |
| Document size / file types | **No limits specified.** Team to propose constraints if the solution requires them | [#18](https://github.com/SinYang13/IS212-2026/discussions/18) |
| Document versioning | Not required — supporting documents are not legal documents, no versions kept | [#21](https://github.com/SinYang13/IS212-2026/discussions/21) |
| Data migration / import | **No migration or import requirement** — the system starts from empty | [#12](https://github.com/SinYang13/IS212-2026/discussions/12) |
| System integration | **Not in scope.** No integration with existing external applications or services required | [#79](https://github.com/SinYang13/IS212-2026/discussions/79), [#12](https://github.com/SinYang13/IS212-2026/discussions/12) |
| Authoritative source | For event information managed by the system, the **Event Planning and Venue Booking System is authoritative** | [#79](https://github.com/SinYang13/IS212-2026/discussions/79) |
| Email notifications | Email is a **notification-channel requirement**, not a general integration requirement | [#79](https://github.com/SinYang13/IS212-2026/discussions/79) |
| Contracts / payments | Handled physically, outside the system | [#24](https://github.com/SinYang13/IS212-2026/discussions/24) |
| Post-event equipment checklist | Kept outside the system | [#85](https://github.com/SinYang13/IS212-2026/discussions/85) |

(Source: is212-discussions-2026-08-31)

### On integration ([#79](https://github.com/SinYang13/IS212-2026/discussions/79), answered 2026-08-31)

The customer's five-part answer is load-bearing for architecture:

1. The external applications currently in use are **not specified**; integration with external tools is **not in scope currently**.
2. The new system is the **primary system** for the event-planning and venue-booking functions in scope; other tools may still be used for out-of-scope activities.
3. No integration with existing external applications is required. **Email notification is a notification-channel requirement, not a general integration requirement** — sending email does not imply an integration workstream with ConnectSphere's mail estate.
4. The new system is the **authoritative source** for event information it manages. There is no reconciliation or sync-conflict problem to solve.
5. Whether any integration is needed for the first release defers to the **final Release 1 scope**, which the customer said would be confirmed separately.

Practical consequence: no calendar sync, no spreadsheet import, no document-store linking, and no two-way data ownership questions in the current scope.

## Open Items

| Item | Status | Reference |
|---|---|---|
| Performance targets (numbers) | Not given — team to propose | brief s8a |
| Authentication method | **Unspecified — team to propose** | [#62](https://github.com/SinYang13/IS212-2026/discussions/62) |
| Account recovery / locked accounts | **Not specified** | [#64](https://github.com/SinYang13/IS212-2026/discussions/64) |
| Audit mechanism (append-only vs table) | Team to propose; requirement is what/who/when | [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |
| Concurrency assumptions and architecture | Team to propose; consider ~500 staff and 3-year growth | [#7](https://github.com/SinYang13/IS212-2026/discussions/7) |
| Document size and file-type limits | Team to propose if required | [#18](https://github.com/SinYang13/IS212-2026/discussions/18) |
| Offline access, i18n, retention, integration | Declared out of scope — revisit if Release 1 scope changes | [#31](https://github.com/SinYang13/IS212-2026/discussions/31), [#28](https://github.com/SinYang13/IS212-2026/discussions/28), [#20](https://github.com/SinYang13/IS212-2026/discussions/20), [#79](https://github.com/SinYang13/IS212-2026/discussions/79) |

Consolidated in [[open-questions]].
