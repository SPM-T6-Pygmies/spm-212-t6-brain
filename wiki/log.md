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
| Approval vs confirmation resolved ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)) — approval commits nothing; confirmation must be blocked while an essential arrangement is incomplete | [[event-lifecycle]], [[event-readiness]] |
| Equipment contradiction raised by T6 ([#2](https://github.com/SinYang13/IS212-2026/discussions/2) vs [#5](https://github.com/SinYang13/IS212-2026/discussions/5)) **resolved** — transfer time governs pre-event availability, Return Day + 1 governs post-event | [[equipment-management]] |
| **Event Operations Manager** appears in the discussions but not in the brief's five-role table — recorded as a gap, not a contradiction | [[user-roles]] |
| Discussion [#50](https://github.com/SinYang13/IS212-2026/discussions/50) text is garbled — three venue time ranges listed, only "AM" labelled, with unexplained 12–1pm and 6–7pm gaps | [[venue-booking]] |
| Double-booking is a hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)), but the capacity rule still carries the brief's hedge word "normally" and was never tightened | [[venue-suitability]] |
| Pooled-vs-serialised equipment modelling ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)) is load-bearing — per-item location ([#2](https://github.com/SinYang13/IS212-2026/discussions/2)) and status ([#23](https://github.com/SinYang13/IS212-2026/discussions/23)) imply per-unit state | [[equipment-management]] |
| No manager role is named for technical staff rostering, unlike coordinator assignment | [[technical-support]] |
| PDPA question in [#32](https://github.com/SinYang13/IS212-2026/discussions/32) was asked but only half-answered | [[reporting]] |
| Both items flagged "unanswered" in the Aug 31 export ([#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#55](https://github.com/SinYang13/IS212-2026/discussions/55)) were in fact resolved in follow-up replies the status flag missed | [[is212-discussions-2026-08-31]] |
| Release 1 scope is **not yet fixed** — deferred in [#79](https://github.com/SinYang13/IS212-2026/discussions/79) and to Week 4 by the brief | [[open-questions]] |

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
| Only **three** cardinalities are stated by the customer, in so many words, anywhere in the source material — two in [[event-planning-venue-booking-system\|brief §4]] *Client Management* (organisation → Event Organisers; organisation → events), one in [[event-planning-venue-booking-system\|brief §4]] *Multi-Session Events* (event → sessions). Every other cardinality in the domain layer is this wiki's inference and is labelled as such | [[domain-model]], [[client-organisation]], [[session]] |
| The deferred-modelling register collects **44** distinct open structural decisions across the fourteen object pages | [[domain-model]] |
| The equipment [#2](https://github.com/SinYang13/IS212-2026/discussions/2)-vs-[#5](https://github.com/SinYang13/IS212-2026/discussions/5) apparent contradiction (flagged by T6, 2026-08-28) resolves as **two rules coexisting at different lifecycle points** — transfer time governs pre-event availability, Return Day + 1 governs post-event availability — not as alternatives | [[equipment-item]] |
| Pooled-vs-serialised equipment ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)) is the single highest-value deferral in the layer; the one binding constraint is that the system must always know available quantity | [[equipment-item]] |
| The invented **PM** and **Night** venue-slot hour labels (e.g. "PM (1pm–6pm)") were found hard-coded in four concept pages — [[venue-booking]], [[event-request-workflow]], [[reporting]], [[change-impact-management]] — and removed. [#50](https://github.com/SinYang13/IS212-2026/discussions/50)'s source text labels only "AM"; [[venue]] now carries the ambiguity in full and leaves the remaining two slots unlabelled | [[venue]] |

**Citation discipline:** a recurring correction throughout this work was distinguishing what a customer **answer** actually says from what the **question** presupposed. For example, [#78](https://github.com/SinYang13/IS212-2026/discussions/78)'s question was phrased "...where an Event Organiser or Coordinator must create the event request?", presupposing both roles may do so — but the answer addressed only minimum lead time and never confirmed or denied that presupposition. Similarly, [#59](https://github.com/SinYang13/IS212-2026/discussions/59) asked about a "deactivated" account, but the answer addressed only ownership transfer and never confirmed account deactivation as a modelled state. Several claims across the domain layer were downgraded to `⚠️ Partial` or hedged with an explicit "the question presupposed X, the answer only confirmed Y" note as a result. Recorded here as a lesson for future ingests: read what the answer says on its own terms, not through the frame of the question that prompted it.

**Integrity checks run:** all 15 domain pages linked from [[index]]; 0 unexpected broken wikilinks (one known false positive — the literal `` `[[wikilinks]]` `` code span in [[booking]]'s naming note); exactly one `[!important]` callout, still in [[index]].

### Linked bare "brief §N" citations

The `Ref` columns of domain-object pages (and one prose mention in this log) cited the customer brief only as inert text — `brief §4`, `brief §6`, etc. — with no way to click through to where that content actually lives in the wiki. `raw/Customer Brief no. 1.md` uses bold text for its section headers, not Markdown headings, so it can't be deep-linked without editing the immutable source; instead, each citation was turned into a piped wikilink to the wiki page that `[[customer-brief-no-1]]`'s own Content map already assigns to that section, keeping "brief §N" as the visible text:

| Ref | Links to |
|---|---|
| §2 | [[connectsphere-event-services]] |
| §3 | [[user-roles]] |
| §4 | [[event-planning-venue-booking-system]] |
| §5 | [[event-request-workflow]] |
| §6 | [[notifications]] |
| §8f | [[non-functional-requirements]] |

256 occurrences across 15 files (14 domain pages + this log). Inside Markdown tables the link's pipe was escaped (`\|`) to avoid splitting table columns; verified column counts were unchanged in every affected table afterward. Other citation spellings in the wiki (`brief s4`, `(Source: customer-brief-no-1, s4)`, used mainly in `wiki/concepts/`) were left as plain text — out of scope for this pass.

### Linked bare "#N" discussion citations

Same problem, different source: `#N` citations (e.g. `#80`, `#35`) refer to individual GitHub Discussion threads on the class board, not to anything with a stable per-number anchor inside the wiki — the two discussion source-summary pages ([[is212-discussions-2026-08-26]], [[is212-discussions-2026-08-31]]) only have headings for a handful of highlighted numbers, nowhere near all ~87. Unlike the brief's §-sections, there was no existing content-map to land on, so each number was linked straight to its exact GitHub thread instead: `#80` → `[#80](https://github.com/SinYang13/IS212-2026/discussions/80)`.

958 occurrences linked across 35 files. Two exclusions:
- Text inside ` ```mermaid ` / ` ```stateDiagram ` code fences (equipment-item.md, equipment-reservation.md, registration.md, booking.md, domain-model.md) was left as plain text — diagram syntax, not prose.
- The two discussion source files themselves were skipped, since their own `#N` mentions are in their own section headings (self-links serve no purpose there).

While scoping this, found that the *previous* brief-§N pass had linked a few citations that sit inside those same code fences (`domain-model.md`'s ER diagram, `equipment-reservation.md` and `registration.md`'s state diagrams) — `[[...|brief §4]]` inside a Mermaid label. Reverted those 8 back to plain `brief §4` text as part of this pass, same reasoning: code fences aren't prose.

### Scrutinised `raw/User Stories Assignment.md` against the standard

Reviewed all 15 story cards in `raw/User Stories Assignment.md` against `raw/IS212 User Story Standard — v1.0.md`'s scrutiny rubric (§13), cross-checking every claimed rule against the sourced domain/concept pages rather than the standard alone. Recorded as a new page, [[user-stories-assignment-scrutiny]], added to [[index]] under a new **Reviews** section since it doesn't fit the four-type wiki taxonomy.

**Headline findings:**

| Finding | Severity |
|---|---|
| The entire `## Registration` section is a byte-for-byte duplicate of the `## Venue` section — no Attendee-registration story exists anywhere, despite registration being one of the most fully specified areas of the real requirements (waiting list, 3-day window, capacity, check-in) | Structural — highest priority |
| "Configure Venue Turnaround and Setup Buffer" is a copy-paste of a different card ("Review and Decide on Venue Booking Requests"); the actual feature is unwritten | Structural |
| "Cancel Venue Booking" gives cancellation authority to Venue Staff, contradicting the customer's hard rule that only the assigned Event Coordinator may cancel, at any stage (`#68`) | Contradicts a committed rule |
| "Assignment of event requests" invents an "eligible coordinators" filter and an assignment "failure" state, both directly contradicted by `#93`/`#95` (no selection criteria are encoded; assignment has no accept/decline step) | Contradicts sourced answers |
| No card in the document carries a Priority, Estimate, or Epic, and none cite any source | Systemic — every card fails Gate A7 and Gate D1 |

Proposed a replacement 5-card Registration set and a rewritten Venue Turnaround card, both drafted to the standard's card anatomy and citing only customer-confirmed facts, with genuine gaps left as open questions rather than invented.

### Correction: Registration section was a save/paste error, now fixed

The `## Registration` duplication flagged above was the assignment author's own copy-paste mistake, not a permanent gap — it has since been replaced in `raw/User Stories Assignment.md` with three real cards (Register for an event, Enable registration for an event, Set registration capacity for an event). Updated [[user-stories-assignment-scrutiny]] §5 to review the real cards instead: two are `NEEDS WORK` (an unresolved mandatory-field assumption on Register; a per-card audit AC on Enable that duplicates the global auditability NFR), one (Set registration capacity) is the best-grounded card in the whole document, correctly but silently resolving two previously-open questions ([[venue-suitability]]'s over-capacity block-vs-warn question, and [[attendee-registration]]'s venue-vs-registration-capacity question). Waiting-list promotion, withdrawal, and check-in remain uncovered by any card — those three draft additions from the original pass were kept, renumbered Reg4–Reg6.

### Added a full proposed rewrite for every non-`READY` card

The scrutiny up to this point identified defects with a "Fix" column but only fully rewrote a handful of cards (Venue Turnaround, the Registration additions). Extended it so every `NEEDS WORK` or `REJECT` verdict now carries a complete replacement card — title, story, and ACs, citing only this wiki's sourced pages and flagging genuine gaps as open questions rather than resolving them silently. This covers all four Users cards, both Equipment cards under review, four of six Venue cards, all three original Registration cards, and all three Event cards (the last split into three independent replacement cards, since the original bundled "review" and "clarification" under one title but only delivered the first).

### Formalised an `outputs/` layer for review artefacts

[[user-stories-assignment-scrutiny]] itself flagged that it didn't fit the wiki's four-type taxonomy and proposed, but deliberately didn't assume, a permanent "reviews" layer as a separate `CLAUDE.md` change. That change is made now: `CLAUDE.md` gains a fifth architecture layer, `outputs/`, for LLM-generated one-off artefacts (reviews, scrutiny passes, ad-hoc analysis) that aren't standing domain knowledge; a fifth frontmatter `type` value, `review`, reserved for pages in that layer; a new **Review** workflow (produce in `outputs/`, frontmatter `type: review`, link from `wiki/index.md`'s **Reviews** section, log it, and fix any correction to standing knowledge directly in `wiki/` rather than leaving it stranded in the review); and the Lint workflow's orphan check now covers `outputs/` as well as `wiki/`.

The scrutiny page itself moved from `wiki/user-stories-assignment-scrutiny.md` to `outputs/user-stories-assignment-scrutiny.md` to match, and its now-resolved "Note on page type" callout was folded into a plain sentence in the intro. `wiki/index.md`'s **Reviews** section needed no change — it already linked the page by wikilink rather than by path, so it resolves correctly regardless of which folder the file lives in.

### Added a Citation linking rule, then applied it retroactively to both `outputs/` pages

`CLAUDE.md` previously only described the `#N`-and-`brief-§N` linking convention inside this log's own "Linked bare ..." entries — nothing required it going forward, so freshly written pages kept landing with bare, unlinked citations (flagged directly against [[user-stories-assignment-scrutiny]] line 259's bare `brief §4`). Added a **Citation linking** section to `CLAUDE.md`: `#N` → `[#N](https://github.com/SinYang13/IS212-2026/discussions/N)`, `brief §N` → a piped wikilink to whatever `[[customer-brief-no-1]]`'s Content map assigns that section (pipe escaped as `\|` inside table cells), applies in prose, table cells, and callouts alike, with only diagram code fences and the discussion source pages' own self-references exempt — written at creation time, not fixed up in a later pass. Also added a matching Lint check (item 8).

Applied the rule retroactively to the two existing `outputs/` pages, which had zero linked citations between them:

| Page | `#N` linked | `brief §N` linked |
|---|---|---|
| [[user-stories-assignment-scrutiny]] | 95 | 11 |
| [[user-stories-proposed-rewrites]] | 54 | 10 |

`[[user-stories-proposed-rewrites]]` (a new page consolidating every proposed rewrite from the scrutiny into a ready-to-paste backlog) was also found un-linked from [[index]] while doing this pass — added to the **Reviews** section so it isn't an orphan.

## 2026-09-05

### Ingested `Week 4 - Customer Briefing.md`

Placed in `raw/` and read in full. It fixes Release 1 scope: 20 core features selected from [[customer-brief-no-1]]'s ~60 functionality areas, due Week 12, with an "adjacency" clause requiring "coherent end-to-end event workflows" across them.

**Created 2 pages:**

- Source summary: [[week-4-customer-briefing]]
- Concept: [[release-1-scope]] — the authoritative in/adjacent/out register (a third `🧩 Adjacent` marker alongside `🎯 Release 1` / `📋 Backlog`, since the adjacency clause rules out a plain binary split)

**Updated 2 pages:**

- [[event-planning-venue-booking-system]] — functionality-areas table gained a Scope column; the "Release 1 scope is not yet fixed" warning resolved to a `[!success]` closing [#79](https://github.com/SinYang13/IS212-2026/discussions/79) and [#11](https://github.com/SinYang13/IS212-2026/discussions/11); "Explicitly out of scope" section distinguished from `📋 Backlog` (denied vs. deferred)
- [[open-questions]] — gained a Scope column keyed to [[release-1-scope]], to show which open questions must be resolved before Week 12 vs. can wait

**Notable findings:**

| Finding | Where |
|---|---|
| Confirmation ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s blocking rule) is a committed hard rule with no feature of its own in the core 20 — inherited via feature 6's "status changes should be consistent with the actions performed" | [[event-readiness]], [[release-1-scope]] |
| Attendee registration (feature 18) ships with no stated ceiling and no way for an Attendee account to come into existence — external onboarding is out of scope and there is no admin role | [[attendee-registration]], [[access-control]], [[release-1-scope]] |
| Equipment is described entirely in type/quantity terms in the core 20; nothing requires per-unit identity, bearing on but not resolving the pooled-vs-serialised deferral ([#13](https://github.com/SinYang13/IS212-2026/discussions/13)) | [[equipment-item]], [[release-1-scope]] |
| The Event Operations Manager gap hardens: feature 5 assigns coordinators passively ("can be assigned"), naming no actor, while the five authorised roles (feature 1) don't include one | [[user-roles]], [[release-1-scope]] |
| Two brief wordings ("help prevent inappropriate double-booking", "not **normally** suitable") are looser than rules already settled elsewhere — the settled answers still govern | [[release-1-scope]] |

**Integrity checks run:** 0 unlinked `#N`/`brief §N` citations in the new/changed content; all `🧩 Adjacent` rows in [[release-1-scope]] cite the core feature that pulls them in; all wikilinks resolve.

### Closed the ingest gap: catalogued in `index.md`

The four pages above were created/updated in the previous session but never catalogued — [[index]] still counted 37 pages and listed 3 sources. Added [[week-4-customer-briefing]] to the Sources table, [[release-1-scope]] to Concepts → Cross-cutting, a "Week 12 release" pointer to Start here, a `raw/Week 4 - Customer Briefing.md` row to Ingestion status, and updated the page count to 39.

### Rolled Release 1 scope markers out to every concept and domain page

CLAUDE.md's Release scope schema requires every page describing functionality to carry exactly one of `🎯 Release 1` / `🧩 Adjacent` / `📋 Backlog` — but until now only [[open-questions]] and [[event-planning-venue-booking-system]] actually had one; the other ~28 pages in `wiki/concepts/` and `wiki/domain/` were silent. Mirrored [[release-1-scope]] (the source of truth) onto all of them, in three batches:

- **Event flow:** [[event-request-workflow]], [[event-lifecycle]], [[event-readiness]], [[change-impact-management]], [[event]], [[event-request]], [[session]], [[change-request]] — a short "Release 1 scope" paragraph after each page's intro.
- **Resources:** [[venue-booking]], [[venue-suitability]], [[equipment-management]], [[technical-support]], [[venue]], [[booking]], [[equipment-item]], [[equipment-reservation]], [[support-request]] — paragraphs on the concept pages; a `Scope` column added to the Attributes/States tables on the domain pages, since venue and booking attributes split across multiple markers.
- **People and cross-cutting:** [[user-roles]], [[access-control]], [[attendee-registration]], [[notifications]], [[non-functional-requirements]], [[reporting]], [[user-account]], [[client-organisation]], [[registration]], [[notification]], [[audit-record]], [[domain-model]] — paragraphs, plus a `Scope` column on [[user-account]]'s Attributes table; [[domain-model]] notes it spans all three markers as a hub and carries no marker of its own.

Pages spanning several functionality areas (e.g. [[venue]], [[registration]], [[user-account]]) got per-attribute or per-row markers rather than one blanket marker, since e.g. Attendee registration and withdrawal are `🎯` while waiting list and attendance recording on the same page are `📋 Backlog`. Every `🧩 Adjacent` marker added cites the specific core feature that pulls it in, mirroring [[release-1-scope]]'s own citations rather than inventing new rationale.

**Integrity checks run:** every wikilink added resolves to an existing page; no new unlinked `#N`/`brief §N` citations were introduced (verified against the diff, not just the file); existing ✅/⚠️/🔶 attribute-status markers were left untouched on all pages — the two marker systems are orthogonal per CLAUDE.md.
