---
title: Audit Record
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, audit, history]
---

# Audit Record

The record captured by the system in support of auditability: **Activity History** (who did what, and when — [[event-planning-venue-booking-system|brief §4]]) and **Change History** (how a field's value evolved over time — [[event-planning-venue-booking-system|brief §4]]), together satisfying the requirement that ConnectSphere can determine "what was changed, who changed it, and when the change was made" ([[non-functional-requirements|brief §8f]]). Process detail lives in [[reporting]] and [[non-functional-requirements]]; this page is the structural reference.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Actor | The [[user-account]] who performed the action or made the change | ✅ Specified | [[non-functional-requirements\|brief §8f]]; [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |
| Timestamp | When the action or change occurred | ✅ Specified | [[non-functional-requirements\|brief §8f]]; [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |
| Related object | The [[event]] (or other record) the entry concerns | ⚠️ Partial | [[non-functional-requirements\|brief §8f]] (implied) |
| Action | For an Activity History entry: which significant action was performed | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Field changed | For a Change History entry: which field's value changed | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; §8f |
| Old value / new value | For a Change History entry: the value before and after the change | ✅ Specified | [[non-functional-requirements\|brief §8f]] |
| Storage mechanism | Append-only store vs. an ordinary editable table | 🔶 Team decision | [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |

Neither source names "related object" as a field in its own right; it is this wiki's structural necessity, not a sourced attribute — without it, an entry's "what changed" ([[non-functional-requirements|brief §8f]]) has no referent to attach to.

## Activity History

Significant actions are recorded, including who performed the action and when, to support accountability and troubleshooting ([[event-planning-venue-booking-system|brief §4]]). Brief §4's wording is not restricted to a single object type — "significant actions," unqualified — so this wiki reads the *coverage* of Activity History as extending wherever a significant action occurs (event review, coordinator assignment, venue booking decisions, equipment reservation, registration, etc.), not solely actions on the [[event]] record. That is a claim about scope of applicability only, not about how the data model attaches these entries — whether each object type carries its own trail or everything rolls up to the owning event's record is a separate, unresolved question (see Open modelling questions). No source enumerates which specific actions count as "significant."

## Change History

Users with appropriate access can review how important **event** information has changed over time ([[event-planning-venue-booking-system|brief §4]]). Unlike Activity History's unqualified "significant actions," [[event-planning-venue-booking-system|brief §4]]'s Change History row names *event information* specifically — this wiki does not extend Change History to other object types without a source basis. Its two binding facts:

- It captures **what changed, who changed it, and when** — the same what/who/when triad as the general auditability requirement ([[non-functional-requirements|brief §8f]]; [#14](https://github.com/SinYang13/IS212-2026/discussions/14)).
- It is **access-restricted**: "Users with appropriate access can review how important event information has changed over time" ([[event-planning-venue-booking-system|brief §4]]) — not available to every user who can view the event.

> [!warning] Activity History's access model is not separately stated
> Brief §4 states an explicit access restriction ("appropriate access") only for Change History. No equivalent sentence restricts who may view Activity History. Treat any assumption that Activity History follows the same restriction as an inference governed by the general [[access-control]] model, not a confirmed rule in its own right.

Documents are not versioned — "as they are not legal documents, we do not need to keep any versions" ([#21](https://github.com/SinYang13/IS212-2026/discussions/21)). [#21](https://github.com/SinYang13/IS212-2026/discussions/21) establishes only that no version history is kept; it says nothing about Change History, diffing, or an add/delete model specifically. Since no source addresses how (or whether) document attachment/removal is captured in either history mechanism, this wiki infers only that a document's *content* cannot be diffed version-to-version, because no versions exist to diff — treat this as an inference from [#21](https://github.com/SinYang13/IS212-2026/discussions/21), not a separate sourced fact, and see Open modelling questions.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | Activity-history and change-history entries recorded against the event | [[non-functional-requirements\|brief §8f]] |
| [[event-request]] | many : 1 | Decision record (approve / reject / return) and pre-approval activity history | [[event-request-workflow\|brief §5]]; §8f |
| [[user-account]] | many : 1 | Every entry has an actor | [[non-functional-requirements\|brief §8f]]; [#14](https://github.com/SinYang13/IS212-2026/discussions/14) |

## Binding rules

- Every entry must capture **what changed, who changed it, and when** — the auditability requirement is stated at this level of generality and no further ([[non-functional-requirements|brief §8f]]; [#14](https://github.com/SinYang13/IS212-2026/discussions/14)).
- Change History is access-restricted to users with appropriate rights; it is not visible to every user who can view the underlying event ([[event-planning-venue-booking-system|brief §4]]).
- Completed and cancelled events, and by extension their history, remain available as historical records — history is not purged when an event closes ([#20](https://github.com/SinYang13/IS212-2026/discussions/20)).
- 🔶 Tamper-proofing is a technical mechanism left to the team: "The requirement is that important actions and changes are recorded so that ConnectSphere can determine what changed, who changed it, and when. The technical mechanism used to achieve this is for you to propose." An append-only store and a regular editable table are both acceptable so long as the what/who/when triad is recoverable ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)).

> [!info] No retention policy — archival and housekeeping are out of scope
> "No retention policy for this system as yet. Completed and cancelled events may remain available as historical records; archival and housekeeping are outside the current functional scope." ([#20](https://github.com/SinYang13/IS212-2026/discussions/20)). This bounds the work: there is no requirement to purge, archive, or age out audit records, and building an archival/housekeeping mechanism is explicitly not required for the current scope.

## Open modelling questions

> [!question]- Team decision required — tamper-proofing mechanism
> Whether the audit store is append-only (no edits or deletes) or an ordinary database table is left entirely to the team; both satisfy the stated requirement ([#14](https://github.com/SinYang13/IS212-2026/discussions/14)).

> [!question]- Team decision required — scope of Activity History beyond the event
> [[event-planning-venue-booking-system|Brief §4]] describes Activity History as covering "significant actions" without naming which object types. Whether every domain object (booking, equipment reservation, registration, support request, change request) carries its own activity trail, or all entries roll up to the owning event's record, is unspecified.

> [!question]- Team decision required — which actions count as "significant"
> Neither source enumerates the set of actions that must be captured in Activity History; only the general purpose (accountability and troubleshooting) is given ([[event-planning-venue-booking-system|brief §4]]).

> [!question]- Team decision required — how document changes are captured in history
> [#21](https://github.com/SinYang13/IS212-2026/discussions/21) confirms only that documents are not versioned; neither source states whether attaching or removing a supporting document is itself logged as an Activity History entry, or how (if at all) Change History treats a document field. Left to the team.

See [[domain-model]] for the full object catalogue.
