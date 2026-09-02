---
title: Equipment Management
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
tags: [equipment, inventory, availability, reservation, maintenance, business-rules]
---

# Equipment Management

**Objects:** [[equipment-item]] · [[equipment-reservation]] · [[event]]

Catalogue, availability, reservation, and condition tracking for ConnectSphere's event equipment. The staffing and review side of the same workflow is covered in [[technical-support]].

## Equipment Catalogue

The system maintains equipment records with the fields defined on [[equipment-item]] — type, description, quantity, physical location, and operational status (Source: customer-brief-no-1).

## Availability: Two Rules That Coexist

Two separate rules constrain when a piece of equipment can be committed to an event. They govern **different ends** of the event.

> [!warning] Contradiction (raised, then resolved)
> On 2026-08-28 the team flagged [#2](https://github.com/SinYang13/IS212-2026/discussions/2) ("transfer and preparation time should be considered when determining availability") as apparently contradicting [#5](https://github.com/SinYang13/IS212-2026/discussions/5) ("equipment is returned to ConnectSphere for assessment and is only available again from Return Day + 1"), and asked which workflow stands. The customer resolved this on 2026-08-31: **both rules apply, at different points in the equipment lifecycle. They are not alternatives to each other.** (Source: is212-discussions-2026-08-31, [#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up)

| Rule | Governs | Source |
|---|---|---|
| Transfer and preparation time from the equipment's current location to the event venue must be considered | **Pre-event** availability | [#2](https://github.com/SinYang13/IS212-2026/discussions/2) |
| Equipment may be collected from **D-1**; after return and inspection it is available again only from **Return Day + 1** | **Post-event** availability | [#5](https://github.com/SinYang13/IS212-2026/discussions/5), restated in [#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up |

### Lifecycle Chain

The full lifecycle chain, as given verbatim by the customer (Source: is212-discussions-2026-08-31, [#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up, 2026-08-31), is diagrammed on [[equipment-item]] (Availability lifecycle).

Note the return leg: equipment goes back **to ConnectSphere** for inspection, not directly to the next venue. Transfer time is therefore consumed on the outbound leg of every reservation, measured from wherever the item currently sits.

> [!question]- Team decision required — representing transfer time and location
> "How the system represents transfer time, preparation time and the equipment's current location is for the solution team to propose" ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)). Options include a per-item location field with a transfer-duration matrix, a flat buffer, or folding transfer into the D-1 window.

### Snapshot Note

In the Aug 26 snapshot, [#2](https://github.com/SinYang13/IS212-2026/discussions/2) was **answered** with only the transfer-time rule and no reconciliation with [#5](https://github.com/SinYang13/IS212-2026/discussions/5). The apparent conflict was raised as a follow-up on 2026-08-28, which re-opened the question, and was resolved 2026-08-31. The Aug 31 snapshot still lists [#2](https://github.com/SinYang13/IS212-2026/discussions/2) under *Unanswered* because it was re-opened by that follow-up, even though the follow-up itself contains the resolution. Cite [[is212-discussions-2026-08-31]] for the reconciled rule.

## No Same-Day Sharing

A single piece of equipment **cannot** be shared between two events on the same day (e.g. a morning event and an afternoon event) (Source: is212-discussions-2026-08-31, [#5](https://github.com/SinYang13/IS212-2026/discussions/5)).

This follows directly from the D-1 / Return Day +1 rule: the effective commitment window for one event spans at minimum from D-1 through Return Day +1, which necessarily swallows any other same-day event. The practical consequence for the [[venue-booking]] three-slot model is that equipment does **not** follow venue slot granularity — a venue can host AM, PM, and Night events on one day, but the same projector cannot serve all three.

## Reservation Rules

| Rule | Position |
|---|---|
| Maximum reservation period | **None** (Source: is212-discussions-2026-08-31, [#82](https://github.com/SinYang13/IS212-2026/discussions/82)) |
| Minimum reservation period | Not stated; [#82](https://github.com/SinYang13/IS212-2026/discussions/82) asked about both max and min, and only the maximum was answered |
| Minimum notice period for equipment requests | **Not specified** — team to propose ([#25](https://github.com/SinYang13/IS212-2026/discussions/25)); see [[technical-support]] |
| Partial reservation when only some items are available | **Allowed** ([#90](https://github.com/SinYang13/IS212-2026/discussions/90)) |
| Effect of a reschedule on existing reservations | Arrangement must be reviewed again; workflow for the team to propose ([#16](https://github.com/SinYang13/IS212-2026/discussions/16)) — see [[change-impact-management]] |

**Snapshot note:** [#82](https://github.com/SinYang13/IS212-2026/discussions/82) was **unanswered** in the Aug 26 snapshot and answered on 2026-08-31 with "No maximum reservation period." Any backlog item written before that date against [#82](https://github.com/SinYang13/IS212-2026/discussions/82) should be revisited.

### Partial Reservation

Technical Support Staff may reserve the equipment that is available and suitable even when other requested items are unavailable. They should identify which requirements can be fulfilled and highlight unavailable items or suggest alternatives. **The Event Coordinator then determines whether the outstanding requirement affects overall event readiness** (Source: is212-discussions-2026-08-31, [#90](https://github.com/SinYang13/IS212-2026/discussions/90)).

Implication: an equipment requirement is not atomic. Its fulfilment state is per-line-item, and the roll-up to "ready / not ready" is a coordinator judgement, not a system computation — see [[event-readiness]].

## Pooled vs Serialised Modelling

- The customer declined to prescribe whether equipment is tracked as individually serialised items or as a pooled quantity counter ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)).
- The single binding constraint: **"the system needs to know the quantity of equipment available"** (Source: is212-discussions-2026-08-31, [#13](https://github.com/SinYang13/IS212-2026/discussions/13)).

> [!question]- Team decision required — pooled vs serialised inventory
> Pooled vs serialised inventory modelling. This choice is load-bearing — the D-1 / Return Day +1 rule and the per-item **location** and **operational status** attributes ([#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#23](https://github.com/SinYang13/IS212-2026/discussions/23)) all imply per-item state, which is awkward to express against a bare pooled counter. A pooled model would need per-unit sub-state to satisfy [#2](https://github.com/SinYang13/IS212-2026/discussions/2) and [#23](https://github.com/SinYang13/IS212-2026/discussions/23) anyway.

## Maintenance, Damage, and Defect Logging

- **Technical Support Staff are responsible for maintaining equipment status** (Source: is212-discussions-2026-08-31, [#9](https://github.com/SinYang13/IS212-2026/discussions/9)). Event Coordinators do not set it. See [[user-roles]] and [[access-control]].
- Equipment can be recorded as unavailable, damaged, or under maintenance ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)).
- Equipment that is damaged, being repaired, or otherwise unavailable must **not** be treated as available for event use (Source: customer-brief-no-1).
- On return, Technical Support Staff can record **defects, damage, missing items, or other issues**, and the operational status of the affected item is updated accordingly (Source: is212-discussions-2026-08-31, [#86](https://github.com/SinYang13/IS212-2026/discussions/86)).
- Reserved equipment becoming unavailable through damage or maintenance is an explicit change-impact scenario in the brief and a [[notifications]] trigger.

### Not in Scope

| Item | Position |
|---|---|
| Expected return date field for equipment under maintenance | **Not specified** — team may propose ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) |
| Automatic flagging / pre-booking when equipment returns to service | **Not specified** — team may propose ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) |
| Full equipment maintenance workflow | Not listed as a functional requirement; recorded by the customer as a *consideration* only ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) |
| Post-event equipment health checklist | **Kept outside the system** — "we do not need to keep track in the system" ([#85](https://github.com/SinYang13/IS212-2026/discussions/85)) |

The [#85](https://github.com/SinYang13/IS212-2026/discussions/85) answer draws a clear scope boundary: the *outcome* of the post-event assessment (defect, damage, status change) is recorded in the system per [#86](https://github.com/SinYang13/IS212-2026/discussions/86), but the **checklist instrument itself is not modelled**. Do not build checklist templates or per-item inspection forms.

## Open Points Summary

| # | Question | Customer position |
|---|---|---|
| 2 | Equipment location / transit downtime | Location exists; transfer + prep time affects pre-event availability; representation **team to propose** |
| 5 | Same-day sharing | Not allowed; D-1 collection, available again Return Day +1 |
| 9 | Who sets damaged/unavailable | Technical Support Staff |
| 13 | Pooled vs serialised | **Team to propose**; quantity must be known |
| 16 | Reschedule effect | Re-review required; workflow **team to propose** |
| 23 | Maintenance return date / pre-booking | Not specified; maintenance workflow not a requirement |
| 82 | Max reservation period | None (answered 2026-08-31; unanswered in Aug 26 snapshot) |
| 85 | Post-event health checklist | Outside the system |
| 86 | Defect logging on return | Supported; status updated accordingly |
| 90 | Partial reservation | Allowed; coordinator judges readiness impact |

See also [[technical-support]], [[event-readiness]], [[change-impact-management]], [[open-questions]].
