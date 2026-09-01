---
title: "Source: Customer Brief no. 1"
type: source-summary
sources:
  - "Customer Brief no. 1.md"
tags: [source, requirements, customer-brief, connectsphere, is212]
---

# Source: Customer Brief no. 1

Initial customer briefing for the IS212 (AY 2026/27 T1) project. Describes [[connectsphere-event-services]]' desired [[event-planning-venue-booking-system]].

## Metadata

| Field | Value |
|---|---|
| Type | Customer briefing (mock) |
| Course | IS212, AY 2026/27 Term 1 |
| Status | Baseline requirements; **deliberately ambiguous by design** |
| Supersedes | — |
| Clarified by | [[is212-discussions-2026-08-26]], [[is212-discussions-2026-08-31]] |

## Framing (read first)

- The brief is **deliberately ambiguous**. No real customer knows precisely what they want on day 1, and the document is written to contain edge cases and conflicting expectations.
- Disambiguation happens only through the scheduled customer Q&A sessions, where the section instructor role-plays the customer. Questions through other channels will not be answered.
- **Each section has a different customer.** Only clarifications from one's own section apply; other sections' answers may be inconsistent.
- The team will **not** build the entire system. The customer identifies the core functionality for Release 1 in Week 4.
- Project instructions and rubrics were to be released Friday of Week 3. Grading emphasises the **scrum process, requirements discovery, software quality, and documentation** over the finished product.
- Recommendation: use Weeks 2–4 for requirements discovery and product backlog construction; do not sprint before Week 4.

## Organisation and background

ConnectSphere is a regional event planning and venue services provider operating across Southeast Asia, with facilities in Singapore and roughly 500 operational staff. It currently coordinates events across email, spreadsheets, shared calendars, messaging apps, online forms, and manual documents, so a single event's information is fragmented across places and owners.

The brief enumerates the resulting pain points: incomplete or duplicated requests, divergent versions of event information, unclear approval status, stale requirements reaching Venue Staff, accidental double-booking, insufficient turnaround time, last-minute changes not propagating, cancelled events still holding reservations, and no reliable record of who changed what and when.

## Content map

| Section | Covers | Wiki page |
|---|---|---|
| 1–2 | Organisation, current process, pain points | [[connectsphere-event-services]] |
| 3 | Target user roles | [[user-roles]] |
| 4 | ~60 functionality areas across 5 tables | [[event-planning-venue-booking-system]] |
| 5 | The 14-step typical event planning process | [[event-request-workflow]] |
| 6 | Notification scenarios | [[notifications]] |
| 7 | Design — desktop and mobile friendly, clarity for non-experts | [[non-functional-requirements]] |
| 8 | Performance, security, usability, reliability, scalability, auditability, maintainability | [[non-functional-requirements]] |

## Key claims

- Five user roles are defined: Event Organiser and Attendee (external); Event Coordinator, Venue Staff, Technical Support Staff (internal). See [[user-roles]].
- Venue suitability depends on date/time, capacity, supported layouts, accessibility, facilities, existing bookings, operating hours, setup requirements, and turnaround time between consecutive events. See [[venue-suitability]].
- Equipment may be unavailable because it is already reserved, located elsewhere, damaged, or under maintenance. See [[equipment-management]].
- The system must distinguish **ordinary edits** from **changes that affect already-confirmed arrangements**. See [[change-impact-management]].
- Events progress through statuses such as draft, submitted, under review, approved, planning, awaiting arrangements, confirmed, completed, cancelled, rejected. See [[event-lifecycle]].
- Access must depend on role *and relationship to an event* — organisers cannot see unrelated clients' events, attendees cannot see internal planning information. See [[access-control]].

## Open items flagged by the brief itself

The brief closes by stating that the customer will continue consulting stakeholders and that **special cases and business rules not described in the initial briefing exist**. Students are expected to surface these via Q&A. See [[open-questions]].
