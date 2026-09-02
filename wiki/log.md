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

### Added a domain-object layer

Created 15 pages in a new `wiki/domain/` layer — a hub plus one page per domain object — and rewired all 15 `wiki/concepts/` pages to link into it.

**Created 15 pages:**

- Hub: [[domain-model]] — object catalogue, ER diagram, modelling-decision register, deferred-modelling register
- 14 domain objects: [[event]], [[event-request]], [[session]], [[change-request]], [[venue]], [[booking]], [[equipment-item]], [[equipment-reservation]], [[support-request]], [[user-account]], [[client-organisation]], [[registration]], [[notification]], [[audit-record]]

**Four decisions:**

| Decision | Rationale |
|---|---|
| "Entity" read as domain object | The task named an "entity layer"; taken to mean the nouns the system stores records about (Event, Venue, Booking, …), not organisations or people — those already have a home in `wiki/entities/` |
| New `wiki/domain/` folder, not `wiki/entities/` | Keeps `wiki/entities/`'s existing meaning (organisations, products, the course itself) intact rather than overloading it with a second sense of "entity" |
| ~14 aggregate roots, not ~25 thin pages | Satellite concepts (e.g. Tentative Hold, Waiting List Entry, Activity History) are folded in as sections on their owning object's page rather than split into their own pages |
| Domain vs. concepts split by question type | Domain pages answer *what is this object, structurally* (attributes, states, relationships, binding rules); concept pages answer *what happens, procedurally* (workflow, sequencing, actors). Each fact gets one home; each layer links to the other for the half it doesn't own |

**Naming constraint:** Obsidian resolves `[[wikilinks]]` by basename, so the bookings **record** is [[booking]] while the existing **process** page keeps its established name, [[venue-booking]] — the two are deliberately different filenames, not a naming collision.

**Notable findings:**

| Finding | Where |
|---|---|
| Neither source contains an ER diagram, glossary, or data dictionary — [[domain-model]] is the first place the object model is assembled anywhere in the project | [[domain-model]] |
| Only **three** cardinalities are stated by the customer, in so many words, anywhere in the source material — two in brief §4 *Client Management* (organisation → Event Organisers; organisation → events), one in brief §4 *Multi-Session Events* (event → sessions). Every other cardinality in the domain layer is this wiki's inference and is labelled as such | [[domain-model]], [[client-organisation]], [[session]] |
| The deferred-modelling register collects **44** distinct open structural decisions across the fourteen object pages | [[domain-model]] |
| The equipment #2-vs-#5 apparent contradiction (flagged by T6, 2026-08-28) resolves as **two rules coexisting at different lifecycle points** — transfer time governs pre-event availability, Return Day + 1 governs post-event availability — not as alternatives | [[equipment-item]] |
| Pooled-vs-serialised equipment (#13) is the single highest-value deferral in the layer; the one binding constraint is that the system must always know available quantity | [[equipment-item]] |
| The invented **PM** and **Night** venue-slot hour labels (e.g. "PM (1pm–6pm)") were found hard-coded in four concept pages — [[venue-booking]], [[event-request-workflow]], [[reporting]], [[change-impact-management]] — and removed. #50's source text labels only "AM"; [[venue]] now carries the ambiguity in full and leaves the remaining two slots unlabelled | [[venue]] |

**Citation discipline:** a recurring correction throughout this work was distinguishing what a customer **answer** actually says from what the **question** presupposed. For example, #78's question was phrased "...where an Event Organiser or Coordinator must create the event request?", presupposing both roles may do so — but the answer addressed only minimum lead time and never confirmed or denied that presupposition. Similarly, #59 asked about a "deactivated" account, but the answer addressed only ownership transfer and never confirmed account deactivation as a modelled state. Several claims across the domain layer were downgraded to `⚠️ Partial` or hedged with an explicit "the question presupposed X, the answer only confirmed Y" note as a result. Recorded here as a lesson for future ingests: read what the answer says on its own terms, not through the frame of the question that prompted it.

**Integrity checks run:** all 15 domain pages linked from [[index]]; 0 unexpected broken wikilinks (one known false positive — the literal `` `[[wikilinks]]` `` code span in [[booking]]'s naming note); exactly one `[!important]` callout, still in [[index]].
