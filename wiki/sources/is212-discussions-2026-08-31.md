---
title: "Source: IS212 Discussions — 2026-08-31 snapshot"
type: source-summary
sources:
  - "IS212 2026 Discussions Aug 31.md"
tags: [source, clarifications, q-and-a, github-discussions, is212, authoritative]
---

# Source: IS212 Discussions — 2026-08-31 snapshot

Current authoritative extract of the [[is212-g9-section]] GitHub Discussions board — the only sanctioned channel for clarifying [[customer-brief-no-1]].

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Extracted | 2026-08-31 13:18 UTC |
| Volume | 87 discussions, 11 categories |
| Answer state | 84 answered · 2 unanswered · 26 labelled `important` |
| Relationship | Strict superset of [[is212-discussions-2026-08-26]] |

**Delta since the Aug 26 snapshot:** +12 discussions, +15 answers, +8 `important` labels. Four of the five previously open questions were resolved, and one was re-opened by a follow-up and then answered.

## What is new in this snapshot

### Approval vs confirmation — #80

The most consequential clarification. Approval means the request contains **sufficient information for planning to proceed**; it commits ConnectSphere to nothing. Confirmation requires that all arrangements deemed essential *for that particular event* are complete — and there is **no fixed list**, since some events need no equipment, technical support, or registration. The system **should prevent confirmation** while a required arrangement is incomplete. See [[event-lifecycle]] and [[event-readiness]].

### Coordinator assignment mechanics — #92–#95

A cluster of T6 questions settled the assignment model: the Event Operations Manager both assigns and reassigns, using an SOP held outside the system; there is no acceptance step and coordinators cannot decline; coordinators may hold concurrent events; the system may assume any reassignment entered has already been approved. See [[event-request-workflow]] and [[user-roles]].

### Equipment availability — #2 follow-up

A contradiction raised by the team between equipment transfer time (#2) and the Return Day + 1 rule (#5) was resolved: **both apply, at different lifecycle points.** Transfer and preparation time governs pre-event availability; Return Day + 1 governs post-event availability. See [[equipment-management]].

### Client-organisation visibility — #81

An Event Organiser may view, edit and update events they are responsible for. Other Organisers in the same client organisation may **view but not edit**. Access to unrelated clients' events is prohibited. See [[access-control]].

### Search vs suitability — #83

Filtering narrows the candidate list by date, time, capacity, accessibility, layout and facilities. Suitability checking evaluates whether a *selected* venue satisfies current requirements. They are distinct functions. See [[venue-suitability]].

### Technical support operations — #85–#90

Six T6 questions on Technical Support Staff: partial reservation is allowed when only some requested items are available, with unavailable items highlighted (#90); clarifications route through the Event Coordinator as liaison (#87); defects on return are logged and the equipment status updated (#86); the post-event health checklist stays **outside** the system (#85); no staff self-declaration of availability (#89); no fixed roster frequency and no mandated assignment mechanism (#88). See [[technical-support]].

### Integration and system of record — #79

No external integration is in scope. The proposed system is the **authoritative source** for the event information it manages. Email notification is a notification-channel requirement, not a general integration requirement. See [[non-functional-requirements]].

### Notification delivery — #55 follow-up

Batch means notifications need not be delivered in real time; they may be grouped at configurable intervals. Real-time delivery is acceptable if technically simpler. There is **no hard delivery-time requirement**. See [[notifications]].

### Protected information — #91

Covers internal planning information, venue and equipment arrangements, attendee information, client-related information, and other non-public event details. Field-level permission design is left to the team. See [[access-control]].

### Equipment reservation period — #82

No maximum reservation period. See [[equipment-management]].

## Still open

Both remaining "unanswered" items are artefacts of the board's status flag: each was **re-opened by a follow-up**, and each received a resolving reply from `SinYang13` on 2026-08-31 that the flag has not caught up with. Neither should be treated as an actual gap.

| Ref | Category | Question | State |
|---|---|---|---|
| #2 | Equipment & Technical Support | Equipment physical location and transfer turnaround | Resolved in the follow-up thread — see [[equipment-management]] |
| #55 | Notifications | Whether notifications are real-time or batch | Resolved in the follow-up thread — see [[notifications]] |

## Scope caveat

Answer #79 defers the Release 1 scope to a separate confirmation ("please refer to the final Release 1 scope to be confirmed on Friday"). Consistent with [[customer-brief-no-1]], **scope is not yet fixed**. See [[open-questions]].
