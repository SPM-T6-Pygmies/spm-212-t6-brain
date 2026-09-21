---
title: "Linear Ticket Audit — Sept 16 Batch"
type: review
sources:
  - "IS212 2026 Discussions Sept 16.md"
  - "IS212 2026 Discussions Sept 8.md"
tags: [review, linear, backlog, tickets, venues, release-1]
---

# Linear Ticket Audit — Sept 16 Batch

Follow-up to [[linear-ticket-audit-sept-8-batch]]. The 2026-09-16 snapshot adds only two discussions — [#122](https://github.com/SinYang13/IS212-2026/discussions/122) (retiring a venue) and [#123](https://github.com/SinYang13/IS212-2026/discussions/123) (setup and turnaround slots), both in Venues — so the audit surface is one project. Ingestion is recorded on [[is212-discussions-2026-09-16]].

**Method.** The full `SPM - IS212` board (SPM-2 – SPM-155) was searched for turnaround, setup, buffer, retire and conflict; every venue-booking ticket was then read. Eleven needed a comment. **Comments only — no ticket body was edited, no status changed and no ticket created.** Nothing was contradicted outright (contrast the Sept 8 audit's nine); the exposure is **stale wording and a missing buffer in conflict detection** on cards that are, in three cases, in the current cycle.

## Comments posted

| Ticket | Status | Why | Ref |
|---|---|---|---|
| **SPM-42** Maintain the venue catalogue | Todo · in cycle | Subtasks SPM-146/147 may expose the now-dead `setup_time_minutes` / `turnaround_time_minutes` columns; no way to retire a venue exists. Flag, not a new requirement | [#123](https://github.com/SinYang13/IS212-2026/discussions/123), [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| **SPM-46** Submit a venue booking request | Todo · in cycle | The overlap block must run over the *padded* slot set, not only the requested slots | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-43** View venue availability calendar | Todo · in cycle | A buffer slot is unbookable but is neither available, confirmed nor blocked — rendering unspecified | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-44** Search and filter venues | Todo · in cycle | "Open slot" may be a neighbour's buffer; retired venues must not be offered if a retire mechanism ships | [#123](https://github.com/SinYang13/IS212-2026/discussions/123), [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| **SPM-23** Configure venue turnaround and setup buffer | Backlog | **Premise superseded** — its ACs describe per-venue durations quantised to slots; the customer fixed the buffer at one adjacent slot. Suggested holding in Backlog | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-19** Automatic venue availability update | Backlog | AC3's "setup time … turnaround time" now has a precise rule; hosts the shared open-question list | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-54** Reschedule an event | Backlog | Its "Resolved" note is incomplete — a moved event moves its buffers, and a buffer crosses midnight | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-20** Cancel venue booking | Backlog | Its open question (does Venue Staff need a cancel trigger for a permanently unusable venue?) is **answered: no**; cancellation must also free buffers | [#122](https://github.com/SinYang13/IS212-2026/discussions/122), [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-21** Indicate venue unavailable | Backlog | A Blocked slot can make an adjacent booking's buffer impossible; refuse-vs-waive unspecified | [#123](https://github.com/SinYang13/IS212-2026/discussions/123) |
| **SPM-105** Flag affected bookings for review | Backlog | Retirement is a **second trigger** of the same behaviour; the open target-status question now has three triggers | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |
| **SPM-71** Notify the coordinator of an availability change | Backlog | Retirement is a further cause of trigger 14 | [#122](https://github.com/SinYang13/IS212-2026/discussions/122) |

## The finding worth keeping

**A per-venue duration became an adjacency rule, and the ticket that carried the duration is the least of the exposure.** [#108](https://github.com/SinYang13/IS212-2026/discussions/108) fixed the granularity and the board's fix (SPM-23, "quantised up to whole slots") followed it faithfully. [#123](https://github.com/SinYang13/IS212-2026/discussions/123) then removed the duration altogether. The cards that matter are not the configuration card but the *consumers* of occupancy — SPM-19, SPM-46, SPM-54, SPM-20 — none of which said anything wrong, and none of which would have noticed: each says "overlap" and each is correct only if "overlap" is defined over the padded set. That definition lives on no ticket. It is now stated in the SPM-19 comment as the place the shared open questions are tracked.

**A consequence the customer may not have intended.** If both buffers always apply and neighbouring events cannot share one, then an event in one slot forbids the next slot at that venue: no back-to-back events, at most every other slot usable. The customer said each buffer "occupies" a slot, and the wiki reads that as subject to the hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)), but it is an **inference**, and it is the single answer most worth confirming — it changes venue throughput and the venue usage report. The wiki records it as a team decision on [[venue-booking]] rather than a settled rule.

## Not done, and why

| Not done | Why |
|---|---|
| Edit SPM-23's body | Comments only were requested. Its ACs are stale; the recommended action is a rewrite or a hold once the buffer-requirement question is decided |
| Create a "retire a venue" ticket | Nobody has asked for the feature. The behaviour the customer specified is already carded as SPM-105 / SPM-71 |
| Migrate the deployed schema | Three gaps (E1–E3) are recorded on [[schema]], each dependent on a team decision. The DDL is unchanged |
| Ticket the 29 unticketed cards in [[backlog-candidate-user-stories]] | Not authorised; see below |

## The unticketed backlog

[[backlog-candidate-user-stories]] draws nine epics and 31 items. Checked against the whole board on 2026-09-21, only Reg6 (SPM-76) and SPK-1 (SPM-77) exist as tickets. The other 29 were **not created**: each fails the standard's readiness gate by design (every row carries a blocking unknown), so bulk-creating them would put 29 unready cards on a shared board. The document has been updated for both new answers (RP2, EC4, VH1 revised; one candidate card considered and declined) so it is current when someone does ingest it.
