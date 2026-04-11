# CLAUDE.md — gt-toolkit (pae23 fork)

Personal fork of [Xexr/gt-toolkit](https://github.com/Xexr/gt-toolkit). A daily
GitHub Action (`.github/workflows/sync-upstream.yml`) pulls updates from
upstream into `main`.

## Installing formulas into the local Gas Town instance

The formulas in `formulas/` are **not** embedded in the `gt` binary, so
`gt upgrade` will not provision them. They must be copied manually into the
town-level formulas directory:

```bash
cp formulas/*.formula.toml "$GT_ROOT/.beads/formulas/"
```

On this machine `$GT_ROOT` resolves to `/Users/pa/gt`, so the concrete target
is `/Users/pa/gt/.beads/formulas/`. That path is search path #3
(orchestrator scope) for `gt formula list` — formulas dropped there are
available to every rig in the town.

After copying, verify with:

```bash
gt formula list | grep -E '(spec|plan|beads)-workflow'
```

You should see `spec-workflow`, `plan-workflow`, `beads-workflow` plus the
eight `*-expansion` formulas.

## When to re-copy

Re-run the `cp` command after **any** of these events:

- A daily upstream sync landed new commits in `formulas/` (check
  `git log --oneline formulas/`)
- You edited a formula in this repo and want to test it locally
- `gt formula list` is missing one of the expected formulas

The copy is idempotent — existing files are overwritten.

## Registry interaction (`.installed.json`)

`$GT_ROOT/.beads/formulas/.installed.json` is the registry that `gt upgrade`
uses to track formulas embedded in the `gt` binary. Our formulas land in
the same directory but are **not** referenced in that file. `gt` classifies
them as `untracked` in `CheckFormulaHealth` (see
`gastown/mayor/rig/internal/formula/embed.go:261`), which means they are:

- **Safe to overwrite** by a future `gt upgrade` *only* if a formula with the
  same filename becomes embedded upstream. That would silently replace our
  version. If that happens, diff and decide.
- **Never auto-updated** from this repo — the `cp` step is the only mechanism.

Do **not** add entries to `.installed.json` by hand. The file is rewritten by
`gt upgrade` based on embedded content hashes; manual entries would be dropped.

## Why not embed instead

Embedding these formulas in the `gt` binary would require forking
`gastownhall/gastown` (or `steveyegge/gastown`), copying the TOMLs into
`internal/formula/formulas/`, and rebuilding with `g:gt-build`. That path is
documented if you want registry tracking, but the `cp`-from-fork approach is
simpler and keeps this repo as the single source of truth for the pipeline
formulas.
