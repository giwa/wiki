# Agent Instructions for giwa/wiki

This repository is a Git-managed LLM Wiki. It is intended to be edited by humans and agents, including Hermes, Codex, and Claude Code.

## Repository Role

- `/Users/ken/wiki` is the stable main checkout.
- `WIKI_PATH` should point to `/Users/ken/wiki` across Hermes profiles.
- The wiki follows the conventions in `SCHEMA.md`.
- `index.md` is the navigation catalog.
- `log.md` is append-only history.
- `raw/` contains immutable source material.

## Mandatory Orientation Before Editing

Before creating or modifying wiki content, read:

1. `SCHEMA.md`
2. `index.md`
3. the recent tail of `log.md`

For large changes, also search existing pages to avoid duplicates.

## Git Workflow

Do not edit stable main directly for normal writing tasks.

Use a branch/worktree workflow:

```bash
cd /Users/ken/wiki
git fetch origin
git switch main
git pull --ff-only
mkdir -p /Users/ken/wiki-worktrees
git worktree add -b wiki/<task-slug> /Users/ken/wiki-worktrees/<task-slug> main
cd /Users/ken/wiki-worktrees/<task-slug>
```

After editing:

```bash
git status
git add .
git commit -m "wiki: <summary>"
git push -u origin HEAD
```

Open a PR and merge only after review/approval expectations are satisfied.

## Main Checkout Policy

- Treat `/Users/ken/wiki` as stable main.
- Reading/querying from stable main is allowed.
- Direct edits to stable main are allowed only for explicit bootstrap or emergency maintenance directed by the user.
- Automated profiles, cron jobs, and gateway-triggered runs must not push directly to `main`.

## Wiki Editing Rules

When ingesting a source:

1. Save the raw source under `raw/` with source frontmatter and body hash where applicable.
2. Search existing pages before creating new ones.
3. Create or update entity/concept/comparison/query pages according to `SCHEMA.md` thresholds.
4. Use `[[wikilinks]]` for meaningful relationships.
5. Update `index.md` for every new page.
6. Append a `log.md` entry for every content-changing action.
7. Preserve provenance and confidence signals.

## SCHEMA.md Review Policy

`SCHEMA.md` defines domain, taxonomy, thresholds, and governance. Changes require human review.

Agents may propose changes on a branch/PR, but must not silently merge schema changes without explicit user approval.

## Git LFS Policy

This repository uses Git LFS for large/binary assets. Track at least:

- `*.pdf`
- `*.png`, `*.jpg`, `*.jpeg`, `*.webp`, `*.gif`
- `*.mp3`, `*.wav`
- `*.mp4`, `*.mov`

Do not bypass LFS for large raw assets.

## Never Do

- Do not overwrite or rewrite `raw/` source files after ingest.
- Do not create duplicate pages without checking `index.md` and searching the wiki.
- Do not use tags that are absent from `SCHEMA.md`.
- Do not update pages without bumping `updated` dates.
- Do not add pages without updating `index.md`.
- Do not make content-changing edits without appending to `log.md`.
- Do not push directly to `main` from automated contexts.
