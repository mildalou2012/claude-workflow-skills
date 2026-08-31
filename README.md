# Claude Product Workflow Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **English**: This file · **中文**: [README.zh-CN.md](README.zh-CN.md)

A skill suite for Claude Code that drives the full product development loop: **requirement interview → feature spec (with acceptance criteria) → system audit (dual-score report)**.

Built for product managers and solo developers: non-programmers can drive AI through "idea → testable plan → measurable health check" using plain language.

## Prerequisites

- [Claude Code](https://claude.com/claude-code) (any recent version)
- Optional: `grilling` skill for the requirement interview step (see [Third-party dependencies](#third-party-dependencies-not-our-assets--bundled-for-integration-only))

## What's included

### First-party skills (MIT, maintained by us)

| Skill | Role | Output |
|---|---|---|
| `project-flow` | Project lifecycle conductor (orchestration layer) | Detects project stage, routes to the tools below |
| `feature-spec` | Feature spec generator | `docs/feature-spec.md`: system positioning / feature modules / per-feature prerequisites, output, effect, test method, acceptance criteria |
| `system-audit` | System inspector | Dual-score report (Health Score + Completion Rate), 14 categories / 76 checklist items, 5 stage-based templates, archived history for trend comparison |

### Third-party dependencies (NOT our assets — bundled for integration only)

The following skills are **not developed by us**. We bundle them for distribution and invoke them via Claude Code's Skill mechanism. Full source info lives in each directory's `SOURCE.md`:

| Skill | Role | Source | Version | Updated |
|---|---|---|---|---|
| `grilling` | Requirement interview engine | Third-party open source (original repo to be traced) | n/a | Installed 2026-08; description localized to Chinese 2026-09-01 |
| `grill-me` | Interview entry (dispatcher) | Third-party open source (original repo to be traced) | n/a | Installed 2026-08; description localized 2026-09-01 |
| `neat-freak` | Knowledge closeout | Third-party open source (original repo to be traced) | 3.0.0 | Installed 2026-08-30; description localized 2026-09-01 |

> **Ownership notice**: code and design of `grilling` / `grill-me` / `neat-freak` belong to their original authors. This repo only: attributes sources, distributes them as-is (with local description changes documented), and invokes them from `project-flow`. If you see this project claiming authorship of them elsewhere, that is inaccurate.

## Installation

Copy the directories under `skills/` into your Claude Code skills directory (install all 6, or only the first-party three — `project-flow`'s interview step depends on `grilling`; without it that step is unavailable, everything else still works):

```bash
# macOS / Linux
cp -r skills/* ~/.claude/skills/

# Windows
# Copy all folders under skills/ to C:\Users\<you>\.claude\skills\
```

## Usage (plain language, no commands to memorize)

| What you want | Say to Claude |
|---|---|
| Clarify an idea before building | "Interview me about X" |
| Generate a feature plan | "Create a feature spec" |
| Check how complete a system is | "Audit X" / "How complete is X?" |
| Not sure what's next | "What's next for X?" (triggers project-flow auto-routing) |

### The standard loop

```
grill-me/grilling (requirement interview) → feature-spec (generate spec) → develop → system-audit (inspect)
```

1. **Interview**: "Interview me about X" → relentless product-manager-style questioning until the direction is clear
2. **Feature spec**: "Create a feature spec" → structured, testable checklist (system positioning / modules / per-feature: prerequisites · output · effect · test method · expected result), saved to `docs/feature-spec.md`
3. **Audit**: "Audit X" → 76 items across 14 categories + completion tracking (implementation **and** acceptance criteria), dual-score report archived; next audit automatically compares

### system-audit stage templates

| Template | Use |
|---|---|
| `full` | First deep inspection, before/after major refactors |
| `core` | 10-minute quick check at any stage |
| `pre-launch` | Pre-launch checklist — all ❌ must be cleared |
| `operation` | Quarterly health check for live systems |
| `outsource-acceptance` | Vendor delivery acceptance (flags out-of-scope development) |

## Language

- **Output language follows your question**: ask in Chinese → output in Chinese; ask in English → output in English. No configuration needed
- Public-facing descriptions are in English; maintainers use a Chinese-localized version locally — same behavior
- Third-party `grilling` output follows your question too (not our asset)

## Quick validation

After installing, verify the skills are loaded — in a Claude Code session say:

```
What's next for this project?
```

or simply ask "audit this system" in any project folder. If `project-flow` replies with stage routing questions (which project, what do you want to do), everything works.

## Example output

`system-audit` produces a dual-score report, archived for trend comparison:

```markdown
# System Audit Report: <system>
Date: 2026-09-01 | Template: core | Method: code inspection + Q&A

## Dual-score overview
- 🟡 Health Score: 75 (Good)
- Feature Completion: 88.5% (10✅ / 3🟡 / 0❌ of 13 planned features)

## Category scores (sorted, weakest 3 highlighted)
## Feature completion matrix
## Findings with evidence
## Priority todo (fix ❌ first, then ⚠️)
## Comparison with previous audit
```

`feature-spec` produces the planning baseline that makes the completion score trustworthy:

```markdown
# <System> Feature Spec
## 1. System positioning   (what it is / where it runs / what it does NOT do)
## 2. Feature module overview (P0/P1/P2)
## 3. Module details — per feature:
   - Description / Prerequisites / Output / Effect
   - Test method / Expected result (acceptance criteria)
## 4. Open items & risks
```

## Design principles

- **Evidence discipline**: every checklist item must cite evidence (files / commands / screenshots); no evidence → ⚠️ "not found", never guess ✅
- **Acceptance criteria gate**: ✅ requires implementation **and** passing acceptance criteria
- **No fabrication**: missing planning baseline → completion marked "cannot evaluate", prompting you to build a feature spec first
- **Trail**: audits archived under `~/.claude/skill-records/system-audit/<system>/`, forming a maturity curve

## Directory structure

```
claude-workflow-skills/
├── README.md                 # English
├── README.zh-CN.md           # 中文
├── LICENSE                   # MIT (first-party skills only; third-party copyrights remain with their authors)
└── skills/
    ├── project-flow/         # First-party · orchestration
    ├── feature-spec/         # First-party · spec generator
    ├── system-audit/         # First-party · inspector
    ├── grilling/             # Third-party · interview engine (see SOURCE.md)
    ├── grill-me/             # Third-party · interview entry (see SOURCE.md)
    └── neat-freak/           # Third-party · knowledge closeout (see SOURCE.md)
```

## License

MIT License — use, modify, and contribute via Issues / PRs.

## Feedback

I've tried this suite briefly on my own small projects — not deeply validated yet, so your mileage may vary. Feedback is very welcome on: checklist coverage, scoring methodology, template structure, UX. Open an Issue or PR!

## Maintainer

阿楼 (mildalou2012) — product manager, not a programmer; built this to run AI-assisted product development.
