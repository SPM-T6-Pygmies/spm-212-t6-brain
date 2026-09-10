---
title: "Source: IS212 Discussions — 2026-09-08 snapshot"
type: source-summary
sources:
  - "IS212 2026 Discussions Sept 8.md"
tags: [source, clarifications, q-and-a, github-discussions, is212, authoritative]
---

# Source: IS212 Discussions — 2026-09-08 snapshot

Current authoritative extract of the [[is212-g9-section]] GitHub Discussions board — the only sanctioned channel for clarifying [[customer-brief-no-1]]. Supersedes [[is212-discussions-2026-08-31]].

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Extracted | 2026-09-08 05:00 UTC |
| Volume | 113 discussions, 11 categories |
| Answer state | 112 answered · **0 unanswered** · 26 labelled `important` |
| Relationship | Strict superset of [[is212-discussions-2026-08-31]] |

## The delta, verified

Both snapshots were split per-discussion and compared block-by-block. The ingestion surface is **28 items**, not 113.

| Change | Count | Which |
|---|---|---|
| New discussions | 26 | [#96](https://github.com/SinYang13/IS212-2026/discussions/96)–[#121](https://github.com/SinYang13/IS212-2026/discussions/121), all answered 2026-09-07 |
| Existing discussions with a new follow-up answer | 2 | [#38](https://github.com/SinYang13/IS212-2026/discussions/38), [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| Status-flag flips, text unchanged | 2 | [#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#55](https://github.com/SinYang13/IS212-2026/discussions/55) — re-opened → answered |
| Removed or edited | 0 | — |

The two flag flips need no action: [[is212-discussions-2026-08-31]] already recorded both as resolved on 2026-08-31 and said the board's flag had not caught up. It has now.

Nearly the whole batch was answered on a single day (2026-09-07) by `josephsungls`, and most of the new questions come from one asker working through the brief systematically. Three carry team tags — [#98](https://github.com/SinYang13/IS212-2026/discussions/98) `[T2]`, [#99](https://github.com/SinYang13/IS212-2026/discussions/99) `[T4]`, and [#119](https://github.com/SinYang13/IS212-2026/discussions/119)–[#121](https://github.com/SinYang13/IS212-2026/discussions/121) `[T1]`.

> [!info] Nothing in this batch is labelled `important`
> The `important` count is unchanged at 26, all of them pre-existing. The customer did not flag any of the 26 new answers as such — but four of them close questions this wiki had standing open, so the label is not a reliable guide to impact here. See [[discussions-sept-8-critical-diffs]] for the ones that actually move.

## What changed the wiki's mind

Four answers contradict or close something a page previously asserted. Full treatment in [[discussions-sept-8-critical-diffs]]; summarised here.

### The Event Operations Manager is a real role — [#101](https://github.com/SinYang13/IS212-2026/discussions/101)

The largest correction in the batch. The EOM **must be implemented as a separate system role**, with only authorised users able to perform coordinator assignment. [[user-roles]] carried an entire *"Gap"* section arguing the role was a briefing omission the team had to resolve; [[user-account]] carried a matching warning. Both were right about the brief and are now overtaken by the customer. Detailed access-control design remains the team's. See [[user-roles]], [[user-account]], [[access-control]].

### Venue capacity varies by room layout — [#112](https://github.com/SinYang13/IS212-2026/discussions/112)

Capacity is **not one number per venue**. Each supported layout carries its own capacity, supplied as part of the venue attributes — the system does not estimate or compute it — and suitability must evaluate against the layout-specific figure. [[venue]] had this as an explicit `🔶 Team decision` ("whether layout is a first-class object… possibly capacity-per-layout"); the customer has answered it. See [[venue]], [[venue-suitability]].

### A withdrawn request state is now grounded — [#103](https://github.com/SinYang13/IS212-2026/discussions/103)

An Event Organiser may ask — **outside the system** — for a submitted request to be withdrawn; the Event Coordinator performs and records the withdrawal, and the withdrawn request must stay distinguishable from draft, submitted, approved and rejected. [[event-request]] and [[schema]] had both explicitly *removed* a request-level `Withdrawn` as "grounded in no source". It is grounded now. Note the Organiser does not self-withdraw. See [[event-request]], [[schema]].

### The slot is the atomic unit of setup and turnaround — [#108](https://github.com/SinYang13/IS212-2026/discussions/108)

One slot is the smallest unit for setup and turnaround time; Release 1 need not support sub-slot durations. [[venue-booking]] had flagged the interaction between per-venue turnaround and the fixed three-slot model as an unresolved team question. Resolved. Paired with [#107](https://github.com/SinYang13/IS212-2026/discussions/107): an event spanning two slots consumes **both in full**. See [[venue-booking]], [[venue]].

## Release 1 scope, sharpened

Five answers speak directly to what must ship by Week 12, and all five narrow rather than widen it — consistent with [[week-4-customer-briefing]].

| Answer | Effect on scope |
|---|---|
| [#118](https://github.com/SinYang13/IS212-2026/discussions/118) | A role-based dashboard is **not** a mandatory Release 1 core feature. [[release-1-scope]] already had this as `📋 Backlog` inferred from the core-20's silence; it is now stated outright |
| [#96](https://github.com/SinYang13/IS212-2026/discussions/96) | Activity History and Change History are **not standalone core features**; no complete audit-history function is required. The auditability NFR is untouched |
| [#100](https://github.com/SinYang13/IS212-2026/discussions/100) | Event categories are not a core feature; the brief's list (conference, workshop, training session, exhibition, meeting, seminar, networking event) is **not exhaustive** and a reasonable subset suffices |
| [#98](https://github.com/SinYang13/IS212-2026/discussions/98) | Event Confirmation need not be a standalone feature, **but** the Release 1 workflow must let a Coordinator confirm an event once its essential arrangements are done, reaching a `Confirmed` status visible to relevant users |
| [#109](https://github.com/SinYang13/IS212-2026/discussions/109) | Holding **multiple venues on tentative hold simultaneously** is not required for Release 1 — consistent with tentative holding already sitting in `📋 Backlog` |

[#115](https://github.com/SinYang13/IS212-2026/discussions/115) adds a sixth: detailed handling of registration withdrawal *arising from event changes or cancellation* is not required for Release 1.

## Onboarding closed for staff and clients — [#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up

The original [#49](https://github.com/SinYang13/IS212-2026/discussions/49) answer ("not in the list of features as yet") left provisioning ambiguous. The follow-up settles it per user class:

| User class | Release 1 position |
|---|---|
| ConnectSphere employees | No self-registration or onboarding feature. **Pre-loaded staff accounts** with appropriate roles and access controls |
| Event Organisers | Same — no onboarding. **Pre-loaded client accounts** may be used for testing |
| Attendees | Attendee Registration remains a required Release 1 feature, but it means **registering for an event, not creating an account**. Attendees must be able to view appropriate event information, register, view their registration status, and withdraw |

> [!warning] This does not close the guest-attendee identity gap
> The answer offers pre-loaded accounts for staff and clients only. An account-less attendee still holds a `registration_reference` and no credential, so *view registration status* and *withdraw* — both named as required above — still have no delivery mechanism. See [[release-1-scope]] and [[access-control]].

## Everything else, by area

### Events, requests and changes

- **Post-submission editing is prohibited** — the Organiser may not edit a submitted request, *including non-significant fields*. All changes route through the Event Coordinator, who processes them in the system. The significant/non-significant distinction survives, but now governs only whether arrangements need revalidation, not who may edit ([#102](https://github.com/SinYang13/IS212-2026/discussions/102)). See [[event-request-workflow]], [[change-impact-management]].
- **Change requests are multi-field and partially approvable** — one request may cover several fields, and some may be accepted while others are not, with reasons communicated back to the Organiser ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)). See [[change-request]].
- **Un-confirming a confirmed event must reach the Organiser** — through the system or another appropriate channel ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)). See [[notifications]], [[event-lifecycle]].
- **Essentialness is decided jointly** — the Event Coordinator determines *together with* the relevant Technical Support Staff whether an incomplete equipment requirement is essential. If essential, confirmation is blocked until resolved ([#106](https://github.com/SinYang13/IS212-2026/discussions/106)). See [[event-readiness]], [[technical-support]].
- **Expected attendance stays event-level** — a separate event-level requirement, not duplicated inside Venue Requirements, but still used for suitability, capacity and booking decisions ([#119](https://github.com/SinYang13/IS212-2026/discussions/119)). See [[event]], [[venue-suitability]].
- **Postponement without a new date is confirmed** — an event may be postponed before a replacement date is known, upgrading [#69](https://github.com/SinYang13/IS212-2026/discussions/69)'s reading from inference to stated ([#121](https://github.com/SinYang13/IS212-2026/discussions/121)). See [[event-lifecycle]].
- **Significant actions, enumerated** — where the team chooses to keep history: submission, approval/rejection, coordinator assignment/reassignment, important information changes, venue booking decisions, equipment reservation changes, confirmation, rescheduling, cancellation. The exact set stays the team's ([#96](https://github.com/SinYang13/IS212-2026/discussions/96)). See [[audit-record]].

### Venues

- An event across two slots consumes **both slots in full** ([#107](https://github.com/SinYang13/IS212-2026/discussions/107)).
- Venue Staff may **suggest** an amended or alternative arrangement, but the Coordinator reviews and updates the booking request before approval — confirming [#45](https://github.com/SinYang13/IS212-2026/discussions/45) that this is not a formal in-system counter-offer ([#110](https://github.com/SinYang13/IS212-2026/discussions/110)).
- The Organiser may **express venue preferences**, possibly outside the system. The Coordinator identifies suitable options. An in-system preferred venue is permitted but must not be treated as a booking or override suitability and approval ([#111](https://github.com/SinYang13/IS212-2026/discussions/111)).
- A venue attribute update **does not auto-revert** existing bookings to `Requested`. Affected bookings are identified for review and the Coordinator notified; the status transition is the team's to propose ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)). See [[booking]], [[change-impact-management]].

### Equipment

- **No fixed default return day.** How the return date is captured — auto-populated or manual — is the team's ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)). This sits underneath the committed Return Day + 1 availability rule, so a settled rule now rests on an unsettled input. See [[equipment-reservation]].
- Equipment requirements **may be changed after reservation**. Affected arrangements are reviewed and updated, and Technical Support Staff re-check suitability and availability; the release/replace/add workflow is the team's ([#114](https://github.com/SinYang13/IS212-2026/discussions/114)). See [[equipment-management]].

### Registration and attendees

- **VIPs may be added when normal registration is full — but capacity is a hard cap.** "The overall event/venue capacity must still not be exceeded" ([#99](https://github.com/SinYang13/IS212-2026/discussions/99)). This gives a customer citation to a rule the wiki had previously reached by implementation inference. It does **not** disturb [#33](https://github.com/SinYang13/IS212-2026/discussions/33)'s routing, under which the VIP list goes to the Coordinator rather than being added by the Organiser directly.
- **Registrations survive a reschedule.** Attendees are informed of the new date/time and allowed to withdraw ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)).
- **No waiting-list cut-off before the event date** is mandated; the 3-day acceptance window still applies once an offer is made. A cut-off rule may be proposed ([#116](https://github.com/SinYang13/IS212-2026/discussions/116)).
- **Check-in has an actor at last.** [#38](https://github.com/SinYang13/IS212-2026/discussions/38)'s follow-up: check-in is performed by an authorised internal staff member already involved in event operations. **No new registration-only role is required**; the team assigns the responsibility to an existing internal role, subject to the access-control model.
- Attendees should be able to reach appropriate event information through the system, but are **not required to use the app** as their only source — updates may also travel through other channels ([#97](https://github.com/SinYang13/IS212-2026/discussions/97)).

### Notifications

- The Event Operations Manager **need not be notified of every submission**, but must be able to *identify* newly submitted events awaiting coordinator assignment ([#117](https://github.com/SinYang13/IS212-2026/discussions/117)). Note this is a worklist or queue requirement, not a notification one — the mechanism is the team's. See [[notifications]].

## Still open

The board now reports **0 unanswered**, so there is no outstanding question on the platform itself. That is not the same as the requirements being settled: this batch closed four standing wiki gaps and opened or preserved several others — most consequentially the guest-attendee credential problem above, the equipment return-day input beneath a fixed availability rule ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)), and the booking status transition after a venue change ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)). [[open-questions]] holds the consolidated register.
