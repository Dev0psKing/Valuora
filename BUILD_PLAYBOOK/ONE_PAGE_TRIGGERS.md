# One-Page Triggers — Valuora Complete Build Script

This is the complete working script. To use it:

1. Start opencode **from the Valuora folder** (`cd C:\Users\USER\Documents\Valuora`
   then `opencode`). You are already talking to `tech-lead` (your default agent) —
   there is no trigger command to type.
2. Copy the prompt under each step and paste it into the session.
3. Let `tech-lead` delegate to the departments.
4. Confirm the **gate** before moving to the next step.

Work strictly top to bottom. Do not skip Phase A — the whole product depends on it.
Only send the "Start the build now" prompt after you have restarted opencode from
this folder.

---

# HOW TO START (do this once, first time)

```
cd C:\Users\USER\Documents\Valuora
opencode
```

Then say (or paste the Setup prompt in Step 0). You do not select or trigger
tech-lead explicitly — it is your default agent.

---

# 0. GLOBAL SETUP

**Copy-paste prompt:**

> Set up the project foundation from `01_PRODUCT/DEVELOPMENT_ROADMAP.md`. Create a monorepo with `backend/`, `frontend/`, and a shared valuation-engine package. Use the stack specified there: Next.js + React + TypeScript + Tailwind CSS for frontend; Node.js + TypeScript for backend; PostgreSQL; decimal-safe math; Jest for tests. Add lint and typecheck scripts for all packages, plus a CI pipeline that runs lint → typecheck → unit tests → integration tests → build → security checks, per `05_ENGINEERING/TESTING_STRATEGY.md` §20. Wire the scripts so the whole repo builds and typechecks. Commit.

**Departments:** tech-lead → backend-dept + docs-dept.
**Gate:** `npm run lint` and `npm run typecheck` pass on the wired skeleton; CI config committed.

---

# A. SHARED VALUATION ENGINE (the honest core)

## A.1 — Build the engine

**Copy-paste prompt:**

> Create the shared, framework-independent valuation-engine package implementing the MVP math exactly per `02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` and `05_ENGINEERING/VALUATION_ENGINE.md`. Implement: financial metrics (revenue growth, CAGR, gross margin, EBITDA margin, net margin), revenue-multiple method, EBITDA-multiple method, normalized-EBITDA adjustments (explicit and documented, never invented), method weighting that must sum to 1, enterprise value, equity value, valuation range (low/central/high), risk score, and confidence score. Use decimal-safe arithmetic only — never binary float. Make it fully deterministic and versioned (`methodologyVersion`, `engineVersion`), and emit an explanation trace per §23. Keep it frameless so it can be reused by the backend now and Python later. Per §24, enforce: weights sum to 1, no negative enterprise value without explicit allowance, currency always known, inputs validated, completed valuation has a snapshot.

**Departments:** tech-lead → valuation-engine (+ data-methodology review).
**Gate:** all engine invariants enforced; determinism verified.

## A.2 — Test the engine

**Copy-paste prompt:**

> Write the complete test suite for the valuation engine per `05_ENGINEERING/TESTING_STRATEGY.md` §3–§6 and `02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` §6. Every formula needs: normal, zero, boundary, invalid, large, and precision cases. Include the standard examples (revenue growth 100→120 = 20%; EBITDA 20M × 3.5 = 70M; EV 100M + cash 10M − debt 20M = equity 90M). Add property tests (weights sum to 1; weighted value stays within method bounds). Add a regression fixture suite of known valuation cases (§6) where any methodology change must explicitly update expected outputs. Run the suite and report real pass/fail.

**Departments:** tech-lead → qa-tester (+ valuation-engine to fix failures).
**Gate:** 100% of formula tests pass; regression suite committed.

## A.3 — Security-review the engine

**Copy-paste prompt:**

> Run `security-auditor` on the shared valuation-engine package: confirm it has no I/O, no SQL, no secrets, no AI dependency, and that determinism is guaranteed (identical inputs + benchmarks + versions ⇒ identical output). Confirm it emits full explanation traces and can never produce a negative enterprise value without an explicitly permitted methodology.

**Departments:** tech-lead → security-auditor (read-only).
**Gate:** no Critical/High findings. **Do not proceed past Phase A until A.1–A.3 are green.**

---

# B. BACKEND (modular monolith)

## B.1 — Backend structure

**Copy-paste prompt:**

> Build the backend modular monolith exactly per `05_ENGINEERING/BACKEND_ARCHITECTURE.md`: `backend/src` with modules `auth, users, businesses, financials, metrics, risk, valuation, benchmarks, scenarios, recommendations, reports, ai, audit, shared`. Each module follows controller/service/repository/domain/validation/tests. Controllers must NOT contain financial formulas — business logic lives in services/domain, and the valuation module CALLS the shared valuation-engine package, never recomputing the math. Use PostgreSQL with parameterized queries or a safe ORM (never raw string SQL). Configure via environment variables / secrets manager. Add a `GET /health` endpoint with app + database checks (do not expose sensitive internals).

**Departments:** tech-lead → backend-dept (+ valuation-engine to wire the engine's API).
**Gate:** backend boots; `/health` OK; no financial formulas in controllers.

## B.2 — Auth, users, businesses, financials, multi-tenancy

**Copy-paste prompt:**

> Implement authentication, users, and role-based access per `05_ENGINEERING/BACKEND_ARCHITECTURE.md` §15: verified email, secure password hashing (Argon2id or bcrypt), OTP-based password reset, JWT/session with short lifetime, and roles OWNER / ADMIN / ANALYST / ADVISOR / INVESTOR / SUPER_ADMIN. Authorization must be checked server-side on every request. Build the businesses module (create/read/update business profile: name, industry, location, years operating, employees, structure) and financials module (revenue, COGS, EBITDA, net income, debt, cash, assets, liabilities). Implement multi-tenancy: every business-scoped request verifies the authenticated user owns or has access to the `businessId` — never trust a client-supplied ID.

**Departments:** tech-lead → backend-dept.
**Gate:** auth + multi-tenancy integration tests pass (TESTING_STRATEGY §7–8).

## B.3 — Security-review auth & tenancy

**Copy-paste prompt:**

> Run `security-auditor` on auth, users, businesses, financials, and multi-tenancy: IDOR checks, RBAC enforcement, password hashing, JWT/session security, injection safety, and tenant isolation (User A must never reach Business B's data).

**Departments:** tech-lead → security-auditor.
**Gate:** no Critical/High findings.

## B.4 — Valuation service + API

**Copy-paste prompt:**

> Implement the valuation service and API per `05_ENGINEERING/CALCULATION_PIPELINE.md`: `POST /valuations` that validates inputs, loads business + financials + approved benchmarks, calls the shared valuation engine (methods, adjustments, weighting, range, confidence), and persists a COMPLETE transactional snapshot (valuation + method results + adjustments + audit record). Completed valuations are immutable and reproducible — never silently mutate; new financial data creates a NEW snapshot. Support idempotency on creation. Expose `GET /valuations/:id` returning the engine's JSON output plus the explanation trace. Return the standardized API error contract on failure.

**Departments:** tech-lead → backend-dept + valuation-engine.
**Gate:** end-to-end valuation integration test passes; re-running with same inputs reproduces identical output.

## B.5 — Security-review valuation service

**Copy-paste prompt:**

> Run `security-auditor` on the valuation service and API: transactional integrity, immutability of completed valuations, idempotency, tenant isolation on valuation read/write, and audit-log completeness. Then run `qa-tester` on financial data integrity per `TESTING_STRATEGY.md` §16.

**Departments:** tech-lead → security-auditor + qa-tester.
**Gate:** no Critical/High; integrity tests pass.

**Phase B gate (all):** an authenticated user can, via the API, create a business, add financials, and receive a reproducible, audit-trailed valuation — with tenant isolation and security verified.

---

# C. FRONTEND (display-only UI)

## C.1 — Frontend structure & auth screens

**Copy-paste prompt:**

> Build the frontend per `04_DESIGN/UI_UX_SPECIFICATION.md` and `05_ENGINEERING/FRONTEND_ARCHITECTURE.md`: Next.js + React + TypeScript + Tailwind CSS. Implement the auth UI (register/login/password reset), business profile screen, financial input forms (with schema validation), and a basic dashboard. Set up a typed API client calling the backend. HARD RULE: the frontend must NEVER reconstruct financial formulas or be a source of truth — all authoritative results come from the backend API. Client-side math is UX feedback only and never final.

**Departments:** tech-lead → frontend-dept (+ backend-dept to confirm API contracts).
**Gate:** frontend builds and talks to backend; no financial formulas implemented client-side.

## C.2 — Valuation results, risk questionnaire, scenario UI

**Copy-paste prompt:**

> Build the valuation results UI per `04_DESIGN` + `FRONTEND_ARCHITECTURE.md`: display the range (low/central/high) with a confidence score — NEVER false precision and no gauges that imply precision. Show method breakdown, risk score, value drivers, and the explanation trace. Visually separate AI explanation areas from authoritative results. Add the risk questionnaire screen and a basic scenario screen (vary revenue/EBITDA/growth, call the backend, show projected vs current). Aim for WCAG 2.1 AA and never convey information by color alone. Give every important number context, its assumption, and an explanation.

**Departments:** tech-lead → frontend-dept.
**Gate:** UI renders real API results; accessibility basics pass.

## C.3 — Frontend security & QA

**Copy-paste prompt:**

> Run `security-auditor` and `qa-tester` on the frontend: confirm no secrets in client code, no authoritative calculations on the client, XSS safety, and that AI explanations are visually and semantically separated from authoritative results. Add frontend unit/component tests and accessibility checks to the suite. Run the suite and report results.

**Departments:** tech-lead → security-auditor + qa-tester.
**Gate:** frontend tests pass; no Critical/High.

**Phase C gate (all):** a user can complete the full happy path in the UI — register → create business → enter financials → risk questionnaire → view valuation range + explanation → run a scenario — with all numbers from the backend.

---

# D. HEALTH SCORE, AI EXPLANATION, REPORTS

## D.1 — Business Health Score

**Copy-paste prompt:**

> Implement the deterministic Business Health Score per `01_PRODUCT/PRD.md` §6 and `MVP_MATHEMATICAL_REQUIREMENTS.md`: sub-scores for financial strength, revenue quality, operational maturity, growth potential, and business risk, plus an overall 0–100 score. This is a deterministic engine calculation, NOT AI. Add full formula tests for every sub-score (normal/zero/boundary/invalid/large/precision).

**Departments:** tech-lead → valuation-engine.
**Gate:** health-score formula tests pass.

## D.2 — AI explanation service

**Copy-paste prompt:**

> Implement the AI explanation/report layer per `01_PRODUCT/DEVELOPMENT_ROADMAP.md` AI Strategy and `05_ENGINEERING/BACKEND_ARCHITECTURE.md` §9: the `ai` service accepts ONLY approved structured context (valuation numbers, risk score, drivers) and generates readable explanations and report narratives. It must NEVER invent numbers, must never be the source of a valuation figure, must never have unrestricted database access, and must be prompt-injection hardened per `TESTING_STRATEGY.md` §12. It should explain *why* a business is worth its amount and assist users with requested financial information. Add AI tests: no invented numbers, no private-data leakage, cannot override the engine.

**Departments:** tech-lead → backend-dept (+ security-auditor for prompt-injection + no-leak review).
**Gate:** AI tests pass; cannot override or invent valuation numbers.

## D.3 — PDF valuation reports

**Copy-paste prompt:**

> Add basic PDF professional valuation report generation per `01_PRODUCT/PRD.md` §9, structuring the report as: executive summary, business overview, financial analysis, normalized financial statements, valuation methodologies, market/income/asset approach, risk analysis, business health analysis, sensitivity analysis, valuation conclusion, and assumptions & limitations. Use ONLY the engine's authoritative outputs — the report is display-only and the numbers must exactly match the engine. Allow download as PDF.

**Departments:** tech-lead → backend-dept + frontend-dept.
**Gate:** report generates from a completed valuation; numbers match the engine exactly.

## D.4 — Full release gate + acceptance

**Copy-paste prompt:**

> Run the full financial release gate (`05_ENGINEERING/TESTING_STRATEGY.md` §18) and full acceptance testing (§17): functional, UX, API, security, test coverage, and documentation all green. Then run `security-auditor` end-to-end across backend + engine + frontend + AI + reports. Report the release-gate result.

**Departments:** tech-lead → qa-tester + security-auditor.
**Gate:** full suite green; no Critical/High; release gate passed.

**Phase D gate (all):** MVP is a coherent, tested, secured product — user can get a valuation, understand it (health score + AI explanation + report).

---

# E. DEPLOY — LIVE AND RUNNING

## E.1 — Environments, CI/CD, secrets, infra

**Copy-paste prompt:**

> Prepare deployment per `05_ENGINEERING/BACKEND_ARCHITECTURE.md` §21 and the infrastructure spec: dev → staging → production. Add a migration workflow (never test a migration first in production), environment/secrets management (DATABASE_URL, AUTH_SECRET, AI_API_KEY — never committed), HTTPS, rate limiting, secure headers, and health checks. Set up Docker + CI/CD that runs lint → typecheck → unit → integration → build → security checks, then deploys to staging.

**Departments:** tech-lead → backend-dept.
**Gate:** staging deploy succeeds; health + smoke tests pass; no secrets in repo.

## E.2 — Real benchmarks with approval gate

**Copy-paste prompt:**

> Seed real approved industry revenue and EBITDA multiples through the `benchmarks` module with an approval workflow, per the benchmarks spec. Unapproved benchmarks must never affect a valuation. Add super-admin tooling to approve benchmark data, and wire approved benchmarks into the engine's multiple lookup so every valuation stores its `benchmarkSnapshot` (version + dataset date + quality).

**Departments:** tech-lead → data-methodology + backend-dept.
**Gate:** benchmarks approved and applied; each valuation stores `benchmarkSnapshot`.

## E.3 — Final review + go live

**Copy-paste prompt:**

> Run the final security review and penetration-style checks per `05_ENGINEERING/TESTING_STRATEGY.md` §13, plus a load/perf smoke test (§14–15). Confirm the financial release gate and full acceptance suite are green on staging, then promote to production. Confirm `GET /health` is green in production. The product is now live.

**Departments:** tech-lead → security-auditor + qa-tester.
**Gate:** production green; all gates passed; `/health` OK in prod.

**Phase E gate (all):** Valuora is LIVE and RUNNING — a user can sign up and complete the full valuation flow in production.

---

# F. POST-LAUNCH ROADMAP (complete the product)

Send each as a separate mission after the MVP is live.

## F.1 — Advanced Valuation (Roadmap Phase 2)

> Add DCF (with `r > g` enforced), asset-based valuation, financial-normalization refinements, method-weighting refinements, and confidence-scoring upgrades per `02_METHODOLOGY/VALUATION_METHODOLOGY.md`, `02_METHODOLOGY/FINANCIAL_MODELLING.md`, `02_METHODOLOGY/ADVANCED_QUANTITATIVE_MODELS.md`, and `05_ENGINEERING/VALUATION_ENGINE.md` §9–20. Add full formula + regression tests for every new method, then rerun the financial release gate.

## F.2 — Business Intelligence (Roadmap Phase 3)

> Add business health score enhancement, value drivers, a risk dashboard, and historical valuation tracking per `01_PRODUCT/PRD.md` and `02_METHODOLOGY/STATISTICAL_ANALYSIS.md`.

## F.3 — Value Improvement Engine (Roadmap Phase 4)

> Add improvement recommendations and value-impact simulations per `01_PRODUCT/PRD.md` §7: deterministic impact estimates from the engine, with AI to explain each recommendation.

## F.4 — Professional Platform (Roadmap Phase 5)

> Add professional PDF reports, an accountant dashboard, a consultant dashboard, and multi-client management per `01_PRODUCT/PRD.md` §9 and the §14 subscription model.

## F.5 — Data Intelligence (Roadmap Phase 6)

> Add industry benchmarking, comparable businesses, an SME financial database, and market intelligence per `01_PRODUCT/BUSINESS_VISION.md` and `02_METHODOLOGY/STATISTICAL_ANALYSIS.md`. This is where the product becomes the financial-intelligence infrastructure described in the Business Vision.

---

# DIRECT DEPARTMENT CALLS (when you know who)

| Want to... | Say |
|------------|-----|
| Change any math/formula | "valuation-engine, ..." |
| Build backend/API/DB/auth | "backend-dept, ..." |
| Build UI | "frontend-dept, ..." |
| Benchmarks/stats/normalization | "data-methodology, ..." |
| Security review (read-only) | "security-auditor, ..." |
| Write/run tests | "qa-tester, ..." |
| Update docs | "docs-dept, ..." |
| Anything multi-step / unsure | "tech-lead, ..." (default) |

---

# GATES TO VERIFY (must be true before the next step)

- [ ] Global Setup: lint + typecheck pass; CI committed.
- [ ] Financial formulas have full test coverage (normal/zero/boundary/invalid/large/precision) and pass.
- [ ] Regression suite committed; methodology changes explicitly update expected outputs.
- [ ] `security-auditor` reports no Critical/High findings at each step.
- [ ] Multi-tenancy tested: user A cannot reach business B's data.
- [ ] Completed valuations immutable + reproducible; snapshots persist.
- [ ] AI cannot invent numbers, cannot override the engine, cannot leak private data.
- [ ] No secrets in code or logs.
- [ ] Tests are actually run and pass — never claim without running.
- [ ] Final: `/health` green in production; user can complete the full flow live.
