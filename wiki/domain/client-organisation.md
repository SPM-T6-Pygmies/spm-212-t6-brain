---
title: Client Organisation
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
  - "IS212 2026 Discussions Oct 1.md"
tags: [domain, client-organisation]
---

# Client Organisation

The external organisation on whose behalf one or more [[user-account]] (Event Organiser) records act; ConnectSphere maintains information about each client organisation and its event history. Process detail lives in [[access-control]]; this page is the structural reference.

**Release 1 scope** ([[release-1-scope]]): this object is `🧩 Adjacent`, pulled in by core feature 1 (User authorisation and authentication) — feature 1 scopes access by "relationship to an event," so a Client Organisation record must exist as data for organiser-to-organisation scoping ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)), though CRUD screens for managing it directly are not required in Release 1.

> [!info] Two of the only three customer-stated cardinalities in this domain layer belong to this object
> Every other cardinality figure elsewhere in `wiki/domain/` (event : booking, equipment-item : equipment-reservation, and so on) is this wiki's structural inference from the brief's prose. Exactly three cardinalities are stated by the customer in so many words anywhere in the source material, and two of the three belong to this object: **one client organisation has many Event Organisers**, and **one client organisation has many events**. "Multiple Event Organisers may belong to the same organisation, and an organisation may have several past and upcoming events" ([[event-planning-venue-booking-system|brief §4]]), reaffirmed at [#65](https://github.com/SinYang13/IS212-2026/discussions/65) ("Multiple Event Organisers may belong to the same client organisation") and [#81](https://github.com/SinYang13/IS212-2026/discussions/81) ("Other Event Organisers belonging to the same client organisation may view those events"). The third stated cardinality — one event has many sessions — belongs to [[session]]'s domain instead: "Some events may contain several sessions within the same day or across multiple days" ([[event-planning-venue-booking-system|brief §4]]). See [[domain-model]] for all three, side by side.

## Attributes

Neither source gives a field list for the organisation record itself beyond membership and event history — [[event-planning-venue-booking-system|brief §4]] states only that "ConnectSphere can maintain information about external client organisations," without enumerating fields.

| Field | Meaning | Status | Ref |
|---|---|---|---|
| Name | Organisation name | ⚠️ Partial | [[event-planning-venue-booking-system\|brief §4]] |
| Event Organiser members | The set of Event Organiser accounts belonging to this organisation | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]]; [#65](https://github.com/SinYang13/IS212-2026/discussions/65) |
| Event history | Past and upcoming events associated with the organisation | ✅ Specified | [[event-planning-venue-booking-system\|brief §4]] |
| Primary contact | Whether a designated primary-contact role exists among the organisation's Event Organisers | 🔶 Team decision | [#65](https://github.com/SinYang13/IS212-2026/discussions/65) |
| Organisation administrator | Whether a client-organisation administrator role exists, distinct from ordinary Event Organisers, to gatekeep intra-organisation access | 🔶 Team decision | [#81](https://github.com/SinYang13/IS212-2026/discussions/81) |

## States

No lifecycle (e.g. active / inactive client) is specified for the Client Organisation record in either source.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[user-account]] (Event Organiser) | 1 : many | Multiple Event Organisers may belong to the same client organisation. That each Event Organiser belongs to *exactly one* is this wiki's reading — [#124](https://github.com/SinYang13/IS212-2026/discussions/124) asked and got a non-answer (see Binding rules) | [[event-planning-venue-booking-system\|brief §4]]; [#65](https://github.com/SinYang13/IS212-2026/discussions/65); [#124](https://github.com/SinYang13/IS212-2026/discussions/124) |
| [[event]] | 1 : many | An organisation may have several past and upcoming events | [[event-planning-venue-booking-system\|brief §4]] |
| [[event-request]] | 1 : many | Requests are authored by the organisation's Event Organiser accounts | [[event-planning-venue-booking-system\|brief §4]] |

## Binding rules

- Event Organisers belonging to the same client organisation may view, but not edit or update, events they are not responsible for; Event Organisers must not access events belonging to an unrelated client organisation ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)).
- Access to a same-organisation event is automatic and organisation-wide ([#81](https://github.com/SinYang13/IS212-2026/discussions/81)). ⚠️ Partial: [#81](https://github.com/SinYang13/IS212-2026/discussions/81) describes the resulting access but is silent on the granting mechanism — it does not state that per-event granting by another Organiser is unavailable, only that no such step is described; see the "silence is not denial" warning below, which applies the same caveat to the administrator question.
- ⚠️ Partial: whether every Event Organiser account must be tied to a client organisation was put to the customer, who replied only "Please refer to project brief" ([#124](https://github.com/SinYang13/IS212-2026/discussions/124), answered 2026-09-28). The brief says Event Organisers represent a client ([[connectsphere-event-services|brief §2]]) and lists a user's organisation as a generic profile field ([[event-planning-venue-booking-system|brief §4]]). **Nothing new follows:** the cardinality and rules on this page are unchanged. The answer is a non-answer, not a confirmation, so "exactly one organisation per Event Organiser" remains this wiki's reading rather than a customer statement.
- No existing client database needs to be imported; there is no data-migration or system-integration requirement for client records ([#12](https://github.com/SinYang13/IS212-2026/discussions/12)).
- No primary-contact role has been specified for a client organisation; the customer invites a proposal only if one is needed ([#65](https://github.com/SinYang13/IS212-2026/discussions/65)).

> [!warning] "No client-organisation administrator" is this wiki's inference, not a stated fact
> [#81](https://github.com/SinYang13/IS212-2026/discussions/81) was asked directly whether "a primary contact or client-organisation administrator control[s] this access." The answer describes the view/edit rule (same-organisation members get automatic view-only access; the responsible Organiser alone can edit) without invoking or ruling out an administrator. Read narrowly, [#81](https://github.com/SinYang13/IS212-2026/discussions/81) supports only that access is **automatic** rather than administrator-granted for *this specific rule* — it does not confirm that no client-organisation administrator role exists anywhere in the system. Combined with [#65](https://github.com/SinYang13/IS212-2026/discussions/65)'s explicit "no primary-contact role has been specified," the safest reading is: no primary-contact role is confirmed, and no administrator role is described, but neither source states outright that an administrator role is excluded.

## Open modelling questions

> [!question]- Team decision required — primary contact or organisation administrator
> Whether to introduce a primary-contact or client-organisation-administrator role. [#65](https://github.com/SinYang13/IS212-2026/discussions/65) confirms none has been specified; [#81](https://github.com/SinYang13/IS212-2026/discussions/81) describes access as automatic per organisation membership without invoking such a role. See the divergence note above.

> [!question]- Team decision required — client organisation attribute set
> What fields ConnectSphere maintains for a client organisation beyond its Event Organiser membership and event history. Brief §4 states only that "ConnectSphere can maintain information about external client organisations," without enumerating them.

See [[domain-model]] for the full object catalogue.
