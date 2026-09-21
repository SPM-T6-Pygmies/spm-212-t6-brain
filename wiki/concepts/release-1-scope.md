---
title: Release 1 Scope
type: concept
sources:
  - "Week 4 - Customer Briefing.md"
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
  - "IS212 2026 Discussions Sept 16.md"
tags: [scope, release-1, backlog, planning, connectsphere]
---

# Release 1 Scope

**Objects:** touches every domain object — see [[domain-model]].

The authoritative in/adjacent/out register for the Week 12 release. Every functionality area in [[customer-brief-no-1]] appears exactly once below. Source of truth for the `🎯 / 🧩 / 📋` markers used across the wiki; if a page disagrees with this register, this register wins.

Set by [[week-4-customer-briefing]], which closed [#79](https://github.com/SinYang13/IS212-2026/discussions/79) and [#11](https://github.com/SinYang13/IS212-2026/discussions/11).

> [!info] Out of Release 1 ≠ out of the project
> The briefing directs teams to "consider features beyond the scope of the first release in your product backlog and C4 models". `📋 Backlog` items keep their wiki pages, their requirements, and their open questions. Nothing here licenses deleting analysis.

## The markers

| Marker | Meaning | Obligation |
|---|---|---|
| `🎯 Release 1` | Named in the briefing's core-20 table | Working code by Week 12 |
| `🧩 Adjacent` | Not named, but a core feature cannot work end-to-end without it | Build the minimum the named feature needs — no more. Every row states which feature pulls it in |
| `📋 Backlog` | Deferred past Release 1 | Model it in the backlog and C4; do not build it |

The `🧩` marker exists because the briefing forbids treating the 20 as isolated: the release must "support coherent end-to-end event workflows across the relevant features", and changes to an event "may also affect bookings, equipment, attendee registration, or notifications".

## Register

### Identity and access

| Area | Scope | Rationale |
|---|---|---|
| User authorisation and authentication | 🎯 Release 1 | Core feature 1 |
| Client management | 🧩 Adjacent | Feature 1 scopes access by "relationship to an event"; [[client-organisation]] must exist as data for organiser-to-organisation scoping ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)). CRUD screens for it are not required |
| User profile management | 📋 Backlog | Not named |

### Request intake

| Area | Scope | Rationale |
|---|---|---|
| Event request creation | 🎯 Release 1 | Core feature 2 |
| Draft event requests | 🎯 Release 1 | Core feature 3 |
| Event categories | 📋 Backlog | Not named; category is absent from feature 2's information list. Now stated outright — categories are "not one of the mandatory Release 1 core features" and the brief's list is not exhaustive, so a reasonable subset suffices ([#100](https://github.com/SinYang13/IS212-2026/discussions/100)) |
| Recurring or similar events (reuse) | 📋 Backlog | Not named. [#75](https://github.com/SinYang13/IS212-2026/discussions/75)'s copy-field question stays open but unbuilt |

### Review, assignment and status

| Area | Scope | Rationale |
|---|---|---|
| Event review and approval | 🎯 Release 1 | Core feature 4 |
| Request withdrawal | 🧩 Adjacent | Not named, but [#103](https://github.com/SinYang13/IS212-2026/discussions/103) requires a withdrawal to be recorded in the system and the withdrawn request to stay distinguishable from the other request states — a fourth exit from feature 4's review, alongside approve, reject and return. The Organiser asks **outside** the system; only the Coordinator performs it, so there is no Organiser-facing action to build. See [[event-request]] |
| Coordinator assignment | 🎯 Release 1 | Core feature 5, including reassignment |
| Event status management | 🎯 Release 1 | Core feature 6 |
| Event information management | 🎯 Release 1 | Core feature 7 |
| Comments and discussion | 🧩 Adjacent | Feature 4 requires coordinators to "request clarification or amendments from the Event Organiser" and the outcome to be visible — a clarification exchange retained against the event is the minimum. A general threaded comment system is not |

### Event content

| Area | Scope | Rationale |
|---|---|---|
| Event programme and agenda | 📋 Backlog | Not named |
| Multi-session events | 📋 Backlog | Not named. [[session]] is a full domain object with no Release 1 mandate — a material simplification of the venue/equipment model |
| Supporting documents | 📋 Backlog | Not named |

### Venue

| Area | Scope | Rationale |
|---|---|---|
| Venue catalogue | 🎯 Release 1 | Core feature 8 |
| Venue availability calendar | 🎯 Release 1 | Core feature 9 |
| Venue search and filtering | 🎯 Release 1 | Core feature 10 |
| Venue suitability checking | 🎯 Release 1 | Core feature 11 |
| Venue booking request | 🎯 Release 1 | Core feature 12 |
| Venue booking approval | 🎯 Release 1 | Core feature 13 |
| Booking conflict detection | 🎯 Release 1 | Core feature 14 |
| Venue unavailability management | 🧩 Adjacent | Feature 9 states outright that "other recorded periods of unavailability should be reflected" — the records must exist and be readable. Whether Venue Staff maintain them in-system is the thin part left to the team |
| Room layout management | 🧩 Adjacent | Layouts are a feature 8 catalogue attribute and a feature 10 filter criterion. Managing the layout vocabulary itself is not required. Note [#112](https://github.com/SinYang13/IS212-2026/discussions/112) makes each supported layout carry its own capacity, so the layout link is what feature 11's capacity check reads — see [[venue]] |
| Organiser-indicated venue preference | 📋 Backlog | Not named. [#111](https://github.com/SinYang13/IS212-2026/discussions/111) permits it ("may be supported as a preference") but does not require it, and is explicit that it must not be treated as a booking or override suitability and approval. Building it is a choice, not an obligation |
| Setup and turnaround time | 🧩 Adjacent | Feature 14 detects "incompatible or overlapping" bookings; [[booking]]'s effective occupancy window is event time plus setup and turnaround. Needed as a venue attribute feeding conflict detection, not as a configuration feature. The customer has since drawn the boundary explicitly: the system "does not need to support setup or turnaround durations smaller than a slot for Release 1" ([#108](https://github.com/SinYang13/IS212-2026/discussions/108)). [#123](https://github.com/SinYang13/IS212-2026/discussions/123) then fixed the rule: setup and turnaround each need **at most one adjacent slot**, crossing midnight, with no multi-slot buffers — so the feature is a conflict-checking rule over neighbouring slots, not a stored duration |
| Venue retirement | 🧩 Adjacent | Not named in the core 20 or the Week 1 brief. [#122](https://github.com/SinYang13/IS212-2026/discussions/122) presumes a venue can be retired and specifies what happens to its future bookings (flagged for review, Coordinators notified, not cancelled or auto-rescheduled) without mandating a retire feature. Load-bearing for feature 10 (search must not offer a retired venue) and feature 12 (a booking request must not target one); the review-and-notify behaviour is the same pattern as feature 14's [#120](https://github.com/SinYang13/IS212-2026/discussions/120) case. **The thinnest `🧩` on this page** — the minimal implementation is a venue status, not a workflow; whether Release 1 builds any retire action is the team's call |
| Tentative holding | 📋 Backlog | Not named. Features 12–14 describe request → approve/reject only. The [#41](https://github.com/SinYang13/IS212-2026/discussions/41) exclusivity rule still binds via the *confirmed booking* half; the hold half is deferred, and [#39](https://github.com/SinYang13/IS212-2026/discussions/39)'s expiry question goes with it. Reinforced by [#109](https://github.com/SinYang13/IS212-2026/discussions/109): placing multiple venues on hold simultaneously is "not required for Release 1" |
| Event capacity management | 📋 Backlog | Not named — see Registration below |

### Equipment and technical support

| Area | Scope | Rationale |
|---|---|---|
| Equipment request management | 🎯 Release 1 | Core feature 15 |
| Equipment availability checking | 🎯 Release 1 | Core feature 16 |
| Equipment reservation | 🎯 Release 1 | Core feature 17 |
| Equipment catalogue | 🧩 Adjacent | Never named, but features 15–17 need equipment types, quantities and technical characteristics to exist before anything can be requested, checked or reserved |
| Equipment maintenance status | 🧩 Adjacent | Feature 16 requires equipment "recorded as unavailable" not to be treated as freely available — so an unavailable/maintenance state must exist. The maintenance *workflow* was already outside scope ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) |
| Technical support requests | 📋 Backlog | Not named. [[support-request]] has no Release 1 mandate |
| Technical staff assignment and rostering | 📋 Backlog | Not named. [#88](https://github.com/SinYang13/IS212-2026/discussions/88) and [#89](https://github.com/SinYang13/IS212-2026/discussions/89) stay open but unbuilt. Note the Technical Support Staff *role* is still in scope via features 1, 15 and 16 |

### Readiness and confirmation

| Area | Scope | Rationale |
|---|---|---|
| Event confirmation | 🧩 Adjacent | No feature names it, but [#98](https://github.com/SinYang13/IS212-2026/discussions/98) requires it **directly**: not a standalone feature, yet "the Release 1 workflow should support an Event Coordinator confirming an event once the required/essential arrangements… have been completed", reaching `Confirmed` and reflecting it to relevant users. The mechanism is the team's. This is now a customer-stated in-release obligation, not an inherited one |
| Event readiness tracking | 🧩 Adjacent | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) blocks confirmation while an essential arrangement is incomplete — a committed hard rule. Feature 6 requires status changes to be "consistent with the actions performed on the event", so the gate is inherited. The minimum is the gate, not a readiness dashboard |
| Outstanding action tracking | 📋 Backlog | Not named. The list-and-chase surface is deferred; only the block itself is required |

### Registration

| Area | Scope | Rationale |
|---|---|---|
| Attendee registration | 🎯 Release 1 | Core feature 18 — view, register, view status, withdraw, plus organiser/coordinator visibility |
| Withdrawal | 🎯 Release 1 | Named explicitly inside feature 18. Scoped by [#115](https://github.com/SinYang13/IS212-2026/discussions/115): withdrawal *arising specifically from event changes or cancellation* is not required for Release 1 — the attendee-initiated verb is |
| Registration period | 🧩 Adjacent | Feature 18 is conditional on registration being "enabled", so at minimum an enabled/disabled state is required. Open/close dates ([#30](https://github.com/SinYang13/IS212-2026/discussions/30)) are the fuller form and are not mandated |
| Registration capacity | 🧩 Adjacent | Feature 18 names no cap, but registering against an event that has an expected attendance and a capacity-checked venue (feature 11) with no ceiling is incoherent. Minimum: a ceiling that stops registration. **No longer only an inference** — [#99](https://github.com/SinYang13/IS212-2026/discussions/99) states that "the overall event/venue capacity must still not be exceeded", even for a manually added VIP on a full event |
| Waiting list | 📋 Backlog | Not named — including the committed 3-day acceptance window ([#44](https://github.com/SinYang13/IS212-2026/discussions/44)) |
| Attendance recording / check-in | 📋 Backlog | Not named |

### Change and cancellation

| Area | Scope | Rationale |
|---|---|---|
| Event change requests | 🎯 Release 1 | Core feature 19 |
| Impact of event changes | 🎯 Release 1 | Feature 19: changes to date, time, expected attendance, venue or equipment requirements "may require existing arrangements to be reconsidered" |
| Rescheduling | 🧩 Adjacent | Date and time are named as important changes in feature 19; a reschedule is that change plus its venue and equipment revalidation |
| Cancellation management | 🧩 Adjacent | `cancelled` is a feature 6 status and "cancellation where applicable" a feature 20 trigger. [#68](https://github.com/SinYang13/IS212-2026/discussions/68)'s rule — only the assigned Event Coordinator may cancel, at any stage — binds wherever it is built |

### Notifications

| Area | Scope | Rationale |
|---|---|---|
| Notification system | 🎯 Release 1 | Core feature 20 |
| Reminder system | 📋 Backlog | Not named. Feature 20 lists event-driven triggers only; [#40](https://github.com/SinYang13/IS212-2026/discussions/40) and [#47](https://github.com/SinYang13/IS212-2026/discussions/47) stay open but unbuilt |

### Discovery, records and reporting

| Area | Scope | Rationale |
|---|---|---|
| Event calendar | 📋 Backlog | Not named. Feature 9's calendar is the **venue availability** calendar, not an event calendar |
| Event search and filtering | 📋 Backlog | Not named. Feature 10's search is **venue** search |
| Role-based dashboard | 📋 Backlog | Not named — and now stated outright: "A role-based dashboard is not listed as a mandatory Release 1 core feature" ([#118](https://github.com/SinYang13/IS212-2026/discussions/118)) |
| Activity history | 📋 Backlog | Not named as a feature — but the auditability NFR ([#14](https://github.com/SinYang13/IS212-2026/discussions/14), [[non-functional-requirements\|brief §8]]) is untouched by the briefing and still binds. Record what changed, who and when; the browsable history surface is deferred. Confirmed by [#96](https://github.com/SinYang13/IS212-2026/discussions/96): history is "not a standalone core feature" and no complete audit-history function is required |
| Change history | 📋 Backlog | Same reading as activity history |
| Event / venue usage / registration reports | 📋 Backlog | Not named. Feature 18's "view appropriate registration information" is a roster **view**, not a report — see below |
| Report generation and exporting | 📋 Backlog | Not named. [#32](https://github.com/SinYang13/IS212-2026/discussions/32)'s unanswered PDPA-on-export risk therefore does not bite in Release 1 |

### Non-functional

| Area | Scope | Rationale |
|---|---|---|
| Performance, security, usability, reliability, scalability, auditability, maintainability | 🎯 Release 1 | The briefing is silent on non-functional requirements, so [[non-functional-requirements\|brief §7]]–[[non-functional-requirements\|brief §8]] bind unchanged across every feature shipped. Silence is not descoping |

## Committed hard rules, by scope

The customer's non-negotiable rules (catalogued in [[index]]) do not all land inside Release 1. Which ones bite:

| Rule | Ref | Scope |
|---|---|---|
| Venues book in AM / PM / Night slots | [#50](https://github.com/SinYang13/IS212-2026/discussions/50) | 🎯 — the time model under features 9, 12, 14 |
| Double-booking blocked outright, not warned | [#35](https://github.com/SinYang13/IS212-2026/discussions/35) | 🎯 — feature 14 |
| Equipment collectable D-1; available again Return Day + 1 | [#5](https://github.com/SinYang13/IS212-2026/discussions/5), [#113](https://github.com/SinYang13/IS212-2026/discussions/113) | 🎯 — features 16, 17. **The rule is committed; its key input is not.** [#113](https://github.com/SinYang13/IS212-2026/discussions/113) states there is no fixed default return day and leaves capture to the team, and while [[schema]] now carries `equipment_reservation.return_date` (migrated 2026-09-09), the column is **nullable with no default**, so feature 16 still cannot compute availability for any reservation that leaves it unset. Storage is solved; population is not. See [[discussions-sept-8-critical-diffs]] |
| Singapore time only; English only | [#36](https://github.com/SinYang13/IS212-2026/discussions/36), [#28](https://github.com/SinYang13/IS212-2026/discussions/28) | 🎯 — non-functional, always binds |
| Confirmation blocked while an essential arrangement is incomplete | [#80](https://github.com/SinYang13/IS212-2026/discussions/80) | 🧩 — inherited via feature 6, no feature of its own |
| Only the assigned Event Coordinator may cancel, at any stage | [#68](https://github.com/SinYang13/IS212-2026/discussions/68) | 🧩 — cancellation is a status, not a feature |
| One active hold **or** confirmed booking per venue and overlapping period | [#41](https://github.com/SinYang13/IS212-2026/discussions/41) | Split — confirmed-booking half 🎯 (feature 14); tentative-hold half 📋 |
| Waiting-list acceptance window is 3 days | [#44](https://github.com/SinYang13/IS212-2026/discussions/44) | 📋 — waiting list is out |
| Events auto-complete 7 days after end date | [#71](https://github.com/SinYang13/IS212-2026/discussions/71) | 📋 — `completed` is a feature 6 status, but automatic closure is not required |

## Risks this scope creates

> [!warning] Attendees can register without an account, but two of feature 18's four verbs still assume one
> Feature 18 requires Attendees to view events, register, check status and withdraw. This wiki long read all four as authenticated actions under feature 1; that was inference, not customer text ([[spm-24-implementation-decisions]]). What the sources say has since got sharper. The [#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up no longer merely leaves onboarding off the feature list: account onboarding and self-registration are **not required for Release 1**, and **pre-loaded accounts are sanctioned** — staff accounts for ConnectSphere employees, client accounts for Event Organisers. Crucially, that offer does not extend to Attendees, and the same answer re-states all four of feature 18's verbs as Release 1 obligations. So [#49](https://github.com/SinYang13/IS212-2026/discussions/49) removes seeded attendee accounts as an escape route while re-confirming the requirement, which tightens this risk rather than relieving it. There is still no admin role ([#52](https://github.com/SinYang13/IS212-2026/discussions/52)). Two of the four are closed. *Register* is: [[schema]] accepts a registration identified by a name-and-email pair, so no account need exist to create one. *View events* is too: the SPM-24 build serves the attendee-facing list and detail pages publicly, showing only events already open for registration, which keeps [[non-functional-requirements|brief §8b]]'s internal planning information out of reach without a login ([[spm-24-implementation-decisions]]). **View status** and **withdraw** are still open — a guest holds a `registration_reference` but no credential, and nothing yet delivers that reference to them. A magic-link or reference-plus-email lookup would close it. Seeded accounts would too, but [#49](https://github.com/SinYang13/IS212-2026/discussions/49) offers those for staff and clients only. Record the route chosen, because "not in scope" does not make the feature demonstrable. See [[access-control]], [[registration]].

> [!success] Resolved — [#99](https://github.com/SinYang13/IS212-2026/discussions/99) supplies the missing registration ceiling
> This entry previously read that feature 18 omits any cap, that unbounded registration against a capacity-checked venue is incoherent, and that `🧩 Adjacent` rested on the team's reasoning rather than the customer's word. The customer has now said it: VIPs may be added manually to a full event, **but "the overall event/venue capacity must still not be exceeded"** ([#99](https://github.com/SinYang13/IS212-2026/discussions/99)). The ceiling is stated, and it survives the strongest counter-example available. What remains unstated is the *behaviour* at the ceiling — [[registration]] refuses a full event outright, which is sound while the waiting list sits in `📋 Backlog` but is still the team's reading of "prevent **or appropriately handle**". See [[attendee-registration]], [[registration]].

> [!success] Resolved — [#98](https://github.com/SinYang13/IS212-2026/discussions/98) hangs confirmation on the Release 1 workflow directly
> This entry previously warned that [#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s blocking rule was a committed constraint with no core-20 feature to attach it to, so a team reading the briefing as an exhaustive build list would ship a `confirmed` status with no gate. The customer has now closed that reading: Event Confirmation need not be a standalone feature, **but** "the Release 1 workflow should support an Event Coordinator confirming an event once the required/essential arrangements for that event have been completed", reaching a `Confirmed` status reflected to relevant users ([#98](https://github.com/SinYang13/IS212-2026/discussions/98)). The gate is therefore in-release on the customer's own word, not only by inheritance from feature 6. [#106](https://github.com/SinYang13/IS212-2026/discussions/106) adds that essentialness is judged by the Coordinator **together with** the relevant Technical Support Staff. The markers below are unchanged; only the justification is stronger. See [[event-readiness]].

> [!question]- Team decision required — how much adjacency to build
> The briefing names 20 features and then requires "coherent end-to-end event workflows" across them, without saying where coherence stops. Every `🧩 Adjacent` row above is this wiki's reading of that clause, not a customer statement. The team should ratify the adjacency list explicitly — under-reading it ships disconnected features, over-reading it rebuilds the whole Week 1 brief under a different name.

## Related pages

[[week-4-customer-briefing]] · [[event-planning-venue-booking-system]] · [[open-questions]] · [[customer-brief-no-1]] · [[event-lifecycle]] · [[event-readiness]] · [[attendee-registration]] · [[access-control]] · [[venue-booking]] · [[equipment-management]] · [[technical-support]] · [[reporting]] · [[non-functional-requirements]] · [[domain-model]]
