---
title: Attendee Registration
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Sept 8.md
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

**Objects:** [[registration]] · [[event]] · [[user-account]]

How Attendees register for events in the [[event-planning-venue-booking-system]], and how capacity, waiting lists, withdrawal, and attendance are handled. Registration is step 11 of the 14-step process and sits downstream of event confirmation — see [[event-lifecycle]].

**Release 1 scope** ([[release-1-scope]]): Attendee registration and Withdrawal (below) are 🎯 Release 1 — core feature 18. Registration Period and Capacity are `🧩 Adjacent` — feature 18 is conditional on registration being "enabled", and registering against a capacity-checked venue with no ceiling is incoherent, so a minimum enabled-state and cap are needed. Waiting List and Attendance Recording are `📋 Backlog` — neither is named in the core 20.

## Preconditions

Registration is available only when **both** conditions hold ([[event-planning-venue-booking-system|brief §4]], [[event-request-workflow|brief §5]]):

1. The event is **confirmed**.
2. Registration is **enabled** for that event ("Where enabled, Attendees can register for confirmed events").

Not every event requires registration. Registration setup is one of the arrangements that may or may not be essential for a given event — "some events may not require equipment, technical support, or attendee registration" (Source: is212-discussions-2026-08-31, [#80](https://github.com/SinYang13/IS212-2026/discussions/80)). See [[event-readiness]].

## Registration Period

Registration open/close dates are set by the Event Coordinator on the [[event]] record, separate from the event dates, and may be extended after closing (Source: is212-discussions-2026-08-31, [#30](https://github.com/SinYang13/IS212-2026/discussions/30), [#42](https://github.com/SinYang13/IS212-2026/discussions/42)). Attendees should only be able to register when registration is available ([[event-planning-venue-booking-system|brief §4]]).

Implications:

- Registration windows are per-event data owned by the Event Coordinator, not derived from the event date.
- Because the period is extensible, closure must be modelled as a **mutable date**, not a terminal state. A closed registration can return to open.
- Registration opening and closing are notification triggers ([[notifications|brief §6]]). See [[notifications]].

> [!question]- Team decision required — re-opened period vs waiting list
> Whether a re-opened registration period is a new period or an edit to the existing one, and whether waiting-list members are promoted before the period re-opens to the public. [#42](https://github.com/SinYang13/IS212-2026/discussions/42) confirms extension is allowed but does not sequence it against the waiting list.

## Capacity

- Registration may be limited according to the capacity defined for the event, and the system "should prevent or appropriately handle registrations beyond the permitted capacity" ([[event-planning-venue-booking-system|brief §4]]).
- Expected attendance and venue capacity are considered throughout planning; a significant attendance change may require the suitability of an existing venue booking to be reviewed ([[event-planning-venue-booking-system|brief §4]]). See [[venue-suitability]] and [[change-impact-management]].
- Registration numbers reaching capacity earlier than expected is named in the brief as a routine change scenario ([[connectsphere-event-services|brief §2]]).
- Multi-session events may have **different attendance limits per session** (Source: is212-discussions-2026-08-31, [#74](https://github.com/SinYang13/IS212-2026/discussions/74)).
- Capacity is a **hard cap with no VIP exemption**: "VIPs may be added manually even when normal registration is full, but the overall event/venue capacity must still not be exceeded" (Source: is212-discussions-2026-09-08, [#99](https://github.com/SinYang13/IS212-2026/discussions/99)). This partially hardens the brief's hedge — if the strongest privileged case cannot exceed the cap, "appropriately handle" cannot mean "admit anyway" on the registration side. It says nothing about the separate venue-suitability rule, which remains hedged — see [[venue-suitability]].

> [!question]- Team decision required — event vs venue vs registration capacity
> The relationship between event capacity, venue capacity, and the registration limit. The brief treats them as related but does not state whether the registration cap is set independently by the coordinator or derived from the booked venue.

## Waiting List

Additional Attendees may join a waiting list when an event reaches capacity ([[event-planning-venue-booking-system|brief §4]], [#44](https://github.com/SinYang13/IS212-2026/discussions/44)). Promotion order, the manual override, and the 3-day acceptance window are defined on [[registration]] (Waiting List Entry) (Source: is212-discussions-2026-08-31, [#26](https://github.com/SinYang13/IS212-2026/discussions/26), [#44](https://github.com/SinYang13/IS212-2026/discussions/44)).

The 3-day window is one of the few **hard numeric business rules** the customer has committed to; it should be treated as a fixed requirement rather than a proposal. FIFO is the default ordering, but the manual override means promotion cannot be a purely automatic queue — an authorised user must be able to promote out of order.

Waiting-list status changes are a notification scenario ([[notifications|brief §6]]). See [[notifications]].

**No late-stage offer cut-off is mandated.** There is "currently no fixed rule requiring waiting-list offers to stop a certain number of days before the event", the 3-day acceptance window "still applies to an offer once it is made", and the team may propose a cut-off or handling rule near the event date "provided it is consistent with the registration period and event capacity" (Source: is212-discussions-2026-09-08, [#116](https://github.com/SinYang13/IS212-2026/discussions/116)). The two fixed facts collide at the tail of the timeline: an offer made two days out cannot run its full 3-day window before the event starts, so any proposal must say what happens to such an offer.

> [!question]- Team decision required — waiting-list override, window expiry and late-stage cut-off
> Who may exercise the manual override ([#26](https://github.com/SinYang13/IS212-2026/discussions/26) does not name a role — the Event Coordinator is the natural candidate given [#30](https://github.com/SinYang13/IS212-2026/discussions/30)); what happens when the 3-day window expires (automatic pass to the next in queue, return to the back of the queue, or removal); and whether offers stop being issued a set number of days before the event, which [#116](https://github.com/SinYang13/IS212-2026/discussions/116) leaves entirely to the team.

## Withdrawal

- Attendees can withdraw their registration where permitted, and "the released place may subsequently become available to another Attendee" ([[event-planning-venue-booking-system|brief §4]]).
- A registered Attendee withdrawing, where that affects availability, is a notification scenario ([[notifications|brief §6]]).

Withdrawal is the primary trigger that opens a spot and starts the 3-day waiting-list acceptance window under [#44](https://github.com/SinYang13/IS212-2026/discussions/44).

### Withdrawal after a reschedule

Existing registrations **may be retained when an event is rescheduled** — a reschedule does not void or re-queue them. Affected attendees "should be informed of the new date/time, either through this system or through another appropriate communication channel, and should be allowed to withdraw if they can no longer attend" (Source: is212-discussions-2026-09-08, [#115](https://github.com/SinYang13/IS212-2026/discussions/115)). Three readings follow:

- Retention is the default, so the reschedule path needs no bulk re-registration and no mass invalidation of [[registration]] records.
- The obligation is to *inform and allow withdrawal*, not to obtain re-confirmation. Nothing requires an attendee to opt back in.
- **The channel is not required to be in-system.** [#115](https://github.com/SinYang13/IS212-2026/discussions/115) permits "another appropriate communication channel", consistent with [#97](https://github.com/SinYang13/IS212-2026/discussions/97) and [#105](https://github.com/SinYang13/IS212-2026/discussions/105). See [[notifications]].

The customer also draws an explicit Release 1 boundary: "for Release 1, detailed handling of registration withdrawal arising specifically from event changes or cancellation is not required" ([#115](https://github.com/SinYang13/IS212-2026/discussions/115)). Ordinary attendee-initiated withdrawal is unaffected and remains a Release 1 obligation; only the change- and cancellation-driven variant is deferred. See [[change-impact-management]] and [[release-1-scope]].

> [!question]- Team decision required — withdrawal cut-off
> Whether withdrawal is permitted up to the event date or has a cut-off. The brief says "where permitted" without defining the permission rule; note that no change cut-off exists for events generally (Source: is212-discussions-2026-08-31, [#8](https://github.com/SinYang13/IS212-2026/discussions/8)).

## Attendees Who Do Not Self-Register

For attendees who are not self-registering through the system — for example VIPs — **the attendee list must be provided to the Event Coordinator for processing** (Source: is212-discussions-2026-08-31, [#33](https://github.com/SinYang13/IS212-2026/discussions/33)).

The question asked whether an *Event Organiser* could manually add attendees; the answer routes this through the **Event Coordinator** instead. Three consequences:

- Event Organisers do not get a bulk-add or manual-add capability of their own. [#99](https://github.com/SinYang13/IS212-2026/discussions/99) does not reverse this: its question again presupposed an Organiser doing the adding, but the answer speaks only to capacity and never revisits the routing, so [#33](https://github.com/SinYang13/IS212-2026/discussions/33) stands.
- The system needs a coordinator-side path to create registration records for people who never authenticate, which must coexist with the capacity and waiting-list rules above.
- That path may be exercised **after normal registration is full** — "VIPs may be added manually even when normal registration is full" — but it does **not** override capacity: "the overall event/venue capacity must still not be exceeded" (Source: is212-discussions-2026-09-08, [#99](https://github.com/SinYang13/IS212-2026/discussions/99)). A manual add is therefore a bypass of the *registration gate*, not of the *cap*.

> [!question]- Team decision required — coordinator-side attendee entry
> The mechanism for coordinator-side entry (manual entry, bulk import, or off-system handover with only headcount recorded), and whether such attendees consume capacity and can be checked in.

## Attendance Recording

Registration and attendance are distinguished — ConnectSphere may record whether registered Attendees ultimately attended ([[event-planning-venue-booking-system|brief §4]]).

**Method:** attendance is **manually checked in at the counter**, either using a **QR code** or **manually by staff** (Source: is212-discussions-2026-08-31, [#38](https://github.com/SinYang13/IS212-2026/discussions/38)).

**Who checks in:** an **authorised internal staff member involved in the event operations**. "No new registration-only role is required. The team may assign this responsibility to an appropriate existing internal role as part of its design, subject to the access-control model" (Source: is212-discussions-2026-09-08, [#38](https://github.com/SinYang13/IS212-2026/discussions/38) follow-up).

- Both paths are on-site and staff-mediated. Attendee self-reporting and automated check-in were asked about and not adopted.
- The follow-up rules out a dedicated check-in/registration-desk role, so no seventh role enters the role model. It leaves **which** existing internal role holds the capability as a 🔶 team decision ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)) — see [[user-roles]] and [[access-control]].
- The constraint is *internal* and *involved in the event operations*, which excludes the Attendee and the Event Organiser but does not itself pick between Event Coordinator, Venue Staff and Technical Support Staff.
- Attendance information may be recorded at completion, along with operational notes and issues (Source: customer-brief-no-1, step 14). See [[event-lifecycle]].
- QR code implies each registration needs a scannable identifier issued to the Attendee.

## Attendee Limits and Visibility

| Question | Answer | Source |
|---|---|---|
| Can an Attendee register for multiple events simultaneously? Any limit? | **No limit has been specified.** The customer invites a proposal if the solution requires restrictions | [#48](https://github.com/SinYang13/IS212-2026/discussions/48) |
| What can an Attendee see? | Events they have registered for **and** events open for registration | [#10](https://github.com/SinYang13/IS212-2026/discussions/10) |
| Do Attendees need an account? | Attendees are supported as external users who register and view registration status; **onboarding features are not listed**, and account onboarding for Release 1 was confirmed unnecessary — with pre-loaded accounts offered for staff and clients but **not** for Attendees. The team's answer: **no** — [[schema]] accepts a registration identified by a name-and-email pair where no account exists | [#53](https://github.com/SinYang13/IS212-2026/discussions/53), [#49](https://github.com/SinYang13/IS212-2026/discussions/49); [[registration]] |
| What must the system let an Attendee do? | Where registration is enabled: **view appropriate event information, register, view registration status, and withdraw** — four required Release 1 actions | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |
| Must Attendees use the app to get event information? | **No.** They "should be able to access appropriate event information through the system, but it is not a requirement that they MUST use the application as the only source"; updates may also travel through other appropriate channels | [#97](https://github.com/SinYang13/IS212-2026/discussions/97) |

Attendees must not be able to view internal planning information ([[non-functional-requirements|brief §8b]]). See [[access-control]].

[#49](https://github.com/SinYang13/IS212-2026/discussions/49) confirms all four attendee actions are Release 1 obligations while offering pre-loaded accounts only to ConnectSphere employees and Event Organisers. The account-less registrant therefore still has no credential behind *view status* and *withdraw* — that gap is live, and [#49](https://github.com/SinYang13/IS212-2026/discussions/49) does not close it. See [[access-control]] and [[release-1-scope]].

## Registration Information Fields

> [!question]- Team decision required — registration fields
> What information an Attendee must provide when registering. Asked directly — "Name only? Dietary needs? Accessibility requirements?" — and returned in full: "Please propose an appropriate set of information based on the event needs." (Source: is212-discussions-2026-08-31, [#22](https://github.com/SinYang13/IS212-2026/discussions/22)).

The phrase "based on the event needs" suggests the field set may vary per event rather than being globally fixed; the brief similarly says Attendees "provide the **required** registration information" ([[event-planning-venue-booking-system|brief §4]]), implying per-event configuration.

## Reporting

Event Organisers and Event Coordinators can view or export attendee information where appropriate and subject to access restrictions; Event Organisers may do so **for events they organise** ([[event-planning-venue-booking-system|brief §4]]; is212-discussions-2026-08-31, [#32](https://github.com/SinYang13/IS212-2026/discussions/32)). See [[reporting]] and [[access-control]].

## Open Items

| Item | Status | Reference |
|---|---|---|
| Registration information fields | **Team to propose** | [#22](https://github.com/SinYang13/IS212-2026/discussions/22) |
| Limits on events per Attendee | **Unspecified — team to propose if needed** | [#48](https://github.com/SinYang13/IS212-2026/discussions/48) |
| Who may exercise the waiting-list manual override | Not named | [#26](https://github.com/SinYang13/IS212-2026/discussions/26) |
| Behaviour when the 3-day acceptance window expires | Team to propose | [#44](https://github.com/SinYang13/IS212-2026/discussions/44) |
| Mechanism for coordinator-entered (non-self-registering) attendees | Team to propose | [#33](https://github.com/SinYang13/IS212-2026/discussions/33) |
| Whether registration capacity is derived from venue capacity or set independently | Team to propose | [[event-planning-venue-booking-system\|brief §4]] |
| Withdrawal cut-off, if any | Team to propose | [[event-planning-venue-booking-system\|brief §4]], [#8](https://github.com/SinYang13/IS212-2026/discussions/8) |
| Whether waiting-list offers stop close to the event date | **No fixed rule — team to propose**, consistent with the registration period and capacity | [#116](https://github.com/SinYang13/IS212-2026/discussions/116) |
| Handling of registrations on a reschedule beyond inform-and-allow-withdrawal | **Team to propose**; detailed change-/cancellation-driven withdrawal is **not required for Release 1** | [#115](https://github.com/SinYang13/IS212-2026/discussions/115) |
| Which existing internal role performs attendance check-in | **Team to propose** — no new registration-only role is permitted | [#38](https://github.com/SinYang13/IS212-2026/discussions/38) |
| How an account-less Attendee is identified when viewing status or withdrawing | **Open** — both actions are required, but pre-loaded accounts are offered to staff and clients only | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |

See [[open-questions]] for the consolidated list.
