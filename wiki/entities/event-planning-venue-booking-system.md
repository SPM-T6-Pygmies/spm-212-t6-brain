---
title: Event Planning and Venue Booking System
type: entity
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "Week 4 - Customer Briefing.md"
tags: [entity, product, system, scope, release-1]
---

# Event Planning and Venue Booking System

The custom-built system to be delivered for [[connectsphere-event-services]] — a single platform replacing the current spread of email, spreadsheets, shared calendars, messaging apps and manual documents.

## Positioning

- Serves **internal and external** users across five briefed roles. See [[user-roles]].
- Must be both **desktop- and mobile-friendly**: external users access from personal devices, internal staff while preparing venues and equipment (Source: customer-brief-no-1).
- Presentation must not assume users understand ConnectSphere's internal processes; status, confirmed arrangements and outstanding actions must be legible to the appropriate user.
- Treated as the **authoritative source** for the event information it manages; no integration with existing external tools is in scope (Source: is212-discussions-2026-08-31, [#79](https://github.com/SinYang13/IS212-2026/discussions/79)).

## Functionality areas

[[customer-brief-no-1]] enumerates roughly sixty functionality areas across five tables. They are grouped here by the concept page that covers them, with the Release 1 scope [[week-4-customer-briefing]] assigned each group.

Group-level markers are a summary only — several groups split. [[release-1-scope]] is the source of truth and carries the per-area register.

| Group | Functionality areas | Scope | Page |
|---|---|---|---|
| Identity and access | User authorisation and authentication · client management · user profile management | 🎯 / 🧩 / 📋 | [[access-control]], [[user-roles]] |
| Request intake | Event request creation · draft event requests · event categories · recurring or similar events | 🎯 / 📋 | [[event-request-workflow]] |
| Review and status | Event review and approval · coordinator assignment · event status management · event information management | 🎯 | [[event-lifecycle]], [[event-request-workflow]] |
| Event content | Comments and discussion · event programme and agenda · multi-session events · supporting documents | 🧩 / 📋 | [[event-request-workflow]] |
| Venue | Catalogue · availability calendar · search and filtering · suitability checking · booking request · booking approval · conflict detection · unavailability · room layouts · setup and turnaround · tentative holding · event capacity | 🎯 / 🧩 / 📋 | [[venue-booking]], [[venue-suitability]] |
| Equipment | Request management · availability checking · reservation · catalogue · maintenance status | 🎯 / 🧩 | [[equipment-management]] |
| Technical support | Technical support requests · technical staff assignment | 📋 | [[technical-support]] |
| Readiness | Event confirmation · readiness tracking · outstanding action tracking | 🧩 / 📋 | [[event-readiness]] |
| Registration | Attendee registration · withdrawal · registration period · registration capacity · waiting list · attendance recording | 🎯 / 🧩 / 📋 | [[attendee-registration]] |
| Change | Event change requests · impact of event changes · rescheduling · cancellation management | 🎯 / 🧩 | [[change-impact-management]] |
| Messaging | Notification system · reminder system | 🎯 / 📋 | [[notifications]] |
| Discovery | Event calendar · event search and filtering · role-based dashboard | 📋 | [[reporting]] |
| Records | Activity history · change history | 📋 | [[non-functional-requirements]] |
| Output | Event reports · venue usage reports · registration reports · report generation and exporting | 📋 | [[reporting]] |

## Quality attributes

Performance, security, usability, reliability and consistency, scalability, auditability and maintainability are all specified in section 8 of the brief. See [[non-functional-requirements]].

## Scope status

> [!success] Resolved — [#79](https://github.com/SinYang13/IS212-2026/discussions/79)
> Release 1 scope is **fixed**. [[week-4-customer-briefing]] names 20 core features as the required content of the Week 12 release, closing [#79](https://github.com/SinYang13/IS212-2026/discussions/79) and [#11](https://github.com/SinYang13/IS212-2026/discussions/11). The remaining ~40 functionality areas above are deferred to the product backlog and C4 models, not cancelled. Full register: [[release-1-scope]].

The 20 are not a list of independent deliverables. The briefing requires the release to "support coherent end-to-end event workflows across the relevant features", which pulls in a set of unnamed but load-bearing areas — the equipment catalogue, the confirmation gate, venue unavailability records. Those are marked `🧩 Adjacent` in [[release-1-scope]] and are this wiki's reading of the clause, not customer statements.

## Explicitly out of scope

Distinct from `📋 Backlog` in [[release-1-scope]]: these were **denied**, not deferred. A backlog item may return in a later release; these were answered "not a requirement".

| Excluded | Ref |
|---|---|
| Contracts and payments | [#24](https://github.com/SinYang13/IS212-2026/discussions/24) |
| Document versioning | [#21](https://github.com/SinYang13/IS212-2026/discussions/21) |
| Data migration and import | [#12](https://github.com/SinYang13/IS212-2026/discussions/12) |
| Integration with existing external services | [#79](https://github.com/SinYang13/IS212-2026/discussions/79), [#12](https://github.com/SinYang13/IS212-2026/discussions/12) |
| Post-event equipment health checklist | [#85](https://github.com/SinYang13/IS212-2026/discussions/85) |
| Offline access | [#31](https://github.com/SinYang13/IS212-2026/discussions/31) |
| Multi-language support | [#28](https://github.com/SinYang13/IS212-2026/discussions/28) |
| Data retention, archival and housekeeping | [#20](https://github.com/SinYang13/IS212-2026/discussions/20) |
| External user onboarding / self-registration | [#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#53](https://github.com/SinYang13/IS212-2026/discussions/53) |
| Account recovery and locked-account handling | [#64](https://github.com/SinYang13/IS212-2026/discussions/64) |
| System configuration and user administration features | [#52](https://github.com/SinYang13/IS212-2026/discussions/52) |
