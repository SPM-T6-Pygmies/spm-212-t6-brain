---
title: Session
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, session, event]
---

# Session

A first-class unit of a multi-session [[event]] — not merely a date range on the event, but a record that can carry its **own venue, own equipment requirements, own timing and own attendance limit** distinct from its parent event and from sibling sessions (#74).

## Attributes

Neither source gives a session attribute list, an identity scheme, or a session-level status set. Only the fact that certain properties *vary per session* is confirmed. Treat the rows below as confirmed properties, not a finalised schema.

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Venue (own) | May differ from the venue of other sessions in the same event | ✅ Specified | #74 |
| Equipment requirements (own) | May differ from the equipment requirements of other sessions | ✅ Specified | #74 |
| Timing (own) | May differ from the timing of other sessions | ✅ Specified | #74 |
| Attendance limit (own) | May differ from the attendance limit of other sessions | ✅ Specified | #74 |
| Identity / name / sequence / description | No naming, ordering, or identity scheme is specified anywhere | 🔶 Team decision | #74 |

## States

Not specified. Neither source names a session-level status; #74 confirms only that timing, venue, equipment and attendance vary per session, not that a session carries an independent lifecycle. 🔶 Team decision — whether a session needs its own status at all, or whether readiness is computed directly from its venue/equipment/registration arrangements (#74).

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[event]] | many : 1 | A session belongs to exactly one event; an event may have one or many sessions | #74 |
| [[booking]] | 1 : 1 (own) | Each session books its own venue, independent of sibling sessions | #74 |
| [[equipment-reservation]] | 1 : many (own) | Each session reserves equipment against its own requirements | #74 |

## Binding rules

- A session's venue, equipment requirements, timing, and attendance limit are independent of its sibling sessions and of the parent event (#74).

## Open modelling questions

> [!question]- Team decision required — session attribute set, identity, and lifecycle
> No attribute list, no identity/naming scheme, and no status set are specified for sessions in either source. The only confirmed facts are that a session may carry its own venue, equipment requirements, timing, and attendance limit, distinct from other sessions of the same event. Everything else — how a session is named, ordered, uniquely identified, or whether it has its own lifecycle — is left to the team, with #74 as the sole source (#74).

See [[domain-model]] for the full object catalogue.
