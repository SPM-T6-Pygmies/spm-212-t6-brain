---
title: Domain Model
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, hub, er-diagram, data-model, glossary]
---

# Domain Model

The hub for the fourteen domain-object pages in `wiki/domain/`. Neither source document contains an ER diagram, a class diagram, a data dictionary, or a glossary — this page is the first place the object model is assembled anywhere in the project. It exists to answer three questions in one place: what are the objects and how do they relate (ER diagram, below), which structural decisions has the customer actually settled (modelling-decision register), and which structural decisions remain open for the team (deferred-modelling register).

Each object's own page remains the authority for its attributes, states, relationships and binding rules, with full citations. This page indexes and cross-references; it does not restate their detail.

## Object catalogue

| Object | What it is | Page |
|---|---|---|
| Client Organisation | The external organisation an Event Organiser acts on behalf of | [[client-organisation]] |
| User Account | A person who interacts with the system, internal or external, carrying identity, contact and role information | [[user-account]] |
| Event Request | The pre-approval artefact an Event Organiser submits; becomes an Event on approval | [[event-request]] |
| Event | The plannable record of a confirmed-or-confirmable occasion | [[event]] |
| Session | A first-class unit of a multi-session event, with its own venue, equipment, timing and attendance limit | [[session]] |
| Venue | A physical event space catalogued by Venue Staff | [[venue]] |
| Booking | The record created when a venue slot is reserved, tentatively or confirmed | [[booking]] |
| Equipment Item | A unit (or type/pool) of technical equipment maintained by Technical Support Staff | [[equipment-item]] |
| Equipment Reservation | The record created when a coordinator-recorded equipment requirement is reviewed and committed | [[equipment-reservation]] |
| Support Request | An event's need for technical support and its staffing | [[support-request]] |
| Registration | A user account (Attendee)'s registration for a confirmed event | [[registration]] |
| Change Request | A record of a change an Event Organiser requests against an event | [[change-request]] |
| Notification | A message sent to a user when something relevant to them changes | [[notification]] |
| Audit Record | Activity History and Change History captured for auditability | [[audit-record]] |

## ER diagram

> [!warning] Everything except three edges below is this wiki's inference, not a customer deliverable
> Exactly **three** cardinalities are stated by the customer, in so many words, anywhere in the source material. Two are in [[client-organisation]]'s domain (brief §4 *Client Management*): "Multiple Event Organisers may belong to the same organisation, and an organisation may have several past and upcoming events" — one client organisation has many Event Organisers, and one client organisation has many events (brief §4; reaffirmed at #65 and #81). The third is in [[session]]'s domain (brief §4 *Multi-Session Events*): "Some events may contain several sessions within the same day or across multiple days" — one event has many sessions. All three are marked `(STATED — ...)` on their edges below. **Every other edge, and every other cardinality figure, in this diagram is this wiki's structural inference from the two sources' prose** — a proposal the team owns and may revise, not a requirement the customer handed down. Individual object pages already flag which of their own Relationships-table rows are read this way; this diagram is the first place all fourteen are drawn together, and inherits the same caveat wholesale. Do not present this diagram to the customer, or read it back into a requirements document, as if it were confirmed.

```mermaid
erDiagram
    %% ── The only three customer-stated cardinalities in this domain layer ──
    CLIENT_ORGANISATION ||--o{ USER_ACCOUNT : "employs (STATED — brief §4, #65)"
    CLIENT_ORGANISATION ||--o{ EVENT : "commissions (STATED — brief §4)"
    EVENT ||--o{ SESSION : "contains (STATED — brief §4)"

    %% ── Everything below is inferred by this wiki, not customer-stated ──
    CLIENT_ORGANISATION ||--o{ EVENT_REQUEST : authors
    EVENT_REQUEST |o--o| EVENT : "becomes, on approval"
    EVENT ||--o{ BOOKING : reserves
    SESSION ||--|| BOOKING : "books own"
    VENUE ||--o{ BOOKING : hosts
    EVENT ||--o{ EQUIPMENT_RESERVATION : reserves
    SESSION |o--o{ EQUIPMENT_RESERVATION : reserves
    EQUIPMENT_ITEM }o--o{ EQUIPMENT_RESERVATION : fulfils
    EVENT ||--o{ SUPPORT_REQUEST : raises
    EVENT ||--o{ REGISTRATION : accepts
    SESSION |o--o{ REGISTRATION : "may scope"
    EVENT ||--o{ CHANGE_REQUEST : receives
    EVENT ||--o{ AUDIT_RECORD : logs
    EVENT_REQUEST ||--o{ AUDIT_RECORD : logs
    EVENT |o--o{ NOTIFICATION : triggers
    EVENT_REQUEST |o--o{ NOTIFICATION : triggers
    BOOKING |o--o{ NOTIFICATION : triggers
    EQUIPMENT_RESERVATION |o--o{ NOTIFICATION : triggers
    REGISTRATION |o--o{ NOTIFICATION : triggers
    CHANGE_REQUEST |o--o{ NOTIFICATION : triggers
    USER_ACCOUNT ||--o{ NOTIFICATION : receives
    USER_ACCOUNT ||--o{ EVENT_REQUEST : authors
    USER_ACCOUNT ||--o{ EVENT : coordinates
    USER_ACCOUNT ||--o{ EVENT : organises
    USER_ACCOUNT ||--o{ BOOKING : requests
    USER_ACCOUNT ||--o{ BOOKING : decides
    USER_ACCOUNT ||--o{ EQUIPMENT_RESERVATION : reviews
    USER_ACCOUNT }o--o{ SUPPORT_REQUEST : "is assigned to"
    USER_ACCOUNT ||--o{ CHANGE_REQUEST : requests
    USER_ACCOUNT ||--o{ REGISTRATION : holds
    USER_ACCOUNT ||--o{ AUDIT_RECORD : generates
```

Notes on specific edges, where this wiki resolved a disagreement or a gap between two object pages' own Relationships tables (see the task report for the full account):

- **`EQUIPMENT_ITEM` ↔ `EQUIPMENT_RESERVATION` is drawn many-to-many.** [[equipment-item]]'s Relationships table states `1 : many`; [[equipment-reservation]]'s Relationships table states `many : many` and explains "a reservation may cover one or more items/types and quantities." The two pages disagree; many-to-many is the more conservative reading (it does not rule out anything either page documents), so that is what this diagram draws.
- **`USER_ACCOUNT` ↔ `EQUIPMENT_RESERVATION` is drawn many-to-one (reviews), not many-to-many.** [[user-account]]'s Relationships table labels this edge `many : many`, but its own description ("A Technical Support Staff account reviews and reserves many equipment reservations") and [[equipment-reservation]]'s Relationships table (`many : 1`, "the Technical Support Staff member who reviewed and reserved") both describe a single reviewing account per reservation, with no source supporting the reverse (several accounts on one reservation). This diagram follows the more specific, better-cited page.
- **`SESSION` ↔ `REGISTRATION`** is drawn from [[registration]]'s Relationships table (`many : 1 (0..1)`, #74); [[session]]'s own Relationships table does not reciprocate this edge. This is an omission on the session page, not a contradiction — no source disputes the relationship, session.md simply doesn't list it.
- The `EVENT_REQUEST ↔ EVENT` edge is drawn as optional–optional (zero-or-one both sides). [[event]] states `1 : 0..1` and [[event-request]] states `0..1 : 1`; read together (each side describing the same link from its own vantage point) these are consistent, not contradictory, and both resolve to an optional one-to-one.
- `USER_ACCOUNT ↔ EVENT` and `USER_ACCOUNT ↔ BOOKING` are drawn as two separate labelled edges each (coordinates/organises; requests/decides), following [[event]] and [[booking]]'s own `(×2)` notation, rather than the single aggregated `many : many` row [[user-account]] uses for the same pairs — the split form is more precise about which of the two roles is which.

## Modelling-decision register

Decisions the customer **settled**, verified against `raw/IS212 2026 Discussions Aug 31.md`. Each is already recorded, with full context, on the domain page that owns it — this table is the index.

| Decision | Ref | Owning page |
|---|---|---|
| Equipment pooled vs serialised is deferred, but the system must always know available quantity | #13 | [[equipment-item]] |
| Tentative Hold is its own record, with confirm/release transitions | #41 | [[booking]] |
| Booking is separate from Event — bookings release, the event record survives | #69 | [[booking]] |
| Venue availability is slotted AM/PM/Night, not continuous | #50 | [[venue]] |
| Approval ≠ Confirmation; confirmation is blocked while an essential arrangement is incomplete | #80 | [[event]] |
| Equipment has two coexisting availability rules — transfer time governs pre-event availability, Return Day + 1 governs post-event availability | #2, #5 | [[equipment-item]] |
| Equipment reservation allows partial fulfilment, line by line | #90 | [[equipment-reservation]] |
| No formal equipment approval workflow — coordinator records, TSS reviews and reserves directly | #19 | [[equipment-reservation]] |
| Session is a first-class child of Event, with its own venue, equipment, timing and attendance limit | #74 | [[session]] |
| Venue suitability checking is distinct from search/filtering | #83 | [[venue]] |
| Completion has a dual trigger — manual, or automatic at end date + 7 days if still open | #71 | [[event]] |
| Coordinator assignment (and reassignment) is push-only by the Event Operations Manager; no accept/decline step | #93, #94, #95 | [[user-account]] |
| Venue Staff's alternative suggestion on rejection is not a formal counter-offer object | #45 | [[booking]] |
| Double-booking is a hard block, not an overridable warning | #35 | [[booking]] |
| No Postponed status — postponement is either a reschedule or a booking release with the event record retained | #69 | [[event]] |

All fifteen rows were checked against the customer's **Answer** text in `raw/IS212 2026 Discussions Aug 31.md` (not merely the question that prompted it) before inclusion; see the task report for the verification detail on each. Two rows share `#69` because that single answer settles two independent structural facts — that a booking and its event are separate records, and that there is no separate Postponed status — each owned by a different part of the model.

## Deferred-modelling register

Every "Team decision required" callout across the fourteen domain-object pages, collected into one worklist. This is a structural register, not a requirements one.

> [!info] How this differs from [[open-questions]]
> [[open-questions]] registers open **requirements** questions — gaps in what the system must do, organised by feature area, with the customer's position and the resulting team obligation. This register is narrower and more mechanical: it collects only the `🔶 Team decision required` callouts already recorded on the fourteen domain-object pages — decisions about how the **data model itself** is shaped (fields, identity, state machines, record boundaries) once the requirements gap is accepted as a gap. The two overlap where a requirements gap and a structural gap are the same thing (e.g. #74's session identity scheme), and diverge everywhere else (e.g. Release 1 scope, in [[open-questions]], has no structural-modelling counterpart here).

| Object | Question | Ref | Page |
|---|---|---|---|
| Audit Record | Tamper-proofing: append-only store vs. an ordinary editable table | #14 | [[audit-record]] |
| Audit Record | Whether Activity History rolls up to the event or every object type carries its own trail | brief §4 | [[audit-record]] |
| Audit Record | Which specific actions count as "significant" for Activity History | brief §4 | [[audit-record]] |
| Audit Record | Whether attaching/removing a supporting document is itself logged, and how | #21 | [[audit-record]] |
| Booking | Tentative hold expiry and auto-release duration/mechanism | #39, #41 | [[booking]] |
| Booking | Whether any response to post-confirmation venue loss is automated | #54 | [[booking]] |
| Booking | Explicit booking state names — inferred from behaviour, not customer-named | #35, #41, #45 | [[booking]] |
| Change Request | Whether the record needs its own status field, separate from the coordinator-decision attribute | brief §4 | [[change-request]] |
| Change Request | How the significance test is technically implemented (static config vs. evaluated against bookings; auto vs. coordinator-initiated) | #4 | [[change-request]] |
| Change Request | How a reschedule's re-review workflow is sequenced and what status changes it produces | #16 | [[change-request]] |
| Client Organisation | Whether a primary-contact or organisation-administrator role exists | #65, #81 | [[client-organisation]] |
| Client Organisation | What fields the organisation record carries beyond membership and event history | brief §4 | [[client-organisation]] |
| Equipment Item | Pooled vs. serialised inventory model (quantity must always be known) | #13 | [[equipment-item]] |
| Equipment Item | How transfer time, preparation time and current location are represented | #2 | [[equipment-item]] |
| Equipment Item | Whether a maintenance record carries an expected-return date, and re-availability automation | #23 | [[equipment-item]] |
| Equipment Reservation | How the reschedule re-review workflow is implemented, sequenced and status-tracked | #16 | [[equipment-reservation]] |
| Event | Whether a major post-confirmation change formally returns the event to Planning, or is represented some other way | #70 | [[event]] |
| Event | Which fields are copied when an event is created by reuse | #75 | [[event]] |
| Event | Whether a Rejected event can be resubmitted | #67 | [[event]] |
| Event | Comment threading model (threaded vs. flat) | #15 | [[event]] |
| Event | Supporting-document size/type limits | #18 | [[event]] |
| Event Request | Whether an Event Coordinator may also create a request on an Organiser's behalf | #78 | [[event-request]] |
| Event Request | Whether a rejected request can be resubmitted | #67 | [[event-request]] |
| Event Request | The mandatory field set for submission | #72 | [[event-request]] |
| Notification | Recipient matrix — which role(s) receive each of the fourteen §6 trigger scenarios | brief §6 | [[notification]] |
| Notification | How a per-user channel preference interacts with the per-type channel configuration | #37 | [[notification]] |
| Notification | Reminder lead time and its configurability | #40 | [[notification]] |
| Notification | Mechanism linking a venue/equipment availability change to the affected upcoming event(s) | #54 | [[notification]] |
| Registration | What information an Attendee must supply when registering | #22 | [[registration]] |
| Registration | What happens to a freed waiting-list spot once the 3-day acceptance window lapses | #44 | [[registration]] |
| Registration | Whether attendance is a field on Registration or a separate linked record | brief §4 | [[registration]] |
| Registration | Whether Registration is scoped to the event as a whole or to an individual session | #74 | [[registration]] |
| Session | Session attribute set, identity/naming/ordering scheme, and whether it has its own lifecycle | #74 | [[session]] |
| Support Request | How Technical Support Staff availability is represented and maintained | #89 | [[support-request]] |
| Support Request | How often the support roster is updated | #88 | [[support-request]] |
| Support Request | Whether staff assignment is system-assigned, manual, or hybrid | #88 | [[support-request]] |
| User Account | Authentication method | #62 | [[user-account]] |
| User Account | Account recovery and lockout handling | #64 | [[user-account]] |
| User Account | Onboarding / self-registration process for external users | #49, #53 | [[user-account]] |
| User Account | Whether an admin role above the five briefed stakeholders exists | #52 | [[user-account]] |
| User Account | Field-level access-control design (categories given, no matrix) | #91 | [[user-account]] |
| Venue | Booking horizon — how far in advance a venue may be booked | #57 | [[venue]] |
| Venue | Suitability-check presentation (inline indicator, post-selection check, or both) | #83 | [[venue]] |
| Venue | Whether Room Layout is a first-class modelled object or an enum attribute | #83 | [[venue]] |

Forty-four distinct items. [[equipment-item]] states the pooled-vs-serialised question twice in its own text (once as a callout in the Binding Rules section, repeated in Open Modelling Questions "for visibility") — counted once here, not twice.

See [[open-questions]] for the requirements-level register, and the individual domain pages linked above for full citation and context on each item.
