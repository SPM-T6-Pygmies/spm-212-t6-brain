---
title: Change Request
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, change-request, event]
---

# Change Request

A record of a change an Event Organiser requests against an already-submitted or already-confirmed [[event]], reviewed and decided by the assigned Event Coordinator. Its central function is to carry the **significance test** that decides whether existing arrangements must be revalidated. Process detail lives in [[change-impact-management]]; this page is the structural reference.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Requesting organiser | The [[user-account]] who requested the change | ✅ Specified | brief §4 |
| Event | The [[event]] the change applies to | ✅ Specified | brief §4 |
| Requested change | Description of the change sought (new date/time, attendance, equipment, etc.) | ✅ Specified | brief §4 |
| Significance flag | Whether the change is classed as significant (triggers revalidation) or ordinary (does not) | ⚠️ Partial | #4 |
| Fields affected | Which event fields the change touches | ⚠️ Partial | #4 |
| Coordinator decision | The Event Coordinator's review outcome on the requested change | ✅ Specified | brief §4 |
| Impact assessment | Record of which arrangements were reviewed again as a result of the change | ⚠️ Partial | brief §4 |

## States

Not specified. Neither source names a status set for the change-request record itself; the record is characterised by its `coordinator decision` attribute rather than a defined lifecycle (brief §4). See Open modelling questions.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | Each change request targets exactly one event | brief §4 |
| [[user-account]] | many : 1 | Requesting Event Organiser | brief §4 |

## Binding rules

- **Significance test**: a change to date/time, attendance, venue requirements, equipment requirements, or anything else that could invalidate a confirmed arrangement counts as **significant** and triggers revalidation of the affected arrangements; ordinary edits that do not touch existing arrangements do not (#4).
- A reschedule must follow the full event-change process and may not bypass required checks or approvals — this applies even when initiated via a calendar drag-and-drop interaction (#3).
- A reschedule requires the equipment arrangement to be reviewed again; there is no guaranteed auto-carry-over (#16).
- There is no change cut-off period before the event date — changes, and the change requests that carry them, remain possible right up to delivery (#8).

## Open modelling questions

> [!question]- Team decision required — independent status field for change requests
> Neither source names a status set for the change-request record itself. The only confirmed fact is that the Event Coordinator reviews and decides on a requested change (brief §4). Whether the record needs its own status field (e.g. pending / approved / rejected) separate from the `coordinator decision` attribute, or whether the decision attribute *is* the status, is left to the team (brief §4).

> [!question]- Team decision required — impact-analysis implementation
> The significance test itself is customer-confirmed (see Binding rules), but its technical implementation — static per-field configuration vs. evaluation against what is currently booked, automatic vs. coordinator-initiated revalidation, and whether affected arrangements are invalidated outright or flagged pending review — is left to the team (#4).

> [!question]- Team decision required — reschedule re-review workflow
> How a reschedule's equipment (and venue) re-review is sequenced, what status changes it produces, and its UX are unspecified: "How you implement that workflow is for you to propose... change of status or UX is for your recommendation" (#16).

See [[domain-model]] for the full object catalogue.
