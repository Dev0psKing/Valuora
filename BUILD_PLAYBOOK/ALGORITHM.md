# Valuora Build Playbook — Step-by-Step Algorithm

**Purpose:** The exact sequence of triggers, departments, and prompts needed to build
Valuora from the `01_PRODUCT` vision through to a running product.

**How to use this document:** You are the boss. You only ever:

1. **Open an opencode session in the `Valuora` folder** (agents load from `.opencode/`).
   ```
   cd C:\Users\USER\Documents\Valuora
   opencode
   ```
   `tech-lead` is your **default agent** — there is no trigger command to type; you
   are already talking to it the moment the session starts.
2. **Paste ONE prompt** to `tech-lead` for the step you want. Use the exact copy-paste
   prompts in `ONE_PAGE_TRIGGERS.md` (that file is the complete working script from
   Global Setup to a finished, running product). You can also just say:
   *"do the [step name] from BUILD_PLAYBOOK/ONE_PAGE_TRIGGERS.md"*.
3. **Wait** — `tech-lead` reads the relevant spec and delegates to subagents
   (`backend-dept`, `valuation-engine`, `frontend-dept`, `data-methodology`,
   `qa-tester`, `security-auditor`, `docs-dept`).
4. **Verify** the phase passed its gates (tests run, security reviewed) before the next phase.

You do **not** assign departments per folder. `tech-lead` does that. This document
tells you *what mission to give tech-lead at each step* and *what gate must pass*
before moving on. If you only want the runnable prompts (not the analysis), use
`ONE_PAGE_TRIGGERS.md` directly — it is the shortest path from setup to completion.

---

## The Big Rule (override everything else)

> **The deterministic valuation engine is the single source of truth for financial
> calculation results. AI only explains; it never computes valuation numbers.**
> The frontend is display-only. Financial formulas are tested before any release.
> And financial correctness is a **release requirement**, not a nice-to-have
> (`05_ENGINEERING/TESTING_STRATEGY.md` §21).

---

## GLOBAL SETUP (run once)

| Step | You say to tech-lead | Department(s) involved | Gate before next |
|------|----------------------|------------------------|------------------|
| 0.1 | "Set up the project foundation: pick the stack from `01_PRODUCT/DEVELOPMENT_ROADMAP.md` (Next.js + React + TS + Tailwind frontend, Node/TS modular-monolith backend, PostgreSQL, decimal-safe math, Jest, standard tooling). Initialise the repo structure: a monorepo with `backend/`, `frontend/`, and `packages/` for the shared valuation engine. Add lint, typecheck, and a CI pipeline (lint → typecheck → unit → integration → build → security) per `05_ENGINEERING/TESTING_STRATEGY.md` §20. Commit." | tech-lead + backend-dept + docs-dept | `npm run lint` and `npm run typecheck` pass on an empty-but-wired skeleton. |

---

## PHASE A — FOUNDATION + SHARED VALUATION ENGINE (the honest core)

MVP-scope math only: arithmetic, percentages, ratios, growth rates, weighted
averages, revenue & EBITDA multiples, enterprise/equity value, basic risk weighting,
business-health score, confidence score. **No DCF, no asset method yet** (see
`02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` priority matrix).

| Step | You say to tech-lead | Department(s) | Gate |
|------|----------------------|---------------|------|
| A.1 | "Create the shared, framework-independent valuation-engine package implementing the MVP math exactly per `02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` and `05_ENGINEERING/VALUATION_ENGINE.md`: financial metrics (revenue growth, CAGR, gross/EBITDA/net margins), revenue-multiple method, EBITDA-multiple method, normalized-EBITDA adjustments (explicit, never invented), method weighting that sums to 1, enterprise value, equity value, valuation range, risk score, confidence score. Use decimal-safe arithmetic only (never binary float). Make it 100% deterministic, versioned (`methodologyVersion`, `engineVersion`), and emit an explanation trace per §23. Keep it frameless so backend and future Python both use it." | tech-lead → valuation-engine (+ data-methodology for scoring review) | **Financial release gate** (`TESTING_STRATEGY.md` §18): all formula tests pass. |
| A.2 | "Write the complete test suite for the valuation engine per `05_ENGINEERING/TESTING_STRATEGY.md` §3–§6 and `02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` §6. Every formula needs normal, zero, boundary, invalid, large, and precision cases. Include the standard examples (revenue growth 100→120 = 20%; EBITDA 20M×3.5 = 70M; EV 100M + cash 10M − debt 20M = equity 90M). Add property tests (weights sum to 1; weighted value within method bounds). Add a regression fixture suite of known valuation cases (§6) that any methodology change must explicitly update." | tech-lead → qa-tester (+ valuation-engine to fix any failing formula) | 100% of formula tests pass; regression suite committed. QA reports real pass/fail. |
| A.3 | "Run `security-auditor` on the valuation engine package: confirm it has no I/O, no SQL, no secrets, no AI dependency, and that determinism is guaranteed (same inputs+benchmarks+versions ⇒ same output). Confirm it emits full explanation traces and cannot produce a negative enterprise value without an explicitly permitted methodology." | tech-lead → security-auditor (read-only) | No Critical/High findings. Any findings routed back to backend/valuation. |

**Phase A done when:** the shared engine is pure, deterministic, fully tested, and
security-clean. This is the crown jewel — do not proceed until A.1–A.3 are green.

---

## PHASE B — BACKEND (modular monolith wrapping the engine)

| Step | You say to tech-lead | Department(s) | Gate |
|------|----------------------|---------------|------|
| B.1 | "Build the backend modular monolith exactly per `05_ENGINEERING/BACKEND_ARCHITECTURE.md`: `backend/src` with modules `auth, users, businesses, financials, metrics, risk, valuation, benchmarks, scenarios, recommendations, reports, ai, audit, shared`. Each module is controller/service/repository/domain/validation/tests. Controllers must NOT contain financial formulas — business logic lives in services/domain, and the valuation module calls the shared valuation engine package, never recomputing math. Wire PostgreSQL with parameterized queries/ORM (never raw string SQL). Add env-config via secrets manager; never commit secrets. Add `GET /health`." | tech-lead → backend-dept (+ valuation-engine to integrate the engine's API) | Backend boots; health endpoint returns OK; no financial formulas in controllers. |
| B.2 | "Implement authentication + users + role-based access per `BACKEND_ARCHITECTURE.md` §15: verified email, secure password hashing (Argon2id/bcrypt), OTP reset, session/JWT with short lifetime, roles OWNER/ADMIN/ANALYST/ADVISOR/INVESTOR/SUPER_ADMIN, server-side authorization on every request. Build the businesses module (create/read/update business profile) and financials module (revenue, COGS, EBITDA, net income, debt, cash, assets, liabilities). Implement multi-tenancy: every business-scoped endpoint verifies the authenticated user owns/accesses the `businessId` — never trust client-supplied IDs." | tech-lead → backend-dept | Auth + multi-tenancy integration tests pass (`TESTING_STRATEGY.md` §7–8). |
| B.3 | "Run `security-auditor` on auth, users, businesses, financials, and multi-tenancy: IDOR checks, RBAC, password hashing, session/JWT security, injection safety, and tenant isolation (User A must never reach Business B's data)." | tech-lead → security-auditor | No Critical/High. |
| B.4 | "Implement the valuation service and API: `POST /valuations` that validates inputs, loads business + financials + approved benchmarks, calls the shared valuation engine (methods, adjustments, weighting, range, confidence), and persists a COMPLETE transactional snapshot (valuation + method results + adjustments + audit record) per `CALCULATION_PIPELINE.md`. Completed valuations are immutable and reproducible — never silently mutate. Support idempotency. Expose `GET /valuations/:id` returning the engine's JSON output (§22 of VALUATION_ENGINE) plus the explanation trace." | tech-lead → backend-dept + valuation-engine | End-to-end valuation integration test passes; snapshot persisted; re-run with same inputs reproduces identical result. |
| B.5 | "Run `security-auditor` on the valuation service and API: transactional integrity, immutability of completed valuations, idempotency, tenant isolation on valuation read/write, audit logging completeness." | tech-lead → security-auditor | No Critical/High. QA confirms financial integrity tests (§16). |

**Phase B done when:** an authenticated user can create a business, enter financials,
and receive a reproducible, audit-trailed valuation through the API, with tenant
isolation and security verified.

---

## PHASE C — FRONTEND (display-only UI)

MVP screens: dashboard, business profile, financial input forms, risk questionnaire,
valuation results, basic scenario. See `04_DESIGN/UI_UX_SPECIFICATION.md` +
`05_ENGINEERING/FRONTEND_ARCHITECTURE.md`.

| Step | You say to tech-lead | Department(s) | Gate |
|------|----------------------|---------------|------|
| C.1 | "Build the frontend per `04_DESIGN` and `FRONTEND_ARCHITECTURE.md`: Next.js + React + TypeScript + Tailwind. Implement auth UI (register/login/reset), business profile screen, financial input forms (with schema validation), and a basic dashboard. Set up a typed API client against the backend. **The frontend must NEVER reconstruct financial formulas or be a source of truth** — all authoritative results come from the backend API. Client-side math is UX feedback only." | tech-lead → frontend-dept (+ backend-dept to confirm API contracts) | Frontend builds and talks to backend; no financial formulas implemented client-side. |
| C.2 | "Build the valuation results UI: display the range (low/central/high) with a confidence score — never false precision, no gauges implying precision. Show the method breakdown, risk score, value drivers, and the explanation trace. Separate AI explanation areas visually from authoritative results. Add the risk questionnaire screen and a basic scenario screen (vary revenue/EBITDA/growth, call backend, show projected vs current). Accessibility target WCAG 2.1 AA — never convey info by color alone. Every important number gets context/assumption/explanation." | tech-lead → frontend-dept | Frontend UI renders real API results; accessibility basics pass. |
| C.3 | "Run `security-auditor` + `qa-tester` on the frontend: confirm no secrets in client code, no authoritative calculations on the client, XSS safety, and that AI explanations are visually and semantically separated from authoritative results. Add frontend unit/component tests and accessibility checks to the suite." | tech-lead → security-auditor + qa-tester | Frontend tests pass; no Critical/High. |

**Phase C done when:** a user can complete the full happy path in the UI —
register → create business → enter financials → risk questionnaire → view valuation
range + explanation → run a scenario — with all numbers coming from the backend.

---

## PHASE D — HEALTH SCORE, AI EXPLANATION, REPORTS (MVP+ polish)

| Step | You say to tech-lead | Department(s) | Gate |
|------|----------------------|---------------|------|
| D.1 | "Implement the **Business Health Score** (financial strength, revenue quality, operational maturity, growth potential, business risk) per `01_PRODUCT/PRD.md` §6 and `MVP_MATHEMATICAL_REQUIREMENTS.md`. It is a deterministic calculation in the engine — not AI. Return sub-scores and the overall 0–100 score; test every sub-score formula." | tech-lead → valuation-engine | Health-score formula tests pass. |
| D.2 | "Implement the **AI explanation/report layer** per `01_PRODUCT/DEVELOPMENT_ROADMAP.md` AI Strategy and `BACKEND_ARCHITECTURE.md` §9: the `ai` service accepts ONLY approved structured context (valuation numbers, risk score, drivers) and generates readable explanations and report narratives. It must NEVER invent numbers, must never be the source of a valuation figure, must never have unrestricted DB access, and must be prompt-injection hardened per `TESTING_STRATEGY.md` §12. Add an AI service that explains *why* a business is worth its amount and the requested financial-data assistance." | tech-lead → backend-dept (+ security-auditor for prompt-injection + no-leak review) | AI tests pass: no invented numbers, private data not leaked, cannot override the engine. |
| D.3 | "Add **basic PDF professional valuation report generation** for the free/pro-paid report product, structuring the report per `PRD.md` §9 (executive summary, business overview, financial analysis, valuation methodologies, risk analysis, health analysis, conclusion, assumptions/limitations) using the engine's authoritative outputs. Keep it display-only and deterministic on the numbers." | tech-lead → backend-dept + frontend-dept | Report generates from a completed valuation; numbers match the engine exactly. |
| D.4 | "Run the full **financial release gate** (`TESTING_STRATEGY.md` §18) and full **acceptance testing** (§17): functional, UX, API, security, test coverage, and documentation all green. Run `security-auditor` end-to-end across backend + engine + frontend + AI + reports." | tech-lead → qa-tester + security-auditor | Full suite green; no Critical/High; release gate passed. |

**Phase D done when:** MVP is a coherent, tested, secured product — the user can get
a valuation, understand it (health score + AI explanation), and export a report.

---

## PHASE E — LIVE / RUNNING (deploy)

| Step | You say to tech-lead | Department(s) | Gate |
|------|----------------------|---------------|------|
| E.1 | "Prepare deployment per `BACKEND_ARCHITECTURE.md` §21 and infrastructure spec: dev → staging → production. Add migration workflow (never test a migration first in prod), env/secrets management (DATABASE_URL, AUTH_SECRET, AI_API_KEY — never committed), HTTPS, rate limiting, secure headers, and a health-check. Set up a Docker/CI/CD that runs lint → typecheck → unit → integration → build → security checks, then deploys to staging." | tech-lead → backend-dept | Staging deploy succeeds; health + smoke tests pass; secrets not in repo. |
| E.2 | "Seed real approved benchmark configuration (industry revenue/EBITDA multiples) via the `benchmarks` module with an approval gate — unapproved benchmarks must never affect a valuation. Add the super-admin to approve benchmark data. Wire the benchmarks into the engine's multiple lookup." | tech-lead → data-methodology + backend-dept | Benchmarks approved and applied; every valuation stores its `benchmarkSnapshot`. |
| E.3 | "Run **final security review + penetration-style checks** (`TESTING_STRATEGY.md` §13) and a load/perf smoke test (§14–15). Confirm the financial release gate and acceptance suite are green on staging. Then promote to production." | tech-lead → security-auditor + qa-tester | Prod deploy green; all gates passed; `GET /health` green in prod. |

**Phase E done when:** Valuora is **live and running** — a user can sign up and
complete the full valuation flow in production.

---

## PHASE F — POST-LAUNCH ROADMAP (the plan doesn't end at MVP)

These map to the remaining `DEVELOPMENT_ROADMAP.md` phases. Trigger each as a
separate mission AFTER the MVP is live:

| Roadmap phase | You say to tech-lead | Departments |
|---------------|----------------------|-------------|
| Phase 2 — Advanced Valuation | "Add DCF, asset-based valuation, financial normalization refinements, method weighting refinements, and confidence-scoring upgrades per `02_METHODOLOGY/VALUATION_METHODOLOGY.md`, `FINANCIAL_MODELLING.md`, `ADVANCED_QUANTITATIVE_MODELS.md` and `VALUATION_ENGINE.md` §9–20. Implement all advanced methods with full formula + regression tests, then rerun the financial release gate." | valuation-engine + qa-tester + security-auditor |
| Phase 3 — Business Intelligence | "Add business health score enhancement, value drivers, risk dashboard, and historical valuation tracking per `PRD.md` and `STATISTICAL_ANALYSIS.md`." | valuation-engine + data-methodology + backend-dept + frontend-dept |
| Phase 4 — Value Improvement Engine | "Add improvement recommendations and value-impact simulations per `PRD.md` §7 and `RECOMMENDATIONS` module. Deterministic impact estimates + AI to explain recommendations." | backend-dept + valuation-engine + frontend-dept |
| Phase 5 — Professional Platform | "Add professional PDF reports, accountant dashboard, consultant dashboard, and multi-client management per `PRD.md` §9 + §14 subscription model." | backend-dept + frontend-dept + docs-dept |
| Phase 6 — Data Intelligence | "Add industry benchmarking, comparable businesses, SME financial database, and market intelligence per `BUSINESS_VISION.md` and `STATISTICAL_ANALYSIS.md`." | data-methodology + backend-dept + frontend-dept |

---

## CHEAT SHEET — "Who does what" (for when you call a department directly)

| If you want to... | Trigger this agent directly |
|-------------------|----------------------------|
| Implement/change any valuation or math formula | `valuation-engine` |
| Build backend endpoints, DB, auth, tenancy | `backend-dept` |
| Build/change UI screens | `frontend-dept` |
| Work on benchmarks, stats, normalization, outliers | `data-methodology` |
| Review security/compliance (read-only, cannot edit) | `security-auditor` |
| Write/run tests, verify a feature | `qa-tester` |
| Write/update product docs (strips wrap-around text) | `docs-dept` |
| Do any multi-step feature / don't know who | `tech-lead` (it delegates) |

---

## FINAL PRINCIPLE

> The engine is the financial source of truth; AI only explains; the frontend only
> displays; tests gate every financial release. Build in the order above and the
> pieces assemble into a running, defensible product.
