# Claude Code Instructions for giwa/wiki

This repository is a Git-managed LLM Wiki. Follow these rules whenever Claude Code edits it.

## Before Editing

1. Read `SCHEMA.md`.
2. Read `index.md`.
3. Read the recent entries in `log.md`.
4. Search existing pages for the topic/entity/concept you are about to edit.
5. Work from a branch/worktree, not directly on stable main.

Stable checkout:

```text
/Users/ken/wiki
```

Recommended worktree root:

```text
/Users/ken/wiki-worktrees/
```

## Worktree Flow

```bash
cd /Users/ken/wiki
git fetch origin
git switch main
git pull --ff-only
mkdir -p /Users/ken/wiki-worktrees
git worktree add -b wiki/<task-slug> /Users/ken/wiki-worktrees/<task-slug> main
cd /Users/ken/wiki-worktrees/<task-slug>
```

Commit from the worktree:

```bash
git status
git add .
git commit -m "wiki: <summary>"
git push -u origin HEAD
```

Then open a PR.

## During Editing

- Keep `raw/` immutable after source capture.
- Use YAML frontmatter on wiki pages.
- Add meaningful `[[wikilinks]]`.
- Update `index.md` for every new page.
- Append to `log.md` for every content-changing action.
- Follow the tag taxonomy in `SCHEMA.md`.
- Mark weak or disputed claims with `confidence`, `contested`, or `contradictions` fields.

## Before Commit

Check:

- `git status`
- new pages are listed in `index.md`
- `log.md` has an entry
- tags are allowed by `SCHEMA.md`
- no large binary assets bypass Git LFS
- no accidental edits were made to immutable raw source files

## Human Review Required

Always require explicit human review before merging changes to:

- `SCHEMA.md`
- tag taxonomy
- source/provenance policy
- page creation thresholds
- automated write policies

## Never Do

- Do not push directly to `main` from an automated context.
- Do not silently change `SCHEMA.md` and merge it.
- Do not create duplicate pages without searching first.
- Do not remove historical log entries.
- Do not rewrite raw sources to fit a synthesis.
