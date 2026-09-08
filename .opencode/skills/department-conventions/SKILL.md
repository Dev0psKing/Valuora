---
name: department-conventions
description: "Repo-wide working conventions for the Valuora departments. Use when starting any task in this repo to align on the department model, the numbered-folder spec layout, the deterministic-engine source-of-truth principle, and the verification requirement."
---

# Department working conventions

Load this when starting work in this repository, regardless of which department
agent you are.

## Department model

- `tech-lead` orchestrates and delegates; it does not implement departments' work by itself.
- Specialists: `valuation-engine`, `backend-dept`, `frontend-dept`,
  `data-methodology`, `security-auditor` (read-only), `qa-tester`, `docs-dept`.

## Source of truth

- The deterministic valuation engine produces financial results; AI only explains.
- The frontend is display only — never reconstruct formulas, never be authoritative.
- Financial calculations use decimal-safe arithmetic, never binary float.

## Spec layout

Read the relevant numbered folder before implementing:

`01_PRODUCT → 02_METHODOLOGY → 03_TECHNICAL → 04_DESIGN → 05_ENGINEERING →
06_BUSINESS → 07_LEGAL`

## Verification

Run the actual tests/lint/typecheck. Never claim a test passed unless you ran it.