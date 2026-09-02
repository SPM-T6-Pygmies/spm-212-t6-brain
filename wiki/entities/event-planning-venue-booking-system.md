---
title: Event Planning and Venue Booking System
type: entity
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [entity, product, system, scope]
---

# Event Planning and Venue Booking System

The custom-built system to be delivered for [[connectsphere-event-services]] — a single platform replacing the current spread of email, spreadsheets, shared calendars, messaging apps and manual documents.

## Positioning

- Serves **internal and external** users across five briefed roles. See [[user-roles]].
- Must be both **desktop- and mobile-friendly**: external users access from personal devices, internal staff while preparing venues and equipment (Source: customer-brief-no-1).
- Presentation must not assume users understand ConnectSphere's internal processes; status, confirmed arrangements and outstanding actions must be legible to the appropriate user.
- Treated as the **authoritative source** for the event information it manages; no integration with existing external tools is in scope (Source: is212-discussions-2026-08-31, [#79](https://github.com/SinYang13/IS212-2026/discussions/79)).

## Functionality areas

The brief enumerates roughly sixty functionality areas across five tables. They are grouped here by the concept page that covers them.

| Group               | Functionality areas                                                                                                                                                                                                                                                   | Page                                            |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| Identity and access | User authorisation and authentication, user profile management, client management                                                                                                                                                                                     | [[access-control]], [[user-roles]]              |
| Request intake      | Event request creation, draft event requests, event categories, recurring or similar events                                                                                                                                                                           | [[event-request-workflow]]                      |
| Review and status   | Event review and approval, coordinator assignment, event status management, event information management                                                                                                                                                              | [[event-lifecycle]], [[event-request-workflow]] |
| Event content       | Event programme and agenda, multi-session events, comments and discussion, supporting documents                                                                                                                                                                       | [[event-request-workflow]]                      |
| Venue               | Venue catalogue, availability calendar, search and filtering, suitability checking, booking request, booking approval, tentative holding, conflict detection, setup and turnaround time, unavailability management, room layout management, event capacity management | [[venue-booking]], [[venue-suitability]]        |
| Equipment           | Equipment catalogue, request management, availability checking, reservation, maintenance status                                                                                                                                                                       | [[equipment-management]]                        |
| Technical support   | Technical support requests, technical staff assignment                                                                                                                                                                                                                | [[technical-support]]                           |
| Readiness           | Event readiness tracking, outstanding action tracking, event confirmation                                                                                                                                                                                             | [[event-readiness]]                             |
| Registration        | Attendee registration, registration capacity, registration period, waiting list, withdrawal, attendance recording                                                                                                                                                     | [[attendee-registration]]                       |
| Change              | Event change requests, impact of event changes, rescheduling, cancellation management                                                                                                                                                                                 | [[change-impact-management]]                    |
| Messaging           | Notification system, reminder system                                                                                                                                                                                                                                  | [[notifications]]                               |
| Discovery           | Event calendar, search and filtering, role-based dashboard                                                                                                                                                                                                            | [[reporting]]                                   |
| Records             | Activity history, change history                                                                                                                                                                                                                                      | [[non-functional-requirements]]                 |
| Output              | Event reports, venue usage reports, registration reports, report generation and exporting                                                                                                                                                                             | [[reporting]]                                   |

## Quality attributes

Performance, security, usability, reliability and consistency, scalability, auditability and maintainability are all specified in section 8 of the brief. See [[non-functional-requirements]].

## Scope status

> [!warning] Release 1 scope is not yet fixed
> The brief states the customer will identify core Release 1 functionality in Week 4, and answer [#79](https://github.com/SinYang13/IS212-2026/discussions/79) defers to a separate scope confirmation. The full functionality list above is the *consideration set*, not the delivery commitment. See [[open-questions]].

## Explicitly out of scope

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
