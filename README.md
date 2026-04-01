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

Governance distributes files to satellite repos. Which files are governed and how drift is handled is policy-declared per file:

- **governed + replace** — machine-owned, drift is overwritten (e.g., `.stagefreight/stagefreight-managed.yml`, `.gitlab-ci.yml`)
- **governed + warn** — governance-seeded, drift emits warning but still replaces
- **local-friendly + warn** — seeded as starter, local edits tolerated
- **local-friendly + ignore** — seeded once, never touched again

`.stagefreight.yml` may itself be governance-seeded or governance-enforced when policy declares it governed. Governance ownership is policy-declared per file, not hardcoded by filename.

Detaching from governance is always possible.
