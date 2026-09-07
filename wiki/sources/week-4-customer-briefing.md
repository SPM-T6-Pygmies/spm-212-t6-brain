---
title: "Source: Week 4 Customer Briefing"
type: source-summary
sources:
  - "Week 4 - Customer Briefing.md"
tags: [source, requirements, scope, release-1, customer-brief, connectsphere, is212]
---

# Source: Week 4 Customer Briefing

The scope-fixing document. Names **20 core features** as the required content of the first release ([[event-planning-venue-booking-system]]), due Week 12. It adds almost no new business rules — its job is to draw a line through the ~60 functionality areas of [[customer-brief-no-1]].

## Metadata

| Field | Value |
|---|---|
| Type | Customer briefing (mock) — release scope statement |
| Course | IS212, AY 2026/27 Term 1 |
| Status | ✅ Authoritative on scope |
| Supersedes | Nothing. It *closes* [#79](https://github.com/SinYang13/IS212-2026/discussions/79) and [#11](https://github.com/SinYang13/IS212-2026/discussions/11), which deferred scope to Week 4 |
| Depends on | [[customer-brief-no-1]] — the briefing calls it "the Week 1 Customer Briefing" and selects from its functionality list |
| Clarified by | — |

> [!info] "Week 1 Customer Briefing" means `Customer Brief no. 1.md`
> This document's only reference to prior material is "the functionality described in the Week 1 Customer Briefing". That is the same document this wiki summarises as [[customer-brief-no-1]]; there is no separate Week 1 file in `raw/`. The two names refer to one source.

## What it settles

| Question | Previously | Now |
|---|---|---|
| Which features are in Release 1? | Deferred to Week 4 ([#79](https://github.com/SinYang13/IS212-2026/discussions/79), [#11](https://github.com/SinYang13/IS212-2026/discussions/11)) and by [[customer-brief-no-1]] itself | **Fixed** — the 20 below. See [[release-1-scope]] |
| What happens to the other ~40 areas? | Unknown | Kept for "your product backlog and C4 models" — deferred, not cancelled |
| Are the 20 independent deliverables? | n/a | **No** — "should not be treated as isolated functionality"; the release must support coherent end-to-end workflows across them |
| Which information does an event request carry? | Field set "not firmed out" ([#72](https://github.com/SinYang13/IS212-2026/discussions/72)) | Nine items enumerated (see below). Mandatory-vs-optional still open |

## The core 20

Verbatim feature names, with the wiki pages that own each. See [[release-1-scope]] for the full in/adjacent/out register including everything *not* on this list.

| # | Feature | What the briefing adds beyond [[customer-brief-no-1]] | Pages |
|---|---|---|---|
| 1 | User Authorisation and Authentication | Re-enumerates exactly the **five** briefed roles; access depends on role **and relationship to an event** | [[access-control]], [[user-roles]], [[user-account]] |
| 2 | Event Request Creation | Enumerates the request's information set for the first time | [[event-request-workflow]], [[event-request]] |
| 3 | Draft Event Requests | Drafts must "remain distinguishable from requests already submitted for review" | [[event-request-workflow]], [[event-request]] |
| 4 | Event Review and Approval | Review outcome must be "visible to the relevant users"; clarification/amendment loop restated | [[event-request-workflow]], [[event-lifecycle]] |
| 5 | Coordinator Assignment | Assignment happens to **submitted** events; reassignment on staff change. No actor named | [[event-request-workflow]], [[user-account]] |
| 6 | Event Status Management | Nine example statuses — one fewer than the Week 1 list | [[event-lifecycle]], [[event]] |
| 7 | Event Information Management | Restates the ordinary-edit vs important-change split | [[change-impact-management]], [[event]] |
| 8 | Venue Catalogue | Attribute list: location, capacity, facilities, accessibility, supported room layouts, operating information | [[venue-booking]], [[venue]] |
| 9 | Venue Availability Calendar | Existing bookings **and other recorded unavailability** must both be reflected | [[venue-booking]], [[venue]] |
| 10 | Venue Search and Filtering | Filter criteria: date, time, expected attendance, location, capacity, accessibility, layout, facilities | [[venue-suitability]] |
| 11 | Venue Suitability Checking | Repeats the Week 1 hedge word "normally" on the capacity rule | [[venue-suitability]] |
| 12 | Venue Booking Request | Request must carry the timing and venue requirements Venue Staff need to assess it | [[venue-booking]], [[booking]] |
| 13 | Venue Booking Approval | Rejection must carry "relevant information or a reason" so planning can continue | [[venue-booking]], [[booking]] |
| 14 | Booking Conflict Detection | A confirmed booking must affect that venue's availability for other events in the period | [[venue-booking]], [[booking]] |
| 15 | Equipment Request Management | Request carries **type, quantity** and technical requirements; Technical Support Staff update it | [[equipment-management]], [[equipment-reservation]] |
| 16 | Equipment Availability Checking | Equipment committed to an incompatible event, or recorded unavailable, is not freely available | [[equipment-management]], [[equipment-item]] |
| 17 | Equipment Reservation | A reservation **reduces the quantity available** to overlapping events and stays bound to its event | [[equipment-management]], [[equipment-reservation]] |
| 18 | Attendee Registration | Four Attendee verbs only: view, register, view status, withdraw. Plus organiser/coordinator visibility | [[attendee-registration]], [[registration]] |
| 19 | Event Change Requests | Changes may be requested **after submission**, not only after approval | [[change-impact-management]], [[change-request]] |
| 20 | Notification System | Ten trigger scenarios named, all drawn from existing wiki triggers | [[notifications]], [[notification]] |

## The adjacency clause

> [!quote] Customer wording — Week 4 briefing
> "These core features should not be treated as isolated functionality. Your software release should support coherent end-to-end event workflows across the relevant features… Changes to an event may also affect bookings, equipment, attendee registration, or notifications. Your design, implementation, and tests should reflect these relationships where they are relevant to the customer requirements."

This clause is why [[release-1-scope]] carries a third marker (`🧩 Adjacent`) rather than a binary in/out split. A feature absent from the 20 is still required if a listed feature cannot work end-to-end without it — the equipment catalogue, for example, is never named but features 15–17 are unbuildable without it.

## Findings

### The request field set is now enumerated

Feature 2 names nine information items: **event name, purpose, description, proposed date and time, expected attendance, venue requirements, accessibility needs, equipment requirements, and registration needs where relevant.** [#72](https://github.com/SinYang13/IS212-2026/discussions/72) said the fields were "not firmed out"; they now are, at the level of *what is captured*. What is still not stated is which are **mandatory for submission** — so [#72](https://github.com/SinYang13/IS212-2026/discussions/72) narrows but does not close. See [[event-request]].

Not carried over from [[customer-brief-no-1]]'s wider list: event category, programme/agenda, room-layout preference as a distinct field, technical-support flag, other special arrangements.

### One status disappeared

[[customer-brief-no-1]] listed ten example statuses; this briefing lists nine. The one dropped is **awaiting arrangements**. Both lists are explicitly examples ("Examples may include…"), so this is not a contradiction — but it removes the only source mention of the state [[event]] models between *planning* and *confirmed* — now carried under the team's own name, `Blocked`. See [[event-lifecycle]], [[schema]].

### Confirmation arrives without a feature

**Event Readiness Tracking**, **Outstanding Action Tracking** and **Event Confirmation** are all absent from the 20 — yet `confirmed` is a listed status (feature 6) and "confirmation" is a listed notification trigger (feature 20). The [#80](https://github.com/SinYang13/IS212-2026/discussions/80) rule that confirmation is **blocked** while an essential arrangement is incomplete is one of the customer's committed hard rules, and it now has to be delivered as a consequence of feature 6's "status changes should be consistent with the actions performed on the event" rather than as a feature in its own right. See [[event-readiness]].

### Registration is much thinner than the specified model

Feature 18 grants Attendees four verbs: view event information, register, view registration status, withdraw. Absent: **waiting list** (and its committed 3-day acceptance window, [#44](https://github.com/SinYang13/IS212-2026/discussions/44)), **registration capacity**, **registration period**, **attendance recording / check-in**. Combined with external-user onboarding already being out of scope ([#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#53](https://github.com/SinYang13/IS212-2026/discussions/53)), Release 1 requires Attendees to log in and register while specifying no way for an Attendee account to come into existence. See [[attendee-registration]], [[access-control]].

### Equipment is described entirely in quantities

Features 15, 16 and 17 speak only of type, quantity, sufficiency and "reduce the quantity considered available". Nothing in the core 20 requires per-unit identity: the two features that would have driven it — per-item physical location ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)) and the maintenance workflow ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) — are out of Release 1 and out of scope respectively. This is evidence bearing on the pooled-vs-serialised deferral ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)), not a decision: the customer still leaves the model to the team. See [[equipment-item]].

### Two wordings are looser than the answers they restate

| Briefing wording | The binding rule | Reading |
|---|---|---|
| "help prevent inappropriate double-booking" (feature 14) | "Double-booking must be blocked outright, not warned" ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)) | [#35](https://github.com/SinYang13/IS212-2026/discussions/35) governs — a scope statement naming a feature area does not re-specify the rule inside it |
| "should not **normally** be treated as suitable when expected attendance exceeds capacity" (feature 11) | Same hedge in [[customer-brief-no-1]]; never tightened | The hedge has now survived two documents, so it is deliberate. Block-vs-warn remains a team decision |

### The Event Operations Manager gap hardens

Feature 1 enumerates the five roles the system authorises. Feature 5 says "submitted events **can be assigned** to an Event Coordinator" — passive, no actor. The Event Operations Manager who performs assignment in [#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93) and [#94](https://github.com/SinYang13/IS212-2026/discussions/94) is therefore, in Release 1, an actor with a required action and no authenticable identity. See [[user-roles]].

## What it does not touch

Non-functional requirements. The briefing is silent on performance, security, usability, reliability, scalability, auditability and maintainability, so [[customer-brief-no-1]] §7–8 continue to bind unchanged across every feature in the release. See [[non-functional-requirements]].

## Related pages

[[release-1-scope]] · [[event-planning-venue-booking-system]] · [[open-questions]] · [[customer-brief-no-1]] · [[is212-discussions-2026-08-31]] · [[domain-model]]
