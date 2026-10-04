---
title: "Week 7 User Stories — New and Modified"
type: review
sources:
  - "Week 7 Customer Changes.md"
  - "Managing Changes – A Guide.md"
  - "GitHub Discussion 128 (live fetch 2026-10-04; summarised in is212-discussions-2026-10-04-addendum)"
  - "SPM - IS212 Linear board (read 2026-10-04)"
tags: [review, user-stories, backlog, release-1, week-7]
---

# Week 7 User Stories — New and Modified

This page collects every user story that [[week-7-change-impact-review]] touches, so they can be read in one place without the schema, test and method detail. It includes the 15 new cards and the existing tickets whose acceptance criteria change. Card text for existing tickets comes from the `SPM - IS212` Linear board as read on 2026-10-04. Each card was drafted and then checked against the user-story standard (INVEST, 3–7 binary ACs, no invented numbers). The results are in [[#Quality pass]].

**Nothing here has been applied to Linear.** Re-read the board before pasting, because it moved three times on 2026-10-04.

## How to read this page

| Mark | Meaning |
|---|---|
| ✏️ **Modified** | An existing ticket. Only the highlighted text changes |
| 🆕 **New** | A new card (W7-xx). All of it is new |
| ==highlighted text== | Added or reworded by Week 7 |
| ~~struck text~~ | Removed by Week 7. Strike it on the ticket rather than deleting it |
| ⏸ | The AC waits on an open customer question. Leave it out of the build until the question is answered |

Unchanged ACs are kept in short form so each card reads whole. The ticket keeps their full wording and citations.

## At a glance

| Card | Title | Kind | Status | Pts | Week 7 |
|---|---|---|---|---|---|
| SPM-46 | Submit a venue booking request | ✏️ | In Review | 5 | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C3]], [[week-7-customer-changes\|Week 7 C4]] |
| SPM-23 | Configure venue turnaround and setup buffer | ✏️ | Backlog | 2 → **3** | [[week-7-customer-changes\|Week 7 C1]] |
| SPM-19 | Automatic availability update on confirmation | ✏️ | Backlog | 5 | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C3]], [[week-7-customer-changes\|Week 7 C4]] |
| SPM-20 | Cancel venue booking | ✏️ | Backlog | 3 | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]], [[week-7-customer-changes\|Week 7 C3]] |
| SPM-21 | Indicate venue is unavailable | ✏️ | Backlog | 3 → **5** | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]] |
| SPM-105 | Flag affected bookings for review | ✏️ | Backlog | 3 → **5** | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]], [[week-7-customer-changes\|Week 7 C3]] |
| SPM-43 | View venue availability calendar | ✏️ | Todo | 5 (re-size) | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]], [[week-7-customer-changes\|Week 7 C4]] |
| SPM-45 | Check venue suitability | ✏️ | Todo | 5 | [[week-7-customer-changes\|Week 7 C3]] |
| SPM-22 | Review and decide on venue booking requests | ✏️ | Backlog | 3 | [[week-7-customer-changes\|Week 7 C3]] |
| SPM-218 | Place a tentative hold | ✏️ | Backlog | – → **3** | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C4]] |
| SPM-219 | Confirm or release a tentative hold | ✏️ | Backlog | – → **2** | [[week-7-customer-changes\|Week 7 C4]] |
| SPM-224 | Book a venue per session (`📋 Backlog`) | ✏️ | Backlog | – | [[week-7-customer-changes\|Week 7 C1]] |
| SPM-54 | Reschedule an event | ✏️ | Backlog | 8 | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C3]] |
| SPM-55 | Cancel an event | ✏️ | Backlog | 8 | [[week-7-customer-changes\|Week 7 C2]], [[week-7-customer-changes\|Week 7 C3]], [[week-7-customer-changes\|Week 7 C4]] |
| SPM-49 | Update event planning information | ✏️ | In Progress | 5 | [[week-7-customer-changes\|Week 7 C6]] |
| SPM-53 | Revalidate arrangements after a significant change | ✏️ | Backlog | 8 | [[week-7-customer-changes\|Week 7 C6]] |
| SPM-48 | Track my event's status | ✏️ | Backlog | 3 | [[week-7-customer-changes\|Week 7 C6]] |
| SPM-62 | Notify the coordinator of the booking outcome | ✏️ | Backlog | 2 | [[week-7-customer-changes\|Week 7 C3]] |
| SPM-63 | Notify venue staff of a cancelled booking | ✏️ | Backlog | 2 | [[week-7-customer-changes\|Week 7 C1]], [[week-7-customer-changes\|Week 7 C2]] |
| SPM-71 | Notify the coordinator of an availability change | ✏️ | Backlog | 5 | [[week-7-customer-changes\|Week 7 C2]] |
| SPM-111 | Notify the organiser that confirmation lapsed | ✏️ | Backlog | 2 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-01 | Search venues by padded availability | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C1]] |
| W7-02 | Request a replacement venue | 🆕 | — | 5 | [[week-7-customer-changes\|Week 7 C2]] |
| W7-03 | Book several venues for one event | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C3]] |
| W7-04 | Expire an unconfirmed hold | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C4]] |
| W7-05 | Warn the coordinator of hold expiry | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C4]] |
| W7-07 | Review an unassigned request | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C5]] |
| W7-08 | Oversee coordinator assignments | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C5]] |
| W7-09 | Reassign an active event's coordinator | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C5]] |
| W7-12 | Safety Officer role | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-13 | See events awaiting a safety check | 🆕 | — | 3 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-14 | Conduct an operational safety check | 🆕 | — | 5 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-15 | Return an event after a safety outcome | 🆕 | — | 5 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-16 | Notify the Safety Officer of an event ready for check | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-17 | Notify the coordinator of the safety outcome | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C6]] |
| W7-18 | Block confirmation until the safety check is approved | 🆕 | — | 2 | [[week-7-customer-changes\|Week 7 C6]] |

There are **21 modified stories** and **15 new cards (45 pts)**. W7-06 was withdrawn because the Lead is the Operations Manager renamed ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). W7-10 and W7-11 are held in reserve until Q3 is answered. Three affected tickets are not stories (an epic, a bug and a decision card). They are listed under [[#Not stories]].

---

# ✏️ Modified stories

## Venues

### ✏️ SPM-46 · Submit a venue booking request
In Review · 5 pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C3]], [[week-7-customer-changes|Week 7 C4]]

As an Event Coordinator, I want to submit a booking request for a specific venue and ~~slot(s)~~ ==time==, so Venue Staff can review it and I get a definite answer.

- **AC1 Submit.** I choose the event, one venue, and one or more dates, each with a start and end time on the 15-minute grid. The request is saved as Requested. Missing fields are refused. *(Already rewritten on the board on 2026-10-04.)*
- **AC2 Room layout.** Unchanged.
- **AC3 Event details for Venue Staff.** Unchanged.
- **AC4 Clash.** ==**Given** the request's padded window (start − the venue's setup time, end + its turnaround time) overlaps the padded window of a Confirmed booking or an **unexpired** Tentative Hold at that venue, **when** I submit, **then** it is blocked outright and the message names the clashing date and times.== Padded windows that only touch are accepted. Rejected, Released, Cancelled ==and Expired== bookings do not block.
- ==**AC5 Multi-venue.** An event may hold more than one live booking. A booking of the *same* event at a different venue is not a clash, and each request is checked on its own.==

*Note:* the 15-minute grid is already built (PRs connectsphere#75 and connectsphere#80), but the padding is not. If those PRs merge first, record the padded check on the card as a known gap rather than reopening it.

### ✏️ SPM-23 · Configure venue turnaround and setup buffer
Backlog · 2 → ==3== pts · [[week-7-customer-changes|Week 7 C1]] · ==Scope: `🧩 Adjacent` → `🎯 Release 1` ([[week-7-customer-changes|Week 7 C1]]); add the `Release 1` label==

As a venue staff member, I want to record the setup time and turnaround/reset time for a venue, so that the venue's occupancy reflects the preparation and reset it actually needs.

- Can set a setup time and a turnaround time independently for each venue ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))
- The values are per venue, not a single system-wide default ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))
- ~~Recorded values are quantised up to whole slots when they feed availability ([#108](https://github.com/SinYang13/IS212-2026/discussions/108))~~
- ==Values are whole minutes, zero or more==
- ==**Given** a venue's setup or turnaround time changes, **then** existing live bookings at that venue are re-checked, and each one that now clashes is flagged for review (SPM-105). No booking is moved or removed==

**Open question (Q11):** must the values be multiples of 15 minutes? The customer's example (30 and 45) is, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) does not say so.

### ✏️ SPM-19 · Automatic venue availability update upon booking confirmation
Backlog · 5 pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C3]], [[week-7-customer-changes|Week 7 C4]]

As a Venue Staff member, I want the system to automatically mark ~~a venue's slot~~ ==the booked time== as confirmed once I approve a booking, so that we prevent double bookings.

- **Given** a pending request for a venue and ~~slot(s)~~ ==time window==, **when** Venue Staff approve it, **then** the calendar shows ==that booking's occupancy window== as Confirmed. ==Each of an event's bookings is updated on its own==
- **Given** a booking is Confirmed, **when** another request's ==padded window overlaps it==, **then** that request is blocked outright ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), [#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- Occupancy includes the venue's setup time before and turnaround time after the published times ([[event-planning-venue-booking-system|brief §4]]). *Unchanged*
- ==An unexpired Tentative Hold blocks like a Confirmed booking. An expired hold does not==

*Also:* retire the buffer-slot questions (a) and (b) in the 2026-09-21 comment.

### ✏️ SPM-20 · Cancel venue booking and free up availability
Backlog · 3 pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C2]], [[week-7-customer-changes|Week 7 C3]]

As the assigned Event Coordinator, I want to cancel a venue booking, so that the venue's ~~slot~~ ==time== is immediately freed up for other coordinators to book.

- Only the assigned Event Coordinator may cancel, at any stage ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)). *Unchanged*
- **Given** a Confirmed booking is cancelled, **then** it becomes Released and the calendar immediately shows ~~the venue's slot(s)~~ ==the booking's padded window== as Available
- ==Cancelling one booking leaves the event's other venue bookings untouched==

~~**Open question:** do Venue Staff ever need their own cancellation trigger?~~ ==**Answered by [[week-7-customer-changes|Week 7 C2]]:** no. Marking a venue unavailable flags the affected bookings (SPM-21, SPM-105). It does not cancel them.==

### ✏️ SPM-21 · Indicate venue is unavailable
Backlog · 3 → ==5== pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C2]] · ==add the `Release 1` label==

As a venue staff member, I want to block a venue's availability for operational reasons such as maintenance or renovation, so that coordinators cannot request bookings during those periods.

- Venue Staff can create an unavailability block on a venue ~~for one or more AM/PM/Night slots per day~~ ==for a period, with a reason category: Maintenance, Equipment failure, Renovation, Safety or Other. Free text is allowed only under Other==
- The calendar shows the blocked ~~slot(s)~~ ==period== as Blocked
- Coordinators cannot complete a request ==whose padded window overlaps the blocked period==
- ==Venue Staff can block a period even when bookings already exist in it==
- ==Existing bookings in the period are flagged for review (SPM-105), not cancelled, and the event is not cancelled==

*Hygiene:* the title says "due to maintenance", but the customer lists five reasons. Consider renaming it to "Mark a venue unavailable".

### ✏️ SPM-105 · Flag affected bookings for review
Backlog · 3 → ==5== pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C2]], [[week-7-customer-changes|Week 7 C3]] · ==add the `Release 1` label==

As an Event Coordinator, I want to be told when a venue attribute ==, its setup or turnaround time, or its availability== changes in a way that affects one of my bookings, so that a booking does not silently continue against a venue that no longer fits.

- **Given** Venue Staff change capacity, layouts, accessibility or facilities ==, setup time or turnaround time, or mark the venue unavailable over a booked period==, **then** affected bookings are identified ([#120](https://github.com/SinYang13/IS212-2026/discussions/120))
- The assigned Coordinator is notified (SPM-71). *Unchanged*
- Affected bookings are never reverted to Requested, auto-cancelled or auto-rebooked ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)). *Unchanged*
- The affected booking is shown as needing review on the calendar (SPM-43) and to the Coordinator. *Unchanged*
- Unaffected bookings at the same venue are left alone ==, and so are the event's bookings at other venues==
- ==The event and the original booking record remain unchanged and readable==

~~**Note:** narrower than [#54](https://github.com/SinYang13/IS212-2026/discussions/54) (unavailability).~~ ==The scope is now wider: this card covers unavailability as well.==
**Open question, target status:** ==team proposal: the booking keeps its status and gains a review flag.==

### ✏️ SPM-43 · View venue availability calendar
Todo · 5 pts (re-size after the sub-issues are re-scoped) · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C2]], [[week-7-customer-changes|Week 7 C4]]

As an Event Coordinator, I want to view a venue's availability calendar showing ~~which AM/PM/Night slots are open, confirmed, or blocked~~ ==when it is available, booked, held or blocked==, so I can identify ~~a viable slot~~ ==a viable time== before requesting a booking.

- ~~Per day, the status of each AM/PM/Night slot~~ ==The calendar is drawn on the 15-minute grid. A period shows as occupied if any booking's padded window (including setup and turnaround) overlaps it==
- Blocked ~~slots~~ ==time== displays ==its reason category== as recorded by Venue Staff
- The calendar reflects current state, not a cached snapshot. *Unchanged*
- ==An unexpired hold shows as "held" with its expiry time. An expired hold shows as available==

*Also:* sub-issues SPM-152 ("slot status derivation") and SPM-153 ("blocked-slot reason") are named for slots and need re-scoping.

### ✏️ SPM-45 · Check venue suitability
Todo · 5 pts · [[week-7-customer-changes|Week 7 C3]]

As the assigned Event Coordinator, I want the system to tell me whether a room I'm considering, or have already requested, fits my event's current needs, so I don't book somewhere that doesn't fit and I notice when a change makes a booked room unsuitable.

- AC1–AC10 and AC12–AC13: *unchanged (see the ticket)*
- **AC11** Each of the event's live booking requests shows its overall suitability ==, checked on its own against that booking's layout. An event with several venues gets one result per booking==

**New open question:** for a multi-venue event, is each venue judged against the event's whole expected attendance or only part of it? Week 7 does not say. Ask the customer; don't assume.

### ✏️ SPM-22 · Review and decide on venue booking requests
Backlog · 3 pts · [[week-7-customer-changes|Week 7 C3]]

As a venue staff member, I want to review pending booking requests and provide approval or rejection details, so that coordinators are clear on the outcome of their requests.

- Venue Staff approve or reject the request. *Unchanged*
- If rejected, they can give a reason and/or an informal alternative ([#45](https://github.com/SinYang13/IS212-2026/discussions/45)). *Unchanged*
- The request's status changes to Approved or Rejected. *Unchanged*
- ==Each booking request is decided on its own, even when the event has other venue bookings==

### ✏️ SPM-218 · Place a tentative hold on a venue ~~slot~~
Backlog · – → ==3== pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C4]] · ==Scope: `📋 Backlog` → `🎯 Release 1` ([[week-7-customer-changes|Week 7 C4]]); add the `Release 1` label==

As ~~[actor not named]~~ ==Venue Staff==, I want to hold a venue ~~slot~~ ==for a time window== while ==an Event Coordinator is finalising== the event, so that the time is not taken by another event while the details are settled.

- Can place a tentative hold on a venue ~~slot or slots~~ ==for a time window== while the event is being finalised ([[event-planning-venue-booking-system|brief §4]])
- ==Every hold has an expiry date/time from the moment it is placed==
- **Given** the venue already carries an ==unexpired== hold or a confirmed booking, **then** a second hold whose ==padded window== overlaps it is refused ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- The held ~~slots~~ ==time== shows as "tentatively held" on the calendar ([[event-planning-venue-booking-system|brief §4]])

~~**Open questions:** who places a hold; whether a hold reserves its buffer slots~~ ==**Answered:** Venue Staff place holds ([[week-7-customer-changes|Week 7 C4]]), and a hold reserves its padded window.== Still open: who sets the expiry and whether it can be extended (Q10, see W7-04).

### ✏️ SPM-219 · Confirm or release a tentative hold
Backlog · – → ==2== pts · [[week-7-customer-changes|Week 7 C4]] · ==Scope: `📋 Backlog` → `🎯 Release 1` ([[week-7-customer-changes|Week 7 C4]]); add the `Release 1` label==

As ==Venue Staff==, I want to turn a tentative hold into a confirmed booking or give it up, so that the ~~slot~~ ==time== is either committed or freed for another event rather than sitting in limbo.

- A hold can be confirmed, becoming a confirmed booking for the same venue and ~~slots~~ ==time== ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- A hold can be released, after which the calendar shows the time as available ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- **Given** a hold is released, **then** another event may hold or book that time ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- ==An expired hold cannot be confirmed==

~~**Open question:** no expiry AC because no number exists.~~ ==**Answered by [[week-7-customer-changes|Week 7 C4]]:** every hold expires. Auto-release is W7-04.== **Check:** the actor is carried over from SPM-218. Week 7 names Venue Staff as the people who *place* holds, but does not say who confirms them.

### ✏️ SPM-224 · Book a venue per session
Backlog · unestimated · [[week-7-customer-changes|Week 7 C1]] · **stays `📋 Backlog`**

As an Event Coordinator, I want to book a different venue for each session of an event, so that a multi-day event can move between rooms without being split into separate events.

- Each session can have its own venue ([#74](https://github.com/SinYang13/IS212-2026/discussions/74)). *Unchanged*
- ~~Session bookings use the venue's fixed AM, PM and Night slots ([#50](https://github.com/SinYang13/IS212-2026/discussions/50))~~ ==Session bookings use start and end times on the 15-minute grid== *(not in the impact review, but [#128](https://github.com/SinYang13/IS212-2026/discussions/128) supersedes [#50](https://github.com/SinYang13/IS212-2026/discussions/50))*
- Overlapping sessions in the same venue are blocked ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), [#41](https://github.com/SinYang13/IS212-2026/discussions/41)). *Unchanged*
- ~~A session occupies whole slots ([#108](https://github.com/SinYang13/IS212-2026/discussions/108), [#107](https://github.com/SinYang13/IS212-2026/discussions/107))~~
- ~~Setup and turnaround occupy at most one adjacent slot each ([#123](https://github.com/SinYang13/IS212-2026/discussions/123))~~
- ==A session's occupancy is its own time plus the venue's setup and turnaround time==

*Do not pull sessions in to deliver multi-venue events.* [[week-7-customer-changes|Week 7 C3]] covers spaces used at the same time under one event, not sessions.

## Events

### ✏️ SPM-54 · Reschedule an event
Backlog · 8 pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C3]]

As the assigned Event Coordinator, I want to change an event's date or time and have its venue and equipment bookings re-checked, so a reschedule can't leave a booking that no longer makes sense.

- A reschedule follows the change-request and revalidation process ([#3](https://github.com/SinYang13/IS212-2026/discussions/3)). *Unchanged*
- **Given** the new date/time makes ==the padded window of **any** of the event's venue bookings== overlap another live booking at the same venue, **then** the reschedule is blocked ([#35](https://github.com/SinYang13/IS212-2026/discussions/35))
- Equipment reservations are re-reviewed against the new dates ([#16](https://github.com/SinYang13/IS212-2026/discussions/16)). *Unchanged*

~~**Resolved:** the slot is the atomic unit; overlap is set intersection, not interval arithmetic.~~ *Delete this note. It is now wrong.*

### ✏️ SPM-55 · Cancel an event
Backlog · 8 pts · [[week-7-customer-changes|Week 7 C2]], [[week-7-customer-changes|Week 7 C3]], [[week-7-customer-changes|Week 7 C4]]

As the assigned Event Coordinator, I want to cancel an event at any stage, so its venue and equipment commitments are released and the people affected are told.

- Only the assigned Event Coordinator may cancel, at any stage ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)). *Unchanged*
- **Given** the event is cancelled, **then** ~~its venue booking is~~ ==**all** of its venue bookings are released, and its unexpired tentative holds are released==
- Equipment reservations no longer hold equipment for the event. *Unchanged*

==*Note:* marking a venue unavailable never cancels the event. Only the actor in AC1 can.==

### ✏️ SPM-49 · Update event planning information
**In Progress** · 5 pts · [[week-7-customer-changes|Week 7 C6]]

As the assigned Event Coordinator, I want to update an event's planning information as arrangements are finalised, so that the event record reflects what has actually been agreed.

- Fields that don't affect arrangements can be edited directly ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)). *Unchanged*
- Edits to date/time, attendance, venue or equipment requirements go through a change request ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)). *Unchanged*
- Edits are attributed and timestamped ([[non-functional-requirements|brief §8f]]). *Unchanged*
- ⏸ ==**Given** the event has a safety approval, **when** an edit to attendance, venue or equipment is approved, **then** that approval no longer counts and the event needs a fresh safety check== *(pending Q7)*

### ✏️ SPM-53 · Revalidate arrangements after a significant change
Backlog · 8 pts · [[week-7-customer-changes|Week 7 C6]]

As the assigned Event Coordinator, I want the system to flag which arrangements need re-review when a significant change request is approved, so a stale venue or equipment booking doesn't silently carry over.

- Affected arrangements are flagged for review by their owning role. *Unchanged*
- Venue Staff review a flagged venue booking. *Unchanged*
- Technical Support Staff review a flagged equipment reservation. *Unchanged*
- An event with an unresolved flag cannot be (re-)confirmed. *Unchanged*
- ⏸ ==A significant change after safety approval sends the event back for a fresh safety check== *(pending Q7)*

==*Note:* when the Safety Officer requests changes, W7-15 reuses this card's flag-for-review mechanism. At 8 points, split this card rather than grow it.==

### ✏️ SPM-48 · Track my event's status
Backlog · 3 pts · [[week-7-customer-changes|Week 7 C6]]

As an Event Organiser, I want to see my event's current status and what changed most recently, so that I always know what stage it is at without having to ask my coordinator.

- Current status is visible at any time. *Unchanged*
- The date of the latest status change is shown. *Unchanged*
- A status change appears without a refresh workaround. *Unchanged*
- Status labels are understandable without knowing ConnectSphere's internal process ([[non-functional-requirements|brief §7]]) ==, including any status added for the safety check==

**Open question (Q8):** should the Organiser see the safety outcome at all?

## Notifications

### ✏️ SPM-62 · Notify the coordinator of the booking outcome
Backlog · 2 pts · [[week-7-customer-changes|Week 7 C3]]

As an Event Coordinator, I want to be notified when Venue Staff approve or reject my booking request, so that I know whether the venue is secured.

- **Given** Venue Staff decide my request, **then** I'm notified of the outcome ==and which venue it concerns==
- A rejection carries the reason and any informal alternative ([#45](https://github.com/SinYang13/IS212-2026/discussions/45)). *Unchanged*
- I can open the booking from the notification. *Unchanged*
- I can mark it as read. *Unchanged*

### ✏️ SPM-63 · Notify venue staff of a cancelled booking
Backlog · 2 pts · [[week-7-customer-changes|Week 7 C1]], [[week-7-customer-changes|Week 7 C2]]

As a Venue Staff member, I want to be notified when a confirmed booking is cancelled, so that I do not prepare a venue for an event that is not happening.

- **Given** the Coordinator cancels a Confirmed booking, **then** Venue Staff are notified ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)). *Unchanged*
- The notification names the venue, the date(s) and the ~~slot(s)~~ ==time window== released
- I can open the calendar and see the freed ~~slot(s)~~ ==time==
- I can mark it as read. *Unchanged*

~~**Open question:** do Venue Staff ever start a cancellation?~~ ==**Answered by [[week-7-customer-changes|Week 7 C2]]:** no. Unavailability flags bookings instead.==

### ✏️ SPM-71 · Notify the coordinator of an availability change
Backlog · 5 pts · [[week-7-customer-changes|Week 7 C2]] · ==Scope: `🧩 Adjacent` → `🎯 Release 1` ([[week-7-customer-changes|Week 7 C2]]); add the `Release 1` label==

As the assigned Event Coordinator, I want to be notified when a venue or equipment availability change affects one of my upcoming events, so that I can make the necessary changes before the event.

- A venue or equipment availability change that affects an upcoming event notifies its Coordinator ([#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#9](https://github.com/SinYang13/IS212-2026/discussions/9)). *Unchanged*
- The notification names the event, the arrangement and the nature of the change ==. For venue unavailability, it also names the reason category==
- The system does not auto-cancel or auto-rebook ([#54](https://github.com/SinYang13/IS212-2026/discussions/54)). *Unchanged*
- An affected booking is flagged, not reverted to Requested ([#120](https://github.com/SinYang13/IS212-2026/discussions/120)). *Unchanged*
- I can open the affected event from the notification ==and start a replacement venue request from it (W7-02)==
- I can mark it as read. *Unchanged*

### ✏️ SPM-111 · Notify the organiser that a confirmed event is no longer confirmed
Backlog · 2 pts · [[week-7-customer-changes|Week 7 C6]]

As an Event Organiser, I want to be told when my confirmed event stops being confirmed, so that I do not keep relying on arrangements that no longer hold.

- **Given** a Confirmed event stops meeting the confirmation criteria, **then** the Organiser is notified ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)). *Unchanged*
- The notification says which arrangement lapsed and what the status is now. *Unchanged*
- It fires whatever the cause: an approved change request, a released booking, a withdrawn equipment reservation, a venue attribute change ==, or a safety approval withdrawn==
- It does not fire for a reschedule or cancellation (SPM-68). *Unchanged*
- Confirmation itself stays with SPM-66. *Unchanged*

---

# 🆕 New stories

## Venues

### 🆕 W7-01 · Search venues by padded availability
Venues · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C1]] · *SPM-44 stays Done (Scenario 2)*

As an Event Coordinator, I want venue search to exclude venues whose setup or turnaround time would clash during my event's time, so that I don't shortlist a venue that will be refused when I submit.

- **Given** an existing booking's padded window at a venue overlaps my event's padded window, **then** that venue is not listed as available
- The check counts my event's buffers at that venue and the other booking's buffers at the same venue
- Each venue's own setup and turnaround values are used, not a system-wide default ([#43](https://github.com/SinYang13/IS212-2026/discussions/43))
- Venues with unavailable time or an unexpired hold overlapping the window are excluded

**Open question (Q11):** must setup and turnaround values be multiples of 15?

### 🆕 W7-02 · Request a replacement venue
Venues · Release 1 · 5 pts · [[week-7-customer-changes|Week 7 C2]]

As the assigned Event Coordinator, I want to request a replacement venue for a booking affected by venue unavailability, so that the event can still go ahead without being re-entered from scratch.

- **Given** a booking is flagged as affected, **when** I start a replacement, **then** venue search opens with the event's date, time, attendance and layout already filled in
- The replacement is an ordinary booking request (SPM-46) reviewed by Venue Staff (SPM-22)
- The replacement is linked to the booking it replaces. The original booking and the event information stay readable throughout
- The event is not cancelled at any point
- **Given** the replacement is confirmed, **then** the original booking no longer blocks the unavailable venue

**Open question:** what status does the original booking end in (Released, or a distinct value)? See [#54](https://github.com/SinYang13/IS212-2026/discussions/54), [#120](https://github.com/SinYang13/IS212-2026/discussions/120) and [#122](https://github.com/SinYang13/IS212-2026/discussions/122). It is still a team decision.

### 🆕 W7-03 · Book several venues for one event
Venues · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C3]]

As an Event Coordinator, I want to book more than one venue for the same event, so that a large event can use a main hall and breakout rooms at the same time.

- An event can have more than one live venue booking at once
- Each booking is checked for suitability, availability and conflicts on its own
- The event page lists all its venue bookings, each with its status
- **Given** one booking is cancelled or rejected, **then** the event's other bookings are unchanged
- The venue arrangement counts as complete only when every live booking is Confirmed and none is flagged for review

**Open question:** does each booking carry its own purpose label (e.g. "Breakout A")? The customer has not said. Propose one rather than assume it.

### 🆕 W7-04 · Expire an unconfirmed hold
Venues (epic SPM-198) · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C4]]

As Venue Staff, I want a tentative hold to lapse at its expiry time, so that a venue is not reserved indefinitely for an event that never firms up.

- **Given** a hold reaches its expiry date/time unconfirmed, **then** it is marked expired and no longer blocks the venue
- An expired hold is never shown or counted as a confirmed booking
- Once a hold expires, another request or hold may take the same time
- The expired hold stays on record with its expiry time

**Open questions (Q10):** who sets the expiry, and can it be extended? Week 7 says neither. Any default duration is a team decision citing [#39](https://github.com/SinYang13/IS212-2026/discussions/39).

### 🆕 W7-05 · Warn the coordinator of hold expiry
Notifications (parent SPM-47; epic SPM-198) · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C4]]

As the assigned Event Coordinator, I want to be told before and when a hold on my event's venue expires, so that I can finalise in time or arrange an alternative.

- **Given** an unconfirmed hold on my event, **then** I'm notified before it expires
- **Given** it expires, **then** I'm notified that it has expired and the venue is no longer held
- Each notification names the venue, the held time and the expiry time
- I receive each notification once per hold

**Open question (Q10):** how long before expiry should the warning go out? Week 7 gives no lead time, so don't invent one. This is the first time-triggered notification, so it needs a scheduled job.

## Events — Event Coordinator Lead

### 🆕 W7-07 · Review an unassigned request
Events · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C5]]

As an Event Coordinator Lead, I want to open an unassigned request and read its basic event information, so that I can choose a suitable coordinator.

- The unassigned queue lists submitted requests with no coordinator. The Lead opens it when they choose; it does not arrive as a notification ([#117](https://github.com/SinYang13/IS212-2026/discussions/117))
- Each row shows the client organisation, the event and its requested date(s)
- Opening a row shows the request's basic event information, read-only
- A withdrawn request does not appear ([#103](https://github.com/SinYang13/IS212-2026/discussions/103))

*Mostly built already* (`operations_view_event_request`). The remaining work is mainly the new title and a test.

### 🆕 W7-08 · Oversee coordinator assignments
Events · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C5]]

As an Event Coordinator Lead, I want to see every coordinator's assignments and the active events, so that I can spot when work needs to move.

- Lists each coordinator with the requests and active events assigned to them
- Covers both event requests and approved events (Planning through Confirmed)
- An event appears under its current coordinator and moves when it is reassigned

*Scope:* **all** coordinators ([#128](https://github.com/SinYang13/IS212-2026/discussions/128)). There is no per-Lead subset. Outcome visibility still follows [#125](https://github.com/SinYang13/IS212-2026/discussions/125)'s "legitimate relationship" rule.

### 🆕 W7-09 · Reassign an active event's coordinator
Events · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C5]]

As an Event Coordinator Lead, I want to reassign the coordinator of an approved event, so that cover can change after planning has started.

- Can reassign an event in Planning, Blocked or Confirmed to a different coordinator, with no acceptance step ([#95](https://github.com/SinYang13/IS212-2026/discussions/95), [#94](https://github.com/SinYang13/IS212-2026/discussions/94))
- The previous coordinator loses access to the event, and the new one gains it
- The new coordinator and the Organiser are notified (SPM-57, SPM-58)
- The reassignment is recorded with who did it and when

**Open question (Q3):** is the outgoing coordinator also a "relevant user" who must be notified?

## Events — Operational Safety Check

### 🆕 W7-12 · Safety Officer role
Users · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C6]]

As ConnectSphere, I want a Safety Officer role, so that only authorised staff can decide an operational safety check.

- Only Safety Officers can record a safety check outcome. Others are refused (SPM-16)
- The role has its own navigation, its own workspace and an access-denied contact row
- At least one seeded Safety Officer account exists
- A Safety Officer sees only events that have reached the safety check ([#125](https://github.com/SinYang13/IS212-2026/discussions/125))

### 🆕 W7-13 · See events awaiting a safety check
Events · Release 1 · 3 pts · [[week-7-customer-changes|Week 7 C6]]

As a Safety Officer, I want a list of events whose venue and technical arrangements are confirmed but not yet safety-checked, so that I know what to review.

- An event enters the list when all its live venue bookings are Confirmed and its equipment requirements are reserved
- Each row shows the event, its date, its expected attendance and its venue(s)
- An event leaves the list once an outcome is recorded. It re-enters if it is sent back for a fresh check
- Events still in planning do not appear

**Open question:** what counts as "technical arrangements confirmed" for an event with no equipment lines?

### 🆕 W7-14 · Conduct an operational safety check
Events · Release 1 · 5 pts · [[week-7-customer-changes|Week 7 C6]]

As a Safety Officer, I want to review an event's operational safety and record approve, reject or request changes, so that unsafe arrangements are caught before preparation starts.

- Shows the inputs the customer listed: expected attendance, each venue's capacity for its chosen layout, accessibility requirements, equipment lines and any known venue restrictions
- Can record exactly one of: Approved, Rejected, Changes requested
- **Given** Rejected or Changes requested, **then** the outcome records which arrangement(s) are affected
- Each outcome is attributed and timestamped, and earlier checks on the same event stay readable

**Open question (Q5):** how does Reject differ from Request changes, and is a rejection final for the arrangement or for the whole event? A rejection cannot cancel the event, because only the Coordinator can cancel ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)). Emergency access and crowd movement have no data in the model, so record them as the Officer's notes.

### 🆕 W7-15 · Return an event after a safety outcome
Events · Release 1 · 5 pts · [[week-7-customer-changes|Week 7 C6]]

As the assigned Event Coordinator, I want an event sent back by the Safety Officer to show me what must change, so that I can rework it and resubmit it for a check.

- **Given** Changes requested or Rejected, **then** the event returns to planning and cannot go on to preparation or confirmation
- The arrangements the Officer named are flagged for re-review by the role that owns each one (reuses SPM-53)
- Once the flagged arrangements are confirmed again, the event re-enters the safety-check list
- The event cannot be confirmed until its latest check is Approved (SPM-50)

**Open question (Q6):** is "the relevant earlier planning stage" always planning, or does it depend on which arrangement is affected?

### 🆕 W7-16 · Notify the Safety Officer of an event ready for check
Notifications (SPM-47) · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C6]] · *ACs drafted here using the SPM-57 pattern. The impact review gave only the title*

As a Safety Officer, I want to be notified when an event is ready for its safety check, so that I can review it without watching the list.

- **Given** an event enters the safety-check list (W7-13), including when it re-enters after being sent back, **then** a notification is sent
- The notification names the event and its date
- I can open the event's safety check from the notification
- I can mark it as read

**Open question:** do all Safety Officers receive it, or only one? Week 7 does not say.

### 🆕 W7-17 · Notify the coordinator of the safety outcome
Notifications (SPM-47) · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C6]] · *ACs drafted here using the SPM-57 pattern. The impact review gave only the title*

As the assigned Event Coordinator, I want to be notified of my event's safety check outcome, so that I can start rework or move on to confirmation.

- **Given** an outcome is recorded, **then** I'm notified with the event and the outcome (Approved, Rejected or Changes requested)
- **Given** Rejected or Changes requested, **then** the notification names the affected arrangement(s)
- I can open the event from the notification
- I can mark it as read

### 🆕 W7-18 · Block confirmation until the safety check is approved
Events · Release 1 · 2 pts · [[week-7-customer-changes|Week 7 C6]] · *SPM-50 stays Done (Scenario 2); depends on W7-14*

As the assigned Event Coordinator, I want confirmation refused until the event's safety check is Approved, so that an event cannot be confirmed while its safety is unreviewed.

- **Given** the event's latest safety check is not Approved, **when** I try to confirm, **then** confirmation is blocked and the message names the pending, rejected or changes-requested check
- **Given** the latest check is Approved and every essential arrangement is complete, **then** confirmation proceeds as before (SPM-50)
- The system enforces the refusal. Hiding the button in the UI is not enough
- An event with no safety check at all counts as not approved

---

## Not stories

These tickets also change, but they are not user stories:

| Ticket | Kind | Change |
|---|---|---|
| SPM-198 Tentative Venue Holding | Epic | `📋 Backlog` → `🎯 Release 1` ([[week-7-customer-changes\|Week 7 C4]]). Remove "do not pull into a Release 1 sprint", add the `Release 1` label, and remove the 21-point estimate, because epics are not estimated. Children: SPM-218, SPM-219, W7-04, W7-05 |
| SPM-143 Refuse reassignment once Approved | Bug | "Operations Manager" → Lead. Point its out-of-scope note at W7-09 |
| SPM-144 Decide arrangement essentiality | Decision | Record that the safety check is a mandatory gate for **every** event, not an arrangement type that can be marked non-essential ([[week-7-customer-changes\|Week 7 C6]]; "due to regulation", [#128](https://github.com/SinYang13/IS212-2026/discussions/128)) |

**Relabel only (Done, no AC change):** SPM-130, 29, 16, 98 and 78 take the Lead's new title. SPM-150's title still says "slot". SPM-209 (`📋 Backlog`) needs a wording change only: it should measure utilisation in time, not slots.

## Quality pass

Each card above was checked against the user-story standard's rubric (Gates A–D). This table lists only the cards with something to fix. Every card not listed passed with no defects.

| Card | Verdict | Issue | Fix |
|---|---|---|---|
| SPM-45 | NEEDS WORK | B1: 13 ACs, which is far over 7 and existed before Week 7 | Split by workflow: "Record event facilities" (AC1–AC2), "Suitability checklist" (AC3–AC10), "Re-check after a change" (AC11–AC12) |
| SPM-49, SPM-53 | NEEDS WORK | D5: the new AC depends on Q7 | Keep it marked ⏸ and build without it until Q7 is answered |
| SPM-53, 54, 55 | Warn | C5: 8 points each, and Week 7 adds to all three | Split before adding more |
| SPM-219 | NEEDS WORK | D1: the actor is inferred from SPM-218. Week 7 names who places holds, not who confirms them | Confirm with the customer, or record it as a team decision |
| W7-02 | Warn | D2: "already filled in" is the team's proposal. Week 7 only says the Coordinator can search for and request a replacement | Keep it, but mark it as a proposal when presenting it to the customer |
| W7-05 | Warn | B2: "before it expires" has no lead time (Q10) | Binary as written (any time before expiry). Don't add a number |
| W7-12 | NEEDS WORK | A3: "As ConnectSphere" names the business, not a role. A5: the outcome restates AC1 | Fold AC1 into W7-14 and keep W7-12 as setup work for seeding, navigation and the access-denied row, or reword the role to the Event Coordinator Lead who needs a checker in place |
| W7-16 | Warn | D1: the recipient (one Officer or all of them) is unstated | Ask before Ready |

**Ask first:** Q3 (W7-09) and Q5 (W7-14 and W7-15) block cards in the current cycle. Q10 and Q11 can fall back to a team decision. The full list is in [[week-7-change-impact-review#Questions for the customer]].
