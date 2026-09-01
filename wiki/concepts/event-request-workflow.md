---
title: Event Request Workflow
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - process
  - workflow
  - event-request
  - coordinator-assignment
  - connectsphere
---

# Event Request Workflow

The end-to-end process by which an event moves from an Event Organiser's initial idea to a delivered and closed event at [[connectsphere-event-services]]. The statuses attached to each step are described in [[event-lifecycle]].

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

(Source: customer-brief-no-1, s5)

**Scope caveat:** The 14 steps are a typical end-to-end flow, not an exhaustive specification. The customer stated there may be other branching or exception flows not yet identified, and that features and flows for Release 1 would be prioritised after further internal stakeholder discussion (Source: is212-discussions-2026-08-31, #11). A subset is acceptable.

## Step 1 — Draft Requests

- Event Organisers can save an event request before submission so incomplete information can be completed or reviewed later (Source: customer-brief-no-1).
- Request content may include event name, description, purpose, preferred dates, expected attendance, venue requirements, accessibility needs, equipment requirements, registration requirements, room-layout preferences, general programme, and other planning information (Source: customer-brief-no-1, s2 and s4).
- Requests are categorised — conference, workshop, training session, exhibition, meeting, seminar, networking event, or another ConnectSphere-defined type.

## Step 2 — Submission and Required Fields

> **Team decision required — minimum required fields.** The customer has **not firmed up which fields are mandatory** for a request to be submittable, and asked the team to propose what is appropriate. There are also no field-length requirements (Source: is212-discussions-2026-08-31, #72).

Related unspecified constraint: **no minimum lead time** has been set between request creation and the event date. The team should propose one if the solution requires it (Source: is212-discussions-2026-08-31, #78).

Also out of scope at submission: **no contract or terms acceptance** is tracked. Contracts and payments are handled physically, outside the system (Source: is212-discussions-2026-08-31, #24). The system therefore holds no commercial or financial data.

## Step 3 — Coordinator Assignment

Assignment is performed by an **Event Operations Manager** — a role that appears only in the Q&A, not in the brief's five-role table (Source: is212-discussions-2026-08-31, #73). See [[user-roles]].

| Question | Answer | Ref |
|---|---|---|
| Who assigns the coordinator? | The Event Operations Manager, after the request is submitted | #73 |
| Auto-assignment or manual? | Manual, by the manager | #73 |
| Is there assignment criteria in the system? | No — it is up to the Event Operations Manager, who follows an SOP maintained **outside** the system | #93 |
| Can a coordinator be on two events concurrently? | **Yes** | #92 |
| Must the coordinator accept the assignment? | **No acceptance step.** Any issue is handled outside the system | #95 |
| Can a coordinator decline? | No decline path in the system | #95 |
| Who may reassign? | The Event Operations Manager. Request and approval for reassignment happen outside the system; the system may assume any reassignment entered by the manager is already approved | #94 |

(Source: is212-discussions-2026-08-31)

Design implication: coordinator assignment is a **simple write, not a workflow**. No approval chain, no acceptance state, no capacity/load-balancing logic is required — only a record of who is assigned, an audit trail of assignment changes, and a notification to the assignee.

## Steps 4–5 — Review, Clarification, and Approval

- The Event Coordinator reviews and contacts the Event Organiser for clarification before confirmation, acting as main liaison (Source: customer-brief-no-1, s2).
- Typical ambiguities cited by the customer: an Organiser requesting a "large room" without stating expected attendance, or a hybrid event without stating video-conferencing needs (Source: customer-brief-no-1).
- Technical Support Staff can record questions or clarification needs against the event, but the **Event Coordinator remains the main liaison** and coordinates the response to the Organiser. The clarification and response are retained with the event record (Source: is212-discussions-2026-08-31, #87).
- Approval means only that information is sufficient for planning to proceed — it commits nothing. See [[event-lifecycle]] for the approved-vs-confirmed distinction.
- Rejected or returned requests retain a record of the decision. Whether they may be resubmitted is unspecified (#67) — see [[event-lifecycle]].

## Comments, Discussion, and Supporting Documents

| Feature | Rule | Ref |
|---|---|---|
| Comments and discussion | Requirement is to **retain** comments, questions and updates with the event record. Threaded vs chronological is a UI/UX decision for the team | #15 |
| Supporting documents | Users can attach programmes, floor plans, presentation requirements, and other materials | brief |
| Document versioning | **Not required.** These are not legal documents, so no versions need be kept | #21 |
| Document size / file types | No limits specified; team to propose if constraints are needed | #18 |

(Source: is212-discussions-2026-08-31)

## Reuse of Previous Event Information

The brief allows information from a previous or recurring event to be reused to reduce repeated data entry, while still allowing different dates and requirements.

> **Team decision required.** Asked whether this means cloning a whole event or copying selected fields, the customer answered that the requirement is "simply to reduce repeated data entry while still allowing the new event to have different dates and requirements. Which information is copied and how it is presented is for you to propose." (Source: is212-discussions-2026-08-31, #75). Fields that must *not* carry over silently — dates, venue booking, equipment reservations, registration state — are an obvious design boundary.

## Multi-Session Events

Some events contain several sessions within a day or across multiple days. The customer confirmed sessions may have **different venues, different equipment requirements, and different attendance limits** for each session (Source: is212-discussions-2026-08-31, #74).

Consequences:

- Venue booking, equipment reservation, and capacity are properties that may be held at **session** level, not only event level. See [[venue-booking]], [[equipment-management]], [[attendee-registration]].
- Event readiness must aggregate across sessions — see [[event-readiness]].
- Extending or splitting a programme is a change with downstream impact — see [[change-impact-management]].
- Venues are booked in three fixed slots (AM 7am–12pm, PM 1pm–6pm, Night 7pm–12am), which constrains how sessions map onto bookings (Source: is212-discussions-2026-08-31, #50).

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
| Minimum mandatory fields for submission | **Unspecified — team to propose** | #72 |
| Minimum lead time before event date | **Unspecified — team to propose** | #78 |
| Which fields are copied when reusing a previous event | **Unspecified — team to propose** | #75 |
| Comment threading model | UI/UX — team to propose | #15 |
| Document size and type limits | **Unspecified — team to propose** | #18 |
| Which of the 14 steps fall into Release 1 | Pending customer's Release 1 scope | #11, #79 |

See [[open-questions]].
