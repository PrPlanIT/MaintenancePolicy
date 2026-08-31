# Maintenance Policy

> *One repo that remembers, so the others don't have to.*

The [StageFreight](https://gitlab.prplanit.com/PrPlanIT/StageFreight) **governance control repo** for the PrPlanIT / HomeLabHD fleet — the one place that owns the three things every managed repo would otherwise duplicate and drift on: **identity** (`orgs:` + `metadata:`), **policy** (the shared `preset/` CI lifecycle), and the **catalog** of governed repos. When its pipeline runs, `stagefreight governance reconcile` resolves the presets, stamps each repo's identity, and commits the managed config back into the satellite — a fleet-wide change is one commit here, not *N* commits everywhere.

<!-- sf:project:start -->
[![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/PrPlanIT/MaintenancePolicy) [![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/PrPlanIT/MaintenancePolicy) [![license](https://raw.githubusercontent.com/PrPlanIT/MaintenancePolicy/main/.stagefreight/scribe/license.svg)](https://github.com/PrPlanIT/MaintenancePolicy/blob/main/LICENSE) [![Open Issues](https://img.shields.io/github/issues/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/issues) [![Open PRs](https://img.shields.io/github/issues-pr/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/pulls) [![Contributors](https://img.shields.io/github/contributors/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/graphs/contributors) [![donate](https://img.shields.io/badge/donate-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/T6T41IT163) [![sponsor](https://img.shields.io/badge/sponsor-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/PrPlanIT)
<!-- sf:project:end -->
<!-- sf:badges:start -->
[![pipeline](https://raw.githubusercontent.com/PrPlanIT/MaintenancePolicy/main/.stagefreight/scribe/pipeline.svg)](https://gitlab.prplanit.com/PrPlanIT/MaintenancePolicy/-/pipelines) [![Last Commit](https://img.shields.io/github/last-commit/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/commits) [![StageFreight](https://img.shields.io/badge/StageFreight-0.10.0-310937?logo=readthedocs&logoColor=white)](https://stagefreight.prplanit.com)
<!-- sf:badges:end -->

## How it works

This repo *is* a StageFreight-managed repo — it runs in **governance mode**. Governance is presence-gated: declaring `governance.profiles` in [`.stagefreight.yml`](.stagefreight.yml) is what activates it; the mode selects this repo's own phase behavior.

```
  MaintenancePolicy (control repo)          satellite repo
  ┌────────────────────────────┐            ┌───────────────────────────┐
  │ orgs:      identity         │  reconcile │ .stagefreight.yml          │
  │ metadata:  branding         │ ─────────▶ │   (managed, sealed)        │
  │ preset/*:  shared policy     │  (publish) │ .stagefreight/preset-cache │
  │ governance.profiles: who    │            │ metadata: (distributed)    │
  └────────────────────────────┘            └───────────────────────────┘
```

The **plan** is previewed in `perform` (no writes); the **apply** — committing to each satellite's forge — runs in `publish`, the phase that owns forge mutation, and only from an accepted commit on the default branch.

## The catalog

Each entry under `governance.profiles.<profile>.repos` is anchored on the repo's forge **location** — org and slug derive from it. A *branded* entry (with `title`/`description`/`topics`/`license`) governs that repo's identity wholesale; a bare-string entry governs CI only and leaves identity to the repo.

| Kind | Governs | Identity author |
| --- | --- | --- |
| **Branded entry** (`at:` + branding) | CI policy **and** identity | the catalog |
| **Location-only** (bare string) | CI policy only | the repo itself |

## Structure

| Path | What it holds |
| --- | --- |
| `.stagefreight.yml` | This repo's own identity + the governance catalog |
| `preset/` | Section-scoped policy fragments (one top-level key per file), distributed to satellites |
| `docs/` | Rationale and reference |
| `claude-code/` | Claude Code project settings distributed to governed repos |

## Governed files

Governance manages specific files in satellite repos, each with one of two behaviors:

- **authoritative** — governance defines the file; drift is replaced.
- **advisory** — governance may seed or validate the file; drift is not overwritten.

Which files are governed, and how, is declared by policy — never hardcoded by filename. Detaching from governance is always possible.

---

Part of the **PrPlanIT** fleet · governed by [StageFreight](https://gitlab.prplanit.com/PrPlanIT/StageFreight)
