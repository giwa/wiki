---
name: wiki-editing
description: Edit this Git-managed LLM Wiki safely using worktrees, provenance, index/log updates, and schema review.
version: 1.0.0
author: giwa/wiki maintainers
---

# Wiki Editing Skill for giwa/wiki

Use this skill when editing, ingesting into, querying, linting, or maintaining this repository.

## Activation

Use when the task mentions this wiki, `/Users/ken/wiki`, `giwa/wiki`, source ingest, wiki pages, `SCHEMA.md`, `index.md`, `log.md`, or repository-specific wiki operations.

## Repository Facts

- Repository: `git@github.com:giwa/wiki.git`
- Stable checkout: `/Users/ken/wiki`
- Shared Hermes environment variable: `WIKI_PATH=/Users/ken/wiki`
- Worktree root: `/Users/ken/wiki-worktrees/`
- Stable main is for read/query and reviewed merges, not routine direct editing.

## Orientation

Every editing session starts by reading:

1. `SCHEMA.md`
2. `index.md`
3. recent `log.md`

Then search existing pages for the target topic to avoid duplicates.

## Worktree Workflow

```bash
cd /Users/ken/wiki
git fetch origin
git switch main
git pull --ff-only
mkdir -p /Users/ken/wiki-worktrees
git worktree add -b wiki/<task-slug> /Users/ken/wiki-worktrees/<task-slug> main
cd /Users/ken/wiki-worktrees/<task-slug>
```

Make edits in the worktree, then:

```bash
git status
git add .
git commit -m "wiki: <summary>"
git push -u origin HEAD
```

Open a PR and merge only according to the review policy.

## Ingest Workflow

1. Capture the raw source under `raw/` with raw frontmatter and sha256 when applicable.
2. Search `index.md` and wiki pages for existing related content.
3. Create/update pages only when `SCHEMA.md` thresholds are met.
4. Add cross-links using `[[wikilinks]]`.
5. Add provenance markers for multi-source synthesis.
6. Update `index.md`.
7. Append to `log.md`.
8. Commit and PR.

## Query Workflow

1. Read `index.md`.
2. Search the wiki when the topic may be broader than the index entry.
3. Read relevant pages.
4. Cite wiki pages in the answer.
5. Save substantial reusable synthesis under `queries/` or `comparisons/` only when it would be costly to rederive.
6. Log filed query pages.

## Lint Workflow

Check for:

- broken `[[wikilinks]]`
- orphan pages
- pages missing from `index.md`
- invalid frontmatter
- tags not listed in `SCHEMA.md`
- `confidence: low` and `contested: true` pages
- pages over about 200 lines
- raw source hash drift

## Review Policy

`SCHEMA.md` changes require human review before merge because they alter taxonomy, thresholds, provenance, and governance.

Automated profiles may propose schema updates in PRs, but must not directly merge schema changes without explicit user approval.

## Git LFS Policy

This repository uses Git LFS for binary and large source assets. Keep `.gitattributes` tracking at least PDFs, common image formats, audio, and video.

## Forbidden Actions

- Do not push directly to `main` from automated contexts.
- Do not edit stable main for routine content changes.
- Do not rewrite immutable raw sources.
- Do not use tags absent from `SCHEMA.md`.
- Do not add pages without `index.md` and `log.md` updates.
- Do not silently merge `SCHEMA.md` changes.
