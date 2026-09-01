---
title: "Source: IS212 Discussions — 2026-08-26 snapshot"
type: source-summary
sources:
  - "IS212 2026 Discussions Aug 26.md"
tags: [source, clarifications, q-and-a, github-discussions, is212]
---

# Source: IS212 Discussions — 2026-08-26 snapshot

First extract of the [[is212-g9-section]] GitHub Discussions board, the sole channel for customer clarification on the [[event-planning-venue-booking-system]].

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Extracted | 2026-08-25 17:27 UTC |
| Volume | 75 discussions, 11 categories |
| Answer state | 69 answered · 5 unanswered · 18 labelled `important` |
| Clarifies | [[customer-brief-no-1]] |
| Superseded by | [[is212-discussions-2026-08-31]] |

> [!warning] Superseded
> This is the earlier of two snapshots of the same board. Every discussion here also appears in [[is212-discussions-2026-08-31]], which resolves four of the five open items. Treat the Aug 31 snapshot as authoritative and use this page only to reconstruct what the team knew as of the Week 2–3 boundary.

## Board conventions

- Questions are titled with the group number, e.g. `[T6] ...`.
- Duplicates may be closed; search before posting.
- Upvoted questions are prioritised during in-person clarification sessions; the remainder are answered ad-hoc.
- Answers come from the customer personas `SinYang13` and `josephsungls`.

## Category volumes

| Category | Count | Unanswered | `important` |
|---|---|---|---|
| Announcements | 1 | — | — |
| Calendar | 3 | — | — |
| Equipment & Technical Support | 9 | 1 | 2 |
| Events | 15 | 1 | 5 |
| General | 5 | 1 | — |
| Non-Functional Requirements | 7 | — | — |
| Notifications | 4 | 1 | — |
| Registration & Attendees | 9 | — | 5 |
| Reporting & Analytics | 3 | — | — |
| Users | 10 | 1 | 3 |
| Venues | 9 | — | 3 |

## Substantive answers first appearing in this snapshot

These were settled by 2026-08-25 and carry through unchanged to the later snapshot.

| Area | Ruling | Ref |
|---|---|---|
| [[event-request-workflow]] | An **Event Operations Manager** assigns the Event Coordinator after submission | #73 |
| [[event-lifecycle]] | Coordinator marks completion; system may auto-complete 7 days after end date | #71 |
| [[event-lifecycle]] | Only the assigned coordinator may cancel; cancellation possible at any stage | #68 |
| [[event-lifecycle]] | Postponement with a new date is a reschedule; without one, release bookings but retain the record | #69 |
| [[venue-booking]] | Venues book in three slots — AM / PM / Night | #50 |
| [[venue-booking]] | Only one active tentative hold or confirmed booking per venue and overlapping period | #41 |
| [[venue-booking]] | Double-booking must be **blocked**, not merely warned | #35 |
| [[venue-booking]] | Setup, teardown and turnaround are recorded per venue | #43 |
| [[equipment-management]] | No same-day equipment sharing; available again only from Return Day + 1 | #5 |
| [[attendee-registration]] | Waiting list is FIFO with manual override; 3-day acceptance window | #26, #44 |
| [[attendee-registration]] | Attendance is checked in manually at the counter, by QR or staff | #38 |
| [[notifications]] | Both in-app and email channels, configurable by notification type | #37 |
| [[non-functional-requirements]] | Singapore timezone only; English only; no offline mode; no data migration | #36, #28, #31, #12 |
| [[user-roles]] | A person may hold multiple roles; no admin role defined | #56, #52 |

## Items still open at this snapshot

All but the last were answered on 2026-08-31.

| Ref | Question | Resolution |
|---|---|---|
| #80 | Difference between an event being *approved* and *confirmed* | Answered 2026-08-31 — see [[event-lifecycle]] |
| #81 | What events Organisers from the same client organisation may access | Answered 2026-08-31 — see [[access-control]] |
| #82 | Maximum or minimum equipment reservation period | Answered 2026-08-31 — see [[equipment-management]] |
| #79 | Existing software and expected integration | Answered 2026-08-31 — see [[non-functional-requirements]] |
| #55 | Whether notifications are real-time or batch | Re-opened by follow-up; clarified 2026-08-31 — see [[notifications]] |

See [[open-questions]] for the consolidated register.
