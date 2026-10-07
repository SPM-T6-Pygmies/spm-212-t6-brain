---
title: Equipment Management
type: concept
sources:
  - Customer Brief no. 1.md
  - IS212 2026 Discussions Aug 26.md
  - IS212 2026 Discussions Aug 31.md
  - IS212 2026 Discussions Sept 8.md
tags: [equipment, inventory, availability, reservation, maintenance, business-rules]
---

# Equipment Management

**Objects:** [[equipment-item]] · [[equipment-reservation]] · [[event]]

Catalogue, availability, reservation, and condition tracking for ConnectSphere's event equipment. The staffing and review side of the same workflow is covered in [[technical-support]].

**Release 1 scope:** Equipment request management, equipment availability checking, and equipment reservation are all `🎯 Release 1` (core features 15–17) — covering the Availability rules, No Same-Day Sharing and Reservation Rules sections below. The equipment catalogue itself (Equipment Catalogue, Pooled vs Serialised Modelling) is `🧩 Adjacent`, pulled in because features 15–17 need equipment types and quantities to exist before anything can be requested, checked or reserved; equipment maintenance status (Maintenance, Damage, and Defect Logging) is likewise `🧩 Adjacent`, pulled in by feature 16's requirement that unavailable equipment not be treated as available. See [[release-1-scope]] for the full register.

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

### The Return Day Has No Default

The post-event rule above is committed and non-negotiable, but the date it pivots on is not. The customer confirmed on 2026-09-07 that **there is currently no fixed default return day**, and that the team may propose how the return date is captured or determined — whether automatically populated or entered manually (Source: is212-discussions-2026-09-08, [#113](https://github.com/SinYang13/IS212-2026/discussions/113)).

This is a `🔶 Team decision` sitting directly underneath a `✅ Specified` rule, and it is load-bearing rather than incidental: every availability answer the system gives for feature 16 depends on knowing when each commitment window closes, and no window closes without a return date. The mechanism the team picks — derive it from the event or session end date, have the Event Coordinator enter it with the requirement, or have Technical Support Staff set it at collection — effectively *becomes* the business rule and should be documented as one.

The **field** is no longer the problem. Migration D1 (2026-09-09) added a nullable `return_date` to the reservation, so the rule is representable and feature 16 has something to compute from ([[schema]]; [[equipment-reservation]] for where the field sits). The gap has moved from "the date cannot be stored" to "the date may not be there": nullable with no default means a reservation can exist with no known window end, and the availability answer for the equipment on it is undefined until one is supplied. Fixing the mechanism is what would let the column become `not null` or acquire a default — until then neither is safe to add.

**Team decision (2026-10-07, [#113](https://github.com/SinYang13/IS212-2026/discussions/113)):** the return date is set automatically to the event's date, and follows it if the date changes; nobody enters it (SPM-275). Events have a single date, so equipment is committed from D-1 through the event's date and is available again the day after. An event with no date cannot be reserved for (SPM-274), so no reservation is left without a return date.

### Lifecycle Chain

The full lifecycle chain, as given verbatim by the customer (Source: is212-discussions-2026-08-31, [#2](https://github.com/SinYang13/IS212-2026/discussions/2) follow-up, 2026-08-31), is diagrammed on [[equipment-item]] (Availability lifecycle).

Note the return leg: equipment goes back **to ConnectSphere** for inspection, not directly to the next venue. Transfer time is therefore consumed on the outbound leg of every reservation, measured from wherever the item currently sits.

> [!success] Resolved — [#2](https://github.com/SinYang13/IS212-2026/discussions/2)
> "How the system represents transfer time, preparation time and the equipment's current location is for the solution team to propose" ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)). The team decided on 2026-10-07 to fold transfer and preparation time into the **D-1 collection day**: no transfer-duration field or buffer (SPM-275). Location stays one field per equipment type, for Technical Support's reference.

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
| Partial reservation when only some items are available | **Allowed** across lines ([#90](https://github.com/SinYang13/IS212-2026/discussions/90)). Within one line: none — reserved in full or marked unfulfilled with a comment (team decision, 2026-10-07, SPM-274) |
| Effect of a reschedule on existing reservations | Arrangement must be reviewed again; workflow for the team to propose ([#16](https://github.com/SinYang13/IS212-2026/discussions/16)) — see [[change-impact-management]] |
| Changing equipment requirements **after** reservation | **Allowed**; affected arrangements reviewed and updated, TSS re-check suitability and availability; release/replace/add workflow for the team to propose ([#114](https://github.com/SinYang13/IS212-2026/discussions/114)) |
| Default return day | **None** — no fixed default; capture mechanism for the team to propose ([#113](https://github.com/SinYang13/IS212-2026/discussions/113)). Team decision (2026-10-07): the event's date, set automatically (SPM-275) |

**Snapshot note:** [#82](https://github.com/SinYang13/IS212-2026/discussions/82) was **unanswered** in the Aug 26 snapshot and answered on 2026-08-31 with "No maximum reservation period." Any backlog item written before that date against [#82](https://github.com/SinYang13/IS212-2026/discussions/82) should be revisited.

### Partial Reservation

Technical Support Staff may reserve the equipment that is available and suitable even when other requested items are unavailable. They should identify which requirements can be fulfilled and highlight unavailable items or suggest alternatives. **The Event Coordinator then determines whether the outstanding requirement affects overall event readiness** (Source: is212-discussions-2026-08-31, [#90](https://github.com/SinYang13/IS212-2026/discussions/90)).

The answer allows partial reservation across *different* items; it does not require reserving part of a single line. The team decided on 2026-10-07 that a line is all-or-nothing: if 5 are requested and 3 are available, Technical Support mark the line unfulfilled with a comment ("only 3 available"), and the coordinator can change the request to 3, which brings it back for review (SPM-274).

[#106](https://github.com/SinYang13/IS212-2026/discussions/106) refines who makes that call. Deciding whether an incomplete equipment requirement is **essential** to the event is a joint determination: "the Event Coordinator should determine together with the relevant Technical Support Staff whether an incomplete equipment requirement is essential to the event." If it is essential, the event must not be confirmed until it is resolved (Source: is212-discussions-2026-09-08, [#106](https://github.com/SinYang13/IS212-2026/discussions/106)). The final act of confirming still follows the existing rule on which role is authorised to confirm an event — [#106](https://github.com/SinYang13/IS212-2026/discussions/106) gives Technical Support Staff a voice in the essentialness judgement, not a confirmation veto. See [[technical-support]] and [[event-readiness]].

Implication: an equipment requirement is not atomic. Its fulfilment state is per-line-item, and the roll-up to "ready / not ready" is a human judgement, not a system computation — and on the essentialness question specifically, a two-role judgement — see [[event-readiness]].

### Changing Requirements After Reservation

Equipment requirements are not frozen once equipment has been reserved. The customer confirmed that they **may be changed after reservation**; where a change affects an existing reservation, the affected equipment arrangements should be reviewed and updated, and Technical Support Staff should re-check suitability and availability where necessary. The exact workflow for **releasing, replacing, or adding** equipment is for the team to propose (Source: is212-discussions-2026-09-08, [#114](https://github.com/SinYang13/IS212-2026/discussions/114)).

This generalises [#16](https://github.com/SinYang13/IS212-2026/discussions/16) (a reschedule forces re-review) from the event's date/time to the requirement itself, and it explains what [#90](https://github.com/SinYang13/IS212-2026/discussions/90)'s partial reservation does next: an unfulfilled line is not a dead end, because the requirement behind it can be revised. Both paths converge on the same TSS re-check, so design them as one mechanism rather than two. The states already documented on [[equipment-reservation]] absorb all three operations without new vocabulary — *release* reuses `Released`, *replace* is a release plus a new line at `Recorded`, *add* is a new line at `Recorded` — so what is genuinely open is sequencing and record identity, not the status set.

## Pooled vs Serialised Modelling

- The customer declined to prescribe whether equipment is tracked as individually serialised items or as a pooled quantity counter ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)).
- The single binding constraint: **"the system needs to know the quantity of equipment available"** (Source: is212-discussions-2026-08-31, [#13](https://github.com/SinYang13/IS212-2026/discussions/13)).

> [!success] Resolved — [#13](https://github.com/SinYang13/IS212-2026/discussions/13)
> The team chose a **pooled** model, built in SPM-40: one record per equipment type, holding the number owned and one location. The per-unit pull this callout used to flag is settled by two 2026-10-07 decisions: transfer time is the D-1 collection day, so no per-unit location is needed ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)), and status is a per-type count of units out of service ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)). The number available for an event is derived: owned, minus out of service, minus units reserved by events whose D-1 → Return Day + 1 window overlaps.

## Maintenance, Damage, and Defect Logging

- **Technical Support Staff are responsible for maintaining equipment status** (Source: is212-discussions-2026-08-31, [#9](https://github.com/SinYang13/IS212-2026/discussions/9)). Event Coordinators do not set it. See [[user-roles]] and [[access-control]].
- Equipment can be recorded as unavailable, damaged, or under maintenance ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)).
- Equipment that is damaged, being repaired, or otherwise unavailable must **not** be treated as available for event use (Source: customer-brief-no-1).
- On return, Technical Support Staff can record **defects, damage, missing items, or other issues**, and the operational status of the affected item is updated accordingly (Source: is212-discussions-2026-08-31, [#86](https://github.com/SinYang13/IS212-2026/discussions/86)).
- Reserved equipment becoming unavailable through damage or maintenance is an explicit change-impact scenario in the brief and a [[notifications]] trigger.
- **Team decision (2026-10-07, [#23](https://github.com/SinYang13/IS212-2026/discussions/23)):** Release 1 records only how many units of each type are **out of service**, set on the equipment catalogue, and leaves them out of availability (SPM-17). Telling unavailable, damaged and under maintenance apart, and logging defects on return ([#86](https://github.com/SinYang13/IS212-2026/discussions/86)), are 📋 Backlog — see [[release-1-scope]].

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
| 2 | Equipment location / transit downtime | Location exists; transfer + prep time affects pre-event availability; representation **team to propose** — decided 2026-10-07: the D-1 collection day covers it |
| 5 | Same-day sharing | Not allowed; D-1 collection, available again Return Day +1 |
| 9 | Who sets damaged/unavailable | Technical Support Staff |
| 13 | Pooled vs serialised | **Team to propose**; quantity must be known — decided: pooled, one record per type (SPM-40) |
| 16 | Reschedule effect | Re-review required; workflow **team to propose** |
| 23 | Maintenance return date / pre-booking | Not specified; maintenance workflow not a requirement. Status decided 2026-10-07: a per-type count of units out of service only |
| 82 | Max reservation period | None (answered 2026-08-31; unanswered in Aug 26 snapshot) |
| 85 | Post-event health checklist | Outside the system |
| 86 | Defect logging on return | Supported; status updated accordingly |
| 90 | Partial reservation | Allowed across lines; coordinator judges readiness impact. Within one line: none (decided 2026-10-07) |
| 106 | Who decides an incomplete requirement is not blocking | Event Coordinator **together with** the relevant Technical Support Staff; if essential, no confirmation until resolved |
| 113 | Default return day | **None** — capture mechanism **team to propose**, though Return Day + 1 depends on it — decided 2026-10-07: the event's date, set automatically |
| 114 | Changing requirements after reservation | Allowed; re-review and TSS re-check; release/replace/add workflow **team to propose** |

See also [[technical-support]], [[event-readiness]], [[change-impact-management]], [[open-questions]].
