---
title: Release 1 — Candidate User Stories
type: review
sources:
  - "wiki/concepts/release-1-scope.md"
  - "Week 4 - Customer Briefing.md"
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 User Story Standard — v1.0.md"
tags: [review, user-stories, backlog, release-1]
---

# Release 1 — Candidate User Stories

A candidate backlog derived directly from [[release-1-scope]]'s register: every `🎯 Release 1` and `🧩 Adjacent` row gets at least one card; every `📋 Backlog` row is deliberately left uncarded here (it still has a wiki page and open questions — just no Release 1 card). Cards follow the card anatomy, ACs, and Gate checks in the IS212 User Story Standard v1.0. Where an existing card in [[user-stories-proposed-rewrites]] already covers a row and matches the Release 1 cut, this document points to it rather than duplicating it; where the existing card assumed scope since narrowed (waiting list, check-in — now `📋 Backlog`), this document supersedes it with a Release 1-scoped rewrite.

> [!info] Status of these cards
> Titles, roles, actions and outcomes are drafted; acceptance criteria cover the happy path and the customer-stated rules found in the wiki. **Priority and Estimate are intentionally left blank** — Gate A7 and the Definition of Ready require a team ordering pass and Fibonacci sizing this document cannot do alone. Several cards carry an **Open question** where the wiki records the point as customer-undecided — per the standard's §11, these are the team's cue to have the conversation, not to invent an answer. No card here is Ready until that conversation happens and the customer confirms.

## Coverage — register row → card

| Register area ([[release-1-scope]])   | Marker | Card(s)                                                                   |
| ------------------------------------- | ------ | ------------------------------------------------------------------------- |
| User authorisation and authentication | 🎯     | U1, U2, U3, U4 *(see [[user-stories-proposed-rewrites]])*                 |
| Client management                     | 🧩     | **ID1**                                                                   |
| Event request creation                | 🎯     | EV2 *(see [[user-stories-proposed-rewrites]])*                            |
| Draft event requests                  | 🎯     | EV2 *(same card — draft-save is AC1; see note under ID2 below)*           |
| Event review and approval             | 🎯     | EV3a, EV3b, EV3c *(see [[user-stories-proposed-rewrites]])*               |
| Coordinator assignment                | 🎯     | EV1 + Reassign companion *(see [[user-stories-proposed-rewrites]])*       |
| Event status management               | 🎯     | **RA1**, plus the status transitions embedded in EV2/EV1/EV3c/RA3/RA4/CH4 |
| Event information management          | 🎯     | **RA2**                                                                   |
| Comments and discussion               | 🧩     | EV3b *(see [[user-stories-proposed-rewrites]])*                           |
| Venue catalogue                       | 🎯     | **VN1**                                                                   |
| Venue availability calendar           | 🎯     | **VN2**                                                                   |
| Venue search and filtering            | 🎯     | **VN3**                                                                   |
| Venue suitability checking            | 🎯     | **VN4**                                                                   |
| Venue booking request                 | 🎯     | **VN5**                                                                   |
| Venue booking approval                | 🎯     | V5 *(see [[user-stories-proposed-rewrites]])*                             |
| Booking conflict detection            | 🎯     | V1 *(see [[user-stories-proposed-rewrites]])*                             |
| Venue unavailability management       | 🧩     | V4 *(see [[user-stories-proposed-rewrites]])*, plus **NT17**              |
| Room layout management                | 🧩     | No dedicated card — satisfied as an attribute on VN1 (see VN1's note)     |
| Setup and turnaround time             | 🧩     | V6 *(see [[user-stories-proposed-rewrites]])*                             |
| Equipment request management          | 🎯     | **EQ2**, plus E2 *(see [[user-stories-proposed-rewrites]])*               |
| Equipment availability checking       | 🎯     | E2 *(see [[user-stories-proposed-rewrites]])*                             |
| Equipment reservation                 | 🎯     | E2 *(see [[user-stories-proposed-rewrites]])*                             |
| Equipment catalogue                   | 🧩     | **EQ1**                                                                   |
| Equipment maintenance status          | 🧩     | E1 *(see [[user-stories-proposed-rewrites]])*                             |
| Event confirmation                    | 🧩     | **RA3**                                                                   |
| Event readiness tracking              | 🧩     | **RA3** (the gate, not a dashboard — per [[release-1-scope]])             |
| Attendee registration                 | 🎯     | **RG1** *(supersedes Reg1 — see callout below)*                           |
| Withdrawal                            | 🎯     | **RG4** *(supersedes Reg5 — see callout below)*                           |
| Registration period                   | 🧩     | Reg2 *(see [[user-stories-proposed-rewrites]])*                           |
| Registration capacity                 | 🧩     | Reg3 *(see [[user-stories-proposed-rewrites]])*, plus **NT18**            |
| Event change requests                 | 🎯     | **CH1**                                                                   |
| Impact of event changes               | 🎯     | **CH2**                                                                   |
| Rescheduling                          | 🧩     | **CH3**                                                                   |
| Cancellation management               | 🧩     | **CH4** *(builds on V2 — see [[user-stories-proposed-rewrites]])*         |
| Notification system                   | 🎯     | **NT1** (mechanism) + **NT2–NT16** (one card per trigger × recipient)     |

`📋 Backlog` rows (waiting list, check-in/attendance recording, event categories, recurring/reuse, multi-session, supporting documents, tentative holding, event capacity management as an ongoing tracker, outstanding action tracking, reminders, event calendar, event/venue search & filtering for events, role-based dashboard, activity/change history browsing, reporting) are out of scope for this backlog by design — see [[release-1-scope]] for why, and [[open-questions]] for what each still owes the customer.

> [!warning] Reg1, Reg4, Reg5, Reg6 in [[user-stories-proposed-rewrites]] predate [[release-1-scope]]
> Those cards were written before the Week 4 briefing fixed the Release 1 cut. Reg1 (Register) and Reg5 (Withdraw) assumed a waiting list that is now `📋 Backlog`; Reg4 (Be Promoted From the Waiting List) and Reg6 (Check In a Registered Attendee) are themselves entirely `📋 Backlog` functionality. **RG1** and **RG4** below replace Reg1 and Reg5 for Release 1 planning; Reg4 and Reg6 should not enter a Release 1 sprint. The original cards remain correct as a *full-scope* backlog for later releases — this is a scope cut, not a defect in the earlier review.

---

## Identity and Access

### ID1 — View my organisation's events

**Story:** As an Event Organiser, I want to see events raised by colleagues in my client organisation, so that we don't duplicate a request or lose context when more than one of us deals with ConnectSphere.

**AC:**
- Can view events belonging to any Event Organiser in the same client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81))
- Cannot edit an event unless assigned as its responsible Event Organiser ([#81](https://github.com/SinYang13/IS212-2026/discussions/81), [#61](https://github.com/SinYang13/IS212-2026/discussions/61))
- Cannot view or edit events belonging to an unrelated client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81), see [[access-control]] "Client Organisation Visibility")
- Given the Event Organiser is reassigned off an event via a change of Event Organiser, then their edit access to that event is removed and the new Organiser's is granted ([#61](https://github.com/SinYang13/IS212-2026/discussions/61), [#59](https://github.com/SinYang13/IS212-2026/discussions/59))

**Open question:** whether a primary contact per client organisation is ever introduced remains unspecified ([#65](https://github.com/SinYang13/IS212-2026/discussions/65)) — this card assumes none, per the confirmed rule that visibility is automatic and organisation-wide with no administering role.

---

## Event Intake

Event request creation and draft requests are already covered by **EV2** in [[user-stories-proposed-rewrites]] — its AC1 ("Can save a draft event request before submission, and return to complete or edit it later") is core feature 3, and the rest of the card is core feature 2. They are kept as one card here: splitting draft-save out would separate a precondition from the workflow it exists inside, which the Workflow-steps splitting pattern argues against — see [[event-request-workflow]] Step 1.

---

## Review, Assignment and Status

EV1 (+ Reassign companion), EV3a, EV3b and EV3c in [[user-stories-proposed-rewrites]] cover coordinator assignment and review/approval unchanged. Two new cards close the remaining core-20 rows.

### RA1 — Track my event's status

**Story:** As an Event Organiser, I want to see my event's current status and what changed most recently, so that I always know what stage it is at without having to ask my coordinator.

**AC:**
- Can see the event's current status at any time (see [[event]] States for the status set)
- Can see the date of the most recent status change
- Given the event's status changes, when the Organiser next views the event, then the displayed status reflects the change without requiring a page refresh workaround
- Status labels and their meaning are understandable without knowledge of ConnectSphere's internal process (Source: customer-brief-no-1, s7)

*Distinct from the Role-Based Dashboard feature (`📋 Backlog`, [[access-control]]): this card is the single event's own status, not a cross-event "events requiring attention" overview.*

### RA2 — Update event planning information

**Story:** As the assigned Event Coordinator, I want to update an event's planning information as arrangements are finalised, so that the event record reflects what has actually been agreed.

**AC:**
- Can edit event fields that do not affect existing arrangements — e.g. name, description, purpose, internal notes ([#4](https://github.com/SinYang13/IS212-2026/discussions/4), see [[change-impact-management]] "Significant vs Ordinary Edits")
- Given an edit touches date/time, expected attendance, venue requirements, or equipment requirements, then the edit is routed through the change-request process (**CH1**) instead of a direct update ([#4](https://github.com/SinYang13/IS212-2026/discussions/4))
- Edits are attributed and timestamped, consistent with the auditability requirement (Source: customer-brief-no-1, s8f)

**Open question:** the exact classification rule for which fields count as "significant" is explicitly left to the team ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)) — this card's AC2 boundary should be confirmed against the team's eventual rule before Ready.

### RA3 — Confirm an event

**Story:** As the assigned Event Coordinator, I want to confirm an event once every arrangement essential to it is complete, so the Event Organiser can rely on what has been arranged.

**AC:**
- Given one or more arrangements identified as essential for this event are still incomplete, when the Coordinator attempts to confirm, then confirmation is blocked and the incomplete arrangement(s) are named ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), see [[event-readiness]] "The Core Rule")
- Given every essential arrangement is complete, when the Coordinator confirms, then the event's status becomes Confirmed even if non-essential follow-up actions remain outstanding ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), answer 4)
- Once confirmed, the Event Organiser can view the confirmed arrangements ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), see [[event-lifecycle]] "Approved vs Confirmed")
- The Event Organiser is notified that the event is confirmed (Source: customer-brief-no-1, s6)

**Open question:** how "essential" is determined and recorded per event is explicitly left to the team ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) — the mechanism AC1 depends on cannot be left implicit; see [[event-readiness]] "Team decision required — how essentiality is determined and recorded."

### RA4 — Mark an event as completed

**Story:** As the assigned Event Coordinator, I want to mark an event as completed after it has taken place, so the record reflects that delivery happened and any closing notes are captured.

**AC:**
- Can mark a Confirmed event as Completed ([[event]] States)
- Can record operational notes at completion ([[event-request-workflow]] Step 14)
- Out of scope for Release 1: automatic completion 7 days after the end date is `📋 Backlog` in [[release-1-scope]] — only the manual action is required ([#71](https://github.com/SinYang13/IS212-2026/discussions/71))

---

## Venue

### VN1 — Maintain the venue catalogue

**Story:** As Venue Staff, I want to create and update a venue's catalogue record — location, capacity, facilities, accessibility, supported room layouts, and operating hours — so that Event Coordinators can find and evaluate real venues.

**AC:**
- Can create a venue record with location, capacity, facilities, accessibility, and operating hours ([[venue]] Attributes)
- Can record which room layouts (classroom, theatre, boardroom, banquet, exhibition, or another) a venue supports ([[venue-suitability]] "Supported Room Layouts")
- Can update an existing venue record's attributes
- Access to maintain any venue is not restricted by venue or location ([#66](https://github.com/SinYang13/IS212-2026/discussions/66))

**Open question:** whether Room Layout should be modelled as its own object (e.g. to support capacity that varies by layout) or stays a plain attribute is left to the team ([#83](https://github.com/SinYang13/IS212-2026/discussions/83), see [[venue]] "Room Layout as a modelled object"). This card assumes the plain-attribute reading; revisit if the team decides otherwise.

### VN2 — View venue availability calendar

**Story:** As an Event Coordinator, I want to view a venue's availability calendar showing which AM/PM/Night slots are open, confirmed, or blocked, so I can identify a viable slot before requesting a booking.

**AC:**
- Can view a venue's calendar showing, per day, the status of each AM/PM/Night slot: available, confirmed, or blocked ([[venue]] States; [#50](https://github.com/SinYang13/IS212-2026/discussions/50))
- Blocked slots display the reason recorded by Venue Staff, where one exists ([[venue-booking]] "Venue Unavailability and Blocking")
- The calendar reflects the current state of all recorded bookings and blocks, not a cached snapshot the Coordinator must manually refresh

### VN3 — Search and filter venues

**Story:** As an Event Coordinator, I want to search and filter venues by date, capacity, accessibility, layout, and facilities, so I can narrow the catalogue down to real candidates before checking suitability in detail.

**AC:**
- Can filter the venue catalogue by capacity, accessibility, supported layout, and available facilities ([[venue-suitability]] "Search/Filtering vs Suitability Checking")
- Can filter by date and slot, excluding venues with no open slot in the searched window
- Filtering and suitability checking are separate steps — search results are a candidate list, not a suitability verdict ([#83](https://github.com/SinYang13/IS212-2026/discussions/83))

### VN4 — Check venue suitability

**Story:** As an Event Coordinator, I want the system to tell me whether a venue I've shortlisted still satisfies my event's current requirements, so I don't submit a booking request for somewhere that doesn't actually fit.

**AC:**
- Given a shortlisted venue, when I request a suitability check, then the system compares the venue's capacity, supported layout, accessibility, and facilities against the event's current requirements and reports whether each is satisfied ([#83](https://github.com/SinYang13/IS212-2026/discussions/83), see [[venue-suitability]] "Suitability Factors")
- Given the event's expected attendance changes after a venue was selected, when I re-check suitability, then the result reflects the new attendance figure, not the figure at the time of first selection (Source: customer-brief-no-1 — "80 to 150 people" example)

**Open question:** whether a venue whose capacity is exceeded by expected attendance is blocked outright or shown as an overridable warning is unresolved — the brief's own rule is hedged ("should **not normally** be treated as suitable") and was never tightened, unlike the double-booking rule ([[venue-suitability]] "Capacity vs Expected Attendance"). Do not invent a threshold or override rule for this AC before the customer answers.

### VN5 — Submit a venue booking request

**Story:** As an Event Coordinator, I want to submit a booking request for a specific venue and slot(s), so Venue Staff can review it and I get a definite answer.

**AC:**
- Can submit a booking request naming one venue, one or more slots (AM/PM/Night) per day, and the event it is for (Source: customer-brief-no-1, [#50](https://github.com/SinYang13/IS212-2026/discussions/50))
- The request carries the event's timing and venue requirements for Venue Staff to review ([[venue-booking]] "Booking Request and Approval")
- Given the requested venue and slot already carry a Confirmed booking for an overlapping period, when the request is submitted, then it is blocked outright, not merely flagged ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), [#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- Venue Staff are notified of the new request ([[notifications]])

---

## Equipment

### EQ1 — Maintain the equipment catalogue

**Story:** As Technical Support Staff, I want to create and update equipment records — type, description, quantity, and location — so requests can be matched against what ConnectSphere actually owns.

**AC:**
- Can create an equipment record with type, description, quantity, and physical location ([[equipment-item]] Attributes)
- Can update the quantity and location of an existing record
- The system tracks quantity available regardless of how individual units are modelled internally ([#13](https://github.com/SinYang13/IS212-2026/discussions/13))
- No import from an existing equipment database is required — the catalogue starts empty ([#12](https://github.com/SinYang13/IS212-2026/discussions/12))

**Open question:** whether equipment is modelled per-unit (serialised) or as a pooled quantity counter is explicitly left to the team ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)) — see [[equipment-item]] "Team decision required — pooled vs serialised inventory." This affects how AC1–2 are eventually implemented, not what they require.

### EQ2 — Specify equipment requirements for an event

**Story:** As the assigned Event Coordinator, I want to record what equipment my event needs — type and quantity — so Technical Support Staff know what to check and reserve.

**AC:**
- Can record one or more equipment requirement lines against an event, each with a type and quantity ([[event]] Attributes — "Equipment requirements"; [[event-request-workflow]] Step 8)
- Can edit a requirement line before Technical Support Staff have reserved against it
- Given a requirement is changed after equipment has already been reserved, then it is treated as a significant change and routed through **CH1**/**CH2** ([#4](https://github.com/SinYang13/IS212-2026/discussions/4))
- Technical Support Staff are notified that a new or changed requirement needs review ([[notifications]])

*Together with E2 in [[user-stories-proposed-rewrites]] (the Technical Support Staff-side matching, availability check and reservation), this closes core feature 15 end to end.*

---

## Readiness and Confirmation

See **RA3** above — [[release-1-scope]] is explicit that only the confirmation gate is required for Release 1, not a readiness dashboard, so no separate "readiness tracking" card exists.

---

## Registration

### RG1 — Register for a confirmed event *(supersedes Reg1 for Release 1)*

**Story:** As an Attendee, I want to register for a confirmed event so that I can secure my place.

**AC:**
- Given an event is not Confirmed, when I search for it, then it is not available for registration ([[event-planning-venue-booking-system|brief §4]], [[event-request-workflow]] Step 11)
- Given an event is Confirmed, registration is enabled, and I am within the registration period, when I submit a valid registration, then a registration is created and shown to me ([#30](https://github.com/SinYang13/IS212-2026/discussions/30), [[event-planning-venue-booking-system|brief §4]])
- Given the event has reached its registration capacity, when I attempt to register, then registration is refused and I am told the event is full (Source: customer-brief-no-1, s4 — waiting list is `📋 Backlog` in [[release-1-scope]], so there is no queue to join in Release 1)
- I receive confirmation of my registration (event name, date, time, venue)

**Open question:** the mandatory information set for registration is explicitly undecided by the customer — "propose an appropriate set of information based on the event needs" ([#22](https://github.com/SinYang13/IS212-2026/discussions/22), see [[registration]]).

### RG4 — Withdraw an event registration *(supersedes Reg5 for Release 1)*

**Story:** As an Attendee, I want to withdraw my registration for an event, so that my place can be released if I can no longer attend.

**AC:**
- Can withdraw a Registered registration ([[event-planning-venue-booking-system|brief §4]])
- Given an Attendee withdraws, then the event's registration count decreases and the freed place becomes available to new registrants — there is no waiting list to promote from in Release 1 ([[release-1-scope]])
- Withdrawal is confirmed to the Attendee

**Open question:** whether withdrawal has a cut-off before the event date is not stated ([[event-planning-venue-booking-system|brief §4]] says only "where permitted"); there is no general change cut-off for events either ([#8](https://github.com/SinYang13/IS212-2026/discussions/8)) — raise with the customer rather than assume a deadline.

*Reg2 (Enable registration) and Reg3 (Set registration capacity) in [[user-stories-proposed-rewrites]] are unchanged — both already match the Release 1 cut.*

---

## Change and Cancellation

### CH1 — Submit a change request

**Story:** As an Event Organiser, I want to request a change to my event — date/time, expected attendance, venue requirements, or equipment requirements — so ConnectSphere can check whether what's already arranged still holds.

**AC:**
- Can submit a change request against my own event, describing the requested change ([[change-request]] Attributes)
- Given the requested change affects date/time, attendance, venue requirements, or equipment requirements, then it is classified as significant and triggers revalidation of the affected arrangement(s) ([#4](https://github.com/SinYang13/IS212-2026/discussions/4))
- Given the requested change affects only fields such as name, description, or internal notes, then it is classified as ordinary and does not trigger revalidation ([#4](https://github.com/SinYang13/IS212-2026/discussions/4))
- Can submit a change request against an already-Confirmed event — there is no change cut-off before the event date ([#8](https://github.com/SinYang13/IS212-2026/discussions/8))
- The assigned Event Coordinator is notified of the request ([[notifications]])

**Open question:** the classification mechanism itself — static per-field configuration vs. evaluation against what is currently booked — is left to the team ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)).

### CH2 — Revalidate arrangements after a significant change

**Story:** As the assigned Event Coordinator, I want the system to flag which arrangements need re-review when a significant change request is approved, so a stale venue or equipment booking doesn't silently carry over.

**AC:**
- Given a significant change is approved, when revalidation runs, then the affected arrangement(s) — venue booking, equipment reservation, registration capacity — are flagged for review by their owning role ([[change-impact-management]] "What Revalidation Means")
- A flagged venue booking is reviewed by Venue Staff against the new date/time, attendance, or requirements ([[change-impact-management]])
- A flagged equipment reservation is reviewed by Technical Support Staff against the new dates ([[change-impact-management]])
- An event with a flagged, unresolved arrangement cannot be (re-)confirmed until the flag clears ([[event-readiness]] "Readiness After Confirmation")

**Open question:** whether an already-Confirmed event moves back to a formal Planning/Awaiting-arrangements state, or stays Confirmed with a "pending revalidation" flag, is left to the team ([#70](https://github.com/SinYang13/IS212-2026/discussions/70), see [[event]] "Open modelling questions").

### CH3 — Reschedule an event

**Story:** As the assigned Event Coordinator, I want to change an event's date or time and have its venue and equipment bookings re-checked, so a reschedule can't leave a booking that no longer makes sense.

**AC:**
- Rescheduling an event follows the same change-request and revalidation process as any other significant change — including when initiated via a calendar drag-and-drop interaction, not only a form ([#3](https://github.com/SinYang13/IS212-2026/discussions/3))
- Given the new date/time creates an overlap with another Confirmed booking for the same venue, then the reschedule is blocked, consistent with the double-booking rule ([#35](https://github.com/SinYang13/IS212-2026/discussions/35))
- Given the event has an equipment reservation, when it is rescheduled, then that reservation is re-reviewed against the new dates rather than carried over automatically ([#16](https://github.com/SinYang13/IS212-2026/discussions/16))

**Open question:** whether per-venue turnaround time is expected to be absorbed within the existing AM/PM/Night gaps or added on top of slot boundaries is unresolved and affects whether some reschedules should be blocked ([[venue-booking]] "Setup and Turnaround Time").

### CH4 — Cancel an event

**Story:** As the assigned Event Coordinator, I want to cancel an event at any stage, so its venue and equipment commitments are released and the people affected are told.

**AC:**
- Only the Event Coordinator assigned to the event may cancel it, and may do so at any stage of its lifecycle ([#68](https://github.com/SinYang13/IS212-2026/discussions/68) — one of the project's committed hard rules, see [[index]])
- Given the event is cancelled, then its venue booking is released, per V2's Confirmed → Released transition ([[user-stories-proposed-rewrites]] V2; [[booking]] States)
- Given the event has an equipment reservation, when it is cancelled, then the reservation no longer holds equipment committed to it (Source: customer-brief-no-1, [[event-lifecycle]] "Cancellation")
- Affected parties, including registered Attendees, are notified of the cancellation (Source: customer-brief-no-1, s6)

---

## Notifications

Brief §6 lists fourteen situations that "may generate a notification" and names a recipient for none of them. NT1 originally carried all fourteen as a single acceptance criterion — a card describing every notification in the system, which is not one piece of work and not testable as one. It is split here by the standard's §7.2 **Variations in data** pattern: the delivery mechanism stays on NT1, and each trigger × recipient pair becomes its own card.

The split resolves the recipient matrix that [[notification]] and [[notifications]] both record as a team decision. Every card names exactly one recipient role, which is what makes it independently testable — and forces the recipient question to be answered card by card rather than deferred wholesale. The resulting matrix is mirrored in [[notification]] "Recipient matrix", which is the source of truth for it.

**Three decisions the split forced**, none of them customer statements:

1. **A trigger with two audiences is two cards.** §6 rows 2, 5 and 9 each reach materially different recipients. Row 9 is the clearest: notifying the Event Organiser and notifying an unbounded set of external Attendees differ in fan-out and in what [[access-control]] permits the message to contain ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)).
2. **No self-notification.** A role is not notified of an action it performed itself — so event confirmation (row 7) is carded for the Organiser only, though [[notification]]'s inferred-audience column names the Coordinator too.
3. **Row 12 collapses without a waiting list.** "Where that affects availability" presupposes promotion from a queue; with the waiting list `📋 Backlog`, a Release 1 withdrawal just frees the place.

**Applies to every card below and is not repeated on each:** the recipient can open the underlying record from the notification and act on it there, and can mark it read. Read/unread state is **not modelled by either source** — [[notification]] "States" records that whether a sent notification is tracked as read, acknowledged, or dismissed is unaddressed. The mark-read AC is the team's minimal proposal for the "acknowledge" half of the requirement and should be confirmed with the customer.

### Register

| Card | Recipient | Trigger (§6 row) | Scope | Linear |
|---|---|---|---|---|
| NT1 | — (mechanism) | all | 🎯 Release 1 | SPM-47 |
| NT2 | Event Operations Manager | new event request submitted (1) | 🎯 Release 1 | SPM-56 |
| NT3 | Assigned Event Coordinator | coordinator assigned (2) | 🎯 Release 1 | SPM-57 |
| NT4 | Event Organiser | coordinator assigned (2) | 🎯 Release 1 | SPM-58 |
| NT5 | Event Organiser | clarification or amendment requested (3) | 🎯 Release 1 | SPM-59 |
| NT6 | Event Organiser | request approved, rejected, or returned (4) | 🎯 Release 1 | SPM-60 |
| NT7 | Venue Staff | venue booking requested (5) | 🎯 Release 1 | SPM-61 |
| NT8 | Requesting Event Coordinator | booking approved or rejected (5) | 🎯 Release 1 | SPM-62 |
| NT9 | Venue Staff | confirmed booking cancelled (5) | 🎯 Release 1 | SPM-63 |
| NT10 | Technical Support Staff | equipment requirement recorded or changed (6) | 🎯 Release 1 | SPM-64 |
| NT11 | Assigned Event Coordinator | equipment confirmed or unavailable (6) | 🎯 Release 1 | SPM-65 |
| NT12 | Event Organiser | event confirmed (7) | 🎯 Release 1 | SPM-66 |
| NT13 | Assigned Event Coordinator | significant change requested (8) | 🎯 Release 1 | SPM-67 |
| NT14 | Event Organiser | rescheduled, postponed, or cancelled (9) | 🎯 Release 1 | SPM-68 |
| NT15 | Registered Attendees | rescheduled, postponed, or cancelled (9) | 🎯 Release 1 | SPM-69 |
| NT16 | Attendees | registration opens or closes (10) | 🎯 Release 1 | SPM-70 |
| NT17 | Assigned Event Coordinator | availability change affects an upcoming event (14) | 🧩 Adjacent — core feature 9 | SPM-71 |
| NT18 | Assigned Event Coordinator | registration reaches capacity (11) | 🧩 Adjacent — core feature 18 | SPM-72 |
| NT19 | Waitlisted Attendee | waiting-list status changes (11) | 📋 Backlog | SPM-73 |
| NT20 | Assigned Event Coordinator | withdrawal affecting availability (12) | 📋 Backlog | SPM-74 |
| NT21 | Assigned Event Coordinator | incomplete arrangements (13) | 📋 Backlog | SPM-75 |

> [!warning] Four Release 1 notifications had no ticket at all before this split
> NT2, NT4, NT11 and NT16 are behaviours [[notifications|brief §6]] names explicitly and no card carried. NT11 is the starkest: "requested equipment is confirmed or found to be unavailable" is §6 row 6 verbatim, and the coordinator who raised the requirement was never told the outcome. They were invisible precisely because notifications were scattered as one-line ACs across other people's cards — which is the argument for the split, not an incidental finding.

### NT1 — Notification delivery mechanism

**Story:** As a system user — invoking the standard's §4.2 "genuinely universal capability" exception: every role receives notifications through the same mechanism, only the triggering scenarios and content differ by role and relationship to the event — I want to be notified when something relevant to an event I'm involved in happens, so I don't have to check manually.

**AC:**
- Notifications are delivered both in-app and by email, configurable per notification type ([#37](https://github.com/SinYang13/IS212-2026/discussions/37))
- A notification never discloses information the recipient is not otherwise entitled to see — e.g. an Attendee's notification never surfaces internal planning information ([#91](https://github.com/SinYang13/IS212-2026/discussions/91))
- Email is treated as a notification-channel requirement, not a general integration requirement ([#79](https://github.com/SinYang13/IS212-2026/discussions/79))
- Out of scope for Release 1: reminders (NT21) and waiting-list status changes (NT19), since the underlying features are `📋 Backlog` ([[release-1-scope]])

**Open question:** batch vs. real-time delivery is explicitly the team's choice, with no delivery-time SLA either way ([#55](https://github.com/SinYang13/IS212-2026/discussions/55)) — not written as an AC per §5.5, since no number was stated. Notification latency must not be treated as a performance target: [[non-functional-requirements|brief §8]]'s performance requirement covers common operations, and notification delivery is not among them.

### NT2 — Notify the Operations Manager of a new event request · 🎯

**Story:** As an Event Operations Manager, I want to be notified when an Event Organiser submits a new event request, so that I can assign a coordinator without polling the pending queue.

**AC:**
- Given an Event Organiser submits an event request, then the Event Operations Manager receives a notification ([[notifications|brief §6]] trigger 1)
- The notification names the requesting client organisation, the event, and its requested date(s)

**Open question:** whether the Event Operations Manager is modelled as a system role with a defined visibility scope at all is an unresolved gap ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93), [#94](https://github.com/SinYang13/IS212-2026/discussions/94); [[user-roles]] "Gap: The Event Operations Manager") — this card's recipient depends on the answer.

### NT3 — Notify a coordinator of their assignment · 🎯

**Story:** As an Event Coordinator, I want to be notified when an event is assigned to me, so that I can begin reviewing it without watching a queue.

**AC:**
- Given the Event Operations Manager assigns me to an event, then I receive a notification ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [[notifications|brief §6]] trigger 2)
- The notification names the event, its client organisation, and its requested date(s)
- The notification is informational, not an invitation — assignment takes effect immediately with no acceptance step ([#95](https://github.com/SinYang13/IS212-2026/discussions/95))
- Given I am reassigned off an event, then the newly assigned coordinator receives this notification and I do not ([#94](https://github.com/SinYang13/IS212-2026/discussions/94))

*Decoupled from EV3a's AC1 (SPM-32).*

### NT4 — Notify the organiser that their event has a coordinator · 🎯

**Story:** As an Event Organiser, I want to be notified when a coordinator is assigned to my event, so that I know who my point of contact at ConnectSphere is.

**AC:**
- Given a coordinator is assigned to my event, then I receive a notification ([[notifications|brief §6]] trigger 2)
- The notification names the assigned Event Coordinator, ConnectSphere's main internal point of contact for the event ([[connectsphere-event-services|brief §2]])
- Given the event is later reassigned, then I receive a further notification naming the new coordinator ([#94](https://github.com/SinYang13/IS212-2026/discussions/94))

### NT5 — Notify the organiser of a clarification request · 🎯

**Story:** As an Event Organiser, I want to be notified when my coordinator asks for clarification or an amendment, so that my request is not stalled waiting on me.

**AC:**
- Given the assigned Event Coordinator sends a clarification or amendment request, then I receive a notification ([[notifications|brief §6]] trigger 3; [[event-request-workflow]] Steps 4–5)
- The notification states what has been asked for
- The clarification and my response are retained with the event record ([#87](https://github.com/SinYang13/IS212-2026/discussions/87))

*Decoupled from EV3b's AC2 (SPM-33).*

### NT6 — Notify the organiser of the review decision · 🎯

**Story:** As an Event Organiser, I want to be notified when my event request is approved, rejected, or returned, so that I know whether planning has begun.

**AC:**
- Given the coordinator approves, rejects, or returns my request, then I receive a notification naming which of the three occurred ([[notifications|brief §6]] trigger 4)
- Given approved, the notification states that planning may begin but that ConnectSphere is not yet committed ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [[event-lifecycle]] "Approved vs Confirmed")
- Given rejected or returned, the notification carries the recorded reason

**Open question:** whether a rejected request can be resubmitted, or rejection is terminal, is left to the team ([#67](https://github.com/SinYang13/IS212-2026/discussions/67)) — it determines whether the act-on-it path applies to rejection as well as return.

*Decoupled from EV3c's AC4 (SPM-34).*

### NT7 — Notify venue staff of a new booking request · 🎯

**Story:** As a Venue Staff member, I want to be notified when a coordinator submits a booking request for a venue I manage, so that requests do not sit unreviewed.

**AC:**
- Given an Event Coordinator submits a venue booking request, then Venue Staff receive a notification ([[notifications|brief §6]] trigger 5)
- The notification names the venue, the date(s), and the AM/PM/Night slot(s) requested ([#50](https://github.com/SinYang13/IS212-2026/discussions/50))

**Open question:** whether Venue Staff are scoped to particular venues or see every venue's requests is not stated — see [[access-control]].

*Decoupled from VN5's AC4 (SPM-46) and V5's AC1 (SPM-22), which stated the same requirement twice — the duplication only became visible once the notifications were gathered in one place.*

### NT8 — Notify the coordinator of the booking outcome · 🎯

**Story:** As an Event Coordinator, I want to be notified when Venue Staff approve or reject my booking request, so that I know whether the venue is secured.

**AC:**
- Given Venue Staff decide a request I submitted, then I receive a notification naming the outcome ([[notifications|brief §6]] trigger 5; [[booking]] States)
- Given rejected, the notification carries the reason and any alternative suggested — an informal suggestion, not a formal counter-offer ([#45](https://github.com/SinYang13/IS212-2026/discussions/45))

*Decoupled from V5's AC5 (SPM-22).*

### NT9 — Notify venue staff of a cancelled booking · 🎯

**Story:** As a Venue Staff member, I want to be notified when a confirmed booking is cancelled, so that I do not prepare a venue for an event that is not happening.

**AC:**
- Given the assigned coordinator cancels a Confirmed booking, then Venue Staff receive a notification ([[notifications|brief §6]] trigger 5; [#68](https://github.com/SinYang13/IS212-2026/discussions/68))
- The notification names the venue and the slot(s) released — the Confirmed → Released transition ([[booking]] States)

*Decoupled from V2's AC3 (SPM-20).*

### NT10 — Notify technical support of an equipment requirement · 🎯

**Story:** As Technical Support Staff, I want to be notified when an event's equipment requirements are recorded or changed, so that I can check availability and reserve in time.

**AC:**
- Given a coordinator records or changes an equipment requirement line, then Technical Support Staff receive a notification ([[notifications|brief §6]] trigger 6)
- The notification names the event, its date(s), and the equipment type(s) and quantity requested
- Given the requirement changed after equipment was already reserved, the notification states that the reservation needs re-review rather than being carried over ([#4](https://github.com/SinYang13/IS212-2026/discussions/4), [#16](https://github.com/SinYang13/IS212-2026/discussions/16))

**Open question:** this addresses the brief's named pain point of Technical Support Staff receiving insufficient notice ([[connectsphere-event-services|brief §2]]), but no minimum lead time is stated anywhere — whether a late requirement should warn as well as notify is unspecified.

*Decoupled from EQ2's AC4 (SPM-41).*

### NT11 — Notify the coordinator of the equipment outcome · 🎯

**Story:** As an Event Coordinator, I want to be notified when requested equipment is confirmed or found to be unavailable, so that I can arrange an alternative before the event.

**AC:**
- Given Technical Support Staff reserve the requested equipment, then the assigned coordinator receives a notification naming the item type(s) and quantity reserved ([[notifications|brief §6]] trigger 6)
- Given the equipment cannot be supplied — none of the type free for the D-1 → Return Day + 1 window ([#5](https://github.com/SinYang13/IS212-2026/discussions/5)), or all candidates unavailable, damaged, or under maintenance ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) — then the coordinator is notified, with the shortfall stated

**Open question:** whether an unavailable-equipment notification should also block or flag confirmation depends on whether that equipment is "essential", and how essentiality is determined is explicitly left to the team ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [[event-readiness]]).

### NT12 — Notify the organiser that their event is confirmed · 🎯

**Story:** As an Event Organiser, I want to be notified when my event is confirmed, so that I can rely on the arrangements ConnectSphere has made.

**AC:**
- Given the assigned coordinator confirms the event, then the Event Organiser receives a notification ([[notifications|brief §6]] trigger 7)
- The notification states that the event is Confirmed — the point at which ConnectSphere is committed, unlike Approved ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))
- The confirmed arrangements are viewable from it ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))

*Decoupled from RA3's AC4 (SPM-50). Organiser only, per the no-self-notification rule above.*

### NT13 — Notify the coordinator of a change request · 🎯

**Story:** As the assigned Event Coordinator, I want to be notified when my Event Organiser requests a significant change, so that arrangements already made can be re-checked promptly.

**AC:**
- Given the Event Organiser submits a change request against an event assigned to me, then I receive a notification ([[notifications|brief §6]] trigger 8)
- The notification states what change is requested and whether it was classified significant or ordinary ([#4](https://github.com/SinYang13/IS212-2026/discussions/4), [[change-impact-management]])
- It fires for change requests against already-Confirmed events too — there is no change cut-off before the event date ([#8](https://github.com/SinYang13/IS212-2026/discussions/8))

*Decoupled from CH1's AC5 (SPM-52).*

### NT14 — Notify the organiser of a reschedule or cancellation · 🎯

**Story:** As an Event Organiser, I want to be notified when my event is rescheduled, postponed, or cancelled, so that I am never the last to know about my own event.

**AC:**
- Given the assigned coordinator reschedules, postpones, or cancels the event, then I receive a notification naming which occurred ([[notifications|brief §6]] trigger 9)
- Given rescheduled, the notification carries the new date and time
- Given cancelled, it states that the venue booking and equipment reservation have been released ([[event-lifecycle]] "Cancellation")

*Decoupled from the Organiser half of CH4's AC4 (SPM-55).*

### NT15 — Notify attendees of a reschedule or cancellation · 🎯

**Story:** As a registered Attendee, I want to be notified when an event I registered for is rescheduled or cancelled, so that I do not turn up to something that is not happening.

**AC:**
- Given an event with registrations is rescheduled, postponed, or cancelled, then every Attendee holding a Registered registration receives a notification ([[notifications|brief §6]] trigger 9)
- Given rescheduled, the notification carries the new date, time, and venue
- The notification carries only attendee-appropriate detail and never discloses internal planning information, the internally recorded reason, or any other attendee's details ([#91](https://github.com/SinYang13/IS212-2026/discussions/91))

**Open question:** whether a reschedule invalidates existing registrations or carries them over is not addressed by any source — the wording, and whether the Attendee must re-confirm, both depend on it.

*Decoupled from the Attendee half of CH4's AC4 (SPM-55). Directly addresses the brief's named pain point of Attendees receiving outdated event information ([[connectsphere-event-services|brief §2]]).*

### NT16 — Notify attendees when registration opens or closes · 🎯

**Story:** As an Attendee, I want to be notified when registration for a confirmed event opens or closes, so that I do not miss my chance to register.

**AC:**
- Given registration is enabled for a Confirmed event, then Attendees receive a notification that registration is open ([[notifications|brief §6]] trigger 10)
- Given registration is disabled, or its period ends, then Attendees receive a notification that it has closed ([#30](https://github.com/SinYang13/IS212-2026/discussions/30))

**Open question — blocks this card:** *which* Attendees receive an opening notification is undefined. Release 1 has no subscription, interest, or invitation model; external attendee onboarding is out of scope ([#49](https://github.com/SinYang13/IS212-2026/discussions/49), [#53](https://github.com/SinYang13/IS212-2026/discussions/53)); there is no admin role to curate a list ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)). A *closing* notification has an obvious recipient set — those already registered. An *opening* one does not. This is the same root cause as [[release-1-scope]]'s "Attendees can register but cannot be created" warning.

### NT17 — Notify the coordinator of an availability change · 🧩

**Scope:** `🧩 Adjacent` — pulled in by **core feature 9 (Venue availability calendar)**, whose requirement that "other recorded periods of unavailability should be reflected" is inert if a change to an already-booked period reaches nobody. Also serves core feature 16 on the equipment side.

**Story:** As the assigned Event Coordinator, I want to be notified when a venue or equipment availability change affects one of my upcoming events, so that I can make the necessary changes before the event.

**AC:**
- Given Venue Staff update a venue's availability, or Technical Support Staff change an equipment item's status, in a way that affects an upcoming event's existing arrangement, then that event's assigned coordinator receives a notification ([[notifications|brief §6]] trigger 14; [#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#9](https://github.com/SinYang13/IS212-2026/discussions/9))
- The notification names the event, the affected arrangement, and the nature of the change
- The system does not auto-cancel or auto-rebook — the Event Coordinator makes the necessary changes ([#54](https://github.com/SinYang13/IS212-2026/discussions/54))

**Open question:** how affected upcoming events are detected, and whether anything beyond raising the notification happens automatically, is left to the team — "propose an appropriate automation mechanism if required" ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)). That detection mechanism is the bulk of this card's cost.

*V4 (SPM-21) lets Venue Staff block availability and E1 (SPM-17) lets Technical Support Staff change equipment status, but neither propagated the consequence to the affected coordinator — the exact gap [#54](https://github.com/SinYang13/IS212-2026/discussions/54) was asked about.*

### NT18 — Notify the coordinator when registration reaches capacity · 🧩

**Scope:** `🧩 Adjacent` — pulled in by **core feature 18 (Attendee registration)**. Registration capacity is itself `🧩` in [[release-1-scope]] on the reasoning that registering without a ceiling against a capacity-checked venue is incoherent; a ceiling nobody is told about is the same defect one step later.

**Story:** As the assigned Event Coordinator, I want to be notified when an event reaches its registration capacity, so that I can decide whether to raise the cap or find a larger venue.

**AC:**
- Given registrations reach the event's registration capacity, then the assigned coordinator receives a notification ([[notifications|brief §6]] trigger 11)
- The notification names the event and its registration count against capacity

**Open question:** whether reaching capacity blocks further registration outright or merely warns is the same unresolved block-vs-warn question flagged against Reg3 — this notification's wording depends on it.

### NT19 — Notify an attendee of a waiting-list status change · 📋

**Scope:** `📋 Backlog` — the waiting list is not in Release 1 ([[release-1-scope]]). Kept for the product backlog and C4 models; do not pull into a Release 1 sprint.

**Story:** As a waitlisted Attendee, I want to be notified when my waiting-list status changes, so that I can accept a released place before the offer expires.

**AC:**
- Given a place is released and I am next in FIFO order, subject to manual override, then I receive a notification offering it ([#26](https://github.com/SinYang13/IS212-2026/discussions/26), [[notifications|brief §6]] trigger 11)
- The notification states the 3-day acceptance window and when it expires ([#44](https://github.com/SinYang13/IS212-2026/discussions/44))
- Can accept or decline the offered place from it
- Given no response within the window, the offer lapses and the next waitlisted Attendee is notified ([#44](https://github.com/SinYang13/IS212-2026/discussions/44))

**Open question:** this card is the only place the committed 3-day rule ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)) becomes user-visible — a non-negotiable customer rule sitting entirely in the backlog, which is worth confirming was intended.

### NT20 — Notify the coordinator of a withdrawal affecting availability · 📋

**Scope:** `📋 Backlog` — §6 trigger 12's qualifier presupposes a waiting list to promote from. With the waiting list `📋`, a Release 1 withdrawal simply decrements the count and frees the place to any new registrant (RG4) — a state change with no audience.

**Story:** As the assigned Event Coordinator, I want to be notified when a registered Attendee withdraws in a way that affects availability, so that I can act on the freed place.

**AC:**
- Given a registered Attendee withdraws from a full event, then the assigned coordinator receives a notification ([[notifications|brief §6]] trigger 12)
- The notification names the event and its resulting registration count against capacity

**Open question:** whether the coordinator needs this at all once waiting-list promotion is automatic (NT19) — if the next Attendee is notified directly, a coordinator notification may be pure noise.

### NT21 — Remind the coordinator of incomplete arrangements · 📋

**Scope:** `📋 Backlog` — the Reminder System is not named in the core 20 ([[release-1-scope]]); feature 20 lists event-driven triggers only. This is the only **time-driven** card in the set and the only reminder category the customer confirmed directly. A documented requirement, not a built one, in Release 1.

**Story:** As the assigned Event Coordinator, I want to be reminded while an upcoming event still has incomplete arrangements, so that nothing essential is left unbooked as the date approaches.

**AC:**
- Given an upcoming event still has an incomplete arrangement or a pending approval, then its assigned coordinator is reminded ([#47](https://github.com/SinYang13/IS212-2026/discussions/47), [[notifications|brief §6]] trigger 13)
- Reminders are delivered by batch, at a configurable frequency ([#47](https://github.com/SinYang13/IS212-2026/discussions/47))
- The reminder names the event, its date, and which arrangements are outstanding

**Open question:** lead time and the configuration surface are left entirely to the team — asked with concrete options and returned in full: "The timing and configurability are for you to propose" ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)).

*This is the reminder framing of the same condition §6 trigger 13 frames as a notification — see [[notification]] "Reminders (time-driven)". Note the interaction with RA3: the [#80](https://github.com/SinYang13/IS212-2026/discussions/80) gate blocks confirmation and **is** in scope; this card would have chased the coordinator beforehand and is not.*

---

## Next steps

1. **Conversation, not just cards.** Every open question listed above is a customer or team decision this document deliberately did not resolve (standard §11). Run these through the Week 12 customer Q&A or a team decision log before any card enters a sprint.
2. **Priority and Estimate.** Order this set 1..N by value ÷ cost, pulling high-risk items forward (the standard's Priority rule) — the unresolved venue-capacity and essentiality-determination questions (VN4, RA3) are good candidates to pull early, since they block estimation of several dependent cards.
3. **Reconcile with [[user-stories-proposed-rewrites]].** Retire Reg1 and Reg5 from active planning in favour of RG1 and RG4; keep Reg4 and Reg6 on the product backlog for a later release per [[release-1-scope]]'s "out of Release 1 ≠ out of the project" rule.
4. **Re-check against [[open-questions]]** before Ready — several open questions here (essentiality, significance classification, capacity block-vs-warn) are the highest-leverage unknowns in the whole backlog and recur across multiple cards.
