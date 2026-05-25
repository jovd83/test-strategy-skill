# Changelog

All notable changes to the `test-strategy-skill` repository are documented in this file.

The format follows Keep a Changelog with lightweight `Added`, `Changed`, and `Fixed` sections.

## [1.0.0] - 2026-05-25

### Added
- Initial release of `test-strategy-skill` as phase 4 of the `test-lifecycle-skill` orchestration chain.
- `SKILL.md` with dispatcher metadata, input/output contracts, 10-step working method, guardrails, and gotchas.
- Reference set:
  - `references/strategy-framework.md` — core methodology and rubrics
  - `references/test-levels-taxonomy.md` — ISTQB-aligned level definitions and coverage matrix
  - `references/test-types-taxonomy.md` — functional, NFR, data, AI, and manual families
  - `references/lane-catalog.md` — canonical type→lane mapping
  - `references/exit-criteria-model.md` — mode-aware exit gates and HITL gate map
  - `references/strategy-doc-schema.md` — JSON sidecar schema
- Helper scripts:
  - `scripts/detect_lanes.py` — runtime-tree lane discovery
  - `scripts/prioritize_scenarios.py` — deterministic scenario scoring
  - `scripts/derive_exit_criteria.py` — mode + risk-class → exit criteria + HITL gates
- Paired examples for three SUT shapes:
  - `examples/webapp-checkout-*` (webapp, high risk, standard mode)
  - `examples/batch-reconciliation-*` (batch, critical risk, full mode)
  - `examples/rest-api-*` (API, high risk, standard mode)
- Evaluation assets:
  - `evals/evals.json` — eval prompts
  - `evals/output-quality-checklist.md` — reviewer checklist for forward-testing
- `README.md` with badges (License, Version, AgentSkills Standard, CI validation, Buy Me a Coffee), "What It Does", "When To Use It", and `npx skills` install instructions
- `.github/workflows/validate.yml` — CI workflow that syntax-checks scripts, validates SKILL.md frontmatter, and confirms example files are present
