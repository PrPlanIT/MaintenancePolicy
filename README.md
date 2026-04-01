# MaintenancePolicy

Central maintenance policy for PrPlanIT repositories. Enforced by [StageFreight](https://github.com/PrPlanIT/StageFreight).

## How it works

This repo IS a StageFreight-managed repo. Its `.stagefreight.yml` declares governance clusters — groups of repos that share lifecycle policy. When CI runs, `stagefreight governance reconcile` resolves presets, generates managed configs, and commits directly to satellite repos.

## Structure

- `preset/` — section-scoped config fragments (one top-level StageFreight key per file)
- `claude-code/` — Claude Code project settings distributed to governed repos
- `docs/` — documentation and rationale
- `precommit/` — pre-commit hook configs (future)
- `renovate/` — Renovate bot configs (future)

## Two-file model

Each governed repo gets:
- `.stagefreight/stagefreight-managed.yml` — machine-owned, generated from this policy repo
- `.stagefreight.yml` — human-authored, local intent and overrides (never touched by reconciler)

Local always wins. Detaching from governance is always possible.
