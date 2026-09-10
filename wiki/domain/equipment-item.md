---
title: Equipment Item
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
tags: [domain, equipment]
---

# Equipment Item

A unit (or type/pool — see Open modelling questions) of technical equipment maintained by [[technical-support]], carrying its own location and operational status, and reserved against events through [[equipment-reservation]]. Process detail lives in [[equipment-management]] and [[technical-support]]; this page is the structural reference.

## Attributes

`Scope` is the Release 1 marker from [[release-1-scope]].

| Field | Meaning | Status | Scope | Ref |
|---|---|---|---|---|
| Type | e.g. projector, microphone, VC unit | ✅ Specified | 🧩 Adjacent (features 15–17) | [[event-planning-venue-booking-system\|brief §4]] |
| Description | Free-form technical detail / specification | ✅ Specified | 🧩 Adjacent (features 15–17) | [[event-planning-venue-booking-system\|brief §4]] |
| Quantity | Number of units available; the system must know this regardless of how the item is modelled internally | ✅ Specified | 🧩 Adjacent (features 15–17) | [#13](https://github.com/SinYang13/IS212-2026/discussions/13) |
| Physical location | The item's current location | ✅ Specified | 🧩 Adjacent (features 15–17) | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) |
| Operational status | One of: available, unavailable, damaged, under maintenance | ✅ Specified | 🧩 Adjacent (feature 16) | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) |
| Transfer / preparation time | Time required to move the item from its current location to the event venue; factored into pre-event availability | ✅ Specified | 🧩 Adjacent (features 15–17) | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) |
| Expected return date (while under maintenance) | Whether a maintenance record carries a target return date | 🔶 Team decision | 📋 Backlog | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) |

Operational status is an explicit, TSS-maintained attribute (see Binding rules); it is a different concept from an item being transiently unavailable because it is already committed to another event's D-1 → Return Day + 1 window (see Availability lifecycle, below). Neither source distinguishes the two by separate field names — treating them as two layers on top of the same "available/not available" question is this wiki's structural proposal, not a customer-specified data model.

## States

### Availability lifecycle (pre/post event)

> [!success] Resolved — [#2](https://github.com/SinYang13/IS212-2026/discussions/2)
> T6 flagged an apparent contradiction between [#2](https://github.com/SinYang13/IS212-2026/discussions/2) (transfer/preparation time governs availability) and [#5](https://github.com/SinYang13/IS212-2026/discussions/5) (equipment is only available again from Return Day + 1) on 2026-08-28. The customer resolved this on 2026-08-31: **both rules apply, at different points in the equipment lifecycle — they are not alternatives.** Transfer time affects *pre*-event availability; the Return Day + 1 rule governs *post*-event availability.

The lifecycle chain, as given by the customer ([#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up, 2026-08-31):

```
Current equipment location
→ transfer/preparation to event venue        (governs PRE-event availability — #2)
→ D-1 collection / event commitment period
→ event use
→ return to ConnectSphere
→ inspection/preparation
→ available again from Return Day +1          (governs POST-event availability — #5)
```

The return leg goes back **to ConnectSphere**, not directly to the next venue — transfer time is consumed on the outbound leg of every reservation, measured from wherever the item currently sits ([#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up). How transfer time, preparation time and current location are represented in the solution is left to the team ([#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up) — see Open modelling questions.

### Operational status (Technical Support Staff-maintained field, [#23](https://github.com/SinYang13/IS212-2026/discussions/23))

This diagram covers **only** the explicit operational-status attribute — the four customer-specified values ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) that Technical Support Staff set directly. It does not include commitment-window availability; that is a separate, derived concept covered next.

```mermaid
stateDiagram-v2
    [*] --> Available
    Available --> Damaged : TSS logs damage/defect, typically on return (#86)
    Available --> UnderMaintenance : TSS withdraws item for maintenance
    Damaged --> Available : TSS clears item back into service
    UnderMaintenance --> Available : TSS clears item back into service
```

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Available | No outstanding condition flag; TSS has not marked the item damaged or under maintenance | Entry: newly catalogued, or TSS clears the item back into service. Exit: TSS marks damaged or under maintenance | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) |
| Damaged | Found damaged, typically at return inspection | Entry: TSS logs a defect/damage ([#86](https://github.com/SinYang13/IS212-2026/discussions/86)). Exit: not specified — trigger and process for clearing damaged status back to available are left to the team (🔶) | [#23](https://github.com/SinYang13/IS212-2026/discussions/23); [#86](https://github.com/SinYang13/IS212-2026/discussions/86) |
| Under maintenance | Withdrawn for repair or maintenance | Entry: TSS places item under maintenance. Exit: not specified — no expected-return-date field, and the maintenance workflow itself is not a functional requirement (🔶) | [#23](https://github.com/SinYang13/IS212-2026/discussions/23) |

The three non-default values above (unavailable, damaged, under maintenance) plus the implicit "available" default are customer-specified ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)); the transition edges between Available/Damaged/Under-maintenance are this wiki's structural placeholder, not a specified process — the customer explicitly stated the equipment maintenance workflow is not currently listed as a functional requirement and is recorded only as a consideration ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)). Note that [#23](https://github.com/SinYang13/IS212-2026/discussions/23)'s own "unavailable" value is not diagrammed separately here — see below.

### Commitment-window availability (derived, [#2](https://github.com/SinYang13/IS212-2026/discussions/2) / [#5](https://github.com/SinYang13/IS212-2026/discussions/5))

A second, independent mechanism can make an item non-reservable even when its operational-status field (above) says nothing is wrong with it: the item is already committed to another event and sitting inside that event's D-1 → Return Day + 1 window (see Availability lifecycle, above). This is **not** part of the [#23](https://github.com/SinYang13/IS212-2026/discussions/23) operational-status field — no source names a distinct field for it, so this diagram is this wiki's structural proposal for the derived state, not a customer-specified attribute.

```mermaid
stateDiagram-v2
    [*] --> Free
    Free --> Committed : reserved for an event — enters its D-1 → Return Day+1 window (#2, #5)
    Committed --> Free : Return Day + 1, after inspection (#5)
```

| State | Meaning | Entry / exit rule | Ref |
|---|---|---|---|
| Free | Not currently committed to any event's D-1 → Return Day+1 window | Entry: Return Day + 1, after inspection, or never reserved. Exit: reserved for an event | [#2](https://github.com/SinYang13/IS212-2026/discussions/2); [#5](https://github.com/SinYang13/IS212-2026/discussions/5) |
| Committed | Inside another event's D-1 → Return Day+1 window; not reservable for a conflicting event | Entry: reserved for an event, entering the window. Exit: Return Day + 1, after inspection — computable from the reservation's `return_date` where that is populated, undefined while it is null, since no default is fixed (🔶 [#113](https://github.com/SinYang13/IS212-2026/discussions/113)) | [#2](https://github.com/SinYang13/IS212-2026/discussions/2); [#5](https://github.com/SinYang13/IS212-2026/discussions/5); [#113](https://github.com/SinYang13/IS212-2026/discussions/113) |

The Committed → Free transition is the one edge in this page whose trigger is conditional on data the customer did not commit to supplying. Its trigger date is Return Day + 1, computed from a return date that is carried on the reservation, not the item: `equipment_reservation.return_date`, added by migration D1 and **nullable** ([[schema]]; see [[equipment-reservation]]). So the edge is computable **when that date is populated** and undefined **when it is null** — the customer has confirmed there is **no fixed default return day**, and how the date is captured or determined, automatically or manually, is for the team to propose ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)). The rule is `✅ Specified`; its input is `🔶 Team decision`. The practical consequence for this page: an item's availability is indeterminate — not "available", not "committed" — for as long as it sits on a reservation with a null return date, because the window it is inside has no known end.

An item is reservable for a new event only when its operational status is Available **and** it is Free (not Committed). The [#23](https://github.com/SinYang13/IS212-2026/discussions/23) answer's own "unavailable" value most likely corresponds to this Committed derived state in practice, but the source does not say the two are the same field — treating them as separate is the safer reading, and is this wiki's structural proposal, not a customer-specified data model.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[equipment-reservation]] | 1 : many | An item (or type/pool) is reserved against events through reservation records | [[event-planning-venue-booking-system\|brief §4]]; [#19](https://github.com/SinYang13/IS212-2026/discussions/19) |
| [[event]] | many : many (via equipment-reservation) | Indirect — an event's equipment need is expressed through its reservations, not a direct link | [[event-planning-venue-booking-system\|brief §4]] |
| [[session]] | many : many (via equipment-reservation) | Multi-session events may reserve different equipment per session | [#74](https://github.com/SinYang13/IS212-2026/discussions/74) |
| [[venue]] | — (transient) | The event venue is the destination of the pre-event transfer, not a persistent ownership relationship | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) |

## Binding rules

- Technical Support Staff are responsible for maintaining equipment operational status ([#9](https://github.com/SinYang13/IS212-2026/discussions/9)). ⚠️ Partial: the question that prompted this answer also asked whether Event Coordinators can flag status themselves; the answer affirms TSS's responsibility but does not explicitly rule out a Coordinator-initiated flag — treat "Coordinators may not set status" as an inference, not a confirmed exclusion ([#9](https://github.com/SinYang13/IS212-2026/discussions/9)).
- No same-day sharing between events: equipment may be collected from D-1 and, after return and inspection, is only available again from Return Day + 1 ([#5](https://github.com/SinYang13/IS212-2026/discussions/5)). ⚠️ Partial in practice: the rule is fixed and the date it needs now has a column to sit in ([[schema]]), but there is **no fixed default return day**, so the window's closing date depends on a population mechanism the team has yet to propose ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)).
- Equipment requirements may be changed after equipment has been reserved; affected arrangements are reviewed and updated, and Technical Support Staff re-check suitability and availability where necessary ([#114](https://github.com/SinYang13/IS212-2026/discussions/114)). An item released by such a change re-enters the Free state.
- No maximum reservation period is specified ([#82](https://github.com/SinYang13/IS212-2026/discussions/82)). ⚠️ Partial: the same question also asked about a minimum period, but the answer ("No maximum reservation period.") addressed only the maximum — no minimum-period statement exists in the source ([#82](https://github.com/SinYang13/IS212-2026/discussions/82)).
- No minimum notice period for equipment requests is specified; propose an approach if the solution requires one ([#25](https://github.com/SinYang13/IS212-2026/discussions/25)).
- The post-event equipment health checklist is kept outside the system — "we do not need to keep track in the system" ([#85](https://github.com/SinYang13/IS212-2026/discussions/85)).
- No equipment data import: no data migration, import, or system-integration requirement for now ([#12](https://github.com/SinYang13/IS212-2026/discussions/12)).
- The equipment maintenance workflow is not currently listed as a potential functional requirement; it is recorded only as a consideration ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)).

> [!question]- Team decision required — pooled vs serialised inventory
> Whether equipment is modelled as individually serialised units or as a pooled quantity counter is explicitly left to the team: "How you model equipment individually or as pooled quantities in the database or UX is for you to propose." The one binding constraint is that **the system must know the quantity of equipment available** ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)). This is the single highest-value deferral in the whole domain layer — per-item **location** ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)) and per-item **operational status** ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) both imply state that lives at the unit level, which pulls toward serialisation even though the customer did not mandate it; a pooled model would still need per-unit sub-state to honour [#2](https://github.com/SinYang13/IS212-2026/discussions/2) and [#23](https://github.com/SinYang13/IS212-2026/discussions/23).

## Open modelling questions

> [!question]- Team decision required — pooled vs serialised inventory
> See the callout above — repeated here for visibility in this section. Quantity must always be known ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)); unit-level modelling is not required but is implied by [#2](https://github.com/SinYang13/IS212-2026/discussions/2) and [#23](https://github.com/SinYang13/IS212-2026/discussions/23).

> [!question]- Team decision required — representing transfer time and location
> "How the system represents transfer time, preparation time and the equipment's current location is for the solution team to propose" ([#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up). Options include a per-item location field with a transfer-duration matrix, a flat buffer per venue pair, or folding transfer time into the existing D-1 window.

> [!question]- Team decision required — maintenance return date and re-availability
> Whether a maintenance record carries an expected-return-date field, and whether the system should automatically flag or pre-book equipment once it becomes available again, are both unspecified; propose if appropriate ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)).

See [[domain-model]] for the full object catalogue.
