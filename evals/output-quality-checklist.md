# Output Quality Checklist

Use this when forward-testing the skill or grading a generated strategy doc.

## Scope Discipline

- The strategy stays at planning level. It does not author test cases or run tests.
- The sections present match the SKILL.md output contract (1–11).
- The sidecar `strategy.json` is present and structurally valid.

## Lane Discipline

- Every recommended lane appears in the `detect_lanes.py` output (or `available_lanes` override).
- Lanes mapped but missing are surfaced as `unavailable` and flagged in Open Questions, not silently dropped.
- Stack-specific overrides applied (e.g., `junit5-skill` for Java SUTs, `playwright-skill` for Next.js).

## Decision Citation

- Every in-scope level cites its driver (intake field, AC ID, or analysis finding ID).
- Every in-scope type cites its driver.
- Lane choices reference detector output and/or stack detection.

## Scoring Integrity

- Scenario scores come from `prioritize_scenarios.py` with values from allowed scales (1/2/4/8/16 × 1–5 × 1–3).
- Categories assigned from the documented thresholds (1–8 Low, 9–32 Medium, 33–96 High, 97–240 Critical).
- No free-text "feels-like-a-high" scoring.

## Exit Criteria Quality

- Thresholds are numeric or boolean, never adjectives.
- Mode-driven baseline applied per `exit-criteria-model.md`.
- Risk-class adjustments applied (critical bumps thresholds; high forces P1=100%; low can relax P2 in Lite).
- `derive_exit_criteria.py` was the source.

## HITL Gates

- Active gates match the chosen mode (lite: 13 only; standard: 4+8+13; full: 4+8+9+12+13).
- Mode/risk mismatch flagged when present (warn or block).

## Manual Track

- Manual routing list is explicit (may be empty), not silently omitted.
- Inherently manual types (UAT, exploratory, OAT, doc review, usability) are routed correctly.

## Stakeholder Usefulness

- Executive summary is specific to the SUT, not a generic template.
- Open Questions are direct, blocker-flagged where applicable, and owner-assigned where known.
- A reviewer can pass the phase-4 HITL gate using only the strategy doc.

## Audit Safety

- The skill does not invent missing inputs. If intake risk_class or SUT type was missing, the skill asked.
- The skill does not promote scope beyond what intake authorized without flagging it.
- The skill writes only `strategy.md` and `strategy.json` (plus optional `scenarios.json` if it sourced one).
