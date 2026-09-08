# AGENTS.md

Working guidelines for every agent operating in this repository.

## Platform identity

Valuora — Business Valuation Intelligence Platform for Nigerian SMEs. Currency: **NGN (₦)**.

## Core principle (non-negotiable)

**The deterministic valuation engine is the single source of truth for financial
calculation results. AI is only an interpretation / explanation layer and must
never generate valuation numbers, risk scores, or financial results.** The
frontend must never become a source of truth either.

## Personnel (department agents)

- `tech-lead` — orchestrator (primary, default)
- `valuation-engine` — deterministic financial math & valuation methods
- `backend-dept` — modular monolith, API, database, multi-tenancy
- `frontend-dept` — UI/UX implementation
- `data-methodology` — benchmarks, normalization, statistics
- `security-auditor` — security & compliance review (read-only)
- `qa-tester` — formula + regression + integration + security + frontend tests
- `docs-dept` — product documentation; strips conversational wrap-around text

## Repository layout

`01_PRODUCT → 02_METHODOLOGY → 03_TECHNICAL → 04_DESIGN → 05_ENGINEERING →
06_BUSINESS → 07_LEGAL`, plus `README.md`.

Consult the relevant spec folder before implementing or changing anything.

## Engineering rules

- **Decimal-safe arithmetic only** for authoritative monetary values (never binary float).
- Every valuation is **reproducible**: inputs snapshot + benchmark snapshot +
  methodology version + engine version. Store the engine (`05_ENGINEERING/VALUATION_ENGINE.md`).
- **Completed valuations are immutable**; new financial data yields a NEW snapshot, never a silent mutation.
- **Multi-tenancy**: verify `businessId` authorization server-side.
- **Financial formulas have the highest test priority**; every formula needs
  normal / zero / boundary / invalid / precision / regression cases.
- **Writer**: never commit secrets or keys; never log them.
- The **frontend never reconstructs financial formulas**; it displays authoritative
  backend results. AI explanations are visually separated from results.

## Documentation conventions

- Pure content documents: **no AI-style wrap-around text** at the start or end.
- Preserve the numbered-folder order and core valuation principles in every doc.
- Legal docs (`07_LEGAL/`) are marked "Draft — For Legal Review".

## Verification

Never claim a test passed unless you actually ran it. Run the project's test and
lint/typecheck commands and report real results.