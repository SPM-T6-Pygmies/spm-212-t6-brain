---
title: Change Request
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Sept 8.md"
tags: [domain, change-request, event]
---

# Change Request

A record of a change an Event Organiser requests against an already-submitted or already-confirmed [[event]], reviewed and decided by the assigned Event Coordinator. Its central function is to carry the **significance test** that decides whether existing arrangements must be revalidated. Process detail lives in [[change-impact-management]]; this page is the structural reference.

**Release 1 scope:** Per [[release-1-scope]], event change requests and the impact-of-change revalidation this page structures are 🎯 Release 1 (core feature 19); rescheduling is 🧩 Adjacent, pulled in by feature 19's naming of date/time as a significant change category.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Requesting organiser | The [[user-account]] who requested the change | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Event | The [[event]] the change applies to | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Requested change | Description of the change sought (new date/time, attendance, equipment, etc.) | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Significance flag | Whether the change is classed as significant (triggers revalidation) or ordinary (does not). Governs revalidation **only** — it never governs who may edit | ⚠️ Partial | [#4](https://github.com/SinYang13/IS212-2026/discussions/4); [#102](https://github.com/SinYang13/IS212-2026/discussions/102) |
| Fields affected | Which event fields the change touches. **A single request may cover several fields at once** | ✅ Specified | [#4](https://github.com/SinYang13/IS212-2026/discussions/4); [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Coordinator decision | The Event Coordinator's review outcome. **Per affected field, not per request** — some changes in one request may be accepted while others are not. Held as `change_request_item.decision` (`Pending` / `Accepted` / `Rejected`) | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Reason / follow-up for a change not accepted | Why a particular field's change was declined, and what happens next; must reach the Event Organiser. Held per item as `change_request_item.decision_reason`, which [[schema]] makes **not-null on a rejected item** | ✅ Specified | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Impact assessment | Record of which arrangements were reviewed again as a result of the change | ⚠️ Partial | [[event-planning-venue-booking-system\|brief §4]] |

### The record is one-to-many over the fields it changes

[#104](https://github.com/SinYang13/IS212-2026/discussions/104) makes the change request a **container, not a single atomic edit**: one request may span several fields, and the coordinator may accept some of them and decline others. Three consequences follow for the model:

| Consequence | Detail | Status | Ref |
|---|---|---|---|
| Outcome is per field | A single approve/reject boolean on the request cannot represent a partial acceptance. The decision, and the reason where a change is declined, attach to the individual requested field-change | ✅ Specified | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Significance is per field | Significance is a property of *what changed*, so a mixed request can carry both significant and ordinary items, and only the significant, accepted ones trigger revalidation | ⚠️ Partial | [#4](https://github.com/SinYang13/IS212-2026/discussions/4); [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| Physical structure | The choice between a child line-item table, a structured JSON payload and one request record per field was left open — "the exact workflow and presentation may be proposed by the team". **The team has since taken it**: a `change_request_item` child table, deployed 2026-09-09 (migration D4). The remaining openness is workflow and presentation, not structure — see below | 🔶 Team decision | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |

### The chosen structure: a `change_request_item` child table

[[schema]] now models the per-field outcome as one `change_request_item` row per requested field change, each carrying `field_name`, `requested_value`, its own `significance_flag`, a `decision` of `Pending` / `Accepted` / `Rejected`, and a `decision_reason`. A partially accepted request is therefore representable: accepted and rejected items coexist under one `change_request`, and significance is evaluated per item, so only the significant *accepted* ones drive revalidation.

The customer's requirement that a declined change carry a reason ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)) is enforced declaratively rather than left to the application: `change_request_item_reason_chk` makes `decision_reason` mandatory whenever `decision = 'Rejected'`. A rejected item without a reason cannot be written at all. What the constraint does **not** enforce is delivery — that the reason or follow-up actually reaches the Event Organiser remains application behaviour, and the channel is still for the team.

> [!warning] Dual representation — the flat columns are no longer authoritative
> `change_request` retains its original flat `significance_flag`, `fields_affected` and `coordinator_decision` columns alongside the new child table, and [[schema]] did not drop them. Once a request has `change_request_item` rows those columns **stop being authoritative** and the item rows are the truth. Nothing in the DDL prevents the two from disagreeing, so any reader — report, notification, UI — that consults the flat columns on a multi-field request will read a stale or lossy answer. Decide per code path which representation is being read, and prefer the items.

## States

Not specified. Neither source names a status set for the change-request record itself; the record is characterised by its `coordinator decision` attribute rather than a defined lifecycle ([[event-planning-venue-booking-system|brief §4]]). Partial acceptance sharpens the gap rather than closing it: because outcomes are per field, any request-level status would have to be **derived** from the `change_request_item` rows (fully accepted / partially accepted / not accepted), and the customer names no such value — the child table supplies the inputs for such a derivation but does not itself define one. See Open modelling questions.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | Each change request targets exactly one event | [[event-planning-venue-booking-system\|brief §4]] |
| [[user-account]] | many : 1 | Requesting Event Organiser | [[event-planning-venue-booking-system\|brief §4]] |
| Requested field change (`change_request_item` in [[schema]]) | 1 : many | One row per field the request touches, each independently significant-or-not and accepted-or-rejected; cascades on delete with its parent request. Not a separately addressable domain object — it has no life outside its request | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |

## Binding rules

- **All post-submission change is coordinator-mediated.** After submission the Event Organiser may not edit directly — including non-significant fields. Changes are communicated to or requested through the Event Coordinator, who processes the update in the system ([#102](https://github.com/SinYang13/IS212-2026/discussions/102)). The change request is therefore the *only* route by which an Organiser alters a submitted event, and the significance test no longer has any bearing on who may edit — only on what must be revalidated.
- **Partial acceptance is permitted.** A single request may cover several fields, some accepted and some not; where a change is not accepted, the reason or follow-up must be communicated to the Event Organiser ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)).
- Equipment requirements **may** be changed after equipment has been reserved. Where the change affects an existing reservation, the affected equipment arrangements are reviewed and updated and Technical Support Staff re-check suitability and availability; the release/replace/add workflow is for the team ([#114](https://github.com/SinYang13/IS212-2026/discussions/114)).
- If an accepted change leaves a confirmed event no longer confirmed, the Event Organiser must be informed ([#105](https://github.com/SinYang13/IS212-2026/discussions/105)).
- **Significance test**: a change to date/time, attendance, venue requirements, equipment requirements, or anything else that could invalidate a confirmed arrangement counts as **significant** and triggers revalidation of the affected arrangements; ordinary edits that do not touch existing arrangements do not ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)).
- A reschedule must follow the full event-change process and may not bypass required checks or approvals — this applies even when initiated via a calendar drag-and-drop interaction ([#3](https://github.com/SinYang13/IS212-2026/discussions/3)).
- A reschedule requires the equipment arrangement to be reviewed again; there is no guaranteed auto-carry-over ([#16](https://github.com/SinYang13/IS212-2026/discussions/16)).
- There is no change cut-off period before the event date — changes, and the change requests that carry them, remain possible right up to delivery ([#8](https://github.com/SinYang13/IS212-2026/discussions/8)).

## Open modelling questions

> [!question]- Team decision required — independent status field for change requests
> Neither source names a status set for the change-request record itself. The only confirmed fact is that the Event Coordinator reviews and decides on a requested change ([[event-planning-venue-booking-system|brief §4]]). Whether the record needs its own status field (e.g. pending / approved / rejected) separate from the `coordinator decision` attribute, or whether the decision attribute *is* the status, is left to the team ([[event-planning-venue-booking-system|brief §4]]). Any such status must now be reconcilable with per-field outcomes — a request whose fields were partly accepted is neither approved nor rejected ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)).

> [!question]- Team decision required — partial-acceptance workflow and presentation
> The **structure** is settled — `change_request_item`, see above — but the workflow and presentation around it are not: how the coordinator is walked through recording a per-field outcome, how a mixed result is presented to the Event Organiser, and by what channel the required reason or follow-up for a declined field is delivered remain for the team, since "the exact workflow and presentation may be proposed by the team" ([#104](https://github.com/SinYang13/IS212-2026/discussions/104)). Also unsettled: whether an item's `Pending` decision may be left indefinitely, and which representation — item rows or the retained flat columns — the rest of the system reads.

> [!question]- Team decision required — impact-analysis implementation
> The significance test itself is customer-confirmed (see Binding rules), but its technical implementation — static per-field configuration vs. evaluation against what is currently booked, automatic vs. coordinator-initiated revalidation, and whether affected arrangements are invalidated outright or flagged pending review — is left to the team ([#4](https://github.com/SinYang13/IS212-2026/discussions/4)).

> [!question]- Team decision required — reschedule re-review workflow
> How a reschedule's equipment (and venue) re-review is sequenced, what status changes it produces, and its UX are unspecified: "How you implement that workflow is for you to propose... change of status or UX is for your recommendation" ([#16](https://github.com/SinYang13/IS212-2026/discussions/16)).

See [[domain-model]] for the full object catalogue.
