---
title: User Stories — Consolidated Proposed Rewrites
type: review
sources:
  - "outputs/user-stories-assignment-scrutiny.md"
tags: [review, user-stories, backlog, rewrites]
---

# User Stories — Consolidated Proposed Rewrites

Every `✅ Proposed rewrite` card and recommended new card from [[user-stories-assignment-scrutiny]], pulled together in one place as a ready-to-paste replacement backlog. The scrutiny document remains the source of truth for *why* each change was made (defect tables, Gate checks, citations) — this page only collects the *what*, in build order by section, so the team can lift cards straight into the backlog without re-reading the full review.

Two cards from the original assignment — **E3** (Clarify requirements with Event Coordinator) and **V3** (Update Venue Specifications in Catalogue) — were verdicted READY and have no proposed rewrite; they're listed as-is in [[user-stories-assignment-scrutiny]] §3 and §4.

All cards still need Priority and Estimate set by the team (Gate A7 — not fixed here) and should be re-checked against [[open-questions]] before entering a sprint.

---

## Users

### U1 — Log in to the system

**Story:** As a user — invoking the §4.2 "genuinely universal capability" exception: the authentication mechanism itself is identical for every role; only the post-login destination varies, and that's expressed as a conditional AC below, not folded into the role — I need to log in with my credentials so that I can access the functions permitted for my role.

**AC:**
- Can log in with a registered email address
- Cannot log in with an unregistered email address or an incorrect password; told only that the credentials are invalid, without indicating which field was wrong
- Given invalid credentials are submitted, then no session is created
- Is taken to the view appropriate to their role after logging in (see [[access-control]] "Role-Based Dashboards")

**Open questions:**
- Authentication method is unspecified by the customer — email/password, SSO, and MFA were all raised and none was chosen ([#62](https://github.com/SinYang13/IS212-2026/discussions/62), see [[user-account]]). This card assumes password-based login only because [[access-control]] treats it as the working default; confirm before build.
- Account recovery and locked-account handling are unspecified ([#64](https://github.com/SinYang13/IS212-2026/discussions/64)) — needs its own card once resolved, don't fold into this one.

*Removed from the original:* "Password is masked as it is entered" (UI hygiene → Definition of Done); the GWT scenario was compressed into the "no session is created" bullet.

---

### U2 — Log out of the system

**Story:** As a user, I need to log out of the system so that my session is not left open and accessible if my device is shared, mobile, or left unattended.

**AC:**
- Can log out from any page in the system
- Is returned to the login page after logging out
- Cannot return to a protected page using the browser back button after logging out

**Open question:** whether a session-inactivity timeout is required at all, and if so what duration, is not stated anywhere in the brief or the discussions (checked against [[access-control]] and [[non-functional-requirements]] §b). Raise with the customer.

*Removed from the original:* the automatic-inactivity-logout AC (no customer source); the "internal staff member" / "venue setup" framing.

---

### U3 — Role-appropriate navigation

**Story:** As a user — §4.2 exception: every role experiences the same mechanism (see only permitted functions), even though the specific functions shown differ — I need to see only the functions available to my role(s) so that I am not presented with actions I am not permitted to perform.

**AC:**
- Can see navigation options for the functions permitted to their role
- Cannot see navigation options for functions not permitted to their role
- Given an account holds more than one role, then navigation shows the union of functions permitted across all roles held ([#56](https://github.com/SinYang13/IS212-2026/discussions/56), see [[user-account]] Binding rules)
- Navigation is usable on both desktop and mobile screen sizes ([[non-functional-requirements]] §c; [[non-functional-requirements|brief §7]])

**Open question:** whether a role change takes effect immediately or only at the account's next login is not stated by the customer — [[user-account]] confirms no account-level lifecycle rule exists at all.

*Added vs. the original:* the multi-role union AC ([#56](https://github.com/SinYang13/IS212-2026/discussions/56)-confirmed rule the original omitted entirely).

---

### U4 — Clear access-denied response

**Story:** As a user — §4.2 exception: the denial mechanism is identical regardless of role — I need to be told clearly when I do not have access to something so that I know to contact the right person rather than assume a system fault.

**AC:** *(unchanged from the original — already passes Gates B and C)*
- Is shown a message stating that they do not have access when they attempt to reach a resource not permitted to them
- The message does not reveal whether the requested record exists or any of its details (see [[access-control]] "Protected Information", [#91](https://github.com/SinYang13/IS212-2026/discussions/91))
- The message indicates who to contact to request access
- Can return to a permitted page from the access-denied message

---

## Equipment

### E1 — Update status of returned equipment

**Story:** As Technical Support Staff, I need to update the operational status of a piece of equipment so that other staff can see whether it's available, damaged, unavailable, or under maintenance before relying on it for an event.

**AC:**
- Can view a piece of equipment's current operational status
- Can set the status to exactly one of: available, unavailable, damaged, or under maintenance — no other values ([#23](https://github.com/SinYang13/IS212-2026/discussions/23), see [[equipment-item]] States)
- Only Technical Support Staff can change equipment status ([#9](https://github.com/SinYang13/IS212-2026/discussions/9) — ⚠️ source confirms TSS's responsibility without explicitly ruling out a Coordinator-initiated flag; working default, not an airtight exclusion)
- Given equipment is returned from an event, when Technical Support Staff record a defect, damage, or missing item, then the operational status is updated to match ([#86](https://github.com/SinYang13/IS212-2026/discussions/86), see [[equipment-reservation]] "Defect / return-inspection log")
- Updated status is immediately reflected on the equipment catalogue and on any in-progress reservation review

*Changed vs. the original:* enumerated the exact four status values instead of "etc."; added the return-inspection/defect-logging AC the title promised but the original never delivered; tightened "wherever the equipment is displayed" to the two screens that actually matter.

---

### E2 — Reserve suitable equipment for an event request

**Story:** As Technical Support Staff, I need to see a list of suitable equipment for an event request so that the event's equipment needs are met without manually cross-checking spreadsheets.

**AC:**
- Can view the equipment requirements recorded for an event request
- Can view possible equipment matches of the requested type for the event's date and time, accounting for the transfer/preparation time needed to move each candidate item from its current location to the venue ([#2](https://github.com/SinYang13/IS212-2026/discussions/2), see [[equipment-item]] "Availability lifecycle")
- List excludes equipment that is operationally unavailable, damaged, or under maintenance ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)), or already committed to another event's D-1 → Return Day+1 window for an overlapping period ([#5](https://github.com/SinYang13/IS212-2026/discussions/5))
- Each listed item shows its type, available quantity, location, and status
- Can reserve listed equipment against the event request, up to the available quantity ([#13](https://github.com/SinYang13/IS212-2026/discussions/13))

**Open question:** how transfer/preparation time and an item's current location are represented is explicitly left to the team ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)) — needed before the "accounting for transfer time" AC can actually be built and tested.

*Changed vs. the original:* fixed the WHY (was restating the action); added the transfer-time factor the original silently dropped; split the "not available or already reserved" bullet into its two distinct customer rules.

---

## Venue

### V1 — Automatic Venue Availability Update upon Booking Confirmation

**Story:** As a Venue Staff member, I want the system to automatically mark a venue's slot as confirmed once I approve a booking, so that we prevent double bookings.

**AC:**
- Given a pending venue booking request for a specific venue and slot(s), when Venue Staff approve and confirm the request, then the system sets the venue's status for those slot(s) to "Confirmed" in the Venue Availability Calendar (see [[venue]] States)
- Given a venue slot is Confirmed, when another booking request is submitted for the same venue and an overlapping slot, then the request is blocked outright, not merely warned ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), [#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- The occupancy used for conflict detection includes the venue's setup time before, and turnaround/reset time after, the published event times — not the published times alone ([[event-planning-venue-booking-system|brief §4]], see [[booking]] "Effective occupancy ≠ published event times")

*Changed vs. the original:* "Unavailable" → "Confirmed" (matches [[venue]]'s actual state name); "those dates and times" → "those slot(s)" (matches the fixed AM/PM/Night model, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)); reshaped into a proper Given/When/Then; added the setup/turnaround padding rule.

---

### V2 — Cancel Venue Booking and Free Up Availability

**Story:** As the assigned Event Coordinator, I want to cancel a venue booking, so that the venue's slot is immediately freed up for other coordinators to book.

**AC:**
- Only the assigned Event Coordinator for the event may cancel its venue booking, and may do so at any stage ([#68](https://github.com/SinYang13/IS212-2026/discussions/68) — one of the project's eight non-negotiable rules, see [[index]]'s "Hard rules" callout)
- Given the assigned Event Coordinator cancels a Confirmed booking, then its status changes to Released and the Venue Availability Calendar immediately shows the venue's slot(s) as Available again (see [[booking]] States)
- Venue Staff are notified that the booking has been cancelled, since they own physical venue preparation ([[notifications]])

**Open question:** whether Venue Staff ever need their own cancellation trigger (e.g. a venue becoming permanently unusable) is a different scenario from Event-Coordinator-initiated cancellation, and [#68](https://github.com/SinYang13/IS212-2026/discussions/68) doesn't speak to it — if needed, that's a separate card.

*Changed vs. the original:* role corrected from "venue staff member" to "the assigned Event Coordinator," per the hard rule the original contradicted; Venue Staff is now recipient of a notification, not the actor performing the cancellation.

---

### V4 — Indicate venue is unavailable due to maintenance

**Story:** As a venue staff member, I want to block a venue's availability for operational reasons such as maintenance or renovation, so that coordinators cannot request bookings during those periods.

**AC:**
- Venue Staff is on the Venue Unavailability Management screen
- They create an unavailability block specifying a venue, one or more affected slots (AM/PM/Night) per affected day, and a reason (e.g. "Maintenance") ([#50](https://github.com/SinYang13/IS212-2026/discussions/50), see [[venue-booking]] "Venue Unavailability and Blocking")
- The Venue Availability Calendar displays the blocked slot(s) as "Blocked" (see [[venue]] States)
- The system prevents Event Coordinators from completing a booking request for this venue if it overlaps a blocked slot

*Changed vs. the original:* "start date/time, end date/time" → "one or more slots (AM/PM/Night) per affected day," matching the customer's fixed slot model.

---

### V5 — Review and Decide on Venue Booking Requests

**Story:** As a venue staff member, I want to review pending booking requests and provide approval or rejection details, so that coordinators are clear on the outcome of their requests.

**AC:**
- The Venue Staff gets notified of a new venue booking request submitted by an Event Coordinator ([[notifications]])
- The Venue Staff reviews the request and indicates whether to approve or reject it
- If rejected, they can input a reason for rejection and/or suggest an alternative venue or arrangement — an informal suggestion, not a formal counter-offer ([#45](https://github.com/SinYang13/IS212-2026/discussions/45), see [[booking]] States)
- The system changes the status of the booking request accordingly, to either "Approved" or "Rejected"
- The system notifies the Event Coordinator of the outcome

*Changed vs. the original:* "Reject" → "Rejected" (the enum value has to actually be reachable); noted the "informal suggestion, not counter-offer" nuance from [#45](https://github.com/SinYang13/IS212-2026/discussions/45).

---

### V6 — Configure Venue Turnaround and Setup Buffer

*(Full rewrite — the original card body was a verbatim copy-paste of V5.)*

**Story:** As a venue staff member, I want to record the setup time and turnaround/reset time for a venue, so that bookings for that venue are automatically padded with the correct preparation and reset windows.

**AC:**
- Can set a setup time (before the published start) and a turnaround/reset time (after the published end) independently for each venue ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))
- Given a venue booking's effective occupancy is calculated, when the venue has a setup and/or turnaround time recorded, then that time is added to the published event start/end to determine the padded occupancy window ([[event-planning-venue-booking-system|brief §4]], *Setup and Turnaround Time*; see [[booking]] "Effective occupancy ≠ published event times")
- Given a new booking request would overlap another booking's padded occupancy window, then the request is blocked, consistent with the double-booking rule ([#35](https://github.com/SinYang13/IS212-2026/discussions/35))
- Setup and turnaround values are per-venue, not a single system-wide default ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))

**Open question:** whether turnaround time sits *on top of* the fixed AM/PM/Night slot boundaries or is expected to be absorbed within the existing unbooked gaps (12–1pm, 6–7pm) is not stated by the customer — [[venue-booking]] flags this tension; raise with the customer.

---

## Registration

### Reg1 — Register for an event

**Story:** As an Attendee, I want to register for an event so that I can secure my place.

**AC:**
- Given an event is not Confirmed, when I search for it, then it is unavailable for registration ([[event-planning-venue-booking-system|brief §4]], [[event-request-workflow]] step 11)
- Given an event is Confirmed, registration is enabled, and I am within the registration period, when I submit a valid registration, then a registration is created and shown to me ([#30](https://github.com/SinYang13/IS212-2026/discussions/30), [[event-planning-venue-booking-system|brief §4]])
- Given the event is at capacity when I submit, then I am placed on the waiting list instead of being registered ([#26](https://github.com/SinYang13/IS212-2026/discussions/26), [[event-planning-venue-booking-system|brief §4]] — see Reg4 below)
- I receive confirmation of my registration (event name, date, time, venue)

**Open question:** what information is mandatory to supply at registration is explicitly undecided by the customer — "propose an appropriate set of information based on the event needs" ([#22](https://github.com/SinYang13/IS212-2026/discussions/22), see [[registration]]) — and may vary per event rather than being one fixed global set.

*Changed vs. the original:* dropped the unqualified "mandatory fields" validation AC in favour of an explicit open question; added the at-capacity → waiting-list branch.

---

### Reg2 — Enable registration for an event

**Story:** As an Event Coordinator, I want to enable or disable registration for an event, so that only events that require attendee sign-up collect registration.

**AC:**
- Given I am the Event Coordinator assigned to an event, when I open its registration settings, then I can set registration to enabled or disabled
- Given registration is disabled, when an Attendee views the event, then they cannot register for it

*Removed from the original:* the "actor, old value, new value, timestamp written to activity history" AC — restates the system-wide auditability requirement ([#14](https://github.com/SinYang13/IS212-2026/discussions/14), [[non-functional-requirements]] §f); state once in the Definition of Done, not per card.

---

### Reg3 — Set registration capacity for an event

**Story:** As an Event Coordinator, I want to set the maximum number of registrations for an event, so that sign-ups stay within what the venue and programme can accommodate.

**AC:**
1. Given registration is enabled, when I enter a capacity, then the value is stored and shown on the event record — set independently of the venue's own capacity, resolving the open question of whether registration capacity is coordinator-set or venue-derived ([[attendee-registration]] "Team decision required — event vs venue vs registration capacity")
2. Given the event has a confirmed venue, when I enter a capacity above the venue's maximum capacity, then the system warns me before saving — resolving the brief's hedged "should not normally be treated as suitable" rule as an overridable warning rather than a hard block ([[venue-suitability]] "Capacity vs Expected Attendance")
3. Given a capacity is set, when I attempt to lower it below the current number of confirmed registrations, then the system warns me and requires confirmation
4. Given the capacity is reached, when the Coordinator views the event, then the registration count and remaining places are visible

*Removed from the original:* the "recorded in the change history" AC — same Definition-of-Done move as Reg2.
*Changed vs. the original:* ACs 1 and 2 now name, in-line, which previously-open customer question each one resolves (Gate D5).

---

### Reg4 — Be Promoted From the Waiting List *(new — no card previously existed)*

**Story:** As an Attendee, I want to be offered a released spot from the waiting list in the order I joined, so that I have a fair chance to attend if space frees up.

**AC:**
- Waiting-list offers are made in FIFO order, subject to a manual override ([#26](https://github.com/SinYang13/IS212-2026/discussions/26))
- Given an Attendee is offered a spot, when they respond within 3 days, then they are registered ([#44](https://github.com/SinYang13/IS212-2026/discussions/44))
- Given an Attendee does not respond within 3 days of being offered a spot, when the window lapses, then the offer expires ([#44](https://github.com/SinYang13/IS212-2026/discussions/44))

**Open questions:**
- What happens to the freed spot once an offer expires — auto-pass to the next queued entry, return this entry to the back of the queue, or removal — is not specified and blocks estimation of the full flow ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)).
- Who may exercise the manual override is not named by the customer; the Event Coordinator is the natural candidate given they own the registration period ([#30](https://github.com/SinYang13/IS212-2026/discussions/30)), but this needs confirmation ([#26](https://github.com/SinYang13/IS212-2026/discussions/26)).

---

### Reg5 — Withdraw an Event Registration *(new — no card previously existed)*

**Story:** As an Attendee, I want to withdraw my registration for an event, so that my place can be released to someone else if I can no longer attend.

**AC:**
- Can withdraw a Registered registration ([[event-planning-venue-booking-system|brief §4]])
- Given an Attendee withdraws and a waiting list exists for that event, when the withdrawal is processed, then the next queued entry is offered the freed spot ([#44](https://github.com/SinYang13/IS212-2026/discussions/44), [[event-planning-venue-booking-system|brief §4]])

**Open question:** whether withdrawal has a cut-off before the event date is not stated ([[event-planning-venue-booking-system|brief §4]] says only "where permitted"); note there is no general change cut-off for events either ([#8](https://github.com/SinYang13/IS212-2026/discussions/8)) — raise with the customer rather than assume a deadline.

---

### Reg6 — Check In a Registered Attendee *(new — no card previously existed)*

**Story:** As *[role not named by the customer — see open question]*, I want to check an Attendee in on arrival by scanning a QR code or entering their details manually, so that ConnectSphere has an accurate attendance record distinct from registration status.

**AC:**
- Can check in an Attendee via QR-code scan or manual entry ([#38](https://github.com/SinYang13/IS212-2026/discussions/38))
- Only Attendees with a Registered status can be checked in ([[event-planning-venue-booking-system|brief §4]] requires registration and attendance to stay distinguishable)
- Attendance is not self-reported and is not auto-marked without staff action ([#38](https://github.com/SinYang13/IS212-2026/discussions/38))

**Open question:** which internal role performs check-in is not named — the source says only "manually by staff" ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)). Do not default to Event Coordinator or Venue Staff without confirming.

---

> [!info] Not yet a card — spike recommended
> The VIP / non-self-registering-attendee case ([#33](https://github.com/SinYang13/IS212-2026/discussions/33): an Event Organiser cannot add attendees directly; the list goes to the Event Coordinator "for processing") has too many unknowns — mechanism (manual entry, bulk import, off-system handover) and capacity/waiting-list interaction are both unspecified. Per the standard's §6.1 (Estimable) and §1 (Spike), this needs a spike, not a sized story, before it becomes Reg7.

---

## Event

### EV1 — Assignment of event requests

**Story:** As an Event Operations Manager, I want to see all pending event requests, so I can assign an Event Coordinator to each event.

**AC:**
- Receive a notification that a new event has been requested ([[notifications]])
- Be able to see all the pending event requests
- Be able to see all Event Coordinators — selection is entirely the manager's own judgement, following an SOP maintained outside the system; the system does not compute eligibility ([#93](https://github.com/SinYang13/IS212-2026/discussions/93), see [[user-account]] Binding rules)
- Be able to assign an Event Coordinator to an event; the assignment takes effect immediately with no acceptance step ([#95](https://github.com/SinYang13/IS212-2026/discussions/95))

**Open question:** whether the Event Operations Manager is formally modelled as a system role at all — with a defined visibility scope — is an unresolved gap between the brief's role table and the discussions ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93), [#94](https://github.com/SinYang13/IS212-2026/discussions/94); see [[user-roles]] "Gap: The Event Operations Manager").

*Changed vs. the original:* "eligible for assignment" → "all Event Coordinators" (contradicted [#93](https://github.com/SinYang13/IS212-2026/discussions/93)'s "no criteria need to be encoded"); removed the assignment-"failure" AC entirely (no failure/decline path per [#95](https://github.com/SinYang13/IS212-2026/discussions/95)); split reassignment out as its own companion card below.

**Companion card — Reassign an Event Coordinator** *(recommended, not previously in the backlog)*

As an Event Operations Manager, I want to reassign an event to a different Event Coordinator, so that coverage can change after initial assignment.
- AC: can reassign an already-assigned event to a different coordinator; the system may assume any reassignment entered has already been approved outside the system, with no in-system approval step ([#94](https://github.com/SinYang13/IS212-2026/discussions/94)).

---

### EV2 — Submission of event requests

**Story:** As an Event Organiser, I want to indicate my event requirements and submit a request, so that ConnectSphere can begin planning without me having to re-enter information later.

**AC:**
- Can save a draft event request before submission, and return to complete or edit it later (brief step 1, see [[event-request-workflow]] Step 1)
- Can submit the request once ready
- Given complete and valid information, when the Event Organiser submits, then they are told the event has been successfully requested
- Given invalid or incomplete information, when the Event Organiser submits, then they are told exactly what is missing or invalid

**Open question:** the mandatory field set for submission is undecided by the customer — "has not been firmed up... please propose an appropriate set" ([#72](https://github.com/SinYang13/IS212-2026/discussions/72), see [[event-request]]).

*Changed vs. the original:* fixed the WHY (was restating the action); added the draft-save AC the brief specifically calls out and the original omitted entirely; surfaced the [#72](https://github.com/SinYang13/IS212-2026/discussions/72) open question explicitly.

---

### EV3 — Review and clarification of event requirements *(split into three cards)*

The original single card's title over-promised and its 2 ACs under-delivered. Replaced with three independently testable, independently valuable cards:

**EV3a — Review an assigned event request**

As the assigned Event Coordinator, I want to see the details of events assigned to me, so I can judge whether enough information has been provided to proceed.
- Receive a notification when an event request is assigned to me ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [[notifications]])
- Can see the full details and requirements submitted for the event

**EV3b — Request clarification on an event request**

As the assigned Event Coordinator, I want to request clarification or amendment from the Event Organiser, so that incomplete or ambiguous requirements are resolved before I decide on the request.
- Can send a clarification/amendment request to the Event Organiser against a specific request ([[event-request-workflow]] Steps 4–5)
- The Event Organiser is notified of the request for clarification ([[notifications]])
- The clarification request and the Organiser's response are retained with the event record (extends the same retain-with-record pattern [#87](https://github.com/SinYang13/IS212-2026/discussions/87) establishes for technical clarifications)
- Given the Organiser resubmits after amendment, then the coordinator's review resumes

**EV3c — Approve, reject, or return an event request**

As the assigned Event Coordinator, I want to approve, reject, or return an event request, so that only requests with sufficient information proceed to planning.
- Can approve a request, which allows planning to begin but commits ConnectSphere to nothing yet ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), see [[event]] States)
- Can reject a request, retaining a decision record (see [[event-request]] States)
- Can return a request to the Organiser for amendment
- The Event Organiser is notified of the decision ([[notifications]])

**Open question:** whether a rejected request can be resubmitted, or rejection is terminal, is left to the team — "propose an appropriate approach if needed" ([#67](https://github.com/SinYang13/IS212-2026/discussions/67)).

---

## Summary — cards in this consolidated set

| # | Card | Status |
|---|---|---|
| U1 | Log in to the system | Rewrite |
| U2 | Log out of the system | Rewrite |
| U3 | Role-appropriate navigation | Rewrite |
| U4 | Clear access-denied response | Rewrite (AC unchanged, role/WHY fixed) |
| E1 | Update status of returned equipment | Rewrite |
| E2 | Reserve suitable equipment for an event request | Rewrite |
| V1 | Automatic Venue Availability Update upon Booking Confirmation | Rewrite |
| V2 | Cancel Venue Booking and Free Up Availability | Rewrite (actor corrected) |
| V4 | Indicate venue is unavailable due to maintenance | Rewrite |
| V5 | Review and Decide on Venue Booking Requests | Rewrite |
| V6 | Configure Venue Turnaround and Setup Buffer | Full rewrite (was a copy-paste bug) |
| Reg1 | Register for an event | Rewrite |
| Reg2 | Enable registration for an event | Rewrite |
| Reg3 | Set registration capacity for an event | Rewrite |
| Reg4 | Be Promoted From the Waiting List | New |
| Reg5 | Withdraw an Event Registration | New |
| Reg6 | Check In a Registered Attendee | New |
| EV1 | Assignment of event requests | Rewrite |
| — | Reassign an Event Coordinator | New (companion to EV1) |
| EV2 | Submission of event requests | Rewrite |
| EV3a–c | Review / Request clarification / Approve-reject-return | Split from EV3 |

E3 and V3 are unchanged (READY as originally written) and are not repeated here — see [[user-stories-assignment-scrutiny]].

See [[user-stories-assignment-scrutiny]] for the full defect analysis behind every change, and [[open-questions]] for the consolidated register of everything still left to the customer or team.
