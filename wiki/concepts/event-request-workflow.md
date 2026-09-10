---
title: Event Request Workflow
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Sept 8.md
tags:
  - process
  - workflow
  - event-request
  - coordinator-assignment
  - connectsphere
---

# Event Request Workflow

**Objects:** [[event-request]] · [[event]] · [[user-account]]

The end-to-end process by which an event moves from an Event Organiser's initial idea to a delivered and closed event at [[connectsphere-event-services]]. The statuses attached to each step are described in [[event-lifecycle]].

**Release 1 scope:** Per [[release-1-scope]], event request creation, draft requests, coordinator assignment, and review/approval (Steps 1–5 above) are 🎯 Release 1 (core features 2–5); comments and discussion are 🧩 Adjacent, pulled in by feature 4's requirement that coordinators can request clarification with the outcome visible; event categories, reuse of previous events, multi-session events, and supporting documents are 📋 Backlog — the customer has since confirmed categories directly ("not one of the mandatory Release 1 core features", [#100](https://github.com/SinYang13/IS212-2026/discussions/100)).

## The 14-Step Typical Process

| Step | Name | Actor | Description |
|---|---|---|---|
| 1 | Draft Event Request | Event Organiser | Creates a draft with initial requirements; may save and continue editing before submission |
| 2 | Submission | Event Organiser | Submits the request to ConnectSphere |
| 3 | Coordinator Assignment | Event Operations Manager | Assigns an Event Coordinator as the main internal point of contact |
| 4 | Review and Clarification | Event Coordinator | Reviews information; requests clarification or amendment where incomplete or unclear |
| 5 | Initial Approval | Event Coordinator | Determines whether planning should proceed; rejected or returned requests retain a decision record |
| 6 | Venue Identification | Event Coordinator | Searches venues satisfying date, time, capacity, layout, accessibility, facilities, setup requirements |
| 7 | Venue Request | Event Coordinator → Venue Staff | Submits booking request; Venue Staff approve, reject, or suggest an alternative |
| 8 | Technical Requirements | Technical Support Staff | Review requested equipment and support; determine what can be provided and reserved |
| 9 | Event Preparation | Coordinator, Venue Staff, Technical Support Staff | Coordinator monitors outstanding arrangements; staff update preparation progress |
| 10 | Event Confirmation | Event Coordinator | Confirms once essential arrangements are complete; Organiser can view confirmed arrangements |
| 11 | Attendee Registration | Attendee | Registers during the permitted period, subject to capacity |
| 12 | Changes Before the Event | Event Organiser → Event Coordinator | Organiser requests changes; Coordinator determines impact on existing arrangements |
| 13 | Event Delivery | Venue Staff, Technical Support Staff | Prepare venue and equipment; users access latest confirmed information |
| 14 | Completion | Event Coordinator | Event marked completed; attendance, operational notes, issues recorded and event closed |

([[event-request-workflow|brief §5]])

**Scope caveat:** The 14 steps are a typical end-to-end flow, not an exhaustive specification. The customer stated there may be other branching or exception flows not yet identified, and that features and flows for Release 1 would be prioritised after further internal stakeholder discussion (Source: is212-discussions-2026-08-31, [#11](https://github.com/SinYang13/IS212-2026/discussions/11)). A subset is acceptable.

## Step 1 — Draft Requests

- Event Organisers can save an event request before submission so incomplete information can be completed or reviewed later (Source: customer-brief-no-1).
- Request content may include event name, description, purpose, preferred dates, expected attendance, venue requirements, accessibility needs, equipment requirements, registration requirements, room-layout preferences, general programme, and other planning information ([[connectsphere-event-services|brief §2]] and [[event-planning-venue-booking-system|brief §4]]).
- **Expected attendance sits outside the venue-requirements group.** It is a separate event-level requirement and is not duplicated inside the Venue Requirements structure, though venue suitability, capacity and booking decisions still consume it. The exact data model is the team's, provided the value is not maintained in two conflicting places (Source: is212-discussions-2026-09-08, [#119](https://github.com/SinYang13/IS212-2026/discussions/119)).
- Requests are categorised — conference, workshop, training session, exhibition, meeting, seminar, networking event, or another ConnectSphere-defined type. That list is **illustrative, not exhaustive**; categories are not a mandatory Release 1 core feature, so no comprehensive taxonomy is required and the team may use a reasonable set drawn from the briefing (Source: is212-discussions-2026-09-08, [#100](https://github.com/SinYang13/IS212-2026/discussions/100)).

## Step 2 — Submission and Required Fields

> [!question]- Team decision required — minimum required fields
> The customer has **not firmed up which fields are mandatory** for a request to be submittable, and asked the team to propose what is appropriate. There are also no field-length requirements (Source: is212-discussions-2026-08-31, [#72](https://github.com/SinYang13/IS212-2026/discussions/72)).

Related unspecified constraint: **no minimum lead time** has been set between request creation and the event date. The team should propose one if the solution requires it (Source: is212-discussions-2026-08-31, [#78](https://github.com/SinYang13/IS212-2026/discussions/78)).

Also out of scope at submission: **no contract or terms acceptance** is tracked. Contracts and payments are handled physically, outside the system (Source: is212-discussions-2026-08-31, [#24](https://github.com/SinYang13/IS212-2026/discussions/24)). The system therefore holds no commercial or financial data.

## After Submission — the Organiser Loses the Pen

Submission is a **hard handover of edit rights**, not merely a routing step (Source: is212-discussions-2026-09-08, [#102](https://github.com/SinYang13/IS212-2026/discussions/102), [#103](https://github.com/SinYang13/IS212-2026/discussions/103)).

| Question | Answer | Ref |
|---|---|---|
| May the Organiser edit the submitted request directly? | **No** — and this holds for non-significant fields too, not only significant ones | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| How does a change get made, then? | Communicated to or requested through the Event Coordinator, who processes the update in the system | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| What is the significant/non-significant distinction still for? | Determining whether **existing arrangements need to be reviewed** — nothing else | [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| May the Organiser withdraw a submitted request? | Only by asking the coordinator **outside the system** while the request is under review; the coordinator processes and records the withdrawal | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| Does a withdrawn request look like a draft or a rejection? | **No** — it must remain distinguishable from draft, submitted, approved, rejected and other request states | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |

Design implications:

- There is **no Organiser-facing edit or withdraw control** on a submitted request. Both are coordinator writes. The Organiser-side surface is a request-a-change path, not a form.
- Significance is decoupled from permission. A one-word typo fix and a date change are equally off-limits to the Organiser after submission; they differ only in whether [[change-impact-management]]'s revalidation fires.
- `Draft` is consequently the only [[event-request]] state with clear Organiser write access. The exception the customer did not address is `Returned`: the 14-step process above has the coordinator return a request for amendment and the Organiser amend and resubmit it (steps 4–5), and [#102](https://github.com/SinYang13/IS212-2026/discussions/102) neither authorises nor forbids that. Treating a return as handing the pen back is the only reading that keeps the amend-and-resubmit loop workable, but it is a reading. See [[event-request]] for the state machine and [[access-control]] for the permission reading.

## Step 3 — Coordinator Assignment

Assignment is performed by an **Event Operations Manager** — a role that appears only in the Q&A, not in the brief's five-role table (Source: is212-discussions-2026-08-31, [#73](https://github.com/SinYang13/IS212-2026/discussions/73)). See [[user-roles]].

| Question | Answer | Ref |
|---|---|---|
| Who assigns the coordinator? | The Event Operations Manager, after the request is submitted | [#73](https://github.com/SinYang13/IS212-2026/discussions/73) |
| Auto-assignment or manual? | Manual, by the manager | [#73](https://github.com/SinYang13/IS212-2026/discussions/73) |
| Is there assignment criteria in the system? | No — it is up to the Event Operations Manager, who follows an SOP maintained **outside** the system | [#93](https://github.com/SinYang13/IS212-2026/discussions/93) |
| Can a coordinator be on two events concurrently? | **Yes** | [#92](https://github.com/SinYang13/IS212-2026/discussions/92) |
| Must the coordinator accept the assignment? | **No acceptance step.** Any issue is handled outside the system | [#95](https://github.com/SinYang13/IS212-2026/discussions/95) |
| Can a coordinator decline? | No decline path in the system | [#95](https://github.com/SinYang13/IS212-2026/discussions/95) |
| Who may reassign? | The Event Operations Manager. Request and approval for reassignment happen outside the system; the system may assume any reassignment entered by the manager is already approved | [#94](https://github.com/SinYang13/IS212-2026/discussions/94) |

(Source: is212-discussions-2026-08-31)

Design implication: coordinator assignment is a **simple write, not a workflow**. No approval chain, no acceptance state, no capacity/load-balancing logic is required — only a record of who is assigned, an audit trail of assignment changes, and a notification to the assignee.

That write lands on the **request itself**. Step 3 runs after submission but before Review (Step 4) and Approval (Step 5), both performed by the assigned coordinator, and at that point no [[event]] record exists yet — so [[schema]] gives `event_request` its own `assigned_coordinator_user_account_id`, null through `Draft` and `Submitted` and set here. Without it nothing would record who was reviewing a request between `Submitted` and `Approved`. See [[event-request]].

## Steps 4–5 — Review, Clarification, and Approval

- The Event Coordinator reviews and contacts the Event Organiser for clarification before confirmation, acting as main liaison ([[connectsphere-event-services|brief §2]]).
- Typical ambiguities cited by the customer: an Organiser requesting a "large room" without stating expected attendance, or a hybrid event without stating video-conferencing needs (Source: customer-brief-no-1).
- Technical Support Staff can record questions or clarification needs against the event, but the **Event Coordinator remains the main liaison** and coordinates the response to the Organiser. The clarification and response are retained with the event record (Source: is212-discussions-2026-08-31, [#87](https://github.com/SinYang13/IS212-2026/discussions/87)).
- Approval means only that information is sufficient for planning to proceed — it commits nothing. See [[event-lifecycle]] for the approved-vs-confirmed distinction.
- Approval also **hands the coordinator assignment over**: the value set at Step 3 is copied from the request onto the [[event]] that approval creates, and the request's copy is then frozen, since `Approved` is terminal for that record. From that point the event holds the live assignment — any reassignment by the Event Operations Manager ([#94](https://github.com/SinYang13/IS212-2026/discussions/94)) writes only there, and the cancel-only-by-the-assigned-coordinator rule ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)) reads only there. See [[schema]].
- Rejected or returned requests retain a record of the decision. Whether they may be resubmitted is unspecified ([#67](https://github.com/SinYang13/IS212-2026/discussions/67)) — see [[event-lifecycle]].
- Review has a fourth exit besides approve, reject and return: the coordinator may **withdraw** the request on the Organiser's behalf, on a request made outside the system while the request is under review ([#103](https://github.com/SinYang13/IS212-2026/discussions/103)). See [[event-lifecycle]].

## Comments, Discussion, and Supporting Documents

Comments, supporting documents, and their constraints (retention, versioning, size/type limits) are defined on [[event]] — see Attributes and Open modelling questions (Source: is212-discussions-2026-08-31, [#15](https://github.com/SinYang13/IS212-2026/discussions/15), [#18](https://github.com/SinYang13/IS212-2026/discussions/18), [#21](https://github.com/SinYang13/IS212-2026/discussions/21)).

## Reuse of Previous Event Information

The brief allows information from a previous or recurring event to be reused to reduce repeated data entry, while still allowing different dates and requirements.

> [!question]- Team decision required — what "reuse a previous event" copies
> Asked whether this means cloning a whole event or copying selected fields, the customer answered that the requirement is "simply to reduce repeated data entry while still allowing the new event to have different dates and requirements. Which information is copied and how it is presented is for you to propose." (Source: is212-discussions-2026-08-31, [#75](https://github.com/SinYang13/IS212-2026/discussions/75)). Fields that must *not* carry over silently — dates, venue booking, equipment reservations, registration state — are an obvious design boundary.

## Multi-Session Events

Some events contain several sessions within a day or across multiple days. The customer confirmed sessions may have **different venues, different equipment requirements, and different attendance limits** for each session (Source: is212-discussions-2026-08-31, [#74](https://github.com/SinYang13/IS212-2026/discussions/74)).

Consequences:

- Venue booking, equipment reservation, and capacity are properties that may be held at **session** level, not only event level. See [[venue-booking]], [[equipment-management]], [[attendee-registration]].
- Event readiness must aggregate across sessions — see [[event-readiness]].
- Extending or splitting a programme is a change with downstream impact — see [[change-impact-management]].
- Venues are booked in three fixed slots, defined on [[venue]], which constrain how sessions map onto bookings (Source: is212-discussions-2026-08-31, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)).

## Downstream Steps

| Step area | Page |
|---|---|
| Venue identification, request, approval (6–7) | [[venue-booking]], [[venue-suitability]] |
| Technical requirements (8) | [[equipment-management]], [[technical-support]] |
| Preparation and confirmation (9–10) | [[event-readiness]] |
| Registration (11) | [[attendee-registration]] |
| Changes before the event (12) | [[change-impact-management]] |
| Completion (14) | [[event-lifecycle]] |

## Open Items

| Item | Status | Ref |
|---|---|---|
| Minimum mandatory fields for submission | **Unspecified — team to propose** | [#72](https://github.com/SinYang13/IS212-2026/discussions/72) |
| Minimum lead time before event date | **Unspecified — team to propose** | [#78](https://github.com/SinYang13/IS212-2026/discussions/78) |
| Which fields are copied when reusing a previous event | **Unspecified — team to propose** | [#75](https://github.com/SinYang13/IS212-2026/discussions/75) |
| Comment threading model | UI/UX — team to propose | [#15](https://github.com/SinYang13/IS212-2026/discussions/15) |
| Document size and type limits | **Unspecified — team to propose** | [#18](https://github.com/SinYang13/IS212-2026/discussions/18) |
| Which of the 14 steps fall into Release 1 | Pending customer's Release 1 scope | [#11](https://github.com/SinYang13/IS212-2026/discussions/11), [#79](https://github.com/SinYang13/IS212-2026/discussions/79) |
| Category set to implement | Team to propose a reasonable set; no comprehensive taxonomy required | [#100](https://github.com/SinYang13/IS212-2026/discussions/100) |
| Data model holding expected attendance without duplicating it | Team to propose; the field is event-level, not nested in venue requirements | [#119](https://github.com/SinYang13/IS212-2026/discussions/119) |
| Whether a withdrawn request may be revived or resubmitted | **Unspecified — team to propose** | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |

See [[open-questions]].
