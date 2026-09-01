---
title: Audit Record
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, audit, history]
---

# Audit Record

The record captured by the system in support of auditability: **Activity History** (who did what, and when — brief §4) and **Change History** (how a field's value evolved over time — brief §4), together satisfying the requirement that ConnectSphere can determine "what was changed, who changed it, and when the change was made" (brief §8f). Process detail lives in [[reporting]] and [[non-functional-requirements]]; this page is the structural reference.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Actor | The [[user-account]] who performed the action or made the change | ✅ Specified | brief §8f; #14 |
| Timestamp | When the action or change occurred | ✅ Specified | brief §8f; #14 |
| Related object | The [[event]] (or other record) the entry concerns | ✅ Specified | brief §8f |
| Action | For an Activity History entry: which significant action was performed | ✅ Specified | brief §4 |
| Field changed | For a Change History entry: which field's value changed | ✅ Specified | brief §4; §8f |
| Old value / new value | For a Change History entry: the value before and after the change | ✅ Specified | brief §8f |
| Storage mechanism | Append-only store vs. an ordinary editable table | 🔶 Team decision | #14 |

## Activity History

Significant actions are recorded, including who performed the action and when, to support accountability and troubleshooting (brief §4). Brief §4's wording is not restricted to a single object type — "significant actions," unqualified — so this wiki treats Activity History as applicable wherever a significant action occurs (event review, coordinator assignment, venue booking decisions, equipment reservation, registration, etc.), not solely against the [[event]] record. No source enumerates which specific actions count as "significant."

## Change History

Users with appropriate access can review how important **event** information has changed over time (brief §4). Unlike Activity History's unqualified "significant actions," brief §4's Change History row names *event information* specifically — this wiki does not extend Change History to other object types without a source basis. Its two binding facts:

- It captures **what changed, who changed it, and when** — the same what/who/when triad as the general auditability requirement (brief §8f; #14).
- It is **access-restricted**: "Users with appropriate access can review how important event information has changed over time" (brief §4) — not available to every user who can view the event.

> [!warning] Activity History's access model is not separately stated
> Brief §4 states an explicit access restriction ("appropriate access") only for Change History. No equivalent sentence restricts who may view Activity History. Treat any assumption that Activity History follows the same restriction as an inference governed by the general [[access-control]] model, not a confirmed rule in its own right.

Supporting documents are excluded from Change History's diffing: they are not versioned, "as they are not legal documents, we do not need to keep any versions" (#21) — so a document's history is limited to add/delete events, not content-level diffs.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | Activity-history and change-history entries recorded against the event | brief §8f |
| [[event-request]] | many : 1 | Decision record (approve / reject / return) and pre-approval activity history | brief §5; §8f |
| [[user-account]] | many : 1 | Every entry has an actor | brief §8f; #14 |

## Binding rules

- Every entry must capture **what changed, who changed it, and when** — the auditability requirement is stated at this level of generality and no further (brief §8f; #14).
- Change History is access-restricted to users with appropriate rights; it is not visible to every user who can view the underlying event (brief §4).
- Completed and cancelled events, and by extension their history, remain available as historical records — history is not purged when an event closes (#20).
- 🔶 Tamper-proofing is a technical mechanism left to the team: "The requirement is that important actions and changes are recorded so that ConnectSphere can determine what changed, who changed it, and when. The technical mechanism used to achieve this is for you to propose." An append-only store and a regular editable table are both acceptable so long as the what/who/when triad is recoverable (#14).

> [!info] No retention policy — archival and housekeeping are out of scope
> "No retention policy for this system as yet. Completed and cancelled events may remain available as historical records; archival and housekeeping are outside the current functional scope." (#20). This bounds the work: there is no requirement to purge, archive, or age out audit records, and building an archival/housekeeping mechanism is explicitly not required for the current scope.

## Open modelling questions

> [!question]- Team decision required — tamper-proofing mechanism
> Whether the audit store is append-only (no edits or deletes) or an ordinary database table is left entirely to the team; both satisfy the stated requirement (#14).

> [!question]- Team decision required — scope of Activity History beyond the event
> Brief §4 describes Activity History as covering "significant actions" without naming which object types. Whether every domain object (booking, equipment reservation, registration, support request, change request) carries its own activity trail, or all entries roll up to the owning event's record, is unspecified.

> [!question]- Team decision required — which actions count as "significant"
> Neither source enumerates the set of actions that must be captured in Activity History; only the general purpose (accountability and troubleshooting) is given (brief §4).

See [[domain-model]] for the full object catalogue.
