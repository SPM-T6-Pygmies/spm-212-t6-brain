---
title: "Source: IS212 Discussions — 2026-10-01 snapshot"
type: source-summary
sources:
  - "IS212 2026 Discussions Oct 1.md"
tags: [source, clarifications, q-and-a, github-discussions, is212, authoritative]
---

# Source: IS212 Discussions — 2026-10-01 snapshot

Current authoritative extract of the [[is212-g9-section]] GitHub Discussions board — the only sanctioned channel for clarifying [[customer-brief-no-1]]. Supersedes [[is212-discussions-2026-09-16]]; itself followed by [[is212-discussions-2026-10-04-addendum]].

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Extracted | 2026-10-01 07:04 UTC |
| Volume | 118 discussions, 11 categories |
| Answer state | 116 answered · **1 unanswered** · 26 labelled `important` |
| Relationship | Strict superset of [[is212-discussions-2026-09-16]] |

## The delta, verified

Both snapshots were split per-discussion and compared block by block. The ingestion surface is **3 items**, not 118.

| Change | Count | Which |
|---|---|---|
| New discussions | 3 | [#124](https://github.com/SinYang13/IS212-2026/discussions/124), [#125](https://github.com/SinYang13/IS212-2026/discussions/125) (answered 2026-09-28), [#126](https://github.com/SinYang13/IS212-2026/discussions/126) (**unanswered**) |
| Existing discussions with changed text | 0 | — |
| Removed | 0 | — |

`important` stays at 26: none of the three is labelled. The export postdates [[week-7-customer-changes]] but answers nothing about it — no discussion mentions the Event Coordinator Lead, the Safety Officer, expiring holds or multi-venue events.

## What the three say

### [#124](https://github.com/SinYang13/IS212-2026/discussions/124) — must an Event Organiser account be tied to an organisation?

Asked whether every Event Organiser account is linked to a client organisation or may be unlinked. The answer, by `josephsungls` on 2026-09-28, is "Please refer to project brief." The brief states that Event Organisers represent a client ([[connectsphere-event-services|brief §2]]) and that a user's organisation is a profile field ([[event-planning-venue-booking-system|brief §4]]), so **nothing new**: an Organiser account carries a [[client-organisation]] link, as the wiki already records. It is a non-answer rather than a confirmation of "exactly one" — the cardinality is unchanged from [[client-organisation]].

### [#125](https://github.com/SinYang13/IS212-2026/discussions/125) — who may see an approval or rejection outcome?

Asked whether only the assigned Coordinator sees the approve/reject page and its outcome, or any non-Attendee role. The customer's answer:

- The outcome should be visible to users with a **legitimate relationship to the event** who **need it for their role** — "such as the responsible Event Organiser and relevant authorised internal users".
- It should **not** automatically be visible to every non-Attendee role simply because of the role.

**Effect on the wiki.** This is a relationship-and-need rule, not a role list, and it matches [[access-control]]'s existing per-event resolution. It closes the question the SPM-34 build raised (who sees a decided request) without naming roles. It also constrains the two roles Week 7 adds: the Lead's oversight of assignments (C5) and the Safety Officer's view (C6) must each rest on a legitimate relationship to the event, not on the role alone. See [[access-control]] and [[event-request]].

### [#126](https://github.com/SinYang13/IS212-2026/discussions/126) — what happens to a booking made between slots? *(unanswered)*

A team member asks: if an Event Organiser submits a booking between 12nn and 1pm, must the Coordinator book both the AM and PM slots, or should the Organiser not be allowed to book inside that gap at all?

This is the only unanswered discussion on the board. It is the **12nn–1pm gap** recorded on [[venue]] since [#50](https://github.com/SinYang13/IS212-2026/discussions/50), now with a live consequence: [[week-7-customer-changes]] C1's worked example ends at 12:45pm, inside the gap. **Update:** the customer's later post [#128](https://github.com/SinYang13/IS212-2026/discussions/128) replaces the three slots with a 15-minute smallest unit of booking, so there is probably no slot gap left to book across and this question is likely moot. It is still unanswered on the board. See [[is212-discussions-2026-10-04-addendum]], [[venue-booking]] and [[open-questions]].

## Everything else

No other discussion changed. Nothing in the delta contradicts a wiki page.

## Still open

The board reported **1 unanswered**: [#126](https://github.com/SinYang13/IS212-2026/discussions/126). Three discussions have been posted or answered since: [#127](https://github.com/SinYang13/IS212-2026/discussions/127)–[#129](https://github.com/SinYang13/IS212-2026/discussions/129), summarised in [[is212-discussions-2026-10-04-addendum]]. Of the ten questions Week 7 raised, [#128](https://github.com/SinYang13/IS212-2026/discussions/128) answered Q1, Q2 and most of Q9; the rest are in [[open-questions]] and [[week-7-change-impact-review]].
