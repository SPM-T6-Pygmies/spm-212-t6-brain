---
title: Change Impact Management
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Sept 8.md
tags:
  - change-management
  - revalidation
  - rescheduling
  - audit
  - connectsphere
---

# Change Impact Management

**Objects:** [[change-request]] · [[event]] · [[booking]] · [[equipment-reservation]] · [[audit-record]]

How the system detects that a change to an event may invalidate arrangements already made, and drives those arrangements back through review. This is the customer's central stated motivation for the system: "Such changes can affect arrangements that have already been made… ConnectSphere therefore expects the proposed system to help users identify and manage these relationships." ([[connectsphere-event-services|brief §2]]).

**Release 1 scope:** Per [[release-1-scope]], event change requests and impact-of-change revalidation are 🎯 Release 1 (core feature 19); rescheduling is 🧩 Adjacent, pulled in by feature 19 naming date/time as a significant change category. Change history and activity history (see below) are 📋 Backlog as browsable surfaces, though the underlying what/who/when record stays 🎯 via the auditability NFR — a reading the customer has since confirmed directly ("not standalone core features, so there is no requirement to implement a complete audit-history function", [#96](https://github.com/SinYang13/IS212-2026/discussions/96)).

## Significant vs Ordinary Edits

The system must distinguish between ordinary edits and changes that may affect arrangements already confirmed (Source: customer-brief-no-1).

> [!quote] Customer answer — [#4](https://github.com/SinYang13/IS212-2026/discussions/4)
> **Changes that may affect existing arrangements should be treated as significant.** Examples include changes to date/time, attendance, venue requirements, equipment requirements, or other changes that could invalidate confirmed arrangements. Ordinary edits that do not affect existing arrangements need not trigger revalidation. The exact implementation is for you to propose.
> (Source: is212-discussions-2026-08-31, [#4](https://github.com/SinYang13/IS212-2026/discussions/4))

| Change | Class | Why |
|---|---|---|
| Date or time | **Significant** | May create a venue or equipment conflict; affects turnaround with adjacent bookings |
| Expected attendance | **Significant** | May exceed venue capacity — 80 to 150 people may make the selected venue unsuitable |
| Venue requirements (layout, accessibility, facilities) | **Significant** | Booked venue may no longer satisfy them |
| Equipment requirements | **Significant** | Additional or changed items may not be reservable |
| Programme extended, shortened, or split into sessions | **Significant** | Affects timing, turnaround, and possibly per-session venue/equipment |
| Technical support requirements | **Significant** | May change staff assignment needs |
| Event name, description, purpose, internal notes | Ordinary | Does not touch arrangements |
| Contact details, comments, attached documents | Ordinary | Does not touch arrangements |

([[connectsphere-event-services|brief §2]] and [[event-planning-venue-booking-system|brief §4]]; is212-discussions-2026-08-31, [#4](https://github.com/SinYang13/IS212-2026/discussions/4))

The customer's definition is **functional, not enumerative**: significance is defined by *whether the change can invalidate an existing arrangement*, and the example list is illustrative. The table above is a reading of that rule, not a customer-supplied list.

> [!info] Significance decides revalidation, never edit rights
> After submission the Event Organiser may **not** directly edit the request — including its non-significant fields. Every change, significant or not, is communicated to or requested through the Event Coordinator, who processes it in the system. The significant/non-significant distinction survives, but its only job is deciding whether existing arrangements must be reviewed again (Source: is212-discussions-2026-09-08, [#102](https://github.com/SinYang13/IS212-2026/discussions/102)). A design that lets Organisers self-serve "minor" edits because they are non-significant misreads the rule. See [[event-request-workflow]] and [[event-request]].

> [!question]- Team decision required — significance classification and revalidation
> The classification rule and the revalidation mechanism are explicitly left to the team ("The exact implementation is for you to propose", [#4](https://github.com/SinYang13/IS212-2026/discussions/4)). Open sub-decisions: whether classification is a static per-field configuration or evaluated against what is currently booked; whether revalidation is automatic or coordinator-initiated; and whether affected arrangements are invalidated outright or flagged for review while remaining held.

## One Request, Several Fields, Mixed Outcomes

A change request is not an atomic yes/no. A single request may cover several fields, and the coordinator may accept some while declining others; where a change is not accepted, the reason or follow-up is communicated to the Event Organiser. The workflow and presentation are for the team (Source: is212-discussions-2026-09-08, [#104](https://github.com/SinYang13/IS212-2026/discussions/104)).

| Consequence | Detail |
|---|---|
| Outcomes are per field | The decision, and the reason where a field's change is declined, attach to the individual requested change, not to the request as a whole |
| Revalidation is scoped to accepted changes | Only the **accepted, significant** items drive arrangements back through review; a declined date change leaves the venue booking untouched |
| Mixed requests are the normal case | A request can hold significant and ordinary items together, so classification runs per field rather than per request |
| Communicating declines is mandatory | "Where a change is not accepted, the relevant reason or follow-up should be communicated to the Event Organiser" ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)) — silence is not an acceptable outcome |

Structural detail lives on [[change-request]].

## What Revalidation Means

When a significant change occurs, the affected arrangements must be **reviewed again** by their owning role rather than silently carried over.

| Arrangement | Revalidated by | Check |
|---|---|---|
| Venue booking | Venue Staff | Availability at new date/time, capacity vs new attendance, supported layout, accessibility, facilities, setup and turnaround windows — see [[venue-suitability]]. A booking is **flagged for review, not auto-reverted** ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)) |
| Equipment reservation | Technical Support Staff | Availability at the new dates, including transfer time from the equipment's current location and the D-1 / Return Day +1 commitment window — see [[equipment-management]]. Requirements **may** be changed after reservation; affected arrangements are reviewed and updated and suitability/availability re-checked ([#114](https://github.com/SinYang13/IS212-2026/discussions/114)) |
| Technical support assignment | Technical Support Staff | Staff availability at the new date/time — see [[technical-support]] |
| Registration | Event Coordinator | Capacity limit vs new attendance; registration period vs new event date; notifying registered attendees — see [[attendee-registration]]. Existing registrations **may be retained** across a reschedule ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)) |
| Programme | Coordinator / Organiser | Session timings still fit the venue slots |

Revalidation outcomes feed back into [[event-readiness]] — an invalidated essential arrangement makes the event non-confirmable again.

## Rescheduling

Events can be rescheduled where necessary; existing venue, equipment, technical-support, and attendee arrangements may need to be reconsidered or updated (Source: customer-brief-no-1).

| Aspect | Rule | Ref |
|---|---|---|
| Equipment on reschedule | **No auto carry-over guaranteed.** A reschedule may affect equipment availability, so the equipment arrangement needs to be reviewed again. The workflow, status changes, and UX are for the team to propose | [#16](https://github.com/SinYang13/IS212-2026/discussions/16) |
| Rescheduling via calendar drag-and-drop | Permitted as a UI choice, but "any rescheduling must still follow the relevant event-change process and should not bypass required checks or approvals" | [#3](https://github.com/SinYang13/IS212-2026/discussions/3) |
| Postponement with a new date | Treated as a rescheduling request; affected arrangements reviewed | [#69](https://github.com/SinYang13/IS212-2026/discussions/69) |
| Postponement with no known date | Release current bookings, retain the event record until a new date is provided. Permitted even though no new date is known at the time of postponement | [#69](https://github.com/SinYang13/IS212-2026/discussions/69), [#121](https://github.com/SinYang13/IS212-2026/discussions/121) |
| Registrations on reschedule | Existing registrations **may be retained**. Affected attendees are informed of the new date/time — through this system or another appropriate channel — and must be allowed to withdraw if they can no longer attend. For Release 1, detailed handling of registration withdrawal arising specifically from event changes or cancellation is **not required** | [#115](https://github.com/SinYang13/IS212-2026/discussions/115) |
| Equipment requirements changed after reservation | Permitted. Where the change affects an existing reservation, the affected equipment arrangements are reviewed and updated and Technical Support Staff re-check suitability and availability; the release/replace/add workflow is for the team | [#114](https://github.com/SinYang13/IS212-2026/discussions/114) |

(Source: is212-discussions-2026-08-31; is212-discussions-2026-09-08)

Answer [#3](https://github.com/SinYang13/IS212-2026/discussions/3) is the key constraint for the team: **no UI path may become a back door around revalidation.** Whatever the interaction — form edit, calendar drag, bulk update — it funnels into the same change-impact process.

Rescheduling also interacts with venue slotting — bookings are made against the AM / PM / Night slots defined on [[venue]] (Source: is212-discussions-2026-08-31, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)) — and setup/teardown/turnaround requirements are recorded **per venue** by Venue Staff (Source: is212-discussions-2026-08-31, [#43](https://github.com/SinYang13/IS212-2026/discussions/43)). A time shift can therefore break turnaround compliance with an adjacent booking even when the venue itself is free.

## No Change Cut-Off

> [!quote] Customer answer — [#8](https://github.com/SinYang13/IS212-2026/discussions/8)
> No change cut-off period is required, but some changes may occur even after confirmation and may require affected arrangements to be reviewed again.
> (Source: is212-discussions-2026-08-31, [#8](https://github.com/SinYang13/IS212-2026/discussions/8))

Consequences:

- Changes remain possible **right up to event delivery**. The team cannot rely on a freeze window to simplify the model.
- The change-impact path must work for confirmed events, not only events in planning.
- Late changes must still reach affected staff and attendees — the brief names "Last-minute changes may not reach all affected staff or attendees" as a current pain point ([[connectsphere-event-services|brief §2]]). See [[notifications]].

> [!question]- Team decision required — lead-time guardrails
> With no customer-mandated cut-off, the team may still propose lead-time guardrails (for example, warning on changes inside a venue's turnaround window) — but as a proposal, not as a customer requirement.

## Confirmed Events Moving Back to Planning

> [!quote] Customer answer — [#70](https://github.com/SinYang13/IS212-2026/discussions/70)
> A major change may require existing arrangements to be reviewed again. You can propose if you need a transition process flow.
> (Source: is212-discussions-2026-08-31, [#70](https://github.com/SinYang13/IS212-2026/discussions/70))

The customer confirms the *need* to re-review but leaves the *state transition* undefined. One consequence is **not** left open, however: if a confirmed event is changed such that it is no longer considered confirmed, the Event Organiser must be informed, through the system or another appropriate channel, with only the mechanism left to the team (Source: is212-discussions-2026-09-08, [#105](https://github.com/SinYang13/IS212-2026/discussions/105)). Whatever representation the team picks — a status move, a pending-revalidation flag, a new state — it must be observable to the Organiser rather than internal-only.

> [!question]- Team decision required — status handling on post-confirmation change
> Options for the team: move the event back to `Planning` / `Blocked`; keep it `Confirmed` but flag affected arrangements as pending revalidation; or introduce an explicit *change under review* state (which would mean widening [[schema]]'s five-value `event_status_chk`). The decision has visible consequences for the Event Organiser and for Attendees of a confirmed event with open registration, since the brief requires that "Users should not be presented with contradictory information regarding event dates, venue bookings, equipment availability, or registration status" ([[non-functional-requirements|brief §8d]]). See [[event-lifecycle]] and [[non-functional-requirements]].

## Venue Becoming Unavailable After Confirmation

An externally-driven change rather than an Organiser-requested one — for example a venue shutdown after a ceiling collapse.

| Aspect | Rule |
|---|---|
| Who updates availability | **Venue Staff** are responsible for updating venue availability when operational circumstances change |
| Is it a notification event | **Yes** — an availability change affecting an upcoming event is a listed notification scenario |
| Who resolves it | The **Event Coordinator** makes the necessary changes |
| Automation | **Team to propose** — "please propose an appropriate automation mechanisms if required" |

(Source: is212-discussions-2026-08-31, [#54](https://github.com/SinYang13/IS212-2026/discussions/54); customer-brief-no-1, s6)

A related and easily-overreached case is an ordinary **venue attribute update** by Venue Staff. Such an update must **not** automatically revert existing bookings to `Requested`. If the change affects an existing booking's suitability or availability, that booking is identified for review and the Event Coordinator notified; the booking may then need to be reconsidered or changed, but the exact status transition is for the team (Source: is212-discussions-2026-09-08, [#120](https://github.com/SinYang13/IS212-2026/discussions/120)). This is the same shape as [#54](https://github.com/SinYang13/IS212-2026/discussions/54): detect, flag, notify the coordinator — never silently unwind an arrangement.

The equipment analogue behaves the same way: reserved equipment may become unavailable through damage or maintenance, Technical Support Staff maintain equipment status (Source: is212-discussions-2026-08-31, [#9](https://github.com/SinYang13/IS212-2026/discussions/9)), and defects found on return update the operational status of the affected item (Source: is212-discussions-2026-08-31, [#86](https://github.com/SinYang13/IS212-2026/discussions/86)).

> [!question]- Team decision required — detecting availability collisions
> Whether the system automatically detects that an availability block collides with an existing confirmed booking, and how forcefully it surfaces this (dashboard flag, notification, blocking alert, automatic arrangement invalidation), is unspecified. The brief's pain point "Cancelled events may continue to occupy venue or equipment reservations" argues for automatic detection rather than manual vigilance.

## Change History and Activity History

The brief treats these as two related but distinct capabilities, structurally defined on [[audit-record]] ([[event-planning-venue-booking-system|brief §4]] and [[non-functional-requirements|brief §8f]]).

The underlying pain point: "It is difficult to determine who changed important event information and when the change occurred." The auditability requirement is that ConnectSphere can determine **what was changed, who changed it, and when** ([[non-functional-requirements|brief §8f]]).

Neither history is a standalone Release 1 core feature, so **no complete audit-history function is required** for the release. Where the team does support history, a significant action is one that materially affects the event or its arrangements; the customer's illustrative list is submission, approval/rejection, coordinator assignment/reassignment, important event information changes, venue booking decisions, equipment reservation changes, confirmation, rescheduling and cancellation. The exact set and its presentation may be proposed by the team (Source: is212-discussions-2026-09-08, [#96](https://github.com/SinYang13/IS212-2026/discussions/96)). Note the limit of this answer: it relaxes the *browsable history feature*, not the auditability NFR that still requires what/who/when to be recoverable ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)) — see [[audit-record]] and [[non-functional-requirements]].

> [!question]- Team decision required — audit mechanism
> The customer declined to mandate a mechanism: "The requirement is that important actions and changes are recorded so that ConnectSphere can determine what changed, who changed it, and when. The technical mechanism used to achieve this is for you to propose." A tamper-proof append-only log is **not** required; a regular database table may suffice (Source: is212-discussions-2026-08-31, [#14](https://github.com/SinYang13/IS212-2026/discussions/14)).

Related: document versioning is explicitly **not** required, as attachments are not legal documents (Source: is212-discussions-2026-08-31, [#21](https://github.com/SinYang13/IS212-2026/discussions/21)) — so change history covers event data, not file revisions. Access to change history is subject to [[access-control]].

## Notification Triggers

Change-related scenarios the brief lists as notification-generating ([[notifications|brief §6]]):

- The Event Organiser requests a significant change.
- An event is rescheduled, postponed, or cancelled.
- A venue booking is changed.
- Requested equipment is confirmed or found to be unavailable.
- A venue or equipment availability change affects an upcoming event.
- An upcoming event still has incomplete arrangements.

Three further triggers come from the September answers (Source: is212-discussions-2026-09-08):

- A confirmed event is changed such that it is **no longer confirmed** — the Event Organiser must be informed ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)).
- An event is rescheduled and registrations are retained — affected attendees must be informed of the new date/time and allowed to withdraw ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)).
- A venue attribute update affects an existing booking's suitability or availability — the booking is flagged for review and the **Event Coordinator** notified ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)).

Each of these may be delivered by the system or by another appropriate channel; the mechanism is the team's to propose in all three cases.

Delivery is batch at a configurable frequency, with no hard latency requirement; real-time delivery is acceptable if simpler (Source: is212-discussions-2026-08-31, [#55](https://github.com/SinYang13/IS212-2026/discussions/55)). See [[notifications]].

## Open Items

| Item | Status | Ref |
|---|---|---|
| Rule and mechanism for classifying significant vs ordinary edits | **Unspecified — team to propose** | [#4](https://github.com/SinYang13/IS212-2026/discussions/4) |
| Revalidation workflow and status changes on reschedule | **Unspecified — team to propose** | [#16](https://github.com/SinYang13/IS212-2026/discussions/16) |
| Transition flow for confirmed → planning after a major change | **Unspecified — team to propose** | [#70](https://github.com/SinYang13/IS212-2026/discussions/70) |
| Automation for venue/equipment unavailability affecting confirmed events | **Unspecified — team to propose** | [#54](https://github.com/SinYang13/IS212-2026/discussions/54) |
| Audit log mechanism and tamper-resistance | **Unspecified — team to propose** | [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |
| Which actions a history surface records, and how it is presented | Team to propose from the customer's illustrative list; no complete audit-history function required for Release 1 | [#96](https://github.com/SinYang13/IS212-2026/discussions/96) |
| Workflow and presentation for partially-accepted change requests | **Unspecified — team to propose**; partial acceptance itself is confirmed | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Channel for telling the Organiser a confirmed event is no longer confirmed | Team to propose; informing them is required | [#105](https://github.com/SinYang13/IS212-2026/discussions/105) |
| Status transition when a venue attribute change affects a live booking | Team to propose; auto-reverting to `Requested` is ruled out | [#120](https://github.com/SinYang13/IS212-2026/discussions/120) |
| Workflow for releasing, replacing or adding equipment after a requirement change | Team to propose | [#114](https://github.com/SinYang13/IS212-2026/discussions/114) |
| Whether to propose voluntary change lead-time guardrails | Team's discretion; no customer requirement | [#8](https://github.com/SinYang13/IS212-2026/discussions/8) |

See [[open-questions]].
