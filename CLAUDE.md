# Wiki — Schema

This file defines the conventions and workflows for maintaining the PM Intern Learning wiki.

## Architecture

- **`raw/`** — Immutable source documents. Never modify these. This is the source of truth.
- **`wiki/`** — LLM-generated markdown pages. The LLM owns this layer entirely.
  - `wiki/sources/` — One summary page per ingested source document.
  - `wiki/concepts/` — Pages for key ideas, frameworks, and methodologies.
  - `wiki/entities/` — Pages for organizations, products, and people.
- **`index.md`** — Content catalog of every wiki page, organized by category.
- **`log.md`** — Chronological append-only record of wiki activity.

## Conventions

- **Links**: Use Obsidian-style `[[wikilinks]]` for all cross-references between wiki pages. Use the page's filename without extension (e.g. `[[decision-making]]`).
- **Frontmatter**: Every wiki page has YAML frontmatter with `title`, `type` (source-summary | concept | entity), `sources` (list of raw files this page draws from), and `tags`.
- **Citations**: When a claim comes from a specific source, note it inline, e.g. `(Source: pm-intern-learning)`.
- **Tone**: Concise, structured, third-person. Use headers, bullet points, and tables. Avoid fluff.
- **Contradictions**: If a new source contradicts an existing wiki page, flag it explicitly with a `> [!warning] Contradiction` callout (see below).

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
5. Create or update entity pages in `wiki/entities/` for organizations, products, people.
6. Update `wiki/index.md` with new/changed pages.
7. Append an entry to `wiki/log.md`.

### Query
1. Read `wiki/index.md` to find relevant pages.
2. Read those pages and synthesize an answer.
3. If the answer is worth preserving, file it as a new wiki page.

### Lint
1. Check for orphan pages (no inbound links).
2. Check for missing pages (referenced but don't exist).
3. Check for stale information superseded by newer sources.
4. Check for missing cross-references between related pages.
5. Suggest new sources or questions to investigate.
