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

## 2026-09-05 (2)

### Produced Release 1 candidate user stories

**Created 1 page:** [[release-1-candidate-user-stories]] (`outputs/release-1-candidate-user-stories.md`, `type: review`).

Worked through [[release-1-scope]]'s register row by row and drafted a candidate card for every `🎯 Release 1` and `🧩 Adjacent` area, skipping `📋 Backlog` rows by design. Where a matching card already existed in [[user-stories-proposed-rewrites]] and still fit the Release 1 cut, this page points to it instead of duplicating it (U1–U4, E1–E2, V1/V2/V4/V5/V6, EV1–EV3c, Reg2–Reg3). 19 new cards were drafted for register rows with no existing card: client-organisation visibility, event status/information management, event confirmation, event completion, five venue cards (catalogue, availability calendar, search/filter, suitability check, booking-request submission), two equipment cards (catalogue, requirement specification), four change/cancellation cards, and one notification card. Every AC traces to a wiki page or a `#N`/brief citation; every unresolved customer question surfaces as an **Open question** rather than an invented rule, per the standard's §11.

**Notable finding:** Reg1 (Register) and Reg5 (Withdraw) in [[user-stories-proposed-rewrites]] were written before [[release-1-scope]] existed and both assume a waiting list, which the Week 4 briefing places in `📋 Backlog`. Superseded them for Release 1 planning with **RG1** and **RG4** (waiting-list branches removed); flagged this explicitly rather than silently editing the older review, since it remains a correct full-scope backlog for later releases. Reg4 and Reg6 (waiting-list promotion, check-in) are themselves entirely out of Release 1 and were excluded from this backlog.

**Updated:** [[index]] — added the new page to the Reviews table, bumped the page count to 40 and the review count to 2.

**Integrity checks run:** every wikilink resolves to an existing page; every `#N`/`brief §N` citation is linked; every `🎯`/`🧩` claim traces to the matching row in [[release-1-scope]]; no `📋 Backlog` functionality was carded.

## 2026-09-05 (3)

### Added a proposed relational schema to [[domain-model]]

Requested: generate database tables, in mermaid, with proposed PK/FK, from the domain model. Added a new "Proposed relational schema (tables, PK/FK)" section to [[domain-model]], between the conceptual ER diagram and the Modelling-decision register, rather than a new page — it is the next layer down from the existing object-level ER diagram, not a fifteenth domain object.

Normalised the fourteen domain-object pages' Attributes and Relationships tables into 26 tables (14 core objects + `ROLE`, `USER_ACCOUNT_ROLE`, `EVENT_ESSENTIAL_ARRANGEMENT`, `EVENT_COMMENT`, `SUPPORTING_DOCUMENT`, `ROOM_LAYOUT`, `VENUE_SUPPORTED_LAYOUT`, `BOOKING_SLOT`, `EQUIPMENT_RESERVATION_LINE`, `SUPPORT_REQUEST_ASSIGNMENT`, `WAITING_LIST_ENTRY`), one full attribute-level `mermaid erDiagram` with PK/FK/UK markers, and a "Schema design decisions" table recording every place normalisation had to make a structural choice the sources leave open — most cite an existing `🔶 Team decision required` callout on the owning page (pooled equipment counter [#13](https://github.com/SinYang13/IS212-2026/discussions/13), polymorphic Audit Record reference [#14](https://github.com/SinYang13/IS212-2026/discussions/14), nullable self-FK for comment threading [#15](https://github.com/SinYang13/IS212-2026/discussions/15), event-reuse self-FK [#75](https://github.com/SinYang13/IS212-2026/discussions/75), credentials as an opaque hash pending [#62](https://github.com/SinYang13/IS212-2026/discussions/62), among others).

Flagged as a candidate physical data model in a warning callout up front — same weight as the conceptual ER diagram's own caveat, not a customer deliverable or a closed team decision.

**Integrity checks run:** all `#N` citations in the new section are linked; all `[[wikilink]]` cross-references resolve to existing pages; no new page created, so [[index]] needed no update.

## 2026-09-06

### Generated a Supabase SQL DDL script from [[domain-model]]

Requested: an SQL script creating the tables and fields for the Supabase SQL editor, written to `outputs/` as a fenced code block. Produced [[supabase-schema-ddl]] — a review-type artefact, not a wiki page, since it's a one-off derivation of an existing model rather than standing domain knowledge.

Translated the "Proposed relational schema (tables, PK/FK)" section of [[domain-model]] into runnable Postgres: all 25 tables (14 domain objects + 11 junction/child tables), `bigint generated always as identity` surrogate PKs, every FK with an explicit `on delete` rule (`cascade` for children and junctions, `restrict` for master data), `check` constraints for every status column, partial unique indexes for one live registration per attendee per event/session, indexes on all 41 foreign keys, `updated_at` triggers, RLS enabled on every table with no policies, and role seed data.

**Verified**, not just written: the script was run end-to-end against Postgres 16 under `ON_ERROR_STOP=1` in a single transaction — clean `COMMIT`, 25 tables created. A first draft contained an index expression sub-querying another table (Postgres rejects it as non-`IMMUTABLE`); caught by that run and replaced with a documented two-option note rather than a broken statement.

**Documented rather than silently dropped:** five rules that cannot be expressed as DDL — the venue double-booking hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35), needs denormalised columns + a partial unique index, or a trigger), confirmation blocked on incomplete essential arrangements ([#80](https://github.com/SinYang13/IS212-2026/discussions/80)), the two equipment availability rules ([#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#5](https://github.com/SinYang13/IS212-2026/discussions/5)), auto-completion at end date + 7 days ([#71](https://github.com/SinYang13/IS212-2026/discussions/71), needs `pg_cron`), and RLS policies (blocked on the role model, [#56](https://github.com/SinYang13/IS212-2026/discussions/56)). Each is listed in a "Known gaps" table on the page.

Status value lists are the script's own best reading of each domain page's state machine, flagged as such — the sources do not enumerate them.

**Updated:** [[index]] — added the new page to the Reviews table; corrected the page count to 42 and the review count to 4 (the header had said 2 reviews while three review pages existed).

**Integrity checks run:** every `#N` citation in the new page is linked; all `[[wikilink]]` cross-references resolve; the `#N` mentions inside the SQL code fence are left unlinked per the code-fence exemption; no `wiki/` page changed, so no domain knowledge was altered.

## 2026-09-06 (2)

### Decoupled notifications from feature tickets into per-trigger user stories

Requested: find every Linear ticket whose acceptance criteria mention being notified, decouple notifications from them, and instead create separate user stories per notification event and the role that must receive and act on it — noting which are Release 1.

**Found 11 tickets** carrying notification ACs as one-line riders on other people's cards: SPM-20, SPM-22 (two), SPM-32, SPM-33, SPM-34, SPM-41, SPM-46, SPM-50, SPM-52, SPM-55, plus the SPM-47 epic itself. Two near-misses were deliberately left alone — SPM-24's "I receive confirmation of my registration" and SPM-28's "Withdrawal is confirmed to the Attendee" are on-screen feedback, not [[notifications|brief §6]] notifications.

**Split by the standard's §7.2 "Variations in data" pattern** into 20 sub-issues of SPM-47 (SPM-56…SPM-75), one per trigger × recipient: 15 `🎯 Release 1`, 2 `🧩 Adjacent` (availability change → core feature 9; registration capacity → core feature 18), 3 `📋 Backlog` (waiting list, withdrawal-affecting-availability, incomplete-arrangement reminders). SPM-47 keeps only what all twenty share — channels, per-type configurability, delivery timing, access-control filtering.

**The split surfaced four Release 1 notifications no ticket carried at all**: a new request reaching the Operations Manager, the Organiser learning who their coordinator is, the coordinator learning whether requested equipment was secured ([[notifications|brief §6]] trigger 6 verbatim), and registration opening or closing. They were invisible *because* notifications were scattered as riders — the argument for the split rather than an incidental finding.

**Resolved a standing team decision, not just a ticket layout.** [[notification]] and [[notifications]] both recorded the recipient matrix as unspecified — §6 names fourteen triggers and no recipients. Naming one recipient per card *is* that matrix, so it is now recorded as a proposal in a new "Recipient matrix" section on [[notification]], with the three structural decisions it forced: a trigger with two audiences is two cards (row 9 reaches both the client contact and external Attendees, bounded differently by [[access-control]], [#91](https://github.com/SinYang13/IS212-2026/discussions/91)); no role is notified of its own action (so event confirmation is carded for the Organiser only); and row 12 collapses without a waiting list. The Recipient attribute stays `⚠️ Partial` — a team proposal the customer has not confirmed is not a customer statement.

**Read/acknowledge state flagged, not invented.** Each card carries "can open the underlying record" and "can mark as read", but [[notification]] "States" records that read/acknowledged/dismissed tracking is unaddressed by both sources, so it is written as an open question on every card rather than as settled AC.

**Updated:** [[notification]] (new Recipient matrix section; Recipient attribute note; recipient-matrix open question now points to the proposal), [[notifications]] (matrix cross-reference, callout and Open Items row), [[release-1-candidate-user-stories]] (NT section expanded from one card to NT1–NT21 with a register table; coverage rows for Notification system, Venue unavailability management and Registration capacity updated).

**Integrity checks run:** all `#N` and `brief §N` citations in the three touched files are linked, including inside table cells and callouts; table cell counts unchanged on every edited row; all `[[wikilink]]` targets resolve. Fixed one pre-existing lint failure found in passing — a `brief s4` variant spelling in [[notifications]]' Open Items table, normalised to [[event-planning-venue-booking-system|brief §4]]. No new wiki page created, so [[index]] needed no update.

## 2026-09-06 (3)

### Stocktake: Linear vs. wiki vs. GitHub Discussions coverage

Requested: check whether any user stories are missing, comparing Linear's backlog against what this wiki has already drafted, including areas that may only surface in the GitHub Discussions rather than Release 1 scope. Produced [[user-story-coverage-stocktake]] (`outputs/`, `type: review`).

**Method:** pulled all 76 Linear `SPM - IS212` issues, matched them against [[release-1-candidate-user-stories]]'s and [[user-stories-proposed-rewrites]]' coverage tables, then walked every `📋 Backlog`/`🧩 Adjacent` row of [[event-planning-venue-booking-system]]'s functionality-areas table for a card anywhere — ticketed or not.

**Headline finding:** Release 1 (`🎯`/`🧩`) coverage between the wiki's candidate backlog and Linear is essentially 1:1 — every drafted card has a matching ticket, including all 21 notification cards. Two anomalies flagged for team confirmation rather than assumed as defects: SPM-30 ("Reassign an Event Coordinator") is marked **Duplicate** in Linear with no other open ticket visibly carrying reassignment; SPM-34 ("Approve or reject an event request")'s title/visible ACs may have dropped the "return to Organiser" outcome that [#67](https://github.com/SinYang13/IS212-2026/discussions/67) and the wiki's source card (EV3c) both carry.

**Sixteen `📋 Backlog`/undecided areas have a citation but no story card anywhere** — not in Linear, not in either output backlog: check-in ([#38](https://github.com/SinYang13/IS212-2026/discussions/38) — closest to ready, since **Reg6** already has full ACs drafted and just needs ticketing), the VIP/bulk-attendee-add spike ([#33](https://github.com/SinYang13/IS212-2026/discussions/33)), technical support requests and staff rostering ([#25](https://github.com/SinYang13/IS212-2026/discussions/25), [#88](https://github.com/SinYang13/IS212-2026/discussions/88), [#89](https://github.com/SinYang13/IS212-2026/discussions/89)), tentative venue holds ([#39](https://github.com/SinYang13/IS212-2026/discussions/39), [#41](https://github.com/SinYang13/IS212-2026/discussions/41)), event categories, recurring/reuse events ([#75](https://github.com/SinYang13/IS212-2026/discussions/75)), multi-session events ([#74](https://github.com/SinYang13/IS212-2026/discussions/74)), supporting documents ([#15](https://github.com/SinYang13/IS212-2026/discussions/15), [#18](https://github.com/SinYang13/IS212-2026/discussions/18), [#21](https://github.com/SinYang13/IS212-2026/discussions/21)), outstanding action tracking, the general reminder system ([#40](https://github.com/SinYang13/IS212-2026/discussions/40)), the event-level calendar and search (distinct from the venue equivalents already carded), the role-based dashboard, activity/change history browsing, and the entire reporting/export area ([#29](https://github.com/SinYang13/IS212-2026/discussions/29), [#32](https://github.com/SinYang13/IS212-2026/discussions/32)). Reporting and Technical Support stand out as the two largest areas with a dedicated concept page (and, for Technical Support, a domain object) but zero cards in either backlog.

Distinguished this from two customer-**denied** items (account recovery [#64](https://github.com/SinYang13/IS212-2026/discussions/64), admin role [#52](https://github.com/SinYang13/IS212-2026/discussions/52)) that are not gaps, just previously-flagged operational risks despite being out of scope by customer answer rather than by deferral.

**Updated:** [[index]] — added the new page to the Reviews table, bumped the page count to 43 and the review count to 5.

**Integrity checks run:** every `#N` citation is linked; every `[[wikilink]]` cross-reference resolves; no `wiki/` domain page was altered, since this is a stocktake of external tooling (Linear) against existing wiki content, not a new sourced finding.

## 2026-09-06 (4)

### Actioned the coverage stocktake: two tickets, two resolutions, one new backlog

Worked through all five recommendations in [[user-story-coverage-stocktake]]. Produced [[backlog-candidate-user-stories]] (`outputs/`, `type: review`) and made two changes in Linear.

**Ticketed (recommendations 1–2).** **SPM-76** "Check in a registered attendee" — Reg6 promoted from [[user-stories-proposed-rewrites]] as-is, role still unnamed, priority and estimate left unset. **SPM-77** "Spike: coordinator-side entry of non-self-registering attendees" ([#33](https://github.com/SinYang13/IS212-2026/discussions/33)) — created under a new `Spike` label, with three questions and **no timebox proposed**, since inventing one is the false precision the standard's §5.5 warns about.

**Both Release 1 anomalies resolved (recommendation 3), and neither was lost scope.** SPM-30's reassignment behaviour became an acceptance criterion on SPM-29, which now reads "assign **and reassign**". SPM-34's missing "return to Organiser" outcome was decoupled to SPM-33 "Request clarification on an event request", whose AC set ends "The status of the event should be marked as *returned*" — approve/reject and return-for-amendment are two decisions with different follow-on behaviour, so the split is the better slice. The two `> [!warning]` callouts in §1 of the stocktake became `> [!success] Resolved` callouts recording each. One residual noted rather than fixed: SPM-29's AC set no longer carries [#94](https://github.com/SinYang13/IS212-2026/discussions/94)'s point that a reassignment may be assumed already approved outside the system.

**Drafted the whole deferred half of the backlog (recommendations 4–5)** — nine epics, 30 story cards and one spike, covering all seventeen §2 areas. Reporting (**EP-B**) and Technical Support (**EP-A**) went first, per recommendation 5: each had a full concept page, and Technical Support a domain object, with zero cards between them.

**Deliberately stopped short of pushing the 30 cards to Linear.** Recommendation 4 asked for a Linear item per area, and the drafting half is done — but the standard's workflow is `AI drafts → team decides → customer confirms` (§11) and pushing 30 unreviewed cards would skip the middle step and produce the bulk-generated backlog §11 explicitly warns against. Recorded as ⏳ in the stocktake, not ✅.

**Method note — inference was kept out of the acceptance criteria.** Four research passes over `raw/` and the wiki returned not just the customer's statements but a list of wiki lines that *read* like customer rules and are actually derivations: the per-role dashboard tables ("Plausible", "Indicative" on their own pages), the reuse "do not copy" field list, the slots-booked-over-slots-available utilisation measure, and the claim that manually-added VIP attendees must appear in registration reports. Every one of these is in an Open question on the relevant card, never in an AC — Gate B6 forbids a rule with no customer source, and a wiki inference is not one.

**Six cards name no role**, written as `*[role not named — see open question]*` rather than filling in a plausible one: who checks attendees in ([#38](https://github.com/SinYang13/IS212-2026/discussions/38)), who allocates Technical Support Staff ([#88](https://github.com/SinYang13/IS212-2026/discussions/88) asked and was answered on a different axis), who may place a tentative venue hold, who browses change history and activity history, who configures reminder frequency, and who counts as an "appropriate user" for an event report. A deliberate Gate A3 failure — a filled-in role makes a card *look* Ready while burying a customer question inside it.

**Findings that outlive the cards.** Every one of the 30 has a blocking unknown, which is why these areas went uncarded in the first place; ten of those need the customer rather than the team and are listed as such. [#52](https://github.com/SinYang13/IS212-2026/discussions/52) and [#47](https://github.com/SinYang13/IS212-2026/discussions/47) are in direct tension — reminder frequency must be configurable, but no admin role exists to configure it. Two areas carry no scope marker anywhere: the VIP path sits under the `🎯` attendee-registration row with nothing in core feature 18 covering it, and primary contact has no row in [[release-1-scope]]'s register at all, despite that register claiming to hold every functionality area exactly once. Also corrected the stocktake's own off-by-one — its prose said sixteen §2 areas, its table has seventeen.

**Six cards deliberately not written**, listed with reasons: a standalone export card (a criterion, not a story), auto-release of an expired hold (no duration exists to build against, [#39](https://github.com/SinYang13/IS212-2026/discussions/39)), an Event Organiser event calendar (would settle an access question [#81](https://github.com/SinYang13/IS212-2026/discussions/81) doesn't reach), per-role dashboard content cards (would launder inference into specification), check-in reversal and no-shows (unstated), and attendance-at-completion (already inside SPM-51, Gate D4).

**Updated:** [[user-story-coverage-stocktake]] (§1 warnings → resolved callouts; all seventeen §2 rows annotated with their new card IDs; recommendations rewritten with status and a "what the drafting pass found" section), [[index]] (new page in the Reviews table, stocktake row rewritten, count 43 → 44 and 5 → 6 reviews).

**Integrity checks run:** every `#N` and `brief §N` citation in both new and edited files is linked, including inside table cells and callouts; the one wikilink pipe landing in a table cell is escaped `\|`; all seventeen §2 rows still have four cells; every `[[wikilink]]` target in the new page resolves against `wiki/` or `outputs/`. No `wiki/` domain page altered — the cards are drafts of team artefacts, not new sourced findings about the domain.

## 2026-09-07

### Scrutinised the deployed schema: 7 critical defects, 4 duplicates, 3 wiki-side root causes

Reviewed `raw/current schema.md` against [[domain-model]], all fifteen `wiki/domain/` pages and [[supabase-schema-ddl]] — the candidate DDL the deployed schema was derived from. Produced [[current-schema-scrutiny]] (`outputs/`, `type: review`).

**Table inventory is clean; the damage is below the column list.** All 25 objects present, correctly named and keyed. But **0 of 48 foreign keys carry an `ON DELETE` clause**, 24 `CHECK` constraints survive against the DDL's 36, and only 5 `UNIQUE` constraints remain — every partial unique index, scope check, time-order check and quantity check was dropped in transit.

**The headline defect is unshippable, not merely wrong.** `event.status` defaults to `'''Planning''::text'::text` — a doubly-quoted literal evaluating to the eleven-character string `'Planning'::text`. It fails the column's own `CHECK`, so every `INSERT INTO event` omitting `status` errors. And `Planning` is not in the allowed list anyway.

**Two status enumerations contain values from no source.** `event.status` omits `Planning` and `Awaiting Arrangements` — the two states [#80](https://github.com/SinYang13/IS212-2026/discussions/80) exists to create — and invents `In Progress`. Without `Awaiting Arrangements` the confirmation gate has nowhere to live and `Approved → Confirmed` becomes a legal single step, inverting the customer's answer. `event_request.status` drops `Returned` (the [[event-request-workflow|brief §5]] step-5 return-for-amendment outcome) and invents `Withdrawn`, making the resubmission loop unrepresentable.

**The user's structural point confirmed, and it cuts both ways.** The wiki *does* separate request state from event state — in prose. `event.status` nonetheless still carries `Draft` and `Approved`, so both tables can sit in `Approved` at once with no rule on which is authoritative, and an `event` row can be in `Draft` — a state that on the wiki's own model means "not yet submitted". The split is half-applied.

**Root cause is in the wiki, not the schema (W1).** [[event-request]] marks `Approved` "terminal for this record"; [[event]]'s `stateDiagram-v2` nonetheless reproduces all five pre-approval states, including `Returned` and `Rejected`. The wiki asserts the split in prose and denies it in the diagram. Both DDLs then guessed differently — which is exactly how `In Progress` and `Withdrawn` got in. Flagged with a recommended fix rather than applied: trimming [[event]]'s state machine is a modelling decision, not a typo.

**Two further wiki-side corrections logged.** [[supabase-schema-ddl]]'s "Known gaps" concedes its status lists are "a best reading of the owning domain page" — but neither `In Progress` nor `Withdrawn` occurs anywhere in the wiki, the brief or the discussions; they are inventions the deployed schema inherited verbatim (W2). And [[domain-model]]'s design-decision prose says staff availability is "left off `USER_ACCOUNT` entirely" while its own ERD block lists `string availability` on that table; both DDLs followed the ERD (W3).

**The customer's one unambiguous hard block is enforced by nothing.** Double-booking is a hard block, not a warning ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)) — yet `booking_slot` now carries a surrogate PK and nothing else. Even the DDL's `unique (booking_id, slot_date, slot)` is gone, so the same booking can claim one slot twice, let alone two `Confirmed` bookings colliding on one venue/date/slot.

**A `🔶` team decision was answered silently in SQL.** `registration.attendee_user_account_id` became nullable and `attendee_name`/`attendee_email`/`registration_reference` appeared — resolving [#22](https://github.com/SinYang13/IS212-2026/discussions/22) without the decision reaching [[registration]] or the deferred-modelling register. It also contradicts that page's mandatory many-to-one ("exactly one Attendee account", [#48](https://github.com/SinYang13/IS212-2026/discussions/48)), and no `CHECK` requires *any* of the three identity columns, so a registration attached to nobody inserts cleanly. Guest registration may be right for [#33](https://github.com/SinYang13/IS212-2026/discussions/33)'s VIP flow — but it belongs on the domain page first.

**Four duplicate pairs identified, and duplication per se is not the finding.** Request/event field duplication is defensible — the request is what was asked for, the event is what is being arranged. The defect is that six of the thirteen pairs are *spelled differently* (`event_name`/`name`, `accessibility_needs`/`accessibility_requirements`, `general_programme`/`programme_agenda`…), and `registration_requirements` (text) → `registration_enabled_flag` (boolean) is lossy. Copy-on-approval therefore needs a hand-maintained mapping. The other three duplicates — technical support on both `event` and `support_request`, `decision_record` on both request and event, and attendance stored *twice* on `registration` as both `Attended`/`No Show` statuses and `checked_in_at` — have no owner and no constraint keeping them in step. The attendance one directly breaches [[event-planning-venue-booking-system|brief §4]]'s requirement that registration and attendance stay distinguishable.

**A gate that cannot be implemented for two of its six types.** `event_essential_arrangement` records *which* arrangements are essential but never whether any is **complete**, so [#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s blocking predicate has no data to read. Completeness is derivable for `venue`, `equipment`, `registration` and weakly for `technical_support` — but `programme` and `other` have no backing table at all.

**One finding deliberately left unverified.** The dump shows no FK indexes, no `updated_at` triggers and no RLS enablement. If it is a complete dump rather than table-only, absent RLS means every table is readable through the Supabase anon key — a direct breach of [[access-control]]. Recorded as M9 with "verify against the live database, not the dump" rather than asserted.

**No `wiki/` page altered.** W1's fix is a modelling decision for the team; W2 and W3 are recorded as recommendations in the review. Applying any of the three before the request/event boundary is settled would just move the contradiction.

**Updated:** [[index]] (new row in the Reviews table, count 44 → 45 and 6 → 7 reviews).

**Integrity checks run:** every `#N` and `brief §N` citation in the new file and this entry is linked, including inside table cells and callouts; wikilink pipes landing in table cells are escaped `\|`; the one callout used is within budget and typed; `current-schema-scrutiny` is unused as a basename across `wiki/` and `outputs/`; every `[[wikilink]]` target resolves.

### Migration 01 drafted — and [[event-planning-venue-booking-system|brief §4]] turns out to name the statuses outright

Followed up [[current-schema-scrutiny]] with [[schema-migration-01]] (`outputs/`, `type: review`): runnable SQL for every defect a source actually grounds, with the rest listed as team decisions rather than guessed at.

**The scrutiny's W1 finding was half wrong, and the correction matters.** Line 131 of [[event-planning-venue-booking-system|brief §4]] (*Event Status Management*) names the statuses verbatim — "for example draft, submitted, under review, approved, planning, **awaiting arrangements**, confirmed, completed, cancelled, or rejected". So [[event]]'s state machine reproducing the pre-approval states is **faithful to the brief**, not a wiki slip; §4 genuinely puts all ten on the event. What the scrutiny got right is that [[event-request]] then claims six of them exclusively, and neither page yields. The brief blurs it; the wiki inherited the blur.

**A grounded allocation exists, and nothing has to be invented.** [[event-request-workflow|brief §5]] line 238 states "An event **request** is approved, rejected, or returned", and §4's *Draft Event Requests* row says "save an **event request** before submission" — both name the request explicitly. That splits the ten cleanly: Draft/Submitted/Under Review/Approved/Rejected to `event_request` (plus `Returned` from §5), and Planning/Awaiting Arrangements/Confirmed/Completed/Cancelled to `event`. Every customer-named status lands on exactly one table, none duplicated, none invented.

**Answered the "should we call it Blocked?" question against the sources, not taste.** No — twice over. `awaiting arrangements` is the customer's own word (§4, above). And `blocked` is *already taken* by §4's *Venue Availability Calendar*: "view when venues are available, tentatively held, confirmed, **blocked**, or otherwise unavailable". Renaming the event status to `Blocked` would collide with an established term for a venue-calendar state in the same system. Noted the honest caveat: §4 says "suitable statuses" and "for example", so the list is proposable — but `Blocked` is the one rename carrying an active cost.

**Confirmed the invented values have zero grounding.** `In Progress` occurs in **no** raw source — only in `raw/original script.md` and `raw/current schema.md`. `Withdrawn` appears in the brief only for *registration* withdrawal (§4 Registration Withdrawal, §6), never for an event request. Both are removed.

**Found a third schema artefact.** `raw/original script.md` (1118 lines) is not the same file as [[supabase-schema-ddl]] — its `event_status_chk` is `('Confirmed','In Progress','Completed','Cancelled')` with `default 'Draft'`, i.e. **already broken** before the deployed schema touched it: the default was not in its own check list. The deployed schema then re-added Draft/Approved and mangled the default into `'''Planning''::text'::text`. So the drift is three-generation, and the current default bug is a second-generation repair of a first-generation break.

**The original script did have everything the deployed schema lost** — verified by count: 49 `on delete` clauses, 41 `create index`, 23 named `_chk` constraints, 3 table-level `unique (...)`, 2 partial unique indexes on `registration`, the `updated_at` trigger loop and the RLS loop. The migration restores these rather than re-deriving them: Part 2 was **generated mechanically**, matching every `CONSTRAINT … FOREIGN KEY` in the deployed schema against the same column's action in the original — all 48 matched, none unmatched, none hand-typed.

**One place the migration goes beyond restoration, and only one.** The original left the double-booking block as a commented choice between options (a) and (b) and picked neither. [#35](https://github.com/SinYang13/IS212-2026/discussions/35) is settled and non-proposable (it is in [[index]]'s hard-rules callout), so Part 4 implements option (a) as that comment recommended — denormalise `venue_id`/`status` onto `booking_slot`, two triggers to keep them in step, then a partial unique index over `('Tentative Hold','Confirmed')`.

**Refused to guess where no target exists.** Part 1 opens with a `raise exception` guard that aborts if any `event_request` row holds `Withdrawn` — there is no customer-named state to map it to, so the team must decide rather than have the script silently pick `Rejected`. Part 0 is a read-only preflight of sixteen counts that must all return zero before Parts 1 and 3 will apply.

**Eight items deliberately left out of the SQL**, each with the reason: the technical-support and `decision_record` duplicates (no source names an owner), guest registration and the attendance split (🔶 [#22](https://github.com/SinYang13/IS212-2026/discussions/22) open; §4 requires the distinction but names no shape), essential-arrangement completion tracking ([#80](https://github.com/SinYang13/IS212-2026/discussions/80) requires the gate, names no storage), pooled-vs-serialised equipment (🔶 [#13](https://github.com/SinYang13/IS212-2026/discussions/13)), a unique login identity (🔶 [#62](https://github.com/SinYang13/IS212-2026/discussions/62)), and `related_support_request_id` on `notification`.

**Verification — what was and was not checked.** No Postgres server is available in this environment (`initdb`/`psql` are libpq client tools only; the Docker daemon is down), so the script was **not executed**, and this entry does not claim it runs. Static checks that were run and pass: balanced parentheses, even `$$` quoting and matched `begin`/`commit` across all 7 SQL blocks; 188 statements parsed with `sqlglot` 30.18 against the `postgres` dialect; all 25 referenced tables and every referenced column verified to exist in the deployed schema, with `booking_slot` correctly lacking the two columns Part 4 adds. Two `sqlglot` rejections were checked and are parser gaps, not SQL errors — it accepts `drop trigger x` and rejects `drop trigger x on t`, which is the inverse of Postgres, where the `ON` clause is mandatory.

**Two defects found in my own draft and fixed before publishing:** the Part 6 smoke-test `insert` was not wrapped in a transaction (it would have committed under psql autocommit) and depended on `user_account`/`client_organisation` row 1 existing — replaced with a catalog query plus an optional self-rolling-back insert that sources its FKs from a `cross join … limit 1`. And a `;` inside a SQL comment in Part 1 would have broken migration runners that split naively on semicolons.

**Not done, and flagged rather than applied:** [[event]] and [[event-request]] still carry the overlapping state machines. The migration's Part 1 split is now grounded well enough to apply to both pages, but changing two domain pages' state machines is a modelling decision for the team, so it is recorded as a prerequisite in the migration rather than made unilaterally.

**Updated:** [[index]] (new row in the Reviews table, count 45 → 46 and 7 → 8 reviews).

**Integrity checks run:** 0 bare `#N` citations outside code fences; 9 of 9 `brief §N` citations linked; 3 table blocks in the new page and all tables in [[index]] verified for consistent cell counts after wikilink pipes were escaped `\|`; no escaped pipes left in prose; all `[[wikilink]]` targets resolve; `schema-migration-01` unused as a basename across `wiki/` and `outputs/`.

## 2026-09-08

### Reconciled the wiki against the applied schema (`wiki/domain/schema.md`)

The team replaced the earlier schema drafts with a single authoritative DDL. Diffed it against [[supabase-schema-ddl]] (whitespace-normalised) and found **four** substantive changes, no more — the rest was reformatting. Propagated each to the pages that own it.

**The delta, in full.** `event_request.status`: default `Submitted` → `Draft`, and `Withdrawn` → `Returned`. `event.status`: default `Draft` → `Planning`, and the enum `Draft/Approved/Confirmed/In Progress/Completed/Cancelled` → `Planning/Blocked/Confirmed/Completed/Cancelled`. `registration`: `attendee_user_account_id` made nullable, joined by `attendee_name`, `attendee_email`, a `registration_reference` UUID, an XOR identity check and a partial unique index on the lower-cased email. Nothing else changed.

**The status split that [[schema-migration-01]] proposed has been applied — with one deviation.** The migration's grounded allocation of [[event-planning-venue-booking-system|brief §4]]'s ten example statuses one-per-table is now in force: six on `event_request` (with `Returned` from [[event-request-workflow|brief §5]]), five on `event`, none duplicated. The deviation is `Blocked` where the migration argued for the customer's own word *awaiting arrangements*. Recorded the rename on [[event]] and in [[domain-model]]'s decision register for traceability back to §4's vocabulary.

> [!success] Resolved — the "blocked collides with the venue calendar" objection does not hold
> [[schema-migration-01]] argued against `Blocked` because [[event-planning-venue-booking-system|brief §4]]'s *Venue Availability Calendar* already uses "blocked" for a venue state, and this entry initially repeated that as a real cost. It is not one, and the schema itself is the disproof: **nine** status values are already shared across enums — `Cancelled` across three tables, and `Confirmed` across `event` and `booking`, which is the exact event-vs-venue pair the objection was about. §4's own calendar sentence ("available, tentatively held, **confirmed**, blocked, or otherwise unavailable") uses `confirmed` while `Confirmed` is simultaneously an event status, and no one has found that ambiguous. Status values are scoped per entity — different column, different table, no shared value domain — so a word is ambiguous only in a UI that renders it without naming its entity, which is equally true of all nine. Objection withdrawn; the caveat has been removed from [[event]], [[domain-model]] and [[schema]], and [[schema]] now documents the per-entity scoping rule instead.

**[[event]]'s state machine was the largest edit.** It had reproduced all eleven statuses, including the six pre-approval ones that [[event-request]] simultaneously claimed as its own — the contradiction [[current-schema-scrutiny]] flagged as W1 and declined to fix unilaterally. The schema settles it, so the machine is now the five post-approval states only, entering at `Planning` on request approval. Dropped the `Awaiting Arrangements → Confirmed` edge in the process: it let an event confirm straight out of the blocked state, which is exactly what [#80](https://github.com/SinYang13/IS212-2026/discussions/80) forbids. Clearing a blockage now routes back through `Planning`. Also removed [[event]]'s duplicate resubmission-after-rejection callout, since `Rejected` is no longer one of its states — [[event-request]] already carried the same callout.

**`event_request` needed no status edits.** Its six states already matched the new check constraint exactly. What changed there is the framing: the `[!warning] The brief blurs Event Request and Event` callout described the split as unresolved and "the single largest open modelling decision in the event cluster". It is now decided, so it became an `[!info]` recording the allocation and its grounding.

**Guest registration is a new team decision, not a schema detail.** The nullable `attendee_user_account_id` plus name/email pair answers [#53](https://github.com/SinYang13/IS212-2026/discussions/53)'s account-vs-guest question in favour of guest registration. Logged it as `🔶 Team decision` on [[registration]] (attributes, an identity binding rule, cardinality `many : 1` → `many : 0..1`) and updated the four pages that carried it as open: [[open-questions]] (two rows), [[access-control]], [[attendee-registration]], [[release-1-scope]].

**Narrowed a Release 1 risk rather than closing it.** [[release-1-scope]]'s "Attendees can register but cannot be created" warning is now only three-quarters true. Feature 18's *register* verb is unblocked; *view status* and *withdraw* are not, because a guest holds a `registration_reference` but no credential. Rewrote the callout to say which verbs are actually still open instead of deleting it.

**Housekeeping.** The file arrived as `wiki/domain/schema.md.md` (double extension) with no frontmatter and no inbound links — three lint failures. Renamed to `schema.md`, gave it frontmatter, a status-enumeration table and a section listing the two rules the DDL cannot express declaratively ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)'s double-booking block and [#80](https://github.com/SinYang13/IS212-2026/discussions/80)'s confirmation gate). Its SQL body had been mangled by an Obsidian paste — every line followed by two blanks, all column alignment lost — so it was rebuilt from the previous DDL's formatting with the four deltas applied, then **verified byte-identical to the user's file after whitespace normalisation** before the original was removed. [[supabase-schema-ddl]] is now superseded rather than deleted, and says so at the top and in [[index]].

**Verification — what was and was not checked.** No Postgres server is available here, so the DDL was **not executed** and this entry does not claim it runs. Static checks that pass: balanced parentheses (263/263), even `$$` quoting, one `begin`/`commit` pair, 25 `create table`, and a parse of 75 statements by `sqlglot` 30.18 against the `postgres` dialect (three plpgsql bodies fall back to opaque `Command` nodes — a known parser limit, not an error). Checked specifically for the defect [[current-schema-scrutiny]] found: **all nine** status columns now have a default that satisfies their own `CHECK`, where the deployed schema had one that did not.

**Integrity checks run:** 0 bare `#N` and 0 bare `brief §N` citations outside code fences across all twelve touched files; no ragged tables (every table block has a consistent unescaped-pipe count); no escaped pipes left in prose; all `[[wikilink]]` targets resolve except three pre-existing generic placeholders in [[log]] and elsewhere (`[[wikilink]]`, `[[wikilinks]]`, `[[...]]`) that are illustrative prose, not links; `schema` unused as a basename elsewhere; [[schema]] has 11 inbound links, so not orphaned.

**Updated:** [[schema]] (new), [[event]], [[event-request]], [[registration]], [[domain-model]], [[event-lifecycle]], [[change-impact-management]], [[attendee-registration]], [[access-control]], [[open-questions]], [[release-1-scope]], [[week-4-customer-briefing]], [[supabase-schema-ddl]], [[index]] (page count 46 → 47, domain objects 15 → 16).

### Added a states diagram to [[event-request]]

[[event]]'s States section carries a `mermaid` `stateDiagram-v2`; [[event-request]]'s did not, despite already having a States table with the same entry/exit-rule shape. Added the missing diagram for the six pre-approval states (`Draft → Submitted → Under review → {Approved | Rejected | Returned}`), following [[event]]'s house style: draw the grounded transitions, and where the source is silent or explicitly deferred, omit the edge and explain the omission in prose immediately below the diagram rather than guessing.

Two edges needed that treatment. `Returned → Submitted` is drawn (not `Returned → Under review`) on the reasoning that [[event-request-workflow|brief §5]] describes resubmission as the same act as the original submission — an inference, not a customer-stated transition, so it's flagged as such in the prose. `Rejected` is drawn terminal, matching the plain reading of brief §5, and the possible `Rejected → Draft`/`Submitted` resubmission edge is deliberately **not** drawn because whether rejection is final was explicitly left to the team ([#67](https://github.com/SinYang13/IS212-2026/discussions/67)) — this mirrors [[event]]'s precedent of omitting the `Blocked → Confirmed` edge to keep an unresolved gate out of the state machine itself. No new open question was added since [#67](https://github.com/SinYang13/IS212-2026/discussions/67) was already tracked in the page's Open modelling questions.

**Updated:** [[event-request]] only — no attribute, relationship, or scope-marker changes, so no other page needed a mirrored edit.

## 2026-09-08 (2)

### SPM-24 implementation, merged back against the schema work

The attendee registration slice for [SPM-24](https://linear.app/pygmies/issue/SPM-24) was built in the `connectsphere` repository — a public list of events open for registration, an event detail page, and a registration form taking full name and email — on a branch cut before [[schema]] landed. Both lines of work independently concluded that Attendees register **without an account**. This entry records the merge, not a second discovery.

**Filed [[spm-24-implementation-decisions]]** in `outputs/`, recording nine decisions the ticket had to make and the items it did not settle. Linked from [[index]] (Reviews; page count 47 → 48, reviews 8 → 9).

**What the branch contributed that the schema work had not:**

- [[access-control]]'s Authentication table still answered "Is authentication required?" with a flat **Yes**, cited to [#62](https://github.com/SinYang13/IS212-2026/discussions/62) and a bare, unlinked `brief s4`. [#62](https://github.com/SinYang13/IS212-2026/discussions/62) is about the authentication *method*; it does not establish who must authenticate. The row now says authentication is required to reach protected information, and a second row records that no source requires an **Attendee** to hold a credential. The citation is normalised to [[event-planning-venue-booking-system|brief §4]] — where the brief actually puts feature 1 — and linked.
- [[registration]]'s [#22](https://github.com/SinYang13/IS212-2026/discussions/22) field set now carries a Release 1 answer (full name and email, fixed for every event) instead of standing wholly open, with the per-event-configurable reading deferred rather than rejected.
- Two rules the build settled and no source addresses: a duplicate registration is refused rather than silently absorbed, and a full event is refused outright, since `📋 Backlog`'s missing waiting list leaves [[event-planning-venue-booking-system|brief §4]]'s "prevent **or appropriately handle**" with only one available meaning.

**What the branch got wrong and did not survive the merge:**

- It withdrew [[release-1-scope]]'s registration risk callout outright, on the reasoning that going account-less removes the problem. It removes it for two of feature 18's four verbs, not all four. *View status* and *withdraw* still need a way to identify a guest, and the narrowed callout that landed with [[schema]] says so — kept, not withdrawn. Two corrections were made to it: its count (it was titled "three of four" while its body named two, and the build makes *view events* public, so it is two), and its opening clause, which still asserted all four verbs are "authenticated actions under feature 1" — the inference this ticket disproved.
- It edited [[supabase-schema-ddl]], which had meanwhile been superseded by [[schema]] and marked provenance-only. Those edits were dropped; [[schema]] already carries the equivalent (`registration_one_live_per_attendee_email_uidx`).
- Its [[domain-model]] ER edits were narrower than [[schema]]'s — no `registration_reference`, and name/email modelled as mandatory rather than nullable. Dropped in favour of the existing model.

**One disagreement is left open rather than resolved.** [[schema]]'s `registration_attendee_identity_chk` is an XOR — a registration carries **either** an account **or** a name-and-email pair. The build treats name and email as always present with the account link optional on top. The two agree on every row Release 1 can produce, since no attendee has an account; they diverge as soon as one does, because under the XOR an account-holder's row carries no email and falls outside the duplicate index. [[schema]] is the model of record, so the XOR stands. Recorded in [[spm-24-implementation-decisions]] §9 so it is not found at migration time.

**Also fixed in passing:** [[spm-24-implementation-decisions]] cited `brief §8b` as [[event-planning-venue-booking-system]] in two places. Per `CLAUDE.md`'s section map §8 and its subsections belong to [[non-functional-requirements]], which is what the rest of the wiki uses. Retargeted.

**Integrity checks run:** the seven files this merge touched carry no bare `#N` or `brief §N` citations outside code fences; the tables edited in [[access-control]], [[registration]], [[open-questions]] and [[index]] keep their original column counts; every `[[wikilink]]` added resolves to an existing page.

**Updated:** [[spm-24-implementation-decisions]] (new), [[access-control]], [[registration]], [[open-questions]], [[release-1-scope]], [[index]].

## 2026-09-08 (3)

### Incremental ingestion — `IS212 2026 Discussions Sept 8.md`

The board was re-exported as a strict superset of the Aug 31 snapshot. Rather than re-ingest 113 discussions, both files were split per-discussion on the `#### [#N]` heading and compared block-by-block, which isolates the real work from the section-count churn a whole-file diff produces.

**The delta, verified.** 26 new discussions ([#96](https://github.com/SinYang13/IS212-2026/discussions/96)–[#121](https://github.com/SinYang13/IS212-2026/discussions/121), all answered 2026-09-07); 2 existing discussions with a new follow-up answer ([#38](https://github.com/SinYang13/IS212-2026/discussions/38), [#49](https://github.com/SinYang13/IS212-2026/discussions/49)); 2 status-flag flips with byte-identical text ([#2](https://github.com/SinYang13/IS212-2026/discussions/2), [#55](https://github.com/SinYang13/IS212-2026/discussions/55)); **0 removed, 0 edited**. Ingestion surface: 28 items, not 113. The board now reports 0 unanswered — [[open-questions]]'s *Still Open in the Snapshot* section had predicted exactly this, since both flagged items were already resolved in substance on 2026-08-31.

**Filed [[discussions-sept-8-critical-diffs]]** in `outputs/`, the headline artefact: of the 28 changes, which change what had already been decided. Seven do. Graded 🔴×3 / 🟠×3 / 🟡×2, plus two pre-existing schema defects the batch made undeniable. Linked from [[index]] under Reviews.

**No answer in this batch reverses a customer position.** Every conflict is between a new answer and an inference the team made in the customer's silence — the expected failure mode, but four had already reached the schema or the scope register.

| Critical diff | What it broke |
|---|---|
| [#101](https://github.com/SinYang13/IS212-2026/discussions/101) — the Event Operations Manager **must** be a separate system role | [[user-roles]]'s entire `## Gap: The Event Operations Manager` section and [[user-account]]'s matching warning. Also closes [[open-questions]]'s *"Feature 5 is unbuildable without an answer"* — the batch's most consequential closure |
| [#112](https://github.com/SinYang13/IS212-2026/discussions/112) — venue capacity **varies by layout**, supplied not computed | [[venue]] modelled capacity as one scalar and carried a `[!question]-` asking this exact question. A cardinality change, not a field edit |
| [#103](https://github.com/SinYang13/IS212-2026/discussions/103) — a request-level `Withdrawn` state is required | [[event-request]] and [[schema]] had both *deleted* it as "grounded in no source"; [[current-schema-scrutiny]] had listed it as a 🔴 defect. Now grounded |
| [#108](https://github.com/SinYang13/IS212-2026/discussions/108) — one slot is the atomic unit of setup/turnaround | [[venue-booking]] had flagged this as an unresolved team tension in as many words |
| [#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up — no onboarding in R1; **pre-loaded** staff and client accounts | Closed [[user-account]]'s onboarding team-decision and [[access-control]]'s open row — for staff and Organisers only |
| [#102](https://github.com/SinYang13/IS212-2026/discussions/102) — the Organiser may **not** edit after submission | A hard access rule no page had stated. Significance now governs revalidation only, never edit rights |
| [#99](https://github.com/SinYang13/IS212-2026/discussions/99) — capacity is a hard cap, even for a VIP | Gave [[registration]]'s inference a citation, and retired [[release-1-scope]]'s *"Registration has no stated ceiling"* warning |

**Two claims were checked and one of them was mine.** The critical-diffs draft asserted that [[schema]] needed a sixth role adding for the Event Operations Manager. It does not — the DDL already seeds `'Event Operations Manager'` in its `role` table, so the schema had quietly been *right* about [#101](https://github.com/SinYang13/IS212-2026/discussions/101) while the concept pages called it a gap. The wiki was internally inconsistent before this batch. Corrected before publishing rather than shipped as a confident wrong statement.

**Four schema divergences recorded, none applied.** [[schema]] documents the *deployed* model, so its DDL was deliberately not edited — rewriting it would falsely claim a migration ran. A new *Divergences* section lists each with its minimum migration:

| | Divergence | Ref |
|---|---|---|
| D1 | `equipment_reservation` has **no date columns at all**. The committed Return Day + 1 rule computes from a date the schema does not hold, so core feature 16 cannot answer correctly | [#113](https://github.com/SinYang13/IS212-2026/discussions/113) |
| D2 | `venue.capacity` is scalar and `venue_supported_layout` is a bare junction — per-layout capacity has nowhere to live | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| D3 | `event_request_status_chk` enumerates six values; the page now models seven | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| D4 | `change_request` is flat — one `coordinator_decision text` cannot express a partial approval, and a declined field's reason has nowhere to go | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |

D1 is the worst and predates this batch: [#113](https://github.com/SinYang13/IS212-2026/discussions/113) merely removed the last escape hatch by confirming there is no default return day the schema could have assumed. D4 blocks core feature 19, which is 🎯 Release 1.

**What was deliberately *not* closed.** The [#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up offers pre-loaded accounts to staff and clients only, while re-confirming all four of feature 18's attendee verbs as Release 1 obligations. It therefore *removes* seeded attendee accounts as an escape route from the guest-credential problem rather than solving it. [[release-1-scope]]'s narrowed registration callout was tightened, not withdrawn. Similarly, [#99](https://github.com/SinYang13/IS212-2026/discussions/99) hardens the **registration** ceiling but leaves [[venue]]'s *"not normally"* suitability hedge untouched — a different rule on a different object — and [#33](https://github.com/SinYang13/IS212-2026/discussions/33)'s VIP routing through the Coordinator survives, since [#99](https://github.com/SinYang13/IS212-2026/discussions/99) addressed capacity and never spoke to who performs the act.

**Method.** Four cluster agents worked disjoint page sets in parallel (Venues / Events+Requests / People+Registration / Equipment); the shared registers — [[release-1-scope]], [[open-questions]], [[schema]], [[domain-model]], [[audit-record]], [[reporting]], [[index]] — were held back and integrated centrally so the parallel work could not collide on them. Agent claims touching the schema were re-verified against the DDL before being recorded; that is how D4 was confirmed and how the role-table error above was caught.

**Updated:** [[is212-discussions-2026-09-08]] (new), [[discussions-sept-8-critical-diffs]] (new), [[is212-discussions-2026-08-31]] (superseded), [[is212-discussions-2026-08-26]] (third-snapshot wording), [[venue]], [[booking]], [[venue-booking]], [[venue-suitability]], [[event]], [[event-request]], [[change-request]], [[event-lifecycle]], [[event-readiness]], [[event-request-workflow]], [[change-impact-management]], [[user-roles]], [[user-account]], [[access-control]], [[registration]], [[attendee-registration]], [[notification]], [[notifications]], [[equipment-management]], [[equipment-item]], [[equipment-reservation]], [[technical-support]], [[release-1-scope]], [[open-questions]], [[schema]], [[domain-model]], [[audit-record]], [[reporting]], [[index]] (48 → 50 pages).

## 2026-09-09

### Schema fix — `assigned_coordinator_user_account_id` added to `event_request`

Raised in conversation: [[event-request-workflow]] Step 3 has the Event Operations Manager assign a coordinator right after submission ([#73](https://github.com/SinYang13/IS212-2026/discussions/73)) — before Review (Step 4) and Approval (Step 5), both performed by that coordinator. But [[schema]]'s DDL only ever carried `assigned_coordinator_user_account_id` on `event`, and [[event]] itself states the `event` row is created *at* approval. Between `Submitted` and `Approved` there was no column recording who was assigned. Not a sourced team decision or an intentional simplification — an oversight, since nothing in the wiki cited a reason for the gap.

Resolved by adding the column to **both** tables, matching the existing request/event duplication pattern (`preferred_date`, `expected_attendance`, `venue_requirements`, etc.): `event_request`'s copy is set at Step 3 and frozen once `Approved` (that state is terminal for the record); `event`'s copy is populated from it at approval and is the one [#94](https://github.com/SinYang13/IS212-2026/discussions/94) reassignment writes to afterward.

**Updated:** [[schema]] (DDL + new `event_request_coordinator_idx` + new "Coordinator assignment spans both tables" section), [[event-request]] (new attribute + relationship row), [[event]] (cross-reference note on the existing row).

### Migrated D1–D4

The four divergences [[discussions-sept-8-critical-diffs]] and [[schema]]'s *Divergences* section had flagged as needing a migration were applied: nullable `return_date` on `equipment_reservation` (D1, [#113](https://github.com/SinYang13/IS212-2026/discussions/113)); `capacity` on `venue_supported_layout` (D2, [#112](https://github.com/SinYang13/IS212-2026/discussions/112)); `'Withdrawn'` added to `event_request_status_chk` (D3, [#103](https://github.com/SinYang13/IS212-2026/discussions/103)); and a new `change_request_item` child table for per-field decisions (D4, [#104](https://github.com/SinYang13/IS212-2026/discussions/104)). D2's open question (whether `venue.capacity` becomes derived or is dropped) and D1's open question (how `return_date` gets populated) remain unresolved team decisions — only the structural gap was closed.

**Updated:** [[schema]] (DDL updated in place — table count 25 → 26, status enumerations table, "Divergences" section renamed "Migrations applied 2026-09-09 (D1–D4)").

## 2026-09-09

### Citation normalisation — 73 variant spellings across 12 files

`CLAUDE.md`'s Citation linking rule says other spellings (`brief s4`, `(Source: customer-brief-no-1, s4)`) "are not equivalent to `brief §N` — normalize to `brief §N` and link it rather than leaving the variant spelling bare." 73 lines were still on the old spelling; they predate the Sept 8 ingestion and were a standing lint failure.

Converted with a scripted pass, unit-tested on eleven edge cases before applying: multi-section forms (`s4, s5`, `s2 and s4`, `s4 and s8f`), the trailing-discussion form (`s4; is212-discussions-…`), the bare-`brief sN`-followed-by-a-discussion-link form, and table rows, which need the wikilink pipe escaped as `\|`. Non-`§N` variants were deliberately left alone — `(Source: customer-brief-no-1)` with no section, named sections like `*Event Capacity Management*`, and `step 14` are all legitimate and none of them is a `brief §N` citation.

`wiki/log.md` was excluded: it is the append-only historical record, and rewriting past entries to a newer convention would falsify what was written at the time. Its four occurrences are the only ones left in the vault.

**One mis-mapping fixed in passing.** `outputs/spm-24-implementation-decisions.md` cited `[[event-planning-venue-booking-system|brief §6]]`. Per `CLAUDE.md`'s section map §6 belongs to [[notifications]], not the system page — §4 does. Retargeted, with a light reword because the sentence already opened with the same wikilink.

### Schema migrated — D1–D4 applied, plus coordinator assignment

The four divergences [[discussions-sept-8-critical-diffs]] recorded were applied to [[schema]], together with a fifth change that was not one of them. Filed [[schema-migration-02]] in `outputs/` recording the SQL, its grounding, what was verified, and what it deliberately does not decide. Linked from [[index]] (Reviews; 10 → 11, pages 50 → 51).

| # | Change | Ref |
|---|---|---|
| D1 | `equipment_reservation.return_date`, nullable | [#113](https://github.com/SinYang13/IS212-2026/discussions/113) |
| D2 | `venue_supported_layout.capacity`, nullable | [#112](https://github.com/SinYang13/IS212-2026/discussions/112) |
| D3 | `'Withdrawn'` added to `event_request_status_chk` | [#103](https://github.com/SinYang13/IS212-2026/discussions/103) |
| D4 | `change_request_item` child table, decline reason enforced by check constraint | [#104](https://github.com/SinYang13/IS212-2026/discussions/104) |
| 5 | `event_request.assigned_coordinator_user_account_id` | [#73](https://github.com/SinYang13/IS212-2026/discussions/73), [#94](https://github.com/SinYang13/IS212-2026/discussions/94) |

Change 5 closes a gap older than the Sept 8 batch: [[event-request-workflow]] Step 3 assigns a coordinator *before* Review and Approval, but no `event` row exists that early, so nothing recorded who was reviewing a request between `Submitted` and `Approved`. The request's copy is frozen at approval; reassignment writes only to `event`.

**D3 reverses [[schema-migration-01]] deliberately.** That migration dropped `Withdrawn` on the reasoning that it "appears in the brief only for **registration** withdrawal", and [[current-schema-scrutiny]] flagged the invented value as critical finding C3. Both were right on the evidence then available; [#103](https://github.com/SinYang13/IS212-2026/discussions/103) supplied the grounding that was missing. Recorded in [[schema-migration-02]] so a reader diffing the two migrations does not conclude one is wrong.

**Verified rather than assumed.** The migration's housekeeping was checked against the DDL, not taken on trust: 26 `create table` against a 26-entry RLS loop, `change_request_item` present in the `updated_at` trigger loop, FK indexes for both new foreign keys, and every new status default satisfying its own `CHECK`. The header's "25 tables" was already corrected to 26.

**Three things the migration did not fix, now tracked rather than buried:**

- **`venue.capacity` was retained** beside the new per-layout figure, so capacity lives in two places with no stated precedence. [#112](https://github.com/SinYang13/IS212-2026/discussions/112) says suitability reads the layout-specific value; the scalar's fate is undecided, so it should be treated as untrusted.
- **`booking` records no room layout at all.** Confirmed against the DDL — neither `booking` nor `booking_slot` has such a column. The system therefore knows every venue's per-layout capacities but not which layout a given booking assumes, so D2 can filter a venue search but cannot validate a booking. Filed as finding **S3** in [[discussions-sept-8-critical-diffs]] and as a new 🎯 row in [[open-questions]].
- **`change_request`'s flat columns survive** beside `change_request_item` and nothing enforces which wins — a dual representation an implementer can write to one half of and read from the other.

**Propagated.** [[event-request]]'s contradiction callout became `[!success] Resolved`, keeping the history legible rather than deleting it, since [[current-schema-scrutiny]] had called the same value a critical defect. [[change-request]] records the chosen structure and the dual-representation trap. [[domain-model]]'s physical ER diagram was **re-drawn** rather than merely annotated — `CHANGE_REQUEST_ITEM` added with its relationship, plus `return_date`, per-layout `capacity` and the request's coordinator column — so the conceptual and physical models are back in sync at 26 tables.

**Updated:** [[schema-migration-02]] (new), [[schema]], [[domain-model]], [[event-request]], [[event]], [[change-request]], [[event-lifecycle]], [[event-request-workflow]], [[venue]], [[venue-suitability]], [[venue-booking]], [[booking]], [[equipment-reservation]], [[equipment-item]], [[equipment-management]], [[open-questions]], [[release-1-scope]], [[index]], [[discussions-sept-8-critical-diffs]], plus the 12 files normalised above.

### Schema and ER diagram scrutinised — consistency audit filed

Reviewed [[schema]]'s 26-table DDL and [[domain-model]]'s two ER diagrams against the fourteen domain-object pages, the sixteen concept pages, the migration artefacts and the raw sources. Filed [[schema-er-consistency-audit]] in `outputs/`. Linked from [[index]] (Reviews; 11 → 12, pages 51 → 52).

**The DDL itself is clean** — verified mechanically, not assumed: 26 `create table` against a 26-entry RLS loop, every index resolving to a real table and column, every `default` satisfying its own `CHECK`, and the `updated_at` trigger list matching exactly the tables carrying that column. No finding in this audit is a broken script.

**Every defect sits at the boundary between the physical model and the wiki that governs it.** One pattern explains most of them: the Sept 8 batch was absorbed by the concept and domain layers but only partially by the physical layer. 44 discussion answers cited by domain-object pages reach *neither* [[schema]] nor [[domain-model]], including [#99](https://github.com/SinYang13/IS212-2026/discussions/99), [#109](https://github.com/SinYang13/IS212-2026/discussions/109) and [#121](https://github.com/SinYang13/IS212-2026/discussions/121).

Six findings graded critical: `equipment_item.operational_status` and `registration.status` enumerate values no source supports (and two of each re-conflate distinctions their pages state explicitly); the [#99](https://github.com/SinYang13/IS212-2026/discussions/99) capacity cap is neither enforced nor computable, since no booking records a layout and `event` has no venue path; `equipment_reservation` holds the Return Day + 1 end of the availability window but has no anchor for the D-1 end; the guest-registration uniqueness index rests on a "Release 1 has no waiting list and no check-in" premise the same file falsifies three times; and `equipment_reservation.status` omits `Under review`, so the re-check edge [#114](https://github.com/SinYang13/IS212-2026/discussions/114) requires cannot be stored.

**Two claims of provenance do not hold.** `raw/current schema.md` and `raw/original script.md` — the documents [[current-schema-scrutiny]] reviews and [[schema-migration-01]] restores constraints "verbatim" from — do not exist in `raw/`. Separately, migrations 01 and 02 applied in order do **not** reproduce the current DDL: migration 01 ships `Awaiting Arrangements` under a titled section arguing against `Blocked`, which [[schema]] ships; migration 01's entire double-booking implementation is absent from [[schema]]; and [[schema]]'s attendee-identity block comes from a third migration no page documents.

**One agent finding was checked and dismissed rather than reported.** `booking_slot_value_chk`'s `('AM','PM','Night')` was flagged as violating [[venue]]'s "do not hard-code the missing labels". It does not — the customer named the three slots in [#50](https://github.com/SinYang13/IS212-2026/discussions/50)'s opening sentence; only the hour-range mapping is inferred, and the DDL stores no hours. Recorded in the audit's "Checked and dismissed" section so the next reader does not re-raise it.

**No `wiki/` page was corrected in this pass.** The audit is diagnostic only; the corrections it implies — including `index.md`'s stale "25-table" description of [[schema]] and the three [[schema-migration-02]] open items silently dropped from both model pages — are held pending review.

**Updated:** [[schema-er-consistency-audit]] (new), [[index]].

## 2026-09-09 (3)

### Backlog candidate cards revised against the Sept 8 batch

[[backlog-candidate-user-stories]] was drafted against the 2026-08-31 snapshot and was the last `outputs/` artefact still reasoning from it — the concept and domain layers had absorbed `raw/IS212 2026 Discussions Sept 8.md` days earlier. Revised it against [[discussions-sept-8-critical-diffs]], adding a **Revised for the 2026-09-08 batch** section at the top that maps each load-bearing answer to the cards it reaches.

**Twenty of the thirty-one items moved; no card was added, deleted or promoted into Release 1.** The batch changed the ground under the cards rather than the cards themselves, which is the useful finding: a backlog of drafts is exactly where new answers land without anyone noticing.

**Four unknowns genuinely closed or downgraded.** Reg6's blocking question — which internal role performs check-in — is answered: no new registration-only role is permitted and the team assigns an existing one ([#38](https://github.com/SinYang13/IS212-2026/discussions/38) follow-up), moving the card from *blocked on the customer* to *blocked on a design decision*. HB2's "which actions are significant" now has an illustrative customer list ([#96](https://github.com/SinYang13/IS212-2026/discussions/96)) — a `🔶 Team decision` with a strong steer, not a specification, mirroring [[audit-record]]'s reading rather than over-claiming it. EC1 stopped being "the thinnest-sourced card in the document": [#100](https://github.com/SinYang13/IS212-2026/discussions/100) both sources it and caps it, since a reasonable fixed category set suffices. And EC4's session-to-slot open question became an acceptance criterion — sessions occupy whole slots ([#108](https://github.com/SinYang13/IS212-2026/discussions/108), [#107](https://github.com/SinYang13/IS212-2026/discussions/107)), which turns its overlap check into set intersection.

**One scope boundary had to be drawn, and it is the reason this pass mattered.** [#118](https://github.com/SinYang13/IS212-2026/discussions/118) defers the role-based dashboard in the customer's own words — but [#117](https://github.com/SinYang13/IS212-2026/discussions/117) requires the Event Operations Manager to be able to identify newly submitted events awaiting Coordinator assignment. That worklist sits squarely inside CV4's territory and is load-bearing for core feature 5. Added an `[!info]` boundary callout to CV4 and a matching note on RC2 so the obligation cannot quietly disappear into a backlog epic.

**Two new customer questions opened.** [#102](https://github.com/SinYang13/IS212-2026/discussions/102) bars the Event Organiser from editing a submitted request — so EC6, whose role is the Organiser, now has to ask whether attaching a document counts as editing. And [#119](https://github.com/SinYang13/IS212-2026/discussions/119) fixes expected attendance as a single event-level figure, which must not be conflated with [#74](https://github.com/SinYang13/IS212-2026/discussions/74)'s per-session attendance *limit* — EC5 is exactly where a second, conflicting copy would be created. The escalated-questions list went from ten to nine: three closed, two opened, renumbered, with the closed three kept in a table rather than deleted.

**One new candidate declined rather than carded.** [#111](https://github.com/SinYang13/IS212-2026/discussions/111) permits an Event Organiser venue-preference field "if the team wishes". It went into **Cards deliberately not written** with the reason: it would sit inside core feature 2's venue-requirements field set, which is 🎯 Release 1, so carding it here would move a Release 1 field into the backlog document.

**Two answers were checked and deliberately not used as they first appeared.** [#105](https://github.com/SinYang13/IS212-2026/discussions/105) and [#115](https://github.com/SinYang13/IS212-2026/discussions/115) confirm further notification triggers, but neither is a *reminder* — RM1's and RM2's categories remain unconfirmed, and the card now says so explicitly to stop a later reader citing them. Likewise [#101](https://github.com/SinYang13/IS212-2026/discussions/101)'s sixth system role does **not** resolve RM3's configurability-versus-no-admin-role tension, and does not weaken TS4's question about who allocates Technical Support Staff — it sharpens it, since the customer has now shown it will create a role when it wants one.

**One pre-existing error corrected while in the file.** The "How to read these cards" section claimed *three* cards name no role; its own summary table showed nine (eight carrying the inline placeholder, plus Reg6 whose actor is left unnamed in prose). Corrected to nine, with the correction noted in place rather than silently overwritten.

**Updated:** [[backlog-candidate-user-stories]], [[index]].

## 2026-09-09 (4)

### Linear board audited against the Sept 8 batch

The concept, domain and schema layers absorbed `raw/IS212 2026 Discussions Sept 8.md` days ago; the backlog did not. Audited all ~60 `SPM - IS212` tickets against [[discussions-sept-8-critical-diffs]] and filed [[linear-ticket-audit-sept-8-batch]].

**Nine acceptance criteria said the opposite of what the customer had said, and one was being built.** SPM-31 (Submit an event request) is assigned and in progress with no criterion for the post-submission edit lock — a draft-then-submit form leaves the Organiser's edit affordance in place by default, which is precisely what [#102](https://github.com/SinYang13/IS212-2026/discussions/102) forbids. Two ACs added before the code lands rather than after. SPM-39, also in progress, carried the softer version of the same error ("cannot edit unless assigned as responsible Organiser" implies the responsible one may).

**Two tickets described work the customer has declined.** SPM-37 (Register an account) is closed: onboarding and self-registration are not required for Release 1 and pre-loaded accounts are sanctioned ([#49](https://github.com/SinYang13/IS212-2026/discussions/49)) — its own open question had asked whether self-service was wanted at all, and the answer is no. SPM-56 (Notify the Ops Manager of a new request) is closed: [#117](https://github.com/SinYang13/IS212-2026/discussions/117) requires discoverability, not notification, so the mechanism was wrong rather than incomplete. Both closures name their compensating control — SPM-98 and SPM-95 — so no capability was dropped along with the card.

**SPM-41 was the subtlest, and worth recording as a pattern.** Its criterion routed a Coordinator's post-reservation equipment change "through the change-request process". [#114](https://github.com/SinYang13/IS212-2026/discussions/114) mandates no such gate — but the deeper fault is that [#102](https://github.com/SinYang13/IS212-2026/discussions/102) makes the Coordinator the person change requests are routed *to*, so the card required the Coordinator to raise a change request with themselves. Two individually reasonable answers combining into an impossible workflow, with neither discussion mentioning the other.

**A required Release 1 verb had no ticket anywhere on the board.** Core feature 18 names four attendee verbs; *view registration status* was carded by none of them — SPM-24/82 show status once at registration, SPM-28/84 withdraw, and nothing let an attendee return and look. Now SPM-102. It compounds an existing exposure: SPM-28 is `Done` and correct but reachable only via a `registration_reference` shown once on a confirmation panel, so two Release 1 verbs rest on an undocumented bearer-token design whose one delivery channel (SPM-87) is unbuilt. Raised SPM-103 to ratify the mechanism rather than leave it an implementation detail.

**No completed ticket needs reopening — and that is a real finding, not an absence of one.** All eight `Done` tickets are consistent with the batch, several vindicated by it: account-less registration is now mandated, the hard capacity refusal now cited ([#99](https://github.com/SinYang13/IS212-2026/discussions/99)), the missing waiting list reconfirmed ([#116](https://github.com/SinYang13/IS212-2026/discussions/116)). The exposure is coverage, not defect.

**One wiki page was corrected at source rather than left to the artefact.** [#102](https://github.com/SinYang13/IS212-2026/discussions/102) had reached [[change-impact-management]] during ingestion but never propagated to [[access-control]], whose Client Organisation Visibility matrix still granted the responsible Event Organiser an unqualified `Edit ✅`. That table is what a developer reads to build the permission check, so the wiki was still specifying the forbidden behaviour. The cell is now scoped to `Draft`, with a fourth consequence recording that the significant/non-significant distinction no longer governs *who* may edit.

**Four sprint cards were carrying work with no acceptance criteria at all** — SPM-78, 95, 96 and 97, where SPM-96's title ("available" coordinators) was the entire specification and implied a system-computed eligibility that [#93](https://github.com/SinYang13/IS212-2026/discussions/93) rules out. All four written. This is the mechanism behind SPM-31's missing lock: an unwritten criterion cannot be contradicted by a new answer, so no amount of diffing catches it.

**Fourteen tickets created (SPM-98 – SPM-111), thirteen edited, two closed. About 35 Tier 2 edits are catalogued but deliberately not applied** — stale rather than contradictory — and listed in the artefact so the next pass has a worklist rather than a re-audit.

**Updated:** [[linear-ticket-audit-sept-8-batch]] (new), [[access-control]], [[index]].

## 2026-09-11

### Tier 2 backlog worklist applied — 22 tickets

Worked the seven largest Tier 2 groups catalogued in [[linear-ticket-audit-sept-8-batch]]'s "Not yet applied" section: SPM-42, 44, 45, 46 (capacity as a scalar), SPM-47, 69, 70, 73 (attendee reachability), SPM-18, 52, 65, 67 (partial outcomes), SPM-17, 18 (equipment return date), SPM-50, 54, 68, 69, 71, 72, 76 (answered open questions), SPM-13, 15 (dashboard wording), and SPM-87 (promotion) — 22 distinct tickets in total.

**Convention used throughout: strike stale wording from the ticket body, explain the removal in a Linear comment, not inline.** Every edited card carries a "Tier 2 correction applied" comment naming exactly what was removed and why, so the body stays as short as the corrected story requires rather than accumulating a visible diff history.

**Two citation errors surfaced while applying the audit's own abbreviated list.** SPM-54's turnaround open question is actually closed by [#108](https://github.com/SinYang13/IS212-2026/discussions/108)/[#107](https://github.com/SinYang13/IS212-2026/discussions/107) (the M1 finding), not by any discussion in the audit's summary row — that row was indicative, not an exhaustive per-ticket map. SPM-71's open question is only partly closed by [#120](https://github.com/SinYang13/IS212-2026/discussions/120): it answers the venue-attribute-change half of the trigger and leaves the equipment-status-change half and the resulting booking status transition (S2) still open. Both corrections are recorded in the audit's "Not yet applied" section, not just in the tickets.

**Two open questions were deliberately kept rather than resolved**, per the audit's own instructions: SPM-45's block-vs-warn question (now cross-linked to SPM-107, which formally tracks it) and SPM-70's "which Attendees receive an opening notification" question (still has no addressable recipient set).

**One systemic fix landed once rather than three times.** SPM-47 (the notification epic) now carries a single "Attendee reachability" note explaining why "open the event from the notification" and "mark the notification as read" don't work for an account-less Attendee ([#49](https://github.com/SinYang13/IS212-2026/discussions/49) follow-up, [#97](https://github.com/SinYang13/IS212-2026/discussions/97)); SPM-69, 70 and 73 strike the affected ACs and link back to it instead of repeating the rationale.

**Roughly 13 smaller Tier 2 edits remain unapplied** — the audit's "largest groups" table was never claimed to be exhaustive. Flagged in [[linear-ticket-audit-sept-8-batch]] as a follow-up worklist item rather than assumed closed.

**Updated:** [[linear-ticket-audit-sept-8-batch]], and 22 Linear tickets (SPM-13, 15, 17, 18, 42, 44, 45, 46, 47, 50, 52, 54, 65, 67, 68, 69, 70, 71, 72, 73, 76, 87).

## 2026-09-21

### Ingested `IS212 2026 Discussions Sept 16.md` — a two-discussion delta

The Sept 16 export is a strict superset of Sept 8 (115 discussions, was 113). Both snapshots were split per-discussion and compared block-by-block: **two new discussions, no edited text, none removed.** The only other difference is a section header ([#49](https://github.com/SinYang13/IS212-2026/discussions/49)'s block trails the Venues "Answered" count, so a naive diff flags it). Both new answers are in Venues, from `josephsungls`. Recorded in [[is212-discussions-2026-09-16]]; [[is212-discussions-2026-09-08]] is now a superseded snapshot.

**[#123](https://github.com/SinYang13/IS212-2026/discussions/123) turned the setup/turnaround duration into an adjacency rule.** Setup and turnaround each need **at most one full slot, the immediately adjacent one, crossing midnight** — AM setup takes the previous day's Night; Night turnaround takes the next day's AM. No multi-slot buffers; the user does not choose the slot. This answers most of the question [[open-questions]] carried ("does a minutes-valued per-venue turnaround still mean anything once slots are atomic?"): no — there is no duration. It also makes the buffer a **conflict-checking obligation on neighbouring slots**, which [[booking]]'s effective-occupancy window and [[venue-booking]]'s conflict section now state. A side-effect worth noting: it corroborates the *order* of the garbled [#50](https://github.com/SinYang13/IS212-2026/discussions/50) slot labels (AM → PM → Night) without supplying any label or hour.

**Left open, deliberately:** whether every booking consumes both buffers, whether neighbouring events may share one, what a buffer does against a Blocked slot, and whether buffers show on the calendar and in utilisation. With both buffers always applied and never shared, adjacent-slot events at one venue become impossible — an **inference** from "occupy" and the hard block ([#35](https://github.com/SinYang13/IS212-2026/discussions/35)), recorded as a team decision, not a rule.

**[#122](https://github.com/SinYang13/IS212-2026/discussions/122) introduced venue retirement — a concept nothing in the brief or the Week 4 briefing mentions.** Future bookings on a retired venue are **not** cancelled or auto-rescheduled; they are flagged for review and Coordinators notified, and the Coordinator and Venue Staff agree an alternative (possibly outside the system). It is the third instance of the [#120](https://github.com/SinYang13/IS212-2026/discussions/120) / [#54](https://github.com/SinYang13/IS212-2026/discussions/54) pattern — one unresolved destination state on [[booking]], now with three triggers. **Scope judgment call:** [[release-1-scope]] carries "Venue retirement" as `🧩 Adjacent` (features 10 and 12: search and booking must not offer a retired venue), flagged as the thinnest `🧩` on the page. [[venue]] gains a `Venue status` attribute as a `🔶 Team decision`.

**Deployed schema not migrated; three gaps recorded on [[schema]].** E1 — the now-dead `setup_time_minutes` / `turnaround_time_minutes` columns; E2 — no venue status, so a retired venue cannot be represented; E3 — the double-booking rule is written over a booking's own `booking_slot` rows and does not see buffer slots. Each depends on a team decision, so the DDL is untouched.

**Updated:** [[is212-discussions-2026-09-16]] (new), [[is212-discussions-2026-09-08]], [[venue-booking]], [[venue]], [[booking]], [[open-questions]], [[release-1-scope]], [[change-impact-management]], [[notifications]], [[notification]], [[venue-suitability]], [[reporting]], [[domain-model]], [[schema]], [[index]].

### Linear board audited against the Sept 16 batch

Read every venue-booking ticket on the `SPM - IS212` board and posted **eleven comments** — SPM-19, 20, 21, 23, 42, 43, 44, 46, 54, 71, 105. Filed [[linear-ticket-audit-sept-16-batch]]. **Comments only:** no ticket body edited, no status changed, no ticket created. Two comments (SPM-20 and SPM-44) were re-posted after a mistyped repository URL in a discussion link.

**Nothing was contradicted outright; the exposure is stale wording and a missing buffer.** SPM-23 ("Configure Venue Turnaround and Setup Buffer") is the one card whose premise is superseded — the customer removed the duration it configures. The cards that matter are its *consumers*: SPM-19, 46, 54 and 20 all say "overlap", each is correct only if overlap is defined over the padded slot set, and that definition lived on no ticket. SPM-42, 43, 44 and 46 are in the current cycle; SPM-42's create/update subtasks may be about to expose columns the customer no longer needs. SPM-20's own open question — does Venue Staff need a cancel trigger for a permanently unusable venue? — is answered: no.

### Backlog candidates: revised, not ticketed

[[backlog-candidate-user-stories]] was checked against the whole board: only Reg6 (SPM-76) and SPK-1 (SPM-77) exist as tickets; the other 29 of its 31 items do not. **They were not created** — each fails the readiness gate by design, and bulk-creating 29 unready cards on a shared board was not something the request clearly authorised. The document was updated for the two new answers instead (RP2, EC4 and VH1 revised; a "Retire a venue" card considered and declined, with reasons) and a revision section added.

**Updated:** [[backlog-candidate-user-stories]], [[linear-ticket-audit-sept-16-batch]] (new).
## 2026-09-15

### EV3c — a decided request stays visible to its Coordinator

Testing SPM-34 in `connectsphere` found that a decision made a request disappear from the Coordinator's view: a rejected request was listed nowhere, and an approved request's event could not be opened from the Coordinator's list of events. EV3c only said the Coordinator *can* approve or reject, so no criterion was contradicted — the same unwritten-criterion gap [[linear-ticket-audit-sept-8-batch]] found behind SPM-31.

**Two acceptance criteria added to EV3c** in [[user-stories-proposed-rewrites]], both grounded in [[week-4-customer-briefing]]'s *Event Review and Approval* row: "The outcome of the review should be visible to the relevant users." A rejected request leaves the review queue and is listed in the Coordinator's archive with its decision record; an approved request leaves it, and its event is listed among the events the Coordinator is running, from which the approved request can be opened. Mirrored on SPM-34 in Linear.

**Deliberately scoped to the Coordinator.** The briefing's "relevant users" also covers the Event Organiser, but that half is already carded (SPM-60, notifying the Organiser of the decision), so repeating it here would create a second owner. Withdrawn requests are not named either: recording a withdrawal is SPM-101, and nothing in the app records one yet.

**Not changed.** [[release-1-candidate-user-stories]] still says EV3c covers review and approval "unchanged", which stays true — the card gained criteria, not a new shape. The criteria require only that an approved request's event is *reachable*; what a Coordinator's own view of their events contains is SPM-137, whose scope question is still open.

**Updated:** [[user-stories-proposed-rewrites]] (EV3c), and Linear SPM-34.

## 2026-09-23

### SPM-101 — withdrawal's two open items settled by the team

[#103](https://github.com/SinYang13/IS212-2026/discussions/103) left two things to the team, and both were decided while SPM-101 was planned and built in `connectsphere`:

- **Which state can be withdrawn.** Only `Under review`. The customer's answer names no other state, so the unsourced `Submitted → Withdrawn` edge stays undrawn — now as a decision, not only as caution.
- **Where the withdrawal is stored.** No new column: status becomes `Withdrawn`, the coordinator's optional note is the decision record, and an [[audit-record]] entry (`action = withdraw`) records who withdrew it and when, in one transaction.

The attribute row stays `🔶 Team decision` with its citation; the decision is written into it. There is still no Organiser-facing withdraw action.

**Updated:** [[event-request]] (Withdrawal record row; States).

