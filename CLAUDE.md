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
- **Contradictions**: If a new source contradicts an existing wiki page, flag it explicitly with a `> ⚠️ Contradiction:` callout.

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
