---
title: "Source: IS212 Discussions — 2026-09-16 snapshot"
type: source-summary
sources:
  - "IS212 2026 Discussions Sept 16.md"
tags: [source, clarifications, q-and-a, github-discussions, is212, authoritative]
---

# Source: IS212 Discussions — 2026-09-16 snapshot

Current authoritative extract of the [[is212-g9-section]] GitHub Discussions board — the only sanctioned channel for clarifying [[customer-brief-no-1]]. Supersedes [[is212-discussions-2026-09-08]].

## Metadata

| Field | Value |
|---|---|
| Repository | `SinYang13/IS212-2026` |
| Extracted | 2026-09-16 06:58 UTC |
| Volume | 115 discussions, 11 categories |
| Answer state | 114 answered · **0 unanswered** · 26 labelled `important` |
| Relationship | Strict superset of [[is212-discussions-2026-09-08]] (and so of [[is212-discussions-2026-08-31]]) |

## The delta, verified

Both snapshots were split per-discussion and compared block-by-block. The ingestion surface is **2 items**, not 115.

| Change | Count | Which |
|---|---|---|
| New discussions | 2 | [#122](https://github.com/SinYang13/IS212-2026/discussions/122), [#123](https://github.com/SinYang13/IS212-2026/discussions/123) — both in **Venues**, both answered by `josephsungls` (2026-09-12 and 2026-09-14) |
| Existing discussions with changed text | 0 | — |
| Removed | 0 | — |
| Header-only differences | 3 | Board totals (113 → 115; 112 → 114 answered) and the Venues category count (17 → 19). [#49](https://github.com/SinYang13/IS212-2026/discussions/49) shows as "changed" in a naive diff only because the Venues "Answered (17 → 19)" section header trails its block — its own text is identical |

`important` stays at 26: neither new answer is labelled. As in [[is212-discussions-2026-09-08]], the label is not a reliable guide to impact — [#123](https://github.com/SinYang13/IS212-2026/discussions/123) reshapes the setup/turnaround model.

## What changed the wiki's mind

### Setup and turnaround are one adjacent slot each — [#123](https://github.com/SinYang13/IS212-2026/discussions/123)

Asked (by a team member, 2026-09-12) how the AM / PM / Night slots interact with the setup/turnaround buffer policy. The customer's answer:

- Setup and turnaround should **each require at most one full slot**. Multi-slot or multi-day buffers are **not required**.
- They use the **immediately adjacent** booking slot, **including across days**:

| Event slot | Setup occupies | Turnaround occupies |
|---|---|---|
| AM | **Night** of the previous day | PM of the same day |
| PM | AM of the same day | **Night** of the same day |
| Night | PM of the same day | **AM** of the following day |

- Users do **not** select an unrelated earlier or later slot for setup or turnaround — the slot is determined by the event's booking.
- The team "may propose the exact UI and implementation mechanism, as long as venue availability and booking-conflict checking reflect these rules."

**Effect on the wiki.** [#108](https://github.com/SinYang13/IS212-2026/discussions/108) fixed the *granularity* (a slot is the smallest unit) and left [#43](https://github.com/SinYang13/IS212-2026/discussions/43)'s per-venue value in place; [[open-questions]] carried the resulting question — does a minutes-valued per-venue turnaround still mean anything once slots are atomic? — as open. [#123](https://github.com/SinYang13/IS212-2026/discussions/123) answers most of it: the buffer is **at most one adjacent slot per side**, so a duration in minutes has nothing to say. It also, for the first time, makes the buffer a **conflict-checking obligation on neighbouring slots** rather than a venue attribute: an event's effective occupancy is its own slot(s) plus up to one slot either side, and the hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)) must reflect it.

It also supplies **ordering evidence** for the garbled [#50](https://github.com/SinYang13/IS212-2026/discussions/50) slot labels: the answer treats AM → PM → Night as the chronological sequence, and Night → next-day AM as adjacent. That corroborates the wiki's inferred label mapping on *order* — not on hours, which remain unlabelled in the source. See [[venue]].

What it leaves open — recorded, not resolved — is on [[venue-booking]], [[booking]] and [[open-questions]]: whether every booking consumes buffers or only where a venue/event requires them; whether two neighbouring events may **share** one buffer slot; what happens when the buffer slot is Blocked or outside operating hours; and whether buffer slots count as utilisation in reports.

### Retiring a venue does not cancel its bookings — [#122](https://github.com/SinYang13/IS212-2026/discussions/122)

Asked whether future bookings on a retired venue should be cancelled, or rescheduled to an alternative. The customer's answer:

- Future bookings are **not** cancelled and **not** automatically rescheduled.
- The affected bookings are **identified for review** and the relevant **Event Coordinators notified**.
- The Coordinator, **together with Venue Staff**, determines an alternative venue or arrangement. This coordination "may also take place outside the system".
- Once an alternative is agreed, the **Coordinator updates the venue arrangement in the system** and reconsiders suitability, availability and booking requirements where applicable.
- The team "may propose the exact workflow and status transitions."

**Effect on the wiki.** Two things.

1. *Venue retirement is a new concept.* [[venue]] modelled availability per **slot** and explicitly said it was not a lifecycle of the Venue record itself; nothing in [[customer-brief-no-1]] or [[week-4-customer-briefing]] mentions retiring a venue. The customer's answer presumes it exists without mandating a feature. The wiki now records it as a venue-level status — see [[venue]] and the scope note in [[release-1-scope]].
2. *It is the same pattern as [#120](https://github.com/SinYang13/IS212-2026/discussions/120) and [#54](https://github.com/SinYang13/IS212-2026/discussions/54), a third time.* Flag for review, notify the Coordinator, the Coordinator acts, no automatic status change, resulting transition left to the team. Three triggers now share one unresolved destination state on [[booking]]. See [[change-impact-management]].

## Everything else

No other discussion changed. Nothing in the delta contradicts an existing wiki page; [#123](https://github.com/SinYang13/IS212-2026/discussions/123) **narrows** the wording of [[venue]], [[venue-booking]], [[booking]] and [[venue-suitability]] on setup/turnaround (per-venue *duration* → adjacent-slot *rule*), which is a refinement of [#108](https://github.com/SinYang13/IS212-2026/discussions/108), not a reversal.

## Still open

The board reports **0 unanswered**. The requirement gaps opened by these two answers are listed under Venues in [[open-questions]].
