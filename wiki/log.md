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
