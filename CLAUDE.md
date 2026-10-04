# Wiki — Schema

This file defines the conventions and workflows for maintaining the PM Intern Learning wiki.

## Architecture

- **`raw/`** — Immutable source documents. Never modify these. This is the source of truth.
- **`wiki/`** — LLM-generated markdown pages. The LLM owns this layer entirely.
  - `wiki/sources/` — One summary page per ingested source document.
  - `wiki/concepts/` — Pages for key ideas, frameworks, and methodologies.
  - `wiki/domain/` — Pages for domain objects: the nouns the system stores records about (Event, Venue, Booking, Equipment Item…). Structure, not process — attributes, states, relationships, cardinality. Concept pages describe what happens; domain pages describe what a thing *is*.
  - `wiki/entities/` — Pages for organizations, products, and people.
- **`outputs/`** — LLM-generated one-off artefacts: reviews, scrutiny passes, ad-hoc analysis of a raw document. Not standing domain knowledge, so it sits outside the four wiki page types — but it's still linked from `wiki/index.md` (so nothing is orphaned) and still logged in `wiki/log.md`.
- **`index.md`** — Content catalog of every wiki page, organized by category.
- **`log.md`** — Chronological append-only record of wiki activity.

## Conventions

- **Links**: Use Obsidian-style `[[wikilinks]]` for all cross-references between wiki pages. Use the page's filename without extension (e.g. `[[decision-making]]`).
- **Frontmatter**: Every wiki page has YAML frontmatter with `title`, `type` (source-summary | concept | entity | domain-object | review), `sources` (list of raw files this page draws from), and `tags`. `review` is reserved for `outputs/` pages.
- **Citations**: When a claim comes from a specific source, note it inline, e.g. `(Source: pm-intern-learning)`. Discussion (`#N`) and brief (`brief §N`) citations follow the stricter linking rule below — write them pre-linked, don't leave them as bare text for a later pass to fix.
- **Tone**: Concise, structured, third-person. Use headers, bullet points, and tables. Avoid fluff.
- **Contradictions**: If a new source contradicts an existing wiki page, flag it explicitly with a `> [!warning] Contradiction` callout (see below).
- **Page names**: Obsidian resolves `[[wikilinks]]` by filename, so two pages sharing a basename in different folders make every link to that name ambiguous. Before creating a page, check the basename is unused across the whole wiki. Domain pages take the singular object noun (`booking`); concept pages take the process name (`venue-booking`).

## Citation linking

`#N` and `brief §N` citations are click-through links, not inert text — link them at the moment you write them, in every location prose can appear: paragraphs, table cells, and callouts alike. A retroactive linking pass is a lint failure waiting to happen, not the normal workflow. `[[wikilink]]` cross-references (see Links, above) already resolve on their own and need no special handling.

| Citation | Rendered as | Rule |
|---|---|---|
| `#N` (GitHub Discussion) | `[#N](https://github.com/SinYang13/IS212-2026/discussions/N)` | Substitute the real number for `N` in both places. |
| `brief §N` | `[[<page>\|brief §N]]` | `<page>` is whatever `[[customer-brief-no-1]]`'s Content map assigns to that section — keep "brief §N" as the visible text. |
| `Week 7 CN` (customer change notice, N = 1–6) | `[[week-7-customer-changes\|Week 7 CN]]` | Substitute the real change number for `N` in the visible text. All six changes live on the one source page. |

Brief section → page map (mirrors `[[customer-brief-no-1]]`'s Content map — that page is the source of truth; update it first if a source cites a section not yet listed, then mirror the change here):

| § | Links to |
|---|---|
| §1–2 | [[connectsphere-event-services]] |
| §3 | [[user-roles]] |
| §4 | [[event-planning-venue-booking-system]] |
| §5 | [[event-request-workflow]] |
| §6 | [[notifications]] |
| §7 | [[non-functional-requirements]] |
| §8 (incl. subsections, e.g. §8f) | [[non-functional-requirements]] |

Rules:

- **Tables**: escape the wikilink's pipe as `\|` so it doesn't split table columns (e.g. `[[event-planning-venue-booking-system\|brief §4]]`). Verify the cell count is unchanged after adding it.
- **Code fences are the only exemption**: text inside ` ```mermaid ` / ` ```stateDiagram ` (or any diagram) fence is syntax, not prose — leave `#N` and `brief §N` unlinked there.
- **Self-reference exemption**: the discussion source-summary pages (`wiki/sources/is212-discussions-*.md`) don't link their own `#N` heading mentions — a self-link serves no purpose. Likewise `wiki/sources/week-7-customer-changes.md` doesn't link its own `Week 7 CN` headings.
- Other citation spellings (`brief s4`, `(Source: customer-brief-no-1, s4)`) are not equivalent to `brief §N` — normalize to `brief §N` and link it rather than leaving the variant spelling bare.

## Attribute status

Domain object pages mark every attribute, rule and relationship with exactly one status. No other values.

| Marker | Meaning |
|---|---|
| `✅ Specified` | The customer stated it. Design to it; do not re-litigate. |
| `⚠️ Partial` | Stated but incomplete — a hedge word, a half-answered question, or a rule with an unspecified parameter. |
| `🔶 Team decision` | Explicitly left to the solution team. **Must** cite the `#n` where it was deferred. |

A `🔶` row with no citation is a lint failure.

## Release scope

The Week 4 customer briefing fixed the Release 1 feature set, and the Week 7 customer change notice (`[[week-7-customer-changes]]`) added six mandatory changes to it. Pages that describe functionality carry exactly one of these markers, and no others.

| Marker | Meaning |
|---|---|
| `🎯 Release 1` | Named in the Week 4 briefing's core-20 table, **or** in a later customer change notice that makes it mandatory for Release 1 (currently only Week 7 C1–C6). Must work by Week 12. A row justified by a change notice must cite the change, e.g. `Week 7 C4`. |
| `🧩 Adjacent` | Not named in the core-20, but load-bearing for one that is — pulled in by the briefing's requirement that the release "support coherent end-to-end event workflows". Must cite which core feature depends on it. |
| `📋 Backlog` | In the Week 1 brief's consideration set, deferred past Release 1. Kept for the product backlog and C4 models, not deleted. |

Rules:

- The register in `[[release-1-scope]]` is the source of truth. A page's marker must match its row there; update that page first, then mirror.
- A `🧩 Adjacent` claim with no named dependent core feature is a lint failure — it is the marker most easily used to smuggle scope back in.
- Scope markers are orthogonal to the attribute-status markers above. An attribute can be `✅ Specified` and `📋 Backlog` at once.
- Nothing is deleted for being out of Release 1. The briefing explicitly directs teams to keep later features in the backlog and C4 models.

## Callouts

Use native Obsidian callouts, not bold-label blockquotes — a typed callout renders distinctly, a bold blockquote looks identical to a source quotation.

Only these five types. Do not invent others.

| Syntax | Use for |
|---|---|
| `> [!question]- Team decision required — <topic>` | Something the customer explicitly left for the team to propose. **Always foldable (`-`) and always given a specific topic** — the collapsed title must say which decision it is. |
| `> [!warning] <title>` | Gaps, contradictions, ambiguous or garbled source text, unsettled scope, tensions between requirements. |
| `> [!quote] Customer answer — #<n>` | Verbatim customer wording where the exact phrasing carries weight. |
| `> [!info] <title>` | A settled constraint or scope boundary that is easy to miss. |
| `> [!success] Resolved — #<n>` | A previously flagged contradiction or open item that has since been closed. |

Rules:

- **Budget.** Callouts mark exceptions, not structure. If a page has more than a handful of non-foldable callouts, the content belongs in a table.
- Prose, bullets and tables are the default. Reach for a callout only when a reader who skims must not miss the item.
- Never nest callouts, and never wrap a whole section in one.
- Reserve `[!important]` for `index.md` only, for the customer's committed non-negotiable rules.

## Workflows

### Ingest a new source
1. Place the source document in `raw/`.
2. Read the source fully.
3. Create a source summary page in `wiki/sources/`.
4. Create or update concept pages in `wiki/concepts/` for each major topic.
5. Create or update domain object pages in `wiki/domain/` for every object the source adds an attribute, state, relationship, cardinality or modelling decision to.
6. Create or update entity pages in `wiki/entities/` for organizations, products, people.
7. Update `wiki/index.md` with new/changed pages.
8. Append an entry to `wiki/log.md`.

### Query
1. Read `wiki/index.md` to find relevant pages.
2. Read those pages and synthesize an answer.
3. If the answer is worth preserving, file it as a new wiki page.

### Review
1. Produce the artefact in `outputs/`, not `wiki/` — a review, scrutiny pass, or other one-off analysis of a raw document isn't standing domain knowledge.
2. Give it frontmatter with `type: review` and `sources` citing the document(s) it scrutinizes.
3. Link it from `wiki/index.md` under a **Reviews** section so it isn't orphaned.
4. Append an entry to `wiki/log.md`.
5. If the review surfaces a correction to standing wiki knowledge (not just a defect in the raw document under review), fix the affected `wiki/` page directly — don't leave the correction stranded in the review artefact.

### Lint
1. Check for orphan pages (no inbound links) across both `wiki/` and `outputs/`.
2. Check for missing pages (referenced but don't exist).
3. Check for stale information superseded by newer sources.
4. Check for missing cross-references between related pages.
5. Suggest new sources or questions to investigate.
6. Check every domain object page has a definition, an attribute table and a relationship table.
7. Check every `🔶 Team decision` row cites a discussion number or brief section.
8. Check every `#N` and `brief §N` citation is linked per the Citation linking rule above (bare, unlinked occurrences outside code fences are a lint failure), including inside table cells and callouts.
