---
title: Reporting
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - reporting
  - exports
  - dashboards
  - audit-trail
  - connectsphere
---

# Reporting

**Objects:** [[audit-record]] · [[registration]] · [[event]] · [[venue]]

The read-and-extract surface of the [[event-planning-venue-booking-system]]: reports, exports, history views, and role-based dashboards. The brief specifies four report types plus export, and two history mechanisms (Source: customer-brief-no-1, s4). The Q&A rounds settled the generation model and the access model, but left almost all report **content** to the team.

**Release 1 scope** ([[release-1-scope]]): every report type, export mechanism, history-browsing surface and dashboard on this page is `📋 Backlog` — none of Event Reports, Venue Usage Reports, Registration Reports, Report Generation and Exporting, Activity History, Change History, or Role-Based Dashboards is named in the core 20. This does not touch the underlying auditability requirement itself — recording what changed, who, and when is 🎯 Release 1 as a non-functional requirement; only the browsable history surface described here is deferred. See [[non-functional-requirements]] and [[audit-record]].

## Report Types

| Report | Audience | Purpose per the brief |
|---|---|---|
| Event Reports | "Appropriate users" | Summaries containing event details, schedule, venue, operational requirements, registration information, and other relevant information |
| Venue Usage Reports | Internal users | Review venue bookings and usage over a selected period to support operational planning |
| Registration Reports | Event Organisers and Event Coordinators | View or export attendee information where appropriate and subject to access restrictions |
| Report Generation and Exporting | All report consumers | Export suitable reports or event information into common formats for meetings, operational use, or record-keeping |

(Source: customer-brief-no-1, s4)

## Generation Model

**On demand only.** Asked whether reports are generated on demand or scheduled (e.g. a weekly automatic report), the customer answered: "The current requirement is for users to generate reports when required." (Source: is212-discussions-2026-08-31, [#34](https://github.com/SinYang13/IS212-2026/discussions/34)).

Consequences:

- No report scheduler, no subscription list, no recurring-delivery mechanism in current scope.
- No report-delivery notifications. Scheduled behaviour in the system is confined to **reminders**, which are a separate concern — see [[notifications]].
- Report generation is a synchronous, user-initiated operation and therefore falls under the "reasonable time" performance expectation in [[non-functional-requirements]].

## Venue Usage Reports

The most under-specified report. Asked directly what a Venue Usage Report contains — occupancy rates, booking frequency, revenue, utilisation by day/time — the customer replied that the requirement is to let internal users review venue bookings and usage over a selected period for operational planning, that **"the exact measures and presentation are not specified yet"**, and that the team should propose appropriate information supporting venue usage and operational planning (Source: is212-discussions-2026-08-31, [#29](https://github.com/SinYang13/IS212-2026/discussions/29)).

> [!info] Revenue is explicitly excluded
> "Revenue is not currently stated as part of this report." ([#29](https://github.com/SinYang13/IS212-2026/discussions/29)). This is consistent with contracts and payments being handled physically and outside the system ([#24](https://github.com/SinYang13/IS212-2026/discussions/24)) — the system holds no pricing or financial data, so no financial measure is derivable.

Fixed inputs the team can build measures from:

| Available dimension | Source |
|---|---|
| Selected reporting period | brief, s4 |
| Booking slots — AM / PM / Night, as defined on [[venue]]; full day = all three | [#50](https://github.com/SinYang13/IS212-2026/discussions/50) |
| Booking state — available, tentatively held, confirmed, blocked, unavailable | brief, s4 |
| Blocked reasons — maintenance, renovation, safety, internal activity | brief, s4 |
| Setup, teardown and turnaround time, recorded **per venue** | [#43](https://github.com/SinYang13/IS212-2026/discussions/43) |
| Venue attributes — capacity, layouts, facilities, accessibility, operating hours | brief, s4 |
| Event category — conference, workshop, training, exhibition, meeting, seminar, networking, etc. | brief, s4 |

The three-slot booking model ([#50](https://github.com/SinYang13/IS212-2026/discussions/50)) is the natural unit of utilisation: slots booked over slots available in the period, sliceable by venue, day, state, and event category. Turnaround time is a genuine occupancy cost and can be reported as consumed capacity distinct from event time. See [[venue-booking]].

> [!question]- Team decision required — venue usage report measures
> Define the measure set and presentation for the venue usage report. Exclude revenue and any financial measure.

## Registration Reports

- Event Organisers can **view or export attendee information for events they organise**, "where appropriate and subject to access restrictions" (Source: is212-discussions-2026-08-31, [#32](https://github.com/SinYang13/IS212-2026/discussions/32); customer-brief-no-1, s4).
- Access is scoped by relationship to the event, consistent with [[access-control]]: an Event Organiser can view, edit and update events for which they are responsible; other organisers in the same client organisation may view but not edit; organisers must never access events of unrelated client organisations ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)).
- Content is bounded by whatever registration fields the team defines — the customer left the required attendee registration information to the team, to be proposed "based on the event needs" ([#22](https://github.com/SinYang13/IS212-2026/discussions/22)). See [[attendee-registration]].
- Registration and attendance are distinct: the system records whether registered Attendees ultimately attended (brief, s4), with check-in done manually at the counter by QR code or by staff ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)). Both belong in registration reporting.
- Waiting-list state is reportable: FIFO with manual override ([#26](https://github.com/SinYang13/IS212-2026/discussions/26)), 3-day acceptance window when a place opens ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)).
- Manually added attendees (VIPs who do not self-register) reach the system via a list provided to the Event Coordinator for processing ([#33](https://github.com/SinYang13/IS212-2026/discussions/33)), so they must appear in registration reports alongside self-registered attendees.

> [!warning] PDPA was asked but not answered
> Question [#32](https://github.com/SinYang13/IS212-2026/discussions/32) asked "Can Event Organisers export attendee data (emails, names) for their own events? **Any privacy/PDPA restrictions?**" The customer answered only the first half — permission to view and export — and did not address privacy or PDPA obligations at all. The phrases "where appropriate" and "subject to access restrictions" are the entire constraint given.

Practical implication: the team must decide unaided how to handle personal data in exports — what fields are exportable, whether exports are logged, whether attendee consent is captured at registration, and whether any field-level redaction applies. [#91](https://github.com/SinYang13/IS212-2026/discussions/91) confirms that attendee information is protected information and that **field-level permissions are for the team to propose**. This is a genuine risk item, not a settled one; it is carried in [[open-questions]].

## Event Reports

- Contents per the brief: event details, schedule, venue, operational requirements, registration information, and other relevant information (Source: customer-brief-no-1, s4).
- Must reflect the authoritative record — for event information it manages, the system is the authoritative source (Source: is212-discussions-2026-08-31, [#79](https://github.com/SinYang13/IS212-2026/discussions/79)). Reports are not reconciled against any external tool.
- Multi-session events may have different timings, venues, equipment requirements and attendance limits per session ([#74](https://github.com/SinYang13/IS212-2026/discussions/74)), so an event report must be able to represent per-session detail rather than a single flat block.
- Readiness state is reportable and directly serves the brief's pain point "it is difficult to identify which upcoming events still have incomplete arrangements" — see [[event-readiness]].

## Export

- The brief asks only for export into "common formats" for meetings, operational use, and record-keeping (Source: customer-brief-no-1, s4). No format was named and none was clarified in Q&A.
- Practical reading: a tabular format for registration and venue usage data, and a document format for event summaries. The choice is unconstrained.
- Export is not an integration mechanism. Integration with external tools is out of scope ([#79](https://github.com/SinYang13/IS212-2026/discussions/79)), so export is a file handed to the user, not a feed.

> [!question]- Team decision required — export formats and audit of exports
> Choose and justify export formats; decide whether exports of personal data are recorded in activity history.

## Activity History and Change History

Two distinct mechanisms, defined structurally on [[audit-record]] (Source: customer-brief-no-1, s4).

- Both serve the auditability requirement: determine what was changed, who changed it, and when (Source: customer-brief-no-1, s8f). The customer left the **technical mechanism** to the team — append-only log or ordinary table both acceptable ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)). See [[non-functional-requirements]].
- Retention is unbounded: no retention policy exists, and completed and cancelled events may remain available as historical records; archival and housekeeping are outside current scope ([#20](https://github.com/SinYang13/IS212-2026/discussions/20)). History therefore accumulates indefinitely by default.
- Supporting documents are **not versioned** — "as they are not legal documents, we do not need to keep any versions" ([#21](https://github.com/SinYang13/IS212-2026/discussions/21)) — so document change history is limited to add/delete events, not content diffs.
- Clarification exchanges are retained with the event record, including Technical Support Staff questions and comments ([#87](https://github.com/SinYang13/IS212-2026/discussions/87)), and comments generally ([#15](https://github.com/SinYang13/IS212-2026/discussions/15)). These form part of the event's history even though comment threading itself is a UI decision.

## Role-Based Dashboards

The brief specifies a per-role overview relevant to each role's responsibilities — "events requiring attention, upcoming bookings, equipment requests, or attendee registrations" (Source: customer-brief-no-1, s4). Dashboards are the standing view; reports are the extract.

| Role | Plausible dashboard content, from confirmed requirements |
|---|---|
| Event Coordinator | Assigned events, outstanding arrangements blocking confirmation ([[event-readiness]]), pending clarifications, change requests to review, incomplete-arrangement reminders ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)) |
| Venue Staff | Pending venue booking requests, upcoming bookings and blocks, setup/turnaround obligations ([[venue-booking]]) |
| Technical Support Staff | Equipment requests to review, reservations, equipment status and returns, on-site support assignments ([[technical-support]]) |
| Event Organiser | Own events and their status, events of their client organisation (view-only, [#81](https://github.com/SinYang13/IS212-2026/discussions/81)), clarification requests, confirmed arrangements |
| Attendee | Registered events and registration status, plus events open for registration ([#10](https://github.com/SinYang13/IS212-2026/discussions/10)) |

Dashboard content is constrained by [[access-control]]: visibility depends on role **and** relationship to the event, and Attendees must not see internal planning information (Source: customer-brief-no-1, s8b).

## Open Items

| Item | Status | Reference |
|---|---|---|
| Venue usage report measures and presentation | **Unspecified — team to propose.** Revenue explicitly excluded | [#29](https://github.com/SinYang13/IS212-2026/discussions/29) |
| PDPA / privacy constraints on attendee export | **Asked, not answered** | [#32](https://github.com/SinYang13/IS212-2026/discussions/32) |
| Export formats | Not specified ("common formats") | brief, s4 |
| Attendee registration fields (bounds report content) | **Team to propose** | [#22](https://github.com/SinYang13/IS212-2026/discussions/22) |
| Field-level permissions on protected information | **Team to propose** | [#91](https://github.com/SinYang13/IS212-2026/discussions/91) |
| Audit mechanism behind activity/change history | Team to propose | [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |
| Dashboard composition per role | Not enumerated by the customer | brief, s4 |

Consolidated in [[open-questions]].
