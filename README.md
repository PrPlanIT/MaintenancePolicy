# MaintenancePolicy

Central maintenance policy for PrPlanIT repositories. Defines lifecycle doctrine enforced by [StageFreight](https://github.com/PrPlanIT/StageFreight).

## Architecture

**Two-file model:** Each governed repo has:
- `.stagefreight/stagefreight-managed.yml` — machine-owned, generated from this policy repo
- `.stagefreight.yml` — human-authored, local intent and overrides

Local always wins. Managed config is replaceable. Detaching from governance is always possible.

## Structure

```
preset/          Reusable config section fragments
governance/      Cluster definitions + repo targeting
policies/        Rationale docs (why these defaults exist)
examples/        Minimal .stagefreight.yml examples
index.yml        Preset path → description mapping
```

## Presets

Section-scoped fragments imported via `preset:` in `.stagefreight.yml`:

| Preset | Scope |
|--------|-------|
| `docker.yml` | Docker registries, dev/stable channels, badges |
| `binary.yml` | Binary archives, dev/stable channels |
| `security.yml` | Security scanning (sbom, full detail) |
| `dependency.yml` | Dependency updates (auto-commit) |
| `docs.yml` | Docs generation (badges, narrator, auto-commit) |
| `commit.yml` | Conventional commits, forge backend |
| `release.yml` | Release policy (security summary, links) |
| `lint-full.yml` | Full lint (all standard modules) |

## Governance

Clusters assign lifecycle doctrine to groups of repos. See `governance/clusters.yml`.

The reconciler generates `.stagefreight/stagefreight-managed.yml` for each targeted repo and commits directly. It never touches the human-authored `.stagefreight.yml`.

## Principles

- `.stagefreight.yml` is the only config grammar
- Local config always overrides managed config
- Repos are multi-capability, never single-class
- Capability detection is runtime inference (StageFreight), not policy
- Governance commits directly — no PRs
- Detaching from governance is always reversible
