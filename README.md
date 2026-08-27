<h1 align="center">Maintenance Policy</h1>

<p align="center"><em>One repo that remembers, so the others don't have to.</em></p>

<p align="center">
<!-- sf:badges:start -->
[![license](https://raw.githubusercontent.com/PrPlanIT/MaintenancePolicy/main/.stagefreight/scribe/license.svg)](https://github.com/PrPlanIT/MaintenancePolicy/blob/main/LICENSE) ![updated](https://raw.githubusercontent.com/PrPlanIT/MaintenancePolicy/main/.stagefreight/scribe/updated.svg) [![governed by](https://img.shields.io/badge/governed_by-StageFreight-310937?logo=readthedocs&logoColor=white)](https://gitlab.prplanit.com/PrPlanIT/StageFreight)
<!-- sf:badges:end -->
</p>

<p align="center">
<!-- sf:project:start -->
[![GitLab](https://img.shields.io/badge/GitLab-source-FC6D26?logo=gitlab)](https://gitlab.prplanit.com/PrPlanIT/MaintenancePolicy) [![GitHub](https://img.shields.io/badge/GitHub-mirror-181717?logo=github)](https://github.com/PrPlanIT/MaintenancePolicy) [![Last Commit](https://img.shields.io/github/last-commit/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/commits) [![Contributors](https://img.shields.io/github/contributors/PrPlanIT/MaintenancePolicy)](https://github.com/PrPlanIT/MaintenancePolicy/graphs/contributors)
<!-- sf:project:end -->
</p>

---

**MaintenancePolicy** is the [StageFreight](https://gitlab.prplanit.com/PrPlanIT/StageFreight) **governance control repo** for the PrPlanIT / HomeLabHD fleet. It is the one place that owns three things every managed repo would otherwise duplicate and drift on:

- **Identity** — who owns a repo (`orgs:`) and how it presents itself (`metadata:` — title, description, topics, license).
- **Policy** — the shared CI lifecycle (`preset/` fragments: build, lint, security, release, scribe, …).
- **Catalog** — the inventory of governed repos, each anchored on its forge location.

When this repo's pipeline runs, `stagefreight governance reconcile` resolves those presets, stamps each governed repo's identity, and commits the managed config back into the satellite — so a fleet-wide change is one commit here, not *N* commits everywhere.

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

<p align="center"><sub>Part of the <strong>PrPlanIT</strong> fleet · governed by <a href="https://gitlab.prplanit.com/PrPlanIT/StageFreight">StageFreight</a> · maintained at <a href="https://gitlab.prplanit.com/PrPlanIT/MaintenancePolicy">gitlab.prplanit.com</a></sub></p>
