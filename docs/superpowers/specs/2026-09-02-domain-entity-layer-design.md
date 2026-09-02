# Design — Domain Entity Layer for the IS212 T6 Wiki

Date: 2026-09-02
Status: Approved for implementation

## Problem

The wiki has 21 pages that are organised almost entirely by **process**. The 15 concept pages
answer *"what happens when…"* — `event-request-workflow`, `venue-booking`,
`change-impact-management`. The 3 entity pages cover an organisation, a product and a course.

Nothing answers *"what **is** a Venue — what fields does it have, what states can it be in, what
does it connect to, and at what cardinality?"*

Those facts exist in the wiki, but each object's structure is smeared across three or four
process pages. A reader cannot assemble the object model without reading most of the wiki.

This matters more than a normal wiki gap because of what the sources do *not* contain:

- `Customer Brief no. 1.md` has **no ER diagram, no class diagram, no data dictionary, no
  glossary, and no explicit entity list**. It is prose plus five functionality-area tables.
- The brief states only **three cardinalities** in the whole document, all in §4: multiple Event
  Organisers per organisation and multiple events per organisation (Client Management, line 125),
  and multiple sessions per event (Multi-Session Events, line 134). Every other relationship in
  the system is inferred.

  > Corrected during implementation. This spec originally claimed "exactly two", counting only the
  > Client Management row. The Task 7 reviewer found the Multi-Session Events sentence — "Some
  > events may contain several sessions within the same day or across multiple days" — which is
  > the same table, the same grammatical pattern, and just as much a stated cardinality. The
  > original claim was wrong and is corrected here and in the affected pages.
- The discussions board settles roughly 15 load-bearing **modelling** decisions (Tentative Hold
  is its own record, Booking is separate from Event, Session is first-class, equipment
  pooled-vs-serialised is deferred to the team) that are currently recorded as *process*
  narrative rather than as structure.

So the domain layer is not transcription. It is the first place the object model is assembled,
and it is the artifact that feeds the class diagram and the product backlog.

## Decisions taken

Four decisions were settled with the user before this design:

| # | Decision | Rationale |
|---|---|---|
| 1 | "Entity" means **domain object** (the nouns the system stores records about), not people/orgs | This is the layer that is genuinely absent and that feeds the deliverables |
| 2 | Domain objects live in a **new `wiki/domain/`** folder | `wiki/entities/` keeps its current meaning and its 3 pages; `CLAUDE.md` gains a page type rather than redefining one |
| 3 | **~14 aggregate roots**, satellites folded in as sections | 32 candidates were found; 25 near-exhaustive pages would produce many 5-line pages and a noisy graph |
| 4 | Layers **split by question type**, not duplicated | Domain pages own structure; concept pages own process. One fact, one home. |

## Non-goals

- No pages for Report, Dashboard or Calendar. These are **views**, not stored objects; they are
  already adequately covered by `concepts/reporting.md`.
- No pages for people, handles or student teams. That is a different sense of "entity" and was
  explicitly not chosen.
- No renaming or restructuring of the existing `wiki/sources/`, `wiki/entities/` pages.
- No change to `raw/`. It is immutable.
- No re-ingest of `raw/llm-wiki.md` as domain content. It remains methodology.

## Architecture

```
wiki/
  sources/     3 pages   unchanged
  concepts/   15 pages   process — "what happens when…"   (edited: trimmed + linked)
  domain/     15 pages   NEW — structure — "what is a…"
  entities/    3 pages   orgs, products, course — unchanged
  index.md               new Domain objects section
  log.md                 one appended entry
```

### Page inventory

14 object pages plus 1 hub. The "Absorbs" column lists satellite objects folded in as sections
rather than given their own page.

| # | Page | Absorbs |
|---|---|---|
| 1 | `event.md` | Event Category, Event Template / reuse, Readiness Item / Outstanding Action, Comment, Supporting Document |
| 2 | `event-request.md` | — |
| 3 | `session.md` | — (first-class per #74: own venue, equipment, timing, attendance limit) |
| 4 | `venue.md` | Room Layout, Venue Unavailability Block, the AM/PM/Night slot model |
| 5 | `booking.md` | Tentative Hold, Booking Conflict |
| 6 | `equipment-item.md` | — |
| 7 | `equipment-reservation.md` | Equipment Requirement line, partial fulfilment, Equipment Defect / return-inspection log |
| 8 | `support-request.md` | Technical Staff Assignment, staff availability |
| 9 | `registration.md` | Waiting List Entry, Attendance Record / check-in |
| 10 | `user-account.md` | Role, Attendee, multi-role, the role capability matrix |
| 11 | `client-organisation.md` | — |
| 12 | `change-request.md` | — |
| 13 | `notification.md` | Reminder |
| 14 | `audit-record.md` | Change History / version record |
| 15 | `domain-model.md` | hub — ER diagram + modelling-decision register |

### Naming constraint (load-bearing)

Obsidian resolves `[[wikilink]]` by **basename**. Two files sharing a basename in different
folders make every link to that name ambiguous. The bookings object therefore **must not** be
named `venue-booking.md`, because `wiki/concepts/venue-booking.md` already exists.

Resolution: the object is `booking.md` (the record); the process page stays `venue-booking.md`
(what happens when one is requested).

All 15 proposed basenames were checked against the 23 existing wiki basenames. No collisions.
Near-misses that are safe because Obsidian matches exactly, not by prefix:

| New | Existing | Safe |
|---|---|---|
| `event-request` | `event-request-workflow` | ✅ |
| `booking` | `venue-booking` | ✅ |
| `notification` | `notifications` | ✅ |
| `registration` | `attendee-registration` | ✅ |
| `user-account` | `user-roles` | ✅ |
| `equipment-item`, `equipment-reservation` | `equipment-management` | ✅ |

Any future domain page must be checked the same way before creation.

## Page format

Every domain page uses this structure. Sections with no content for a given object are omitted
rather than left empty.

```markdown
---
title: Venue
type: domain-object
sources:
  - "Customer Brief no. 1.md"
  - "IS212 2026 Discussions Aug 31.md"
tags: [domain, venue]
---

# Venue

One or two sentences: what this object is and why it exists. No preamble.

## Attributes

| Field | Meaning | Status | Ref |
|---|---|---|---|
| capacity | Maximum occupancy | ✅ Specified | brief §4 |
| turnaround time | Reset time between consecutive events; set **per venue** | ✅ Specified | #43 |
| booking horizon | How far ahead a venue may be booked | 🔶 Team decision | #57 |

## States

Only where the object has a real lifecycle. Mermaid `stateDiagram-v2`, plus a table of
each state's meaning and the rule that governs entry or exit.

## Relationships

| Related object | Cardinality | Rule | Ref |
|---|---|---|---|
| [[booking]] | 1 : many | Bookings never overlap on the same venue and slot | #35, #41 |

## Binding rules

Constraints on this object that are stated, not proposable. Short bullets with refs.

## Open modelling questions

> [!question]- Team decision required — venue booking horizon
> Body, with the #n where it was deferred.
```

### Status vocabulary

Three values only. This column is the point of the page — it separates what the customer has
settled from what T6 must still propose.

| Marker | Meaning |
|---|---|
| ✅ Specified | The customer stated it. Design to it; do not re-litigate. |
| ⚠️ Partial | Stated but incompletely — a hedge word, a half-answered question, or a rule with an unspecified parameter. |
| 🔶 Team decision | Explicitly left to the solution team. Must carry the `#n` where it was deferred. |

Every `🔶` row must cite a real discussion number or brief section. A `🔶` with no citation is a
lint failure.

### Citation format

Reuse the existing wiki convention: `#n` for a discussion number, `brief §n` for a brief
section. Where a claim needs attribution in prose, `(Source: is212-discussions-2026-08-31, #41)`
as already used elsewhere in the wiki.

## The hub page

`wiki/domain/domain-model.md` carries three things:

1. **A Mermaid `erDiagram`** of all 14 objects with their cardinalities. Obsidian renders Mermaid
   natively. This is the wiki's first synthesised artifact — neither source contains one.
2. **The modelling-decision register** — a table of the decisions the board actually settled,
   each with its `#n`. Examples: Tentative Hold is its own record with confirm/release
   transitions (#41); Booking is separate from Event, so bookings release while the event record
   survives (#69); venue availability is slotted AM/PM/Night, not continuous (#50); approval ≠
   confirmation (#80); Session is a first-class child of Event (#74); equipment has two
   independent availability rules that coexist rather than conflict (#2 follow-up, #5).
3. **The deferred-modelling register** — every `🔶` in the layer, collected, so the team has one
   list of what it must decide. Cross-linked with `concepts/open-questions.md`, which keeps its
   role as the register of open *requirements* questions; the hub covers open *structural* ones.

## Ripple

### Concept pages (~15 files, edits)

Each concept page keeps its process narrative in full. Two changes:

- An **Objects** line directly under the H1, linking the domain pages the process operates on.
- Scattered attribute and state enumerations are **replaced by a link** to the canonical domain
  page. For example `concepts/venue-booking.md` stops restating the AM/PM/Night slot definition
  and instead says bookings are made against the slots defined on `[[venue]]`.

Process rules, workflow steps, sequencing, and the existing callouts stay untouched. This is
trimming and linking, not rewriting. Where a concept page's only statement of a fact is the one
being trimmed, the fact must first exist on the domain page — no fact may be deleted before its
new home is written.

### `CLAUDE.md`

| Section | Change |
|---|---|
| Architecture | Add `wiki/domain/` — "Pages for domain objects: the nouns the system stores records about. Structure, not process." |
| Frontmatter | Add `domain-object` to the `type` enum |
| New subsection | Status vocabulary (✅ / ⚠️ / 🔶) and the rule that 🔶 must carry a citation |
| New subsection | The basename-collision rule for new page names |
| Ingest workflow | New step between current 4 and 5: "Create or update domain object pages in `wiki/domain/` for each object the source adds attributes, states, relationships or modelling decisions to." |
| Lint workflow | Two new checks: every domain page has attributes, relationships and a definition; every 🔶 row carries a citation |

### `index.md`

New **Domain objects** section, grouped the same way the concepts section is grouped (event
flow / resources / people and access / participation / cross-cutting), each row linking a page
with a one-line description. The existing `[!important]` callout and all other sections are
unchanged, except the page count line.

### `log.md`

One appended entry under `## 2026-09-02`, following the existing style: what was created, the
decisions taken, notable findings, and the integrity checks run.

## Verification

The work is complete when all of the following pass:

| Check | Method |
|---|---|
| No broken wikilinks | Every `[[target]]` in the vault resolves to an existing file basename |
| No duplicate basenames | `find wiki -name '*.md' -exec basename {} \; \| sort \| uniq -d` returns empty |
| Frontmatter valid | All 15 new pages have `title`, `type: domain-object`, `sources`, `tags` |
| No orphans | Each of the 15 new pages has at least one inbound link from `index.md` or a concept page |
| Citations present | No `🔶` table row lacks a `#n` or `brief §n` reference |
| Mermaid parses | The `erDiagram` and each `stateDiagram-v2` block is syntactically valid |
| No fact lost | Each fact trimmed from a concept page is present on the domain page that replaced it |

## Scale

15 files created, ~18 edited (15 concept pages, `CLAUDE.md`, `index.md`, `log.md`).
Nothing in `raw/` is touched.
