---
title: Attendee Registration
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
tags:
  - registration
  - attendees
  - waiting-list
  - capacity
  - connectsphere
---

# Attendee Registration

How Attendees register for events in the [[event-planning-venue-booking-system]], and how capacity, waiting lists, withdrawal, and attendance are handled. Registration is step 11 of the 14-step process and sits downstream of event confirmation — see [[event-lifecycle]].

## Preconditions

Registration is available only when **both** conditions hold (Source: customer-brief-no-1, s4, s5):

1. The event is **confirmed**.
2. Registration is **enabled** for that event ("Where enabled, Attendees can register for confirmed events").

Not every event requires registration. Registration setup is one of the arrangements that may or may not be essential for a given event — "some events may not require equipment, technical support, or attendee registration" (Source: is212-discussions-2026-08-31, #80). See [[event-readiness]].

## Registration Period

| Rule | Detail | Source |
|---|---|---|
| Separate dates | There is a **registration start date and end date, separate from the event date** | #30 |
| Who sets them | The **Event Coordinator** | #30 |
| Enforcement | Attendees should only be able to register when registration is available | brief s4 |
| Extension / re-opening | The registration period **may be extended** — registration can be re-opened after closing, e.g. when additional capacity becomes available | #42 |

(Source: is212-discussions-2026-08-31, #30, #42)

Implications:

- Registration windows are per-event data owned by the Event Coordinator, not derived from the event date.
- Because the period is extensible, closure must be modelled as a **mutable date**, not a terminal state. A closed registration can return to open.
- Registration opening and closing are notification triggers (Source: customer-brief-no-1, s6). See [[notifications]].

> [!question]- Team decision required — re-opened period vs waiting list
> Whether a re-opened registration period is a new period or an edit to the existing one, and whether waiting-list members are promoted before the period re-opens to the public. #42 confirms extension is allowed but does not sequence it against the waiting list.

## Capacity

- Registration may be limited according to the capacity defined for the event, and the system "should prevent or appropriately handle registrations beyond the permitted capacity" (Source: customer-brief-no-1, s4).
- Expected attendance and venue capacity are considered throughout planning; a significant attendance change may require the suitability of an existing venue booking to be reviewed (Source: customer-brief-no-1, s4). See [[venue-suitability]] and [[change-impact-management]].
- Registration numbers reaching capacity earlier than expected is named in the brief as a routine change scenario (Source: customer-brief-no-1, s2).
- Multi-session events may have **different attendance limits per session** (Source: is212-discussions-2026-08-31, #74).

> [!question]- Team decision required — event vs venue vs registration capacity
> The relationship between event capacity, venue capacity, and the registration limit. The brief treats them as related but does not state whether the registration cap is set independently by the coordinator or derived from the booked venue.

## Waiting List

| Rule | Detail | Source |
|---|---|---|
| Supported? | Yes — additional Attendees may join a waiting list when an event reaches capacity | brief s4, #44 |
| Promotion order | **FIFO**, with **manual override** permitted | #26 |
| Acceptance window | **3 days** for a waitlisted Attendee to accept when a spot opens, before it passes on | #44 |

(Source: is212-discussions-2026-08-31, #26, #44)

The 3-day window is one of the few **hard numeric business rules** the customer has committed to; it should be treated as a fixed requirement rather than a proposal. FIFO is the default ordering, but the manual override means promotion cannot be a purely automatic queue — an authorised user must be able to promote out of order.

Waiting-list status changes are a notification scenario (Source: customer-brief-no-1, s6). See [[notifications]].

> [!question]- Team decision required — waiting-list override and window expiry
> Who may exercise the manual override (#26 does not name a role — the Event Coordinator is the natural candidate given #30), and what happens when the 3-day window expires: automatic pass to the next in queue, return to the back of the queue, or removal.

## Withdrawal

- Attendees can withdraw their registration where permitted, and "the released place may subsequently become available to another Attendee" (Source: customer-brief-no-1, s4).
- A registered Attendee withdrawing, where that affects availability, is a notification scenario (Source: customer-brief-no-1, s6).

Withdrawal is the primary trigger that opens a spot and starts the 3-day waiting-list acceptance window under #44.

> [!question]- Team decision required — withdrawal cut-off
> Whether withdrawal is permitted up to the event date or has a cut-off. The brief says "where permitted" without defining the permission rule; note that no change cut-off exists for events generally (Source: is212-discussions-2026-08-31, #8).

## Attendees Who Do Not Self-Register

For attendees who are not self-registering through the system — for example VIPs — **the attendee list must be provided to the Event Coordinator for processing** (Source: is212-discussions-2026-08-31, #33).

The question asked whether an *Event Organiser* could manually add attendees; the answer routes this through the **Event Coordinator** instead. Two consequences:

- Event Organisers do not get a bulk-add or manual-add capability of their own.
- The system needs a coordinator-side path to create registration records for people who never authenticate, which must coexist with the capacity and waiting-list rules above.

> [!question]- Team decision required — coordinator-side attendee entry
> The mechanism for coordinator-side entry (manual entry, bulk import, or off-system handover with only headcount recorded), and whether such attendees consume capacity and can be checked in.

## Attendance Recording

Registration and attendance are distinguished — ConnectSphere may record whether registered Attendees ultimately attended (Source: customer-brief-no-1, s4).

**Method:** attendance is **manually checked in at the counter**, either using a **QR code** or **manually by staff** (Source: is212-discussions-2026-08-31, #38).

- Both paths are on-site and staff-mediated. Attendee self-reporting and automated check-in were asked about and not adopted.
- Attendance information may be recorded at completion, along with operational notes and issues (Source: customer-brief-no-1, step 14). See [[event-lifecycle]].
- QR code implies each registration needs a scannable identifier issued to the Attendee.

## Attendee Limits and Visibility

| Question | Answer | Source |
|---|---|---|
| Can an Attendee register for multiple events simultaneously? Any limit? | **No limit has been specified.** The customer invites a proposal if the solution requires restrictions | #48 |
| What can an Attendee see? | Events they have registered for **and** events open for registration | #10 |
| Do Attendees need an account? | Attendees are supported as external users who register and view registration status; **onboarding features are not listed** | #53 |

Attendees must not be able to view internal planning information (Source: customer-brief-no-1, s8b). See [[access-control]].

## Registration Information Fields

> [!question]- Team decision required — registration fields
> What information an Attendee must provide when registering. Asked directly — "Name only? Dietary needs? Accessibility requirements?" — and returned in full: "Please propose an appropriate set of information based on the event needs." (Source: is212-discussions-2026-08-31, #22).

The phrase "based on the event needs" suggests the field set may vary per event rather than being globally fixed; the brief similarly says Attendees "provide the **required** registration information" (Source: customer-brief-no-1, s4), implying per-event configuration.

## Reporting

Event Organisers and Event Coordinators can view or export attendee information where appropriate and subject to access restrictions; Event Organisers may do so **for events they organise** (Source: customer-brief-no-1, s4; is212-discussions-2026-08-31, #32). See [[reporting]] and [[access-control]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Registration information fields | **Team to propose** | #22 |
| Limits on events per Attendee | **Unspecified — team to propose if needed** | #48 |
| Who may exercise the waiting-list manual override | Not named | #26 |
| Behaviour when the 3-day acceptance window expires | Team to propose | #44 |
| Mechanism for coordinator-entered (non-self-registering) attendees | Team to propose | #33 |
| Whether registration capacity is derived from venue capacity or set independently | Team to propose | brief s4 |
| Withdrawal cut-off, if any | Team to propose | brief s4, #8 |

See [[open-questions]] for the consolidated list.
