# Wiki Schema

## Domain
This repository is a shared, Git-managed LLM Wiki. The initial domain is intentionally broad: curated knowledge that the owner wants to preserve, connect, and query over time. Narrower topic-specific conventions should be added here after human review.

## Conventions
- File names: lowercase, hyphens, no spaces (for example, `transformer-architecture.md`).
- Every wiki page starts with YAML frontmatter.
- Use `[[wikilinks]]` to link between pages. Aim for at least 2 outbound links per page when meaningful.
- When updating a page, always bump the `updated` date.
- Every new wiki page must be added to `index.md` under the correct section.
- Every action that changes wiki content must be appended to `log.md`.
- `raw/` contains source material and is immutable after ingest. Corrections and interpretation belong in wiki pages, not raw sources.
- On pages that synthesize 3+ sources, append provenance markers such as `^[raw/articles/source-file.md]` to paragraphs whose claims come from a specific source.

## Frontmatter

```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

`confidence`, `contested`, and `contradictions` are optional, but recommended for opinion-heavy, fast-moving, or conflicting topics.

## Raw Source Frontmatter

Raw sources should include frontmatter so future re-ingests can detect drift:

```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest of the body below this frontmatter>
---
```

## Tag Taxonomy

Initial approved tags:

- Meta: `meta`, `summary`, `query`, `comparison`, `timeline`, `open-question`
- Source types: `article`, `paper`, `transcript`, `documentation`, `meeting-notes`
- Entities: `person`, `organization`, `product`, `project`, `repository`, `model`, `tool`
- Concepts: `concept`, `architecture`, `workflow`, `policy`, `research`, `implementation`
- Quality: `contested`, `low-confidence`, `needs-review`

Rule: every tag on a page must appear in this taxonomy. If a new tag is needed, propose a `SCHEMA.md` change for human review first.

## Page Thresholds

- Create a page when an entity or concept appears in 2+ sources OR is central to one source.
- Add to an existing page when a source mentions something already covered.
- Do not create pages for passing mentions, minor details, or things outside the domain.
- Split a page when it exceeds roughly 200 lines.
- Archive a page when its content is fully superseded: move it to `_archive/`, remove it from `index.md`, update inbound links, and log the action.

## Entity Pages

One page per notable entity. Include:

- Overview / what it is
- Key facts and dates
- Relationships to other entities via `[[wikilinks]]`
- Source references

## Concept Pages

One page per concept or topic. Include:

- Definition / explanation
- Current state of knowledge
- Open questions or debates
- Related concepts via `[[wikilinks]]`

## Comparison Pages

Side-by-side analyses. Include:

- What is being compared and why
- Dimensions of comparison, preferably as a table
- Verdict or synthesis
- Sources

## Update Policy

When new information conflicts with existing content:

1. Check dates; newer sources may supersede older ones.
2. If genuinely contradictory, note both positions with dates and sources.
3. Mark the contradiction in frontmatter using `contested: true` and/or `contradictions:`.
4. Flag for human review in the report or PR body.

## Repository Governance

- `/Users/ken/wiki` is the stable main checkout.
- All writing work should happen on a branch/worktree, not directly on stable main.
- Automated profiles may read stable main, but must not commit or push directly to `main`.
- `SCHEMA.md` changes require human review. Agents may propose schema changes in PRs, but should not silently merge schema changes without explicit approval.
