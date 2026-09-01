---
title: Log
type: log
sources: []
tags: [meta, log]
---

# Log

Chronological, append-only record of wiki activity. Newest entries at the bottom.

## 2026-09-02

### Wiki initialised

- Created structure: `wiki/sources/`, `wiki/concepts/`, `wiki/entities/`, `wiki/index.md`, `wiki/log.md`.
- Catalogued 3 un-ingested documents in `raw/`.

### Ingested all 3 raw sources

Read `Customer Brief no. 1.md`, `IS212 2026 Discussions Aug 26.md`, and `IS212 2026 Discussions Aug 31.md` in full. Established that the Aug 31 discussions export is a **strict superset** of the Aug 26 export — same board, later snapshot, +12 discussions and +15 answers — so Aug 31 is treated as authoritative and Aug 26 is retained only as a point-in-time record.

**Created 21 pages:**

- 3 source summaries: [[customer-brief-no-1]], [[is212-discussions-2026-08-26]], [[is212-discussions-2026-08-31]]
- 15 concepts: [[event-request-workflow]], [[event-lifecycle]], [[event-readiness]], [[change-impact-management]], [[venue-booking]], [[venue-suitability]], [[equipment-management]], [[technical-support]], [[user-roles]], [[access-control]], [[attendee-registration]], [[notifications]], [[non-functional-requirements]], [[reporting]], [[open-questions]]
- 3 entities: [[connectsphere-event-services]], [[event-planning-venue-booking-system]], [[is212-g9-section]]

**Notable findings recorded during ingestion:**

| Finding | Where |
|---|---|
| Approval vs confirmation resolved (#80) — approval commits nothing; confirmation must be blocked while an essential arrangement is incomplete | [[event-lifecycle]], [[event-readiness]] |
| Equipment contradiction raised by T6 (#2 vs #5) **resolved** — transfer time governs pre-event availability, Return Day + 1 governs post-event | [[equipment-management]] |
| **Event Operations Manager** appears in the discussions but not in the brief's five-role table — recorded as a gap, not a contradiction | [[user-roles]] |
| Discussion #50 text is garbled — three venue time ranges listed, only "AM" labelled, with unexplained 12–1pm and 6–7pm gaps | [[venue-booking]] |
| Double-booking is a hard block (#35), but the capacity rule still carries the brief's hedge word "normally" and was never tightened | [[venue-suitability]] |
| Pooled-vs-serialised equipment modelling (#13) is load-bearing — per-item location (#2) and status (#23) imply per-unit state | [[equipment-management]] |
| No manager role is named for technical staff rostering, unlike coordinator assignment | [[technical-support]] |
| PDPA question in #32 was asked but only half-answered | [[reporting]] |
| Both items flagged "unanswered" in the Aug 31 export (#2, #55) were in fact resolved in follow-up replies the status flag missed | [[is212-discussions-2026-08-31]] |
| Release 1 scope is **not yet fixed** — deferred in #79 and to Week 4 by the brief | [[open-questions]] |

**Integrity checks run:** 0 broken wikilinks, 0 unexpected orphans, frontmatter valid on all 21 pages.

### Applied Obsidian callout conventions

Converted the wiki's ad-hoc bold-label blockquotes into native Obsidian callouts. Previously, verbatim customer quotations and team-decision annotations both rendered as identical grey blockquotes; they are now visually distinct.

- Defined a five-type vocabulary in `CLAUDE.md` (`question`, `warning`, `quote`, `info`, `success`), plus `important` reserved for `index.md`.
- Converted 57 blockquotes across 17 pages.
- All 41 **Team decision required** callouts were made **foldable** and given a specific topic in the title, so a reader sees a scannable list of decision headlines rather than 41 identical banners.
- Added one `[!important]` callout to [[index]] listing the customer's committed, non-negotiable rules — the small set of constraints that are stated rather than proposable.
- Net cost: +499 words (~2%), so the pages remain compact for retrieval.

### Noted: new un-ingested source

`raw/llm-wiki.md` appeared during this session. It describes the **LLM-wiki pattern itself** (architecture, ingest/query/lint operations, indexing and logging) rather than any ConnectSphere or IS212 subject matter. It is therefore **methodology, not domain content**, and was deliberately not ingested into `wiki/concepts/` — doing so would mix guidance about how this wiki works with knowledge about what it contains. Its natural home is `CLAUDE.md`. Awaiting a decision.
