---
title: Backlog Candidate User Stories — the Uncarded Areas
type: review
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 26.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "Week 4 - Customer Briefing.md"
  - "IS212 User Story Standard — v1.0.md"
  - "outputs/user-story-coverage-stocktake.md"
tags: [review, user-stories, backlog, deferred-scope, epics]
---

# Backlog Candidate User Stories — the Uncarded Areas

Draft cards for every area that [[user-story-coverage-stocktake]] §2 found with **a citation but no story card anywhere** — not in Linear, not in [[release-1-candidate-user-stories]], not in [[user-stories-proposed-rewrites]]. Written to the **IS212 User Story Standard v1.0** (`raw/`): nine epics, 30 story cards and one spike.

Everything here is `📋 Backlog` or undecided. Nothing in this document is proposed for Release 1. It exists because [[release-1-scope]]'s own rule is that out of Release 1 ≠ out of the project — the Week 4 briefing directs teams to keep deferred features "in your product backlog and C4 models", and a wiki paragraph alone does not satisfy that.

> [!warning] These are drafts, not a backlog
> The standard's workflow is `AI drafts → team decides → customer confirms` (§11), and the middle step is not optional. A card here has a **Card** and a **Confirmation** but no **Conversation** behind it (§2) — for most of these areas the conversation with the customer never happened, which is exactly why they were never carded. Treat the Open questions on each card as the agenda for that conversation, not as footnotes.

## Count correction

The stocktake's prose says "sixteen `📋`/undecided areas in §2"; its own table carries **seventeen** rows. The extra one is the primary-contact row, which the accompanying [[log]] entry also omits from its list. All seventeen are covered below.

## How to read these cards

**Priority and estimate are deliberately blank on every card.** Priority is a strict 1..N ordering across the whole backlog (§3) and cannot be set for a subset; estimation is a team activity with no velocity baseline yet (§8). Every card therefore fails Gate A7 by design and none is Ready.

**Three cards name no role.** Where the customer never named an actor, the card says so — `*[role not named — see open question]*` — rather than picking one. This is a deliberate Gate A3 failure. The standard's rule for the reviewing agent is explicit: "Never resolve an ambiguity by inventing the answer — raise it as a question." Filling in a plausible role would make the card *look* Ready while burying a customer question inside it.

**Derived rules are not written as acceptance criteria.** Several wiki pages carry reasonable inferences that read like customer rules — the per-role dashboard tables, the reuse "do not copy" field list, the claim that VIP attendees appear in registration reports. Where the research trail flagged a line as this wiki's derivation rather than a customer statement, it appears in Open questions, never in an AC. Gate B6 forbids a rule with no customer source; a wiki inference is not a customer source.

**Scope markers.** Each epic carries the marker its areas hold in [[release-1-scope]]'s register, which is the source of truth. Two areas have no register row at all and are flagged where they appear.

## Epic register

Every story belongs to exactly one epic (§1). Epics are not estimated and are never "done" — each disappears when replaced by its stories.

| Epic | Covers (stocktake §2 rows) | Scope | Cards |
|---|---|---|---|
| **EP-A** — Technical Support Coordination | Technical support requests; technical staff assignment and rostering | `📋 Backlog` | TS1–TS5 |
| **EP-B** — Reporting and Export | Event / venue usage / registration reports; report generation and exporting | `📋 Backlog` | RP1–RP4 |
| **EP-C** — Cross-Event Visibility | Event calendar; event search and filtering; role-based dashboard | `📋 Backlog` | CV1–CV4 |
| **EP-D** — History Browsing | Activity history; change history (browsable surface only) | `📋 Backlog` | HB1–HB2 |
| **EP-E** — Event Content Enrichment | Event categories; recurring/reuse; multi-session events; supporting documents | `📋 Backlog` | EC1–EC6 |
| **EP-F** — Tentative Venue Holding | Tentative venue holds | `📋 Backlog` | VH1–VH2 |
| **EP-G** — Attendance and Non-Self-Registering Attendees | Check-in / attendance recording; VIP / non-self-registering attendees | `📋 Backlog` (check-in) · no register row (VIP path) | Reg6 · SPK-1 |
| **EP-H** — Readiness Chasing and Reminders | Outstanding action tracking; general reminder system | `📋 Backlog` | RC1–RC2 · RM1–RM3 |
| **EP-I** — Client Organisation Contacts | Primary contact per client organisation | No register row — see CO1 | CO1 |

---

## EP-A — Technical Support Coordination

`📋 Backlog`. Neither technical support requests nor technical staff assignment is named in the Week 4 core 20 ([[release-1-scope]]). The Technical Support Staff *role* stays in scope via core features 1, 15 and 16 — this epic is the work that role does **beyond** equipment reservation, which is where the gap sits.

Along with EP-B, the largest completely uncarded area: [[technical-support]] is a full concept page and [[support-request]] a full domain object, with zero cards between them until now. [[support-request]]'s own framing is the warning label for this whole epic — the object is "largely left to the team", the sources fix a handful of binding rules and specify no lifecycle, no assignment mechanism and no availability model.

### TS1 — Request Technical Support

**Story:** As an Event Coordinator, I want to record that an event requires technical support and describe what is needed, so that Technical Support Staff learn about the requirement in time to act on it rather than at short notice.

**AC:**
- Can record against an event that technical support is required, together with a description of the support needed ([[event-planning-venue-booking-system|brief §4]])
- Can record whether the support is needed before the event, during it, or both ([[event-planning-venue-booking-system|brief §4]])
- An event can be planned with no technical support requirement at all — recording one is optional, not a step in every event's path ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))
- A recorded requirement is visible to any Technical Support Staff member, not only to staff associated with a particular venue, equipment type or location ([#66](https://github.com/SinYang13/IS212-2026/discussions/66))

**Open questions:**
- No minimum notice period is specified for technical support requests — "Please propose an appropriate approach if this is required in your solution" ([#25](https://github.com/SinYang13/IS212-2026/discussions/25)). [[technical-support]] notes this sits alongside two other unset time bounds — no minimum event-request lead time ([#78](https://github.com/SinYang13/IS212-2026/discussions/78)) and no venue booking horizon ([#57](https://github.com/SinYang13/IS212-2026/discussions/57)) — and that if the team introduces one lead-time rule, all three should be designed consistently.
- Whether a support requirement is a distinct record or an attribute of the event is unstated. It affects whether TS2 and TS4 act on one object or three.

*The WHY traces to the brief's own pain point — "Technical Support Staff may receive insufficient notice about upcoming equipment or support requirements" ([[connectsphere-event-services|brief §2]]) — not to an invented efficiency claim.*

### TS2 — Review a Technical Support Requirement

**Story:** As a Technical Support Staff member, I want to review an event's technical support requirement and record whether it can be provided, so that the Event Coordinator knows whether that arrangement is settled or still blocking.

**AC:**
- Can view the technical support requirement recorded against an event ([[event-request-workflow|brief §5]] Step 8)
- Can record a determination of whether the requested support can be provided ([[event-request-workflow|brief §5]] Step 8)
- Can update the recorded preparation information as work progresses, without the Event Coordinator having to ask ([[event-request-workflow|brief §5]] Step 9)
- Given technical support has been identified as essential for that event and its requirement is not yet settled, then the event cannot be confirmed ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))

**Open questions:**
- No source specifies a lifecycle or a named status set for a support request ([[support-request]] States) — whether "can be provided" is a status field, a free-text note, or a checkbox is a team decision, and it determines what AC 2 actually tests.
- Whether a support requirement can be **partially** fulfilled the way equipment can is unstated. [#90](https://github.com/SinYang13/IS212-2026/discussions/90) establishes partial fulfilment for equipment only; do not assume it generalises.

**Dependency:** TS1 (no requirement to review without one). Declared per INVEST-I tolerance rather than pretended away.

### TS3 — Raise a Clarification on a Support Requirement

**Story:** As a Technical Support Staff member, I want to record a question against an event when the support requirement is unclear, so that the Event Coordinator can get an answer from the Event Organiser instead of the ambiguity surviving to the event day.

**AC:**
- Can record questions, comments, or clarification needs against the event ([#87](https://github.com/SinYang13/IS212-2026/discussions/87))
- The Event Coordinator is the party who coordinates the response with the Event Organiser ([#87](https://github.com/SinYang13/IS212-2026/discussions/87))
- The clarification and its response are retained with the event record ([#87](https://github.com/SinYang13/IS212-2026/discussions/87))

**Open questions:**
- [#87](https://github.com/SinYang13/IS212-2026/discussions/87) confirms the Coordinator's liaison role but does **not** state that Technical Support Staff are barred from contacting the Organiser directly. [[technical-support]] states the bar flatly; [[support-request]] flags the same reading as an inference. The two pages disagree in confidence and [#87](https://github.com/SinYang13/IS212-2026/discussions/87) does not settle it — confirm before writing an AC that forbids direct contact.
- No notification trigger for a technical-support clarification exists in [[notifications|brief §6]]'s list. Whether raising one notifies the Coordinator is unstated, so no notification AC appears here.
- Threaded vs flat comments is a UI/UX decision ([#15](https://github.com/SinYang13/IS212-2026/discussions/15)).

### TS4 — Allocate Technical Support Staff to an Event

**Story:** As *[role not named by the customer — see open question]*, I want to allocate Technical Support Staff to an event, so that a named person is accountable for on-site support on the day rather than it being assumed.

**AC:**
- Can allocate one or more Technical Support Staff to an event that requires on-site support ([[event-planning-venue-booking-system|brief §4]], [[connectsphere-event-services|brief §2]])
- Allocation is made according to the staff member's availability and the event's requirements — the only selection criteria the customer states ([[event-planning-venue-booking-system|brief §4]])
- Events that do not require technical support carry no allocation ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))
- Any Technical Support Staff member may be allocated to any event; allocation is not restricted by venue, equipment type or location ([#66](https://github.com/SinYang13/IS212-2026/discussions/66))

**Open questions:**
- **Who performs the allocation is unstated.** [#88](https://github.com/SinYang13/IS212-2026/discussions/88) asked "Who does the rostering?" and the answer addressed only the system-vs-manual axis. No manager role equivalent to the Event Operations Manager is named for Technical Support Staff anywhere in the brief or the discussions ([[user-roles]], [[support-request]]). This is the card's blocking question.
- Whether allocation is system-computed or manual is explicitly the team's — "There is no hard requirement of system assign or manual for assigning the support staff" ([#88](https://github.com/SinYang13/IS212-2026/discussions/88)).
- Whether a Technical Support Staff member may decline an allocation is unstated. [#95](https://github.com/SinYang13/IS212-2026/discussions/95)'s no-acceptance-step answer covers **Event Coordinators only** — do not transfer it.
- Whether one staff member can hold concurrent allocations, and any workload cap, is unstated. [#92](https://github.com/SinYang13/IS212-2026/discussions/92)'s "Yes" likewise covers Event Coordinators only.
- How many staff may be allocated per event is unstated — [[event-planning-venue-booking-system|brief §4]] says "suitable Technical Support Staff" in the plural and names no bound.
- No notification trigger exists for a staff member learning of their own allocation; [[notifications|brief §6]] lists coordinator assignment only.

### TS5 — See Technical Support Staff Availability

**Story:** As *[the role that performs technical staffing — not named, see TS4]*, I want to see which Technical Support Staff are available for an event's date and time, so that I allocate someone who is genuinely free rather than discovering the clash on the day.

**AC:**
- Can see availability information for Technical Support Staff at the point of allocating them to an event ([#89](https://github.com/SinYang13/IS212-2026/discussions/89))
- No self-declaration step is required of a Technical Support Staff member — availability is maintained without asking staff to enter it ([#89](https://github.com/SinYang13/IS212-2026/discussions/89))
- No fixed update cadence is imposed on the availability information ([#88](https://github.com/SinYang13/IS212-2026/discussions/88))

**Open question:** how availability is maintained is explicitly the team's — "The system only needs to maintain sufficient availability information to support staff assignment. How availability is maintained can be proposed by students" ([#89](https://github.com/SinYang13/IS212-2026/discussions/89)). [[technical-support]] notes a minimal design — a lead picks a name, with no availability calendar at all — satisfies the letter of both answers. Until the team chooses between deriving availability from existing allocations and maintaining it explicitly, this card cannot be sized: the two designs are not the same size of work. This is a **design decision the team owns**, not missing knowledge, so it is a story with a blocking decision rather than a spike (§6.1).

**Dependency:** TS4. This card has no standalone value if nothing consumes the availability view.

---

## EP-B — Reporting and Export

`📋 Backlog`. None of Event Reports, Venue Usage Reports, Registration Reports or Report Generation and Exporting is named in the core 20 ([[release-1-scope]]). Core feature 18's "view appropriate registration information" is a roster **view**, not a report.

The other of the two largest uncarded areas: [[reporting]] is a full concept page with no card in either output backlog until now.

**On the missing "export" card.** Export is deliberately carried *inside* each report card rather than split out as its own story. A standalone "export a report" card has no beneficiary and no standalone value — it is a criterion, not a story (§6.1, Small: too small). Splitting instead by report type applies the **Variations in data** pattern (§7.2), which is the split that produces independently valuable slices.

### RP1 — Generate an Event Summary

**Story:** As *[role not named — the brief says only "appropriate users", see open question]*, I want to generate a summary of a single event covering its details, schedule, venue, operational requirements and registration information, so that I can take a complete picture into an operational meeting without assembling it by hand from several screens.

**AC:**
- Can generate a summary for a single event on demand — the requirement is for users to generate reports when required, not on a schedule ([#34](https://github.com/SinYang13/IS212-2026/discussions/34))
- The summary covers the event's details, schedule, venue, operational requirements and registration information ([[event-planning-venue-booking-system|brief §4]])
- Content in the summary is limited to what the requesting user's role and relationship to the event permit them to see ([#91](https://github.com/SinYang13/IS212-2026/discussions/91), [[non-functional-requirements|brief §8b]])
- The summary is produced from the system's own records; no reconciliation against external tools is performed, as no integration is in scope ([#79](https://github.com/SinYang13/IS212-2026/discussions/79))

**Open questions:**
- Which roles count as "appropriate users" is unstated. The brief names audiences for the other three report types and not for this one; the asymmetry is in the source.
- No export file format is named anywhere in the brief or the Q&A. [[reporting]]'s tabular-vs-document reading is that page's practical suggestion, not a sourced constraint.
- Whether the summary must represent per-session detail for a multi-session event is unstated. [[reporting]] asserts it must; that is the wiki's derivation from [#74](https://github.com/SinYang13/IS212-2026/discussions/74), which speaks about sessions and not about reports. If confirmed, this card gains a dependency on EC3.

### RP2 — Review Venue Usage Over a Period

**Story:** As an internal user — invoking the §4.2 universal-capability exception, since [[event-planning-venue-booking-system|brief §4]] scopes this to "internal users" as one population and the behaviour does not differ between Event Coordinator, Venue Staff and Technical Support Staff — I want to review venue bookings and usage across a period I choose, so that I can plan operations against how the venues are actually being used.

**AC:**
- Can select a reporting period and see venue bookings and usage across it ([[event-planning-venue-booking-system|brief §4]], [#29](https://github.com/SinYang13/IS212-2026/discussions/29))
- Usage is expressed in the venue's booking slots — AM, PM and Night, a full day being all three ([#50](https://github.com/SinYang13/IS212-2026/discussions/50))
- Given a venue has setup or turnaround time recorded, then the occupancy shown for its bookings includes that padding rather than the published event times alone ([#43](https://github.com/SinYang13/IS212-2026/discussions/43), [[booking]])
- No revenue or other financial measure appears — the customer excluded revenue explicitly and the system holds no pricing data ([#29](https://github.com/SinYang13/IS212-2026/discussions/29))

**Open questions:**
- The measure set is explicitly the team's: "The exact measures and presentation are not specified yet; please propose appropriate information that supports venue usage and operational planning" ([#29](https://github.com/SinYang13/IS212-2026/discussions/29)). [[reporting]]'s slots-booked-over-slots-available suggestion is that page's proposal, not a customer statement, and is not written as an AC here. The card cannot be sized until the measures are chosen.
- Whether the report covers one venue or all venues is unstated.

### RP3 — View and Export My Event's Attendees

**Story:** As an Event Organiser, I want to view and export the attendee information for an event my organisation is responsible for, so that I can run the door and follow up with participants without asking ConnectSphere for a list each time.

**AC:**
- Can view attendee information for an event the Organiser is responsible for ([#32](https://github.com/SinYang13/IS212-2026/discussions/32), [#81](https://github.com/SinYang13/IS212-2026/discussions/81))
- Can export that attendee information ([#32](https://github.com/SinYang13/IS212-2026/discussions/32))
- Cannot view or export attendee information for an event belonging to an unrelated client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81), [[non-functional-requirements|brief §8b]])
- Registration and attendance appear as distinct facts — a registered attendee who did not attend is distinguishable from one who did ([[event-planning-venue-booking-system|brief §4]], [#38](https://github.com/SinYang13/IS212-2026/discussions/38))

**Open questions:**
- **[#32](https://github.com/SinYang13/IS212-2026/discussions/32) asked directly about privacy and PDPA restrictions on exporting attendee names and emails, and the answer addressed only permission.** "Where appropriate and subject to access restrictions" is the entire constraint given. Raise PDPA obligations with the customer before building an export of personal data; do not treat silence as permission.
- Field-level permissions are explicitly the team's to propose ([#91](https://github.com/SinYang13/IS212-2026/discussions/91)) — which attendee fields appear in the export is not settled.
- No export file format is named.
- Whether an Organiser can export attendees for a **same-organisation** event they are not personally responsible for is unstated. [#81](https://github.com/SinYang13/IS212-2026/discussions/81) grants view without edit for those events but does not speak to export.

### RP4 — Export a Registration List for an Assigned Event

**Story:** As an Event Coordinator, I want to export the registration list for an event I am coordinating, so that staff working the door have a roster to check people against.

**AC:**
- Can view and export attendee information for an event the Coordinator is assigned to ([[event-planning-venue-booking-system|brief §4]], [#32](https://github.com/SinYang13/IS212-2026/discussions/32))
- The exported list shows each attendee's registration status ([[event-planning-venue-booking-system|brief §4]])
- Attendee information the Coordinator's role and relationship to the event do not permit is excluded ([#91](https://github.com/SinYang13/IS212-2026/discussions/91))
- Out of scope: delivering the list to any external system. Export produces a file handed to the user, not a feed — no integration is in scope ([#79](https://github.com/SinYang13/IS212-2026/discussions/79))

**Open questions:**
- Which registration fields the list carries depends on the registration information set, which is explicitly the team's to propose "based on the event needs" ([#22](https://github.com/SinYang13/IS212-2026/discussions/22)).
- The same PDPA question as RP3 applies and is no less live for an internal role.

*Written as a separate card from RP3 rather than one card for both roles: [[event-planning-venue-booking-system|brief §4]] names Event Organisers and Event Coordinators for registration reports, and their access scopes differ — the Organiser's by client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)), the Coordinator's by assignment. Different acceptance criteria is the tell that they are different stories (§4.2).*

---

## EP-C — Cross-Event Visibility

`📋 Backlog` on all three areas. Each is the **event-level** twin of a venue-level feature already in Release 1, which is precisely why they were missed: core feature 9 is the *venue availability* calendar, core feature 10 is *venue* search, and neither RA1/SPM-48 (one event's own status) nor U3/SPM-15 (which functions a role can see) is a cross-event overview.

### CV1 — View My Events in a Calendar

**Story:** As an Attendee, I want to see in calendar form both the events I have registered for and the events currently open for registration, so that I can plan around what I have committed to and find something I can still join.

**AC:**
- The calendar shows events the Attendee has registered for ([#10](https://github.com/SinYang13/IS212-2026/discussions/10))
- The calendar shows events currently open for registration ([#10](https://github.com/SinYang13/IS212-2026/discussions/10))
- The calendar shows no internal planning information ([[non-functional-requirements|brief §8b]])
- Given an event is not Confirmed, then it does not appear as open for registration ([[event-planning-venue-booking-system|brief §4]])

**Open questions:**
- Time granularity — 30-minute intra-day versus a wider weekly view — is a UI/UX decision the customer returned to the team ([#6](https://github.com/SinYang13/IS212-2026/discussions/6)).
- How far forward "upcoming" extends, and whether past events are viewable in calendar form, are unstated. [#20](https://github.com/SinYang13/IS212-2026/discussions/20) keeps completed events as historical records but says nothing about calendar rendering.
- Whether a multi-session event renders as one entry or one per session is unstated.

*The Attendee is the only role the customer addressed in a calendar context ([#10](https://github.com/SinYang13/IS212-2026/discussions/10)), which is why this card comes first in the epic.*

### CV2 — View the Event Calendar

**Story:** As an internal user — §4.2 universal-capability exception: the calendar mechanism is identical for every internal role and only the visible set differs, which is expressed as an AC below rather than folded into the role — I want to see upcoming events in calendar form, so that I can see what is coming without opening each event record in turn.

**AC:**
- The calendar shows upcoming events the user's role and relationship to the event permit them to see ([[event-planning-venue-booking-system|brief §4]], [#91](https://github.com/SinYang13/IS212-2026/discussions/91))
- Given an event is rescheduled from the calendar, then the reschedule follows the event-change process and does not bypass any required check or approval ([#3](https://github.com/SinYang13/IS212-2026/discussions/3))
- No day-of-week, weekend or holiday distinction is applied — all days are treated equally ([#46](https://github.com/SinYang13/IS212-2026/discussions/46))
- Out of scope: venue availability. Recorded periods of venue unavailability belong to the venue availability calendar (VN2/SPM-43, core feature 9), which stays a separate surface

**Open questions:**
- Whether the calendar is view-only or supports drag-to-reschedule is a UI/UX decision returned to the team ([#3](https://github.com/SinYang13/IS212-2026/discussions/3)) — but the answer attaches a hard rule regardless of which is chosen, which is why AC 2 is on the card and the interaction style is not.
- Which event statuses appear — whether drafts, submitted or cancelled events are rendered — is unstated.
- An **Event Organiser** calendar is a third card this epic needs and does not yet have: [#81](https://github.com/SinYang13/IS212-2026/discussions/81) gives Organisers a different visible set (own events editable, same-organisation events view-only, unrelated organisations barred). It is not drafted here because the customer has not said whether the event calendar surfaces same-organisation events at all, and guessing would decide an access question by implication.

### CV3 — Search and Filter Events

**Story:** As an internal user — §4.2 exception, since [[event-planning-venue-booking-system|brief §4]] scopes event search to "Users" without differentiating — I want to search events by name, client organisation, date, venue, status or assigned coordinator, so that I can reach a specific event directly instead of browsing every record.

**AC:**
- Can search events by event name, client organisation, date, venue, status, and assigned coordinator ([[event-planning-venue-booking-system|brief §4]])
- Results contain only events the requesting user's role and relationship permit them to see ([#91](https://github.com/SinYang13/IS212-2026/discussions/91), [[non-functional-requirements|brief §8b]])
- Given the requester is an Event Organiser, then results never include an event belonging to an unrelated client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81))
- Completed and cancelled events remain findable as historical records ([#20](https://github.com/SinYang13/IS212-2026/discussions/20))

**Open questions:**
- **Category is one of the seven criteria the brief names, and it is missing from AC 1 on purpose.** Event categories are themselves `📋 Backlog` with no data source in Release 1 ([[release-1-scope]]), so the criterion cannot be built until EC1 lands. Declared as a dependency rather than silently dropped.
- Sorting and saved searches on the general event list are a UI/UX decision returned to the team ([#27](https://github.com/SinYang13/IS212-2026/discussions/27)).
- Free-text versus faceted search, default result ordering, pagination and result limits are all unstated.
- Whether Attendees may search events, as distinct from browsing open ones ([#10](https://github.com/SinYang13/IS212-2026/discussions/10)), is unstated. If yes, that is a second card with a different visible set.

### CV4 — See an Overview for My Role

**Story:** As an internal user — §4.2 exception: [[event-planning-venue-booking-system|brief §4]] states the same capability for "each internal or external role", and the mechanism does not vary — I want an overview of the items needing my attention, so that I can start work from one place instead of hunting for the events that concern me.

**AC:**
- Given a user opens the system, then they see an overview scoped to their own responsibilities ([[event-planning-venue-booking-system|brief §4]])
- The overview contains only information the user's role and relationship to the event permit ([#91](https://github.com/SinYang13/IS212-2026/discussions/91), [[non-functional-requirements|brief §8b]])
- Given an account holds more than one role, then the overview covers the union of what those roles permit — the same reading U3/SPM-15 already applies to navigation ([#56](https://github.com/SinYang13/IS212-2026/discussions/56))
- Out of scope on this card: the per-role content set. It is specified below as an open question, not assumed

**Open questions:**
- **The customer enumerated dashboard content for no role.** [[event-planning-venue-booking-system|brief §4]] gives four examples introduced by "such as" — events requiring attention, upcoming bookings, equipment requests, attendee registrations — and nothing per role. The per-role tables in [[reporting]], [[access-control]] and [[event-readiness]] are this wiki's reconstructions, labelled "Plausible" and "Indicative" on their own pages. **They are not AC input.** Until the customer confirms composition, this card establishes that the overview exists and is access-bounded, and no more.
- The Event Operations Manager has no dashboard definition and no stated visibility scope ([[user-roles]]), and is absent from the brief's role table entirely — yet it holds the coordinator-assignment authority ([#93](https://github.com/SinYang13/IS212-2026/discussions/93)). Any overview for that role is undefined at the source.
- Whether a multi-role user sees one merged overview or switches context is unstated; the union reading in AC 3 is the wiki's, carried here only because the ticketed U3 card already uses it.
- Whether the overview is a distinct screen or the landing view, and its refresh behaviour, are unstated.

*[[non-functional-requirements|brief §7]] requires an event's status, confirmed arrangements and outstanding actions to be "easy for the appropriate user to understand". It is not written as an AC: it fails T1 (not binary) and it is a global usability standard, so it belongs in the Definition of Done (§5.5), not on this card.*

**Relation:** overlaps RC2 by design — RC2 is the Coordinator's chase list, CV4 is the surface a chase list would sit on. Order them together or the second will re-litigate the first.

---

## EP-D — History Browsing

`📋 Backlog` — **the browsing surface only.**

> [!info] The write side is already in Release 1
> The underlying requirement that actions and changes be recorded — actor, timestamp, what changed — is `🎯 Release 1` as a non-functional requirement ([#14](https://github.com/SinYang13/IS212-2026/discussions/14), [[non-functional-requirements|brief §8f]]), independent of which functional area produced the change. Only the user-facing surfaces below are deferred: Release 1 requires that the record **exist**, not that it be **browsable** ([[audit-record]]). Cards here must not be read as descoping the NFR.

### HB1 — Review How an Event's Information Changed

**Story:** As *[role not named — the brief says "users with appropriate access", see open question]*, I want to review how an event's important information has changed over time, so that when an arrangement no longer matches what was agreed I can see what was altered and by whom.

**AC:**
- Can see, for an event, each recorded change with the field that changed, its old and new value, who changed it, and when ([[event-planning-venue-booking-system|brief §4]], [[non-functional-requirements|brief §8f]])
- Change history is restricted to users with appropriate access — it is not visible to everyone who can view the event ([[event-planning-venue-booking-system|brief §4]])
- Change history for completed and cancelled events remains available; it is not purged when the event closes ([#20](https://github.com/SinYang13/IS212-2026/discussions/20))
- Supporting documents carry no version history, so a document change cannot be diffed ([#21](https://github.com/SinYang13/IS212-2026/discussions/21))

**Open questions:**
- Which roles hold "appropriate access" is unstated — no role is named anywhere.
- Whether change history extends beyond *event* information to other object types is unstated; [[audit-record]] deliberately does not extend it without a source.
- The storage mechanism is explicitly the team's — "The technical mechanism used to achieve this is for you to propose" ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)). That is a task inside this story, not an AC (T4).
- AC 4 states what the *customer* said about documents. [[reporting]]'s stronger claim — that document change history is limited to add/delete events — is an inference [[audit-record]] itself downgrades, and is not carried here.

### HB2 — Review an Event's Activity History

**Story:** As *[role not named — no audience is named for Activity History at all, see open question]*, I want to see the significant actions taken on an event with who performed each and when, so that I can settle a dispute about what happened without reconstructing it from memory.

**AC:**
- Can see recorded significant actions for an event, each with its actor and the time it occurred ([[event-planning-venue-booking-system|brief §4]], [[non-functional-requirements|brief §8f]])
- Approve, reject and return decisions on the event's request appear in the history ([[event-request-workflow|brief §5]] Step 5)
- Activity history for completed and cancelled events remains available ([#20](https://github.com/SinYang13/IS212-2026/discussions/20))

**Open questions:**
- **Which actions count as "significant" is enumerated by no source** ([[audit-record]]). The team must propose the set and have it confirmed — it determines this card's entire size, and no estimate is meaningful before it.
- [[event-planning-venue-booking-system|brief §4]] states an access restriction only for Change History. **No sentence restricts who may view Activity History.** Any assumption that HB1's restriction applies here is an inference governed by the general access-control model, not a confirmed rule ([[audit-record]]) — which is why no access AC appears on this card and one does appear on HB1.
- Whether each domain object carries its own trail or all entries roll up to the owning event is unstated ([[domain-model]]).
- Whether attaching or removing a supporting document is itself logged is unstated.

---

## EP-E — Event Content Enrichment

`📋 Backlog` on all four areas. None appears in core feature 2's information list, which carries event name, purpose, description, proposed date and time, expected attendance, venue requirements, accessibility needs, equipment requirements and registration needs — and not category, programme, sessions or documents ([[week-4-customer-briefing]]).

### EC1 — Classify an Event by Category

**Story:** As an Event Coordinator, I want events to carry a category such as conference, workshop or exhibition, so that I can find and group events of the same type when searching and reporting.

**AC:**
- An event can be assigned a category — conference, workshop, training session, exhibition, meeting, seminar, networking event, or another type ConnectSphere defines ([[event-planning-venue-booking-system|brief §4]])
- The set of categories is extensible beyond those seven examples ([[event-planning-venue-booking-system|brief §4]])
- Category is available as a criterion when searching events ([[event-planning-venue-booking-system|brief §4]])

> [!warning] This is the thinnest-sourced card in the document
> **No discussion exists on event categories at all** — the topic was never asked about and never volunteered. Everything past the single [[event-planning-venue-booking-system|brief §4]] sentence is unstated, so every question below is a *customer* question, not a team decision. Do not let this card enter a sprint on the strength of its plausibility.

**Open questions:**
- Whether category is mandatory, single- or multi-valued, free text or a controlled list is unstated.
- Who maintains the vocabulary is unstated. [[event-planning-venue-booking-system|brief §4]] says "defined by ConnectSphere" without naming a role, and there is no admin role — system configuration and user administration are not defined as a business role ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)).
- Whether category drives any behaviour — venue suitability, defaults, routing — or is purely descriptive, is unstated.
- **The actor and the beneficiary differ.** Whoever creates the event assigns the category; the value accrues to whoever searches and reports later. §4.2 warns that this usually means the card is two stories or has the wrong role. Resolve in the conversation before splitting blindly.
- **Wiki inconsistency to settle first:** [[event-request-workflow]] states that requests are categorised, but [[event-request]]'s attribute table has no category field, and [[event-planning-venue-booking-system|brief §4]] says "**Events** can be classified", not requests. Decide whether category is set at request time or on the event, and fix whichever page is wrong.

### EC2 — Create an Event by Reusing a Previous One

**Story:** As an Event Organiser, I want to start a new event request from a previous event's information, so that I do not re-enter the same requirements for an event my organisation runs regularly.

**AC:**
- Can create a new event request seeded from a previous event's information ([[event-planning-venue-booking-system|brief §4]], [#75](https://github.com/SinYang13/IS212-2026/discussions/75))
- The new event can be given dates different from the event it was seeded from ([#75](https://github.com/SinYang13/IS212-2026/discussions/75), [[event-planning-venue-booking-system|brief §4]])
- The new event can be given requirements different from the event it was seeded from ([#75](https://github.com/SinYang13/IS212-2026/discussions/75), [[event-planning-venue-booking-system|brief §4]])

**Open questions:**
- **Which fields are copied, and how the reuse is presented, are explicitly the team's** — "Which information is copied and how it is presented is for you to propose" ([#75](https://github.com/SinYang13/IS212-2026/discussions/75)). The card cannot pass Definition of Ready until the copy set is agreed; it is the whole substance of the story.
- [[event-request-workflow]] proposes that venue bookings, equipment reservations and registration state must not carry over. **That is the wiki's addition** — the customer named only "different dates and requirements". It belongs in the copy-set decision above, not in an AC.
- Whether "recurring" implies a repeating series with generated instances, as distinct from one-off copying, is unstated. [[event-planning-venue-booking-system|brief §4]]'s row is titled "Recurring **or Similar** Events" but its text and [#75](https://github.com/SinYang13/IS212-2026/discussions/75)'s answer describe only reuse of information. If a true series is wanted, this is a different and much larger card.
- Whether reuse is restricted to events of the same client organisation, whether the source event must be Completed, and whether the link back to the source is retained, are all unstated.

### EC3 — Record an Event's Sessions

**Story:** As an Event Coordinator, I want to record an event's sessions with their own timings, so that an event running across a day or several days is planned as its parts rather than as one undifferentiated block.

**AC:**
- An event can have one or many sessions, within the same day or across multiple days ([[event-planning-venue-booking-system|brief §4]])
- Each session carries its own timing, independent of its sibling sessions ([#74](https://github.com/SinYang13/IS212-2026/discussions/74))
- A session belongs to exactly one event ([[event-planning-venue-booking-system|brief §4]])

**Open questions:**
- No source gives a session naming, ordering or identity scheme ([[session]]) — a team decision that blocks estimation, since "one session per event" and "an ordered, individually addressable programme" are very different builds.
- Whether a session has its own status or lifecycle is unstated; [[session]] deliberately proposes none.
- Whether a session can be cancelled independently of its event is unstated.

*This is the thin vertical slice through the workflow (§7.2, Workflow steps); EC4 and EC5 enhance it. Sessions are the one area where the customer stated a cardinality outright — [[client-organisation]] notes it as one of only three in the whole domain layer.*

### EC4 — Book a Venue per Session

**Story:** As an Event Coordinator, I want to book a different venue for each session of an event, so that a multi-day event can move between rooms without being split into separate events.

**AC:**
- Each session can have its own venue, which may differ from that of other sessions in the same event ([#74](https://github.com/SinYang13/IS212-2026/discussions/74))
- Session venue bookings are placed against the venue's fixed AM, PM and Night slots ([#50](https://github.com/SinYang13/IS212-2026/discussions/50))
- Given two sessions of the same event request overlapping periods in the same venue, then the second is blocked by the double-booking rule ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), [#41](https://github.com/SinYang13/IS212-2026/discussions/41))

**Open questions:**
- How a session's timing maps onto the three fixed slots when it does not align with a slot boundary is unstated.
- Whether a session's venue booking follows the same request-and-approval flow as an event-level booking is unstated — assumed by convenience elsewhere, stated nowhere.

**Dependency:** EC3.

### EC5 — Set Equipment and Attendance per Session

**Story:** As an Event Coordinator, I want each session to carry its own equipment requirements and attendance limit, so that one session's needs are not applied to the whole event.

**AC:**
- Each session can carry its own equipment requirements, independent of its sibling sessions ([#74](https://github.com/SinYang13/IS212-2026/discussions/74))
- Each session can carry its own attendance limit, independent of its sibling sessions ([#74](https://github.com/SinYang13/IS212-2026/discussions/74))
- A session may carry no equipment requirement at all — per-session requirements are optional ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))

**Open questions:**
- Whether registration is per-session or per-event is unstated, which leaves the relationship between a session's attendance limit and the event's registration capacity undefined. This is the question that decides whether EC5 is small or large.
- Whether event readiness aggregates across sessions is asserted by [[event-readiness]] but derived, not stated — [#80](https://github.com/SinYang13/IS212-2026/discussions/80) speaks about events and [#74](https://github.com/SinYang13/IS212-2026/discussions/74) about sessions, and neither joins them.
- Whether the four properties [#74](https://github.com/SinYang13/IS212-2026/discussions/74) confirms — timing, venue, equipment, attendance limit — are exhaustive is unstated.

**Dependency:** EC3.

### EC6 — Attach Supporting Documents to an Event

**Story:** As an Event Organiser, I want to attach documents such as programmes and floor plans to my event, so that the Coordinator planning it has the materials without a separate email trail.

**AC:**
- Can attach one or more documents to an event ([[event-planning-venue-booking-system|brief §4]])
- An attached document is visible to users whose role and relationship to the event permit it ([#91](https://github.com/SinYang13/IS212-2026/discussions/91), [[non-functional-requirements|brief §8b]])
- Documents are not versioned — uploading a replacement does not retain the previous file, because these are not legal documents ([#21](https://github.com/SinYang13/IS212-2026/discussions/21))

**Open questions:**
- No document size or file-type limit is specified — "Please propose an appropriate approach if your solution requires such constraints" ([#18](https://github.com/SinYang13/IS212-2026/discussions/18)). [[open-questions]]' judgement that unbounded upload is not a safe default is the wiki's, not the customer's, and is not written as an AC.
- **Whether documents may be deleted at all is unstated.** [#21](https://github.com/SinYang13/IS212-2026/discussions/21)'s question offered "added/deleted" and the answer addressed only versioning.
- Whether Attendees may see documents is unstated. [[non-functional-requirements|brief §8b]] bars Attendees from "internal planning information" but does not classify documents as such.
- Whether documents attach to an event request or a session as well as to an event is unstated — [[event-request]] does not model them at all.

*Note that **Comments and Discussion** is a different area with a different marker — `🧩 Adjacent`, pulled in by core feature 4's clarification requirement ([[release-1-scope]]) — and is not part of this epic despite sitting beside documents in the brief.*

---

## EP-F — Tentative Venue Holding

`📋 Backlog`. Core features 12–14 describe request → approve/reject only; "tentative" and "hold" appear nowhere in the core 20. [#41](https://github.com/SinYang13/IS212-2026/discussions/41)'s exclusivity rule still binds through its *confirmed booking* half, which VN5/SPM-46 and V1/SPM-19 already carry — this epic is the deferred hold half.

### VH1 — Place a Tentative Hold on a Venue Slot

**Story:** As *[actor not named by the customer — see open question]*, I want to hold a venue slot while the event is still being finalised, so that the slot is not taken by another event during the days it takes to settle the details.

**AC:**
- Can place a tentative hold on a venue's slot or slots while the event is still being finalised ([[event-planning-venue-booking-system|brief §4]])
- Given a venue slot already carries an active tentative hold or a confirmed booking, then a second tentative hold on that venue for an overlapping period is refused ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- The held slots display as "tentatively held" on the venue availability calendar ([[event-planning-venue-booking-system|brief §4]])

**Open questions:**
- **Who may place a hold is named in no source.** [[booking]]'s "coordinator places a hold" label is that page's inference and the page says so. The Event Coordinator is the natural candidate given they own venue booking and cancellation ([#68](https://github.com/SinYang13/IS212-2026/discussions/68)), but this needs confirming.
- What [[event-planning-venue-booking-system|brief §4]]'s qualifier "where permitted by ConnectSphere's business process" gates on is unstated.
- How the rules governing holds "may differ from confirmed bookings" beyond the exclusivity rule is unstated — the brief flags a difference and does not describe it.
- How many holds one event may carry, whether a hold needs Venue Staff approval, and whether the Event Organiser can see it, are all unstated.

### VH2 — Confirm or Release a Tentative Hold

**Story:** As *[actor not named — see VH1]*, I want to turn a tentative hold into a confirmed booking or give it up, so that the slot is either committed or freed for another event rather than sitting in limbo.

**AC:**
- A tentative hold can be confirmed, becoming a confirmed booking for the same venue and slots ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))
- A tentative hold can be released, after which the venue availability calendar shows the slots as available again ([#41](https://github.com/SinYang13/IS212-2026/discussions/41), [[event-planning-venue-booking-system|brief §4]])
- Given a hold is released, then another event may place a hold or a confirmed booking on those slots ([#41](https://github.com/SinYang13/IS212-2026/discussions/41))

**Open question:** expiry is doubly deferred — "No duration or auto-release rule has been specified for tentative holds. Please propose an appropriate approach if this is required" ([#39](https://github.com/SinYang13/IS212-2026/discussions/39)), and "The expiry period and release mechanism are for you to propose" ([#41](https://github.com/SinYang13/IS212-2026/discussions/41)). [[venue-booking]] warns that a hold which never expires functions as a de facto confirmed booking and silently blocks the venue — the same failure mode as the brief's own pain point that cancelled events continue to occupy reservations ([[connectsphere-event-services|brief §2]]). **No expiry AC is written here because no number exists to write** (§5.5); the team must choose one first, and a third card — auto-release on expiry — follows once they have.

**Dependency:** VH1.

---

## EP-G — Attendance and Non-Self-Registering Attendees

Check-in is `📋 Backlog` ([[release-1-scope]]). The VIP path has **no register row of its own** — it sits under the `🎯` Attendee-registration row while nothing in core feature 18 covers coordinator-side entry. That marker gap is itself a finding.

Both items in this epic were actioned in Linear alongside this document; they are restated here so the epic is not a stub.

### Reg6 — Check In a Registered Attendee · ticketed as **SPM-76**

Drafted in full in [[user-stories-proposed-rewrites]] and unchanged here. Three ACs — check-in by QR-code scan or manual entry, only Registered attendees may be checked in, attendance neither self-reported nor auto-marked ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)) — with the role left unnamed because the source says only "manually by staff".

**Still open:** which internal role performs check-in. No role description in [[user-roles|brief §3]]'s table and no discussion attaches check-in to a named role. Also unstated: what the QR code encodes and when it is issued, whether check-in is per-event or per-session, no-show handling, correcting or reversing a check-in, walk-ins, and the timing window relative to event start. None of these is drafted as a card, because each would be invented rather than sourced.

### SPK-1 — Coordinator-side entry of non-self-registering attendees · ticketed as **SPM-77**

A spike, not a story, and the only spike in this document — one in 31 items, comfortably inside the standard's ~1 in 10 tolerance (§1). [#33](https://github.com/SinYang13/IS212-2026/discussions/33) settles the routing ("the attendee list will have to be provided to the Event Coordinator for processing") and none of the mechanism, so the card is Estimable-blocked rather than merely large.

Three questions to close it: the delivery and entry mechanism; whether these attendees consume registration capacity and can be waitlisted; and whether they can be checked in and appear in registration reports. Timebox left to the team — proposing one would be exactly the false precision §5.5 warns about.

**Note for question 3:** [[reporting]] asserts that manually-added attendees "must appear in registration reports alongside self-registered attendees". That is the wiki's inference, not a customer statement, and the spike should test it rather than inherit it.

---

## EP-H — Readiness Chasing and Reminders

Both areas `📋 Backlog`. The **confirmation gate** is `🧩 Adjacent` and already carded as RA3/SPM-50 — "the minimum is the gate, not a readiness dashboard" ([[release-1-scope]]). This epic is the list-and-chase surface the gate does not include, plus the reminder categories beyond the one confirmed in [#47](https://github.com/SinYang13/IS212-2026/discussions/47).

### RC1 — See What Is Blocking an Event's Confirmation

**Story:** As an Event Coordinator, I want to see which of an event's arrangements are still incomplete, so that I chase the right person instead of discovering the blocker when confirmation fails.

**AC:**
- Can see, for an event, which arrangements identified as essential for that event are still incomplete ([#80](https://github.com/SinYang13/IS212-2026/discussions/80), [[event-planning-venue-booking-system|brief §4]])
- Incomplete non-essential follow-ups are shown as distinct from blocking ones — non-essential items may remain outstanding without preventing confirmation ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))
- Given every arrangement essential to that event is complete, then nothing blocking is listed and the event can be confirmed ([#80](https://github.com/SinYang13/IS212-2026/discussions/80))
- A partially fulfilled equipment requirement appears as outstanding with the unfulfilled items identified; whether the shortfall affects readiness is the Coordinator's judgement, not the system's ([#90](https://github.com/SinYang13/IS212-2026/discussions/90))

**Open question:** **how essentiality is determined and recorded per event is unspecified, and it is the highest-value design decision on this card.** The customer was explicit that there is no single fixed list — "Essential arrangements are those required for that particular event to proceed… There is no single fixed list that applies to every event" ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)). [[event-readiness]] sets out candidate approaches — derive the required set from the request's declared requirements, let the Coordinator mark each arrangement, or a hybrid — but those are the wiki's candidates, not a customer answer. The confirmation gate already in Release 1 depends on this mechanism, so it cannot stay implicit, and this card cannot be sized before it is chosen.

### RC2 — See Outstanding Actions Across My Events

**Story:** As an Event Coordinator, I want a list of my upcoming events that still have incomplete arrangements, so that I can work through them in order of urgency instead of opening each event to find out.

**AC:**
- Can see, in one place, upcoming events assigned to the Coordinator that still have incomplete arrangements ([[event-planning-venue-booking-system|brief §4]], [[connectsphere-event-services|brief §2]])
- The list identifies which arrangement is incomplete on each event, not merely that one is ([[event-planning-venue-booking-system|brief §4]])
- Events with no incomplete arrangements do not appear in the list

**Open questions:**
- Whether outstanding actions are discrete tracked items with owners and due dates, or a view derived from the state of the arrangements, is unstated by every source. The two are not the same card.
- Who may create, assign, close or dismiss an outstanding action is unstated, as are escalation, ageing and priority.
- Whether Venue Staff and Technical Support Staff get their own outstanding-action lists is unstated. [[event-request-workflow|brief §5]] Step 9 has them "update the relevant preparation information", and [[connectsphere-event-services|brief §2]] names the Technical Support Staff notice problem, but no source gives either role a list. [[event-readiness]]'s per-role table is that page's proposal.
- The only chase mechanism the customer confirmed is the batched reminder in [#47](https://github.com/SinYang13/IS212-2026/discussions/47) — whether chasing is notification-only or an in-app worklist is undecided, and RM3 is the other half of the answer.

**Relation:** CV4. RC2 is plausibly a panel on the role-based overview rather than a screen; sequence them together.

### RM1 — Be Reminded of an Upcoming Event

**Story:** As an Attendee, I want a reminder before an event I am registered for takes place, so that I do not miss something I signed up for weeks earlier.

**AC:**
- Given an event the Attendee is registered for is approaching, then the Attendee receives a reminder ([[event-planning-venue-booking-system|brief §4]])
- The reminder is delivered by in-app notification, email, or both, according to the configuration for that notification type ([#37](https://github.com/SinYang13/IS212-2026/discussions/37))
- Reminders need not be delivered in real time; they may be grouped and sent at a configurable interval, and there is no hard delivery-time requirement ([#55](https://github.com/SinYang13/IS212-2026/discussions/55))
- A reminder discloses nothing the recipient is not entitled to see ([#91](https://github.com/SinYang13/IS212-2026/discussions/91))

**Open questions:**
- **How far in advance and how often reminders fire, and the surface on which that is configured, are explicitly the team's** — "The timing and configurability are for you to propose" ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)). No lead time appears in any AC above, because none exists to cite (§5.5).
- Of the four reminder categories [[event-planning-venue-booking-system|brief §4]] lists, **only the incomplete-arrangements variant received a direct Q&A confirmation** ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)). The upcoming-event category is stated in the brief's feature description and confirmed nowhere. Confirm the category is wanted before building it.

### RM2 — Be Reminded of a Registration Deadline

**Story:** As an Attendee, I want a reminder before an event's registration closes, so that I do not miss the window for an event I intend to join.

**AC:**
- Given an event's registration end date is approaching and the Attendee has not registered, then the Attendee receives a reminder ([[event-planning-venue-booking-system|brief §4]])
- The reminder is driven by the event's registration end date, which is separate from the event date ([#30](https://github.com/SinYang13/IS212-2026/discussions/30))
- Delivery follows the same channel configuration and batching rules as every other notification ([#37](https://github.com/SinYang13/IS212-2026/discussions/37), [#55](https://github.com/SinYang13/IS212-2026/discussions/55))

**Open questions:**
- Who counts as a "relevant user" for a registration deadline is unstated — the brief says only "relevant users", and AC 1's "has not registered" narrowing is a reasonable reading that the customer has not confirmed. Raise it.
- Lead time and configurability, as RM1 ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)).
- This category, like RM1's, has no direct Q&A confirmation.

### RM3 — Configure Reminder Frequency

**Story:** As *[role not named — see open question]*, I want to change how often batched reminders are sent, so that the volume matches how closely events are actually being chased rather than being fixed at build time.

**AC:**
- The frequency at which batched notifications and reminders are sent can be changed ([#47](https://github.com/SinYang13/IS212-2026/discussions/47), [#55](https://github.com/SinYang13/IS212-2026/discussions/55))
- Channel configuration is held per notification type — not per user, and not as a single setting across all types ([#37](https://github.com/SinYang13/IS212-2026/discussions/37))
- The configured frequency governs the incomplete-arrangement reminders, which the customer confirmed are required, batched and configurable ([#47](https://github.com/SinYang13/IS212-2026/discussions/47))

**Open questions:**
- **Who configures this is unstated, and there is no role to give it to.** System configuration and user administration are not defined as a business role ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)) — yet the customer requires configurability. This is a live contradiction between two customer answers and needs raising, not resolving in design.
- Whether configuration is global, per notification type, or per event is unstated.
- [[event-planning-venue-booking-system|brief §4]]'s User Profile Management row mentions "communication preferences", which points at a per-user setting, while [#37](https://github.com/SinYang13/IS212-2026/discussions/37) puts configuration per notification type. Neither source reconciles the two ([[notification]]).

---

## EP-I — Client Organisation Contacts

No register row of its own. Primary contact is not a brief functionality area — it exists as a `🔶 Team decision` attribute under Client management (`🧩 Adjacent`) in [[client-organisation]], and as a `📋` row in [[open-questions]]. [[release-1-scope]] claims every functionality area appears exactly once in its register and wins over any disagreeing page; this item is outside that claim entirely. **Resolving the marker is a prerequisite to prioritising the card.**

### CO1 — Designate a Primary Contact for a Client Organisation

**Story:** As an Event Coordinator, I want to know which Event Organiser is the primary contact for a client organisation, so that I have someone to approach when an event's own Organiser is unreachable.

**AC:**
- One Event Organiser within a client organisation can be designated its primary contact ([#65](https://github.com/SinYang13/IS212-2026/discussions/65))
- The primary contact is visible to internal users planning that organisation's events ([#65](https://github.com/SinYang13/IS212-2026/discussions/65))
- Designating a primary contact changes no Event Organiser's access — same-organisation Organisers may still view but not edit events they are not responsible for ([#81](https://github.com/SinYang13/IS212-2026/discussions/81))

> [!warning] The one card here the customer did not ask for
> [#65](https://github.com/SinYang13/IS212-2026/discussions/65) answers that "no primary-contact role has been specified" and invites a proposal only "if this is needed". Gate D2 forbids scope the customer never asked for, and this card sits closest to that line of anything in this document. It is drafted so the option is visible and costed, **not** because it should be built. If the team cannot say what breaks without it, delete it — §7.3 counts finding something to drop as the split succeeding.

**Open questions:**
- Whether a client-organisation **administrator** role exists was asked directly in [#81](https://github.com/SinYang13/IS212-2026/discussions/81) and not answered. [[client-organisation]] warns that "no client-organisation administrator" is that page's inference, not a stated fact — [#81](https://github.com/SinYang13/IS212-2026/discussions/81) supports only that same-organisation access is automatic rather than administrator-granted for that specific rule.
- Delegation between Event Organisers is unsupported, though change of Event Organiser is catered for ([#61](https://github.com/SinYang13/IS212-2026/discussions/61)) — which may already cover the unreachable-Organiser case this card's WHY appeals to. Check before building.

---

## Summary — every card in this set

| Card | Epic | Title | Role | Blocking unknown |
|---|---|---|---|---|
| TS1 | EP-A | Request Technical Support | Event Coordinator | Notice period ([#25](https://github.com/SinYang13/IS212-2026/discussions/25)) |
| TS2 | EP-A | Review a Technical Support Requirement | Technical Support Staff | No lifecycle specified |
| TS3 | EP-A | Raise a Clarification on a Support Requirement | Technical Support Staff | Direct-contact bar unconfirmed |
| TS4 | EP-A | Allocate Technical Support Staff to an Event | **Not named** | Who allocates ([#88](https://github.com/SinYang13/IS212-2026/discussions/88)) |
| TS5 | EP-A | See Technical Support Staff Availability | **Not named** | Availability model ([#89](https://github.com/SinYang13/IS212-2026/discussions/89)) |
| RP1 | EP-B | Generate an Event Summary | **Not named** | Who are "appropriate users" |
| RP2 | EP-B | Review Venue Usage Over a Period | Internal user (§4.2) | Measure set ([#29](https://github.com/SinYang13/IS212-2026/discussions/29)) |
| RP3 | EP-B | View and Export My Event's Attendees | Event Organiser | PDPA on export ([#32](https://github.com/SinYang13/IS212-2026/discussions/32)) |
| RP4 | EP-B | Export a Registration List for an Assigned Event | Event Coordinator | Registration field set ([#22](https://github.com/SinYang13/IS212-2026/discussions/22)) |
| CV1 | EP-C | View My Events in a Calendar | Attendee | Granularity ([#6](https://github.com/SinYang13/IS212-2026/discussions/6)) |
| CV2 | EP-C | View the Event Calendar | Internal user (§4.2) | Which statuses appear |
| CV3 | EP-C | Search and Filter Events | Internal user (§4.2) | Depends on EC1 for category |
| CV4 | EP-C | See an Overview for My Role | Internal user (§4.2) | Per-role content unenumerated |
| HB1 | EP-D | Review How an Event's Information Changed | **Not named** | Who has "appropriate access" |
| HB2 | EP-D | Review an Event's Activity History | **Not named** | Which actions are "significant" |
| EC1 | EP-E | Classify an Event by Category | Event Coordinator | No discussion exists at all |
| EC2 | EP-E | Create an Event by Reusing a Previous One | Event Organiser | Copy set ([#75](https://github.com/SinYang13/IS212-2026/discussions/75)) |
| EC3 | EP-E | Record an Event's Sessions | Event Coordinator | Identity scheme ([#74](https://github.com/SinYang13/IS212-2026/discussions/74)) |
| EC4 | EP-E | Book a Venue per Session | Event Coordinator | Session-to-slot mapping |
| EC5 | EP-E | Set Equipment and Attendance per Session | Event Coordinator | Registration per session or event |
| EC6 | EP-E | Attach Supporting Documents to an Event | Event Organiser | Size/type limits ([#18](https://github.com/SinYang13/IS212-2026/discussions/18)) |
| VH1 | EP-F | Place a Tentative Hold on a Venue Slot | **Not named** | Who may hold |
| VH2 | EP-F | Confirm or Release a Tentative Hold | **Not named** | Expiry ([#39](https://github.com/SinYang13/IS212-2026/discussions/39), [#41](https://github.com/SinYang13/IS212-2026/discussions/41)) |
| Reg6 | EP-G | Check In a Registered Attendee · **SPM-76** | **Not named** | Which role checks in ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)) |
| SPK-1 | EP-G | Non-self-registering attendee intake · **SPM-77** | — (spike) | Mechanism ([#33](https://github.com/SinYang13/IS212-2026/discussions/33)) |
| RC1 | EP-H | See What Is Blocking an Event's Confirmation | Event Coordinator | How essentiality is recorded ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) |
| RC2 | EP-H | See Outstanding Actions Across My Events | Event Coordinator | Tracked items or derived view |
| RM1 | EP-H | Be Reminded of an Upcoming Event | Attendee | Lead time ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)) |
| RM2 | EP-H | Be Reminded of a Registration Deadline | Attendee | Who is a "relevant user" |
| RM3 | EP-H | Configure Reminder Frequency | **Not named** | No admin role exists ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)) |
| CO1 | EP-I | Designate a Primary Contact | Event Coordinator | Whether it is wanted at all ([#65](https://github.com/SinYang13/IS212-2026/discussions/65)) |

Every row has a blocking unknown. That is the honest state of this half of the backlog, not a defect in the drafting: these seventeen areas went uncarded because nobody had the conversation, and a card cannot manufacture one.

## Cards deliberately not written

Finding work to *not* do is the point of a split (§7.3). Six candidates were considered and dropped:

| Not written | Why |
|---|---|
| A standalone "export a report" card | No beneficiary and no standalone value — a criterion, not a story (§6.1). Export lives inside RP1–RP4. |
| Auto-release of an expired tentative hold | No duration or auto-release rule exists to build against ([#39](https://github.com/SinYang13/IS212-2026/discussions/39)). Follows VH2 once the team sets an expiry. |
| An Event Organiser event calendar | Would decide, by implication, whether same-organisation events appear on a calendar — an access question [#81](https://github.com/SinYang13/IS212-2026/discussions/81) does not reach. |
| Per-role dashboard content cards | The customer enumerated content for no role. Writing five cards from the wiki's "Plausible" tables would launder inference into specification. |
| Reversing or correcting a check-in; no-shows; walk-ins | Entirely unstated. Listed as questions on Reg6 instead. |
| Recording attendance at event completion | Already inside SPM-51 "Mark an event as completed" ([[event-request-workflow\|brief §5]] Step 14). Gate D4 — no overlap with sibling cards. |

## Questions this document raises for the customer

Distinct from team decisions. These need the customer, and several block more than one card.

1. **PDPA and privacy on attendee export** ([#32](https://github.com/SinYang13/IS212-2026/discussions/32)) — asked once, answered only as a permission question. Blocks RP3 and RP4.
2. **Which internal role performs check-in** ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)) — "the staff" is not a role. Blocks Reg6 (SPM-76).
3. **Who allocates Technical Support Staff** ([#88](https://github.com/SinYang13/IS212-2026/discussions/88) asked and was answered on a different axis) — blocks TS4 and TS5, and is the same shape of gap as the unmodelled Event Operations Manager.
4. **Who may place a tentative venue hold** — named nowhere. Blocks EP-F entirely.
5. **Dashboard content per role** — the brief's "such as" examples are not a specification. Blocks CV4 and shapes RC2.
6. **Which actions count as "significant"** for activity history — blocks HB2 and determines its size.
7. **Who configures reminder frequency**, given the customer requires configurability ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)) and denies an admin role ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)) — a direct tension between two answers.
8. **Anything at all about event categories** — the only area in this document with no discussion behind it.
9. **Is a primary contact wanted** ([#65](https://github.com/SinYang13/IS212-2026/discussions/65)) — a yes/no that either creates CO1 or deletes it.
10. **Whether "recurring" means a repeating series** or only field copying ([#75](https://github.com/SinYang13/IS212-2026/discussions/75)) — the difference between a small card and a large one.

## Related pages

[[user-story-coverage-stocktake]] · [[release-1-candidate-user-stories]] · [[user-stories-proposed-rewrites]] · [[user-stories-assignment-scrutiny]] · [[release-1-scope]] · [[open-questions]] · [[event-planning-venue-booking-system]] · [[technical-support]] · [[support-request]] · [[reporting]] · [[access-control]] · [[audit-record]] · [[event-readiness]] · [[notifications]] · [[notification]] · [[session]] · [[venue-booking]] · [[booking]] · [[client-organisation]] · [[attendee-registration]] · [[event-request-workflow]]
