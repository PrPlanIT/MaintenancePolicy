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

## Governed files

Governance may manage specific files in satellite repos. Each governed file has one of two behaviors:

- **authoritative** — governance defines the file; drift is replaced
- **advisory** — governance may seed or validate the file; drift is not overwritten

Which files are governed and their behavior is declared by policy, not hardcoded by filename.

Examples:
- `.stagefreight/stagefreight-managed.yml` → authoritative
- `.gitlab-ci.yml` → authoritative
- `.claude/settings.json` → authoritative
- `.stagefreight.yml` → may be authoritative or advisory depending on policy

Detaching from governance is always possible.
