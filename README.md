# Test Strategy Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-orange.svg)](https://github.com/jovd83/test-strategy-skill)
[![AgentSkills Standard](https://img.shields.io/badge/AgentSkills-Standard-green.svg)](https://agentskills.io)
[![Validate](https://github.com/jovd83/test-strategy-skill/actions/workflows/validate.yml/badge.svg)](https://github.com/jovd83/test-strategy-skill/actions/workflows/validate.yml)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=flat&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/jovd83)

`test-strategy-skill` is an Agent Skill that produces a structured, auditable **test strategy** for a software change. It sits at phase 4 of the `test-lifecycle-skill` orchestration chain, between requirement analysis and test design.

## What It Does

Given a system under test (SUT), this skill decides — before any test case is designed — **what** to test, **how** to test it, and **what done looks like**:

- Selects test levels (unit / component / integration / contract / system / acceptance) based on SUT type and risk class
- Selects test types per level (functional, NFR, data, AI-eval, manual) using a documented rubric
- Maps each (level, type) pair to an automation lane skill — **only recommending lanes that are actually installed**
- Prioritizes scenarios deterministically via `impact × likelihood × coverage_priority`
- Splits work explicitly between automated lanes (phase 9a) and the manual track (phase 9b)
- Derives mode-aware exit criteria that downstream phases can mechanically check
- Produces a HITL gate map matching the chosen oversight mode (lite / standard / full)
- Writes both a human-readable `strategy.md` and a machine-readable `strategy.json` sidecar

## When To Use It

Use this skill whenever you need to **plan, scope, or decide what to test** — even if you don't say "strategy":

| Situation | Example prompt |
|---|---|
| New feature or change request | "Plan testing for this checkout redesign; high risk, standard mode." |
| Batch or background job | "Derive a full-mode strategy for the nightly reconciliation batch." |
| API or service | "Pick lanes and exit criteria for the new payments REST API." |
| Pre-release readiness | "What should we test before shipping the v2 auth service?" |

This skill is **not** the right tool for:
- Designing individual test cases → use `test-design-orchestrator` (phase 5)
- Analyzing requirement quality → use `test-analysis-skill` (phase 3)
- Running or authoring tests → use the execution lane skills (phases 8–10)

## Installation

Install from GitHub using the `skills` CLI:

```bash
npx skills add jovd83/test-strategy-skill
```

Or copy the folder manually into a location your agent scans:

```bash
~/.agents/skills/test-strategy-skill/
<project>/.agents/skills/test-strategy-skill/
```

## How To Use It

Invoke by name in any prompt:

```
Use $test-strategy-skill to plan testing for this webapp checkout redesign; risk is high, mode standard.
Use $test-strategy-skill to derive a Full-mode strategy for the nightly reconciliation batch.
Use $test-strategy-skill to pick lanes and exit criteria for the new payments REST API.
```

The skill consumes intake summary, HITL mode, acceptance criteria, and analysis findings. It produces `strategy.md` + `strategy.json`.

## Repository Layout

```text
test-strategy-skill/
├── SKILL.md
├── README.md
├── CHANGELOG.md
├── references/
│   ├── strategy-framework.md
│   ├── test-levels-taxonomy.md
│   ├── test-types-taxonomy.md
│   ├── lane-catalog.md
│   ├── exit-criteria-model.md
│   └── strategy-doc-schema.md
├── scripts/
│   ├── detect_lanes.py
│   ├── prioritize_scenarios.py
│   └── derive_exit_criteria.py
├── examples/
│   ├── webapp-checkout-intake.md
│   ├── webapp-checkout-strategy.md
│   ├── batch-reconciliation-intake.md
│   ├── batch-reconciliation-strategy.md
│   ├── rest-api-intake.md
│   └── rest-api-strategy.md
└── evals/
    ├── evals.json
    └── output-quality-checklist.md
```

## Scripts

All three support `--json` for machine-readable output.

### `detect_lanes.py`

Scans the runtime skills tree (`~/.agents/skills/`) and reports which mapped lane skills are present. Run this before recommending lanes.

```bash
python scripts/detect_lanes.py --json
python scripts/detect_lanes.py --root /custom/path
```

### `prioritize_scenarios.py`

Deterministic scoring (`impact × likelihood × coverage_priority`) against the allowed scales.

```bash
python scripts/prioritize_scenarios.py --input scenarios.json --top 20 --json
```

### `derive_exit_criteria.py`

Produces a mode-aware exit-criteria set and HITL gate map.

```bash
python scripts/derive_exit_criteria.py --mode standard --risk-class high --json
```

## Memory Model

Runtime-only. The skill does not persist anything across invocations. Strategy artifacts are project-local and managed by the caller.

## Standards Alignment

- ISTQB test levels and types taxonomy (adapted for modern stacks)
- IEEE 829 / ISO 29119 test plan section structure (selectively borrowed)
- Risk-based testing (impact × likelihood)
- Agent Skills progressive-disclosure model and SKILL.md frontmatter conventions

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for the full release history.

## License

[MIT](LICENSE)
