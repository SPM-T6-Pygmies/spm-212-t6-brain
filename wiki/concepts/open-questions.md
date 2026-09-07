---
title: Open Questions
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Aug 26.md
  - Week 4 - Customer Briefing.md
tags:
  - open-questions
  - decisions
  - risks
  - backlog
  - scope
  - connectsphere
---

# Open Questions

**Objects:** Spans all fifteen domain objects — see [[domain-model]] for the structural register these questions inform.

Consolidated register of everything the customer left to the team or left unanswered, as of the **2026-08-31** discussion snapshot (87 discussions). Every row is a decision the team must make and document — the customer will not make it for them.

`Ref` numbers are GitHub discussion numbers in the [[is212-g9-section]] repository, unless marked `brief`.

`Scope` is the Release 1 marker from [[release-1-scope]] — 🎯 in the core 20, 🧩 adjacent to a core feature, 📋 backlog. It sets **when** the question must be answered, never **whether**.

> [!info] Two open-question registers
> This page tracks open **requirements** questions — gaps in what the system must do, organised by feature area, with the customer's position and the resulting team obligation. [[domain-model]] maintains a separate, narrower register of open **structural/modelling** questions (44 items) — decisions about how the data model itself is shaped (fields, identity, state machines, record boundaries), once a requirements gap is accepted as a gap. The two overlap where a requirements gap and a structural gap are the same thing (e.g. [#74](https://github.com/SinYang13/IS212-2026/discussions/74)'s session identity scheme), and diverge everywhere else (e.g. Release 1 scope, tracked only here, has no structural-modelling counterpart there).

## How to Read the Customer's Positions

| Customer phrasing | What it means | Team obligation |
|---|---|---|
| "Please propose an appropriate approach" | The rule is genuinely undecided. The customer will accept a reasoned proposal | **Decide, document the assumption, and be able to defend it** |
| "Not specified" / "not currently a requirement" | Out of scope for now, but not denied. May return with the Release 1 scope | Record as an explicit exclusion; do not silently build it |
| "This is a UI/UX question" | Design freedom, but functional rules behind it still bind | Design freely; do not bypass process rules |
| "Handled outside the system" | Deliberate scope boundary | Model the resulting state, not the process |

## Scope Is Now Fixed

> [!success] Resolved — [#79](https://github.com/SinYang13/IS212-2026/discussions/79)
> [[week-4-customer-briefing]] names **20 core features** as the required content of the Week 12 release, closing both [#79](https://github.com/SinYang13/IS212-2026/discussions/79) ("Please refer to the final Release 1 scope to be confirmed on Friday") and [#11](https://github.com/SinYang13/IS212-2026/discussions/11) ("further internal discussions… to determine the features and flows to be prioritised for Release 1"). The full in/adjacent/out register is [[release-1-scope]].

What this changes for the rest of this register:

- **Nothing is closed by being out of Release 1.** The briefing directs teams to keep deferred features "in your product backlog and C4 models". A `📋 Backlog` question is still a question the team must answer for design and documentation — it just isn't code by Week 12.
- **Urgency is now tiered, not uniform.** A question attached to a `🎯 Release 1` feature must be decided before that feature is built. A question attached to a `📋 Backlog` feature can be carried. The `Scope` column added to each table below records which.
- **The briefing's silences are not answers.** It is a scope statement, not a specification. Where its wording is looser than an existing discussion answer — "help prevent inappropriate double-booking" (feature 14) against [#35](https://github.com/SinYang13/IS212-2026/discussions/35)'s outright block — the discussion answer governs.
- Two questions narrowed rather than closed: [#72](https://github.com/SinYang13/IS212-2026/discussions/72)'s field set is now enumerated but its mandatory subset is not, and [#4](https://github.com/SinYang13/IS212-2026/discussions/4)'s change categories are restated but the classification rule is still the team's.
- Exclusions recorded below stand. The briefing added no new ones and reversed none.

> [!warning] Scope created three new questions
> Fixing scope opened gaps of its own: Attendees must register (feature 18) with no specified way for their accounts to exist; registration has no stated capacity ceiling; and the committed confirmation-block rule ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) has no feature to hang on. All three are detailed in [[release-1-scope]] and are tracked in the relevant tables below.

## Events

See [[event-lifecycle]], [[event-request-workflow]], [[change-impact-management]].

| Question                                                                                                                               | Ref                                                           | Scope | Customer position                                                                                                                                                                                                                                                                                             | Team action                                                                                                                                                                                              |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Is there a minimum lead time before the event by which a request must be created?                                                      | [#78](https://github.com/SinYang13/IS212-2026/discussions/78) | 🎯    | "No minimum lead time has been specified for now. Please propose an appropriate approach if your solution requires one."                                                                                                                                                                                      | Decide whether to enforce a lead time; if so, make it configurable rather than hard-coded                                                                                                                |
| Which fields are mandatory for an event request to be submittable?                                                                     | [#72](https://github.com/SinYang13/IS212-2026/discussions/72) | 🎯    | **Narrowed.** [[week-4-customer-briefing]] now enumerates the nine information items a request carries; [#72](https://github.com/SinYang13/IS212-2026/discussions/72)'s "we have not firmed out the fields as yet" and the absence of field-length requirements still stand for which of them are *mandatory* | Define the mandatory subset of the nine named fields and the validation rules; this drives the draft vs submitted boundary (feature 3)                                                                   |
| Can a rejected event be resubmitted, or is rejection final?                                                                            | [#67](https://github.com/SinYang13/IS212-2026/discussions/67) | 🎯    | "Please propose an appropriate approach if needed."                                                                                                                                                                                                                                                           | Decide: terminal rejection, return-to-draft for amendment, or a separate *returned* outcome distinct from *rejected*                                                                                     |
| What does "reuse information from a previous event" copy in practice?                                                                  | [#75](https://github.com/SinYang13/IS212-2026/discussions/75) | 📋    | Requirement is only to reduce repeated data entry; "which information is copied and how it is presented is for you to propose"                                                                                                                                                                                | Define the clone/copy field set and what is deliberately not carried over (dates, requirements)                                                                                                          |
| Can a confirmed event move back to planning after a major change?                                                                      | [#70](https://github.com/SinYang13/IS212-2026/discussions/70) | 🧩    | "A major change may require existing arrangements to be reviewed again. You can propose if you need a transition process flow"                                                                                                                                                                                | Define the backward transitions in the state machine                                                                                                                                                     |
| Where exactly is the significant-vs-ordinary change threshold implemented?                                                             | [#4](https://github.com/SinYang13/IS212-2026/discussions/4)   | 🎯    | Categories given (date/time, attendance, venue requirements, equipment requirements); "the exact implementation is for you to propose"                                                                                                                                                                        | Specify the change-classification rule and the re-validation it triggers                                                                                                                                 |
| **How is the [#80](https://github.com/SinYang13/IS212-2026/discussions/80) confirmation block delivered without a readiness feature?** | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) | 🧩    | **Raised by the Week 4 scope.** Event Readiness Tracking and Event Confirmation are both absent from the core 20, yet `confirmed` is a feature 6 status and confirmation a feature 20 notification trigger. The block is a committed hard rule with no feature to hang on                                     | Define the essential-arrangement set and enforce the block as part of feature 6's "status changes consistent with the actions performed". A readiness dashboard is not required. See [[event-readiness]] |
| Is the exact status set and transition map fixed?                                                                                      | brief                                                         | 🎯    | Statuses listed only "for example"                                                                                                                                                                                                                                                                            | Propose and document the state machine                                                                                                                                                                   |
| How should indefinite postponement be represented?                                                                                     | [#69](https://github.com/SinYang13/IS212-2026/discussions/69) | 🧩    | Release bookings, retain the event record — but no status was named                                                                                                                                                                                                                                           | Decide status vs flag vs cleared-dates approach                                                                                                                                                          |

## Venues

See [[venue-booking]], [[venue-suitability]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| How long can a tentative hold last? Auto-release? Configurable? | [#39](https://github.com/SinYang13/IS212-2026/discussions/39) | 📋 | "No duration or auto-release rule has been specified for tentative holds. Please propose an appropriate approach if this is required." | Decide hold duration, expiry and release mechanism; note only one active hold or confirmed booking may exist per venue and overlapping period ([#41](https://github.com/SinYang13/IS212-2026/discussions/41)) |
| How far in advance can venues be booked (booking horizon)? | [#57](https://github.com/SinYang13/IS212-2026/discussions/57) | 📋 | "No booking horizon has been specified. Please propose an appropriate approach if your solution requires one." | Decide whether to cap forward booking; make configurable if enforced |
| Should the system automate propagation when a confirmed venue becomes unavailable? | [#54](https://github.com/SinYang13/IS212-2026/discussions/54) | 🧩 | Venue Staff update availability; the change is a notification scenario; the coordinator makes changes; "please propose an appropriate automation mechanisms if required" | Decide how much is automatic vs coordinator-driven |
| How is suitability checking presented and triggered relative to search/filtering? | [#83](https://github.com/SinYang13/IS212-2026/discussions/83) | 🎯 | Filtering narrows; suitability assesses a selected venue. "The exact presentation and implementation are for the solution team to propose" | Decide whether suitability is an inline indicator, a post-selection check, or both, and when it re-triggers |

## Equipment & Technical Support

See [[equipment-management]], [[technical-support]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| Is there a minimum notice period for equipment and technical support requests? | [#25](https://github.com/SinYang13/IS212-2026/discussions/25) | 📋 | "No minimum notice period has been specified. Please propose an appropriate approach if this is required in your solution." | Decide whether to enforce notice; interacts with the D-1 collection and Return Day +1 rules ([#5](https://github.com/SinYang13/IS212-2026/discussions/5)) |
| Does maintenance have an expected return date? Should the system flag and pre-book when equipment frees up? | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) | 📋 | "Expected return date and automatic pre-booking are not specified; please propose if appropriate." | Decide whether to model an expected return date and any queue/pre-booking behaviour |
| Is equipment maintenance a workflow in the system? | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) | 📋 | **"Currently, we have not listed equipment maintenance workflow as a potential functional requirement, would record as a consideration"** | Treat as out of scope but flag as a likely future requirement; status updates alone ([#9](https://github.com/SinYang13/IS212-2026/discussions/9), [#86](https://github.com/SinYang13/IS212-2026/discussions/86)) are in scope |
| Is equipment modelled as serialised individual items or pooled quantities? | [#13](https://github.com/SinYang13/IS212-2026/discussions/13) | 🎯 | "How you model equipment individually or as pooled quantities in the database or UX is for you to propose. **However, the system needs to know the quantity of equipment available.**" | Choose the model. Note the constraint set: physical location per item ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)), no same-day sharing and available again only from Return Day +1 ([#5](https://github.com/SinYang13/IS212-2026/discussions/5)) — these bias toward identifiable units |
| How are equipment location, transfer time and preparation time represented? | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) | 🧩 | "How the system represents transfer time, preparation time and the equipment's current location is for the solution team to propose." | Model transfer/preparation as consumed pre-event availability |
| Are Technical Support Staff rostered manually or system-assigned? How often is the roster updated? | [#88](https://github.com/SinYang13/IS212-2026/discussions/88) | 📋 | "No fixed roster update frequency is required. **There is no hard requirement of system assign or manual** for assigning the support staff." | Choose the assignment mechanism and justify it |
| How is Technical Support Staff availability modelled? | [#89](https://github.com/SinYang13/IS212-2026/discussions/89) | 📋 | "No separate staff self-declaration of availability is currently required. The system only needs to maintain sufficient availability information to support staff assignment. **How availability is maintained can be proposed by students.**" | Decide the availability data model — derived from assignments, or explicitly maintained |
| What happens to equipment reservations when an event is rescheduled? | [#16](https://github.com/SinYang13/IS212-2026/discussions/16) | 🎯 | A reschedule may affect availability, so the arrangement must be reviewed again; "how you implement that workflow is for you to propose" | Define carry-over vs re-request, and the status changes involved |

## Users & Access

See [[access-control]], [[user-roles]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| What authentication method is expected? | [#62](https://github.com/SinYang13/IS212-2026/discussions/62) | 🎯 | "Secure authentication is required, but the authentication method is not specified. **Please propose an appropriate approach.**" | Choose and justify (email/password, SSO, MFA); document as an architectural decision |
| How do external users recover a forgotten password or locked account? | [#64](https://github.com/SinYang13/IS212-2026/discussions/64) | 📋 | **"Account recovery and locked-account handling are not specified in the current requirements."** | Out of scope as stated, but a system with authentication and no recovery path is operationally incomplete — raise it and record the assumption |
| How do external users get accounts (self-registration vs invitation)? | [#49](https://github.com/SinYang13/IS212-2026/discussions/49) | 🧩 | **"The onboarding process feature is currently not in the list of features as yet."** | Out of scope; but external users must exist for registration and event requests to work — state the assumption about how accounts come to exist |
| **Who is authorised to assign a coordinator?** | [#52](https://github.com/SinYang13/IS212-2026/discussions/52), [#93](https://github.com/SinYang13/IS212-2026/discussions/93) | 🎯 | **Raised by the Week 4 scope.** Feature 1 enumerates exactly five authorisable roles; feature 5 says submitted events "can be assigned" without naming an actor. The Event Operations Manager who assigns in [#73](https://github.com/SinYang13/IS212-2026/discussions/73)/[#93](https://github.com/SinYang13/IS212-2026/discussions/93)/[#94](https://github.com/SinYang13/IS212-2026/discussions/94) is not one of the five | Decide which authenticable role performs assignment in Release 1 — an Event Coordinator self-assigning, a sixth role, or a seeded manager account. Feature 5 is unbuildable without an answer. See [[user-roles]] |
| Do Attendees need an account to register? | [#53](https://github.com/SinYang13/IS212-2026/discussions/53) | 🧩 | Attendees are supported as external users who can register and view registration status; "**External user onboarding features are not listed in the brief yet**" | **Decided — guest registration.** [[schema]] identifies a registration by either an account or a name-and-email pair. What else the guest path collects is still open under [#22](https://github.com/SinYang13/IS212-2026/discussions/22) — see [[registration]] |
| Is there a designated primary contact per client organisation? | [#65](https://github.com/SinYang13/IS212-2026/discussions/65) | 📋 | "No primary-contact role has been specified. **Please propose an appropriate approach if this is needed.**" | Decide whether client organisations need an owner/admin concept; note [#81](https://github.com/SinYang13/IS212-2026/discussions/81) already grants same-organisation view access without one |
| Is there an admin role for system configuration and user management? | [#52](https://github.com/SinYang13/IS212-2026/discussions/52) | 🧩 | "We do not have a system-configuration feature specified. User administration is also not defined as a separate business role. **If your solution requires user-administration permissions, you may propose an appropriate approach together with the existing access-control requirements.**" | Decide whether to introduce an administrator; note the **Event Operations Manager** already exists as an unlisted actor for coordinator assignment ([#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#93](https://github.com/SinYang13/IS212-2026/discussions/93), [#94](https://github.com/SinYang13/IS212-2026/discussions/94)) |
| What are the field-level access rules for protected information? | [#91](https://github.com/SinYang13/IS212-2026/discussions/91) | 🎯 | Protected information defined broadly; "**the exact access-control design and field-level permissions may be proposed by the solution team**" | Produce a role × resource × field permission matrix |

## Registration

See [[attendee-registration]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| What information must an Attendee provide when registering? | [#22](https://github.com/SinYang13/IS212-2026/discussions/22) | 🎯 | "**Please propose an appropriate set of information based on the event needs.**" | Define the registration field set; decide whether fields are per-event configurable. Drives registration report content in [[reporting]] |
| **Is there a ceiling on registrations for an event?** | brief | 🧩 | **Raised by the Week 4 scope.** Feature 18 names no registration capacity, no waiting list and no attendance recording, yet the venue was chosen by a capacity check (feature 11) against an expected attendance | Decide and document a ceiling. Unbounded registration against a capacity-checked venue is incoherent, but the customer has not stated a cap — record it as an assumption, not a requirement. See [[attendee-registration]] |
| Can an Attendee register for multiple events simultaneously? Any limits? | [#48](https://github.com/SinYang13/IS212-2026/discussions/48) | 📋 | "No limit has been specified on the number of events an Attendee may register for. **Please propose an appropriate approach if your solution requires restrictions.**" | Decide whether to cap concurrent registrations or detect time-clashing registrations |
| Do Attendees need accounts at all? | [#53](https://github.com/SinYang13/IS212-2026/discussions/53) | 🧩 | See Users & Access above — onboarding not in the feature list | **Decided — no**, for the registration path: [[schema]] supports account-less registrants. Account provisioning for the other external role (Event Organiser) is still open under [#49](https://github.com/SinYang13/IS212-2026/discussions/49) |

## Notifications

See [[notifications]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| How far in advance and how often should reminders fire? Configurable? | [#40](https://github.com/SinYang13/IS212-2026/discussions/40) | 📋 | "**The timing and configurability are for you to propose.**" | Define default reminder schedule and make it configurable; incomplete-arrangement reminders are confirmed required ([#47](https://github.com/SinYang13/IS212-2026/discussions/47)) |
| Batch or real-time delivery? | [#55](https://github.com/SinYang13/IS212-2026/discussions/55) | 🎯 | Marked **re-opened by a follow-up** in the snapshot, but resolved by SinYang13 on 2026-08-31: notifications need not be real-time, may be grouped at configurable intervals, real-time is acceptable if simpler, and there is **no hard delivery-time requirement** | Treat as answered; choose the simpler mechanism and keep frequency configurable |
| Is email an integration workstream? | [#79](https://github.com/SinYang13/IS212-2026/discussions/79) | 🎯 | No — "**a notification-channel requirement rather than a general integration requirement**". Both in-app and email channels required, configurable per notification type ([#37](https://github.com/SinYang13/IS212-2026/discussions/37)) | Build email as a channel; do not scope an integration project |

## Reporting

See [[reporting]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| What does a Venue Usage Report contain? | [#29](https://github.com/SinYang13/IS212-2026/discussions/29) | 📋 | "The exact measures and presentation are not specified yet; **please propose appropriate information** that supports venue usage and operational planning. **Revenue is not currently stated as part of this report.**" | Define measures from booking slots, states and turnaround time. Exclude revenue — the system holds no financial data ([#24](https://github.com/SinYang13/IS212-2026/discussions/24)) |
| Are there PDPA or privacy restrictions on exporting attendee data? | [#32](https://github.com/SinYang13/IS212-2026/discussions/32) | 🧩 | **Asked but not answered.** The customer confirmed only that Event Organisers may view or export attendee information for their own events, "where appropriate and subject to access restrictions" | Treat as an open risk. Decide exportable fields, export logging, and consent capture; align with [#91](https://github.com/SinYang13/IS212-2026/discussions/91) field-level permissions |
| What export formats are required? | brief | 📋 | Only "common formats" stated | Choose formats and justify |

## Non-Functional

See [[non-functional-requirements]].

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| Are there document size or file-type limits? | [#18](https://github.com/SinYang13/IS212-2026/discussions/18) | 📋 | "**No document size or file-type limits have been specified. Please propose an appropriate approach if your solution requires such constraints.**" | Set limits and an allowed-type list; unbounded upload is not a safe default |
| Must the audit log be tamper-proof, or is a regular table sufficient? | [#14](https://github.com/SinYang13/IS212-2026/discussions/14) | 🎯 | "The requirement is that important actions and changes are recorded so that ConnectSphere can determine what changed, who changed it, and when. **The technical mechanism used to achieve this is for you to propose.**" | Choose the mechanism; the *what/who/when* record is the non-negotiable part |
| How many concurrent users? Microservices? | [#7](https://github.com/SinYang13/IS212-2026/discussions/7) | 🎯 | "The system should support ConnectSphere's expected growth over the next three years. **The architecture, including whether microservices are appropriate, is for you to propose.** Do also consider the nature of business and staff strength." | State concurrency assumptions explicitly. Anchors: ~500 staff, 3-year horizon, Singapore-only venues |
| What are the performance targets? | [[non-functional-requirements\|brief §8a]] | 🎯 | Only "reasonable time" for named common operations | Define measurable acceptance criteria |
| Is offline access required? | [#31](https://github.com/SinYang13/IS212-2026/discussions/31) | 📋 | "**Offline access is not specified as a requirement for now**" — despite the on-site mobile use case in s7 | Record as an exclusion; note the residual risk of poor venue connectivity |
| Is multi-language support required? | [#28](https://github.com/SinYang13/IS212-2026/discussions/28) | 📋 | "Multiple-language support is not currently specified. **You may assume English for now.**" | Record as an exclusion; avoid hard-coding that blocks later i18n |
| Is there a data retention or archival policy? | [#20](https://github.com/SinYang13/IS212-2026/discussions/20) | 📋 | "**No retention policy for this system as yet.** Completed and cancelled events may remain available as historical records; archival and housekeeping are outside the current functional scope." | Record as an exclusion; note unbounded growth of events and audit history |
| Is integration with existing systems required? | [#79](https://github.com/SinYang13/IS212-2026/discussions/79), [#12](https://github.com/SinYang13/IS212-2026/discussions/12) | 📋 | Integration with external tools **not in scope**; **no data migration or import**; the new system is the **authoritative source** for event information it manages; Release 1 integration needs defer to the confirmed scope | Record as an exclusion; do not build importers or sync. Re-check when Release 1 scope lands |
| Timezone handling | [#36](https://github.com/SinYang13/IS212-2026/discussions/36) | 🎯 | Singapore timezone only — all owned venues are in Singapore | Record as a fixed constraint, not an open item |

## UI/UX Deferrals

The customer answered these with "This is a UI/UX question, please propose and do the appropriate" or equivalent. Design freedom is granted, but the underlying process rules still bind.

| Question | Ref | Scope | Customer position | Team action |
|---|---|---|---|---|
| Should the calendar show 30-minute intra-day breakdown or a wider (e.g. weekly) view? | [#6](https://github.com/SinYang13/IS212-2026/discussions/6) | 🎯 | UI/UX — propose | Design the calendar granularity; note venue booking is slot-based (AM/PM/Night, [#50](https://github.com/SinYang13/IS212-2026/discussions/50)), which caps meaningful granularity |
| Is the calendar view-only, or can users drag and drop to reschedule? | [#3](https://github.com/SinYang13/IS212-2026/discussions/3) | 📋 | UI/UX — propose. **"However, any rescheduling must still follow the relevant event-change process and should not bypass required checks or approvals."** | If drag-and-drop is offered, it must route through the change process, not mutate bookings directly |
| Are comments threaded or flat/chronological? | [#15](https://github.com/SinYang13/IS212-2026/discussions/15) | 🧩 | "The requirement is to retain comments, questions and updates with the event record. Whether comments are threaded or chronological is a UI/UX design decision." | Design freely; retention with the event record is the real requirement |
| Do users need sorting or saved searches on the general event list? | [#27](https://github.com/SinYang13/IS212-2026/discussions/27) | 📋 | UI/UX — propose | Design freely |
| Do Attendees see all events open for registration, or only their own? | [#10](https://github.com/SinYang13/IS212-2026/discussions/10) | 🎯 | "They definitely can view events that they register for and also those open for registration. If the question is on UI/UX, please propose and do the appropriate" | Both views required; presentation is free |

## Still Open in the Snapshot

The 2026-08-31 export lists **2 unanswered** discussions. Both are marked `re-opened by a follow-up` rather than never-answered, and both received a resolving reply from SinYang13 on 2026-08-31 that the export's status flag does not yet reflect.

| Ref | Topic | State |
|---|---|---|
| **[#2](https://github.com/SinYang13/IS212-2026/discussions/2)** | Does equipment have a physical location, and does moving it between venues affect turnaround (transit downtime)? | Marked **re-opened by a follow-up** (Isaidchia, 2026-08-28, flagging an apparent contradiction with [#5](https://github.com/SinYang13/IS212-2026/discussions/5)). **SinYang13 posted a resolving answer on 2026-08-31**: both rules apply at different points in the lifecycle — transfer/preparation time affects **pre-event** availability, and the Return Day +1 rule governs **post-event** availability. They are not alternatives. Representation of transfer time, preparation time and current location is for the team to propose |
| [#55](https://github.com/SinYang13/IS212-2026/discussions/55) | Batch vs real-time notifications | Marked re-opened; resolved 2026-08-31 — no hard delivery-time requirement, configurable batching, real-time acceptable |

> [!success] Resolved — [#2](https://github.com/SinYang13/IS212-2026/discussions/2)
> The contradiction raised in [#2](https://github.com/SinYang13/IS212-2026/discussions/2) is now closed. The lifecycle the customer set out is: current equipment location → transfer/preparation to event venue → D-1 collection / event commitment period → event use → return to ConnectSphere → inspection/preparation → **available again from Return Day +1** (Source: is212-discussions-2026-08-31, [#2](https://github.com/SinYang13/IS212-2026/discussions/2)). See [[equipment-management]].

## Items Explicitly Handled Outside the System

Not open questions, but boundaries worth restating so they are not accidentally built.

| Item | Ref |
|---|---|
| Contracts and payments — handled physically | [#24](https://github.com/SinYang13/IS212-2026/discussions/24) |
| Coordinator assignment SOP — the Event Operations Manager's own process | [#93](https://github.com/SinYang13/IS212-2026/discussions/93) |
| Coordinator assignment acceptance/decline — "handled outside the system" | [#95](https://github.com/SinYang13/IS212-2026/discussions/95) |
| Reassignment request and approval — system may assume already approved | [#94](https://github.com/SinYang13/IS212-2026/discussions/94) |
| Post-event equipment health checklist — kept outside the system | [#85](https://github.com/SinYang13/IS212-2026/discussions/85) |
| Document versioning — not required, not legal documents | [#21](https://github.com/SinYang13/IS212-2026/discussions/21) |

## Related Pages

[[event-lifecycle]] · [[event-request-workflow]] · [[event-readiness]] · [[change-impact-management]] · [[venue-booking]] · [[venue-suitability]] · [[equipment-management]] · [[technical-support]] · [[attendee-registration]] · [[notifications]] · [[access-control]] · [[user-roles]] · [[non-functional-requirements]] · [[reporting]] · [[connectsphere-event-services]] · [[event-planning-venue-booking-system]] · [[is212-g9-section]] · [[release-1-scope]] · [[customer-brief-no-1]] · [[is212-discussions-2026-08-26]] · [[is212-discussions-2026-08-31]] · [[week-4-customer-briefing]]
