# One-Page Triggers — Valuora

Start a session in this folder, then send `tech-lead` one of these. It reads the
specs and delegates to the right departments. Don't advance a phase until its gate
is green (tests run, security reviewed, no Critical/High).

## Phase order

0. **Global Setup** → A. **Shared Valuation Engine** → B. **Backend** →
C. **Frontend** → D. **Health/AI/Reports** → E. **Deploy** → F. **Post-launch roadmap**

## The magic phrases

| Phase | Send this to `tech-lead` |
|-------|---------------------------|
| **0 Setup** | "Set up the project foundation from `01_PRODUCT/DEVELOPMENT_ROADMAP.md`: monorepo with backend/ + frontend/ + shared engine package, stack, lint/typecheck, and CI running lint → typecheck → unit → integration → build → security." |
| **A.1 Engine** | "Create the shared frameless valuation-engine package implementing the MVP math exactly per `02_METHODOLOGY/MVP_MATHEMATICAL_REQUIREMENTS.md` and `05_ENGINEERING/VALUATION_ENGINE.md`. Decimal-safe only, deterministic, versioned, with explanation traces." |
| **A.2 Tests** | "Write the complete valuation-engine test suite per `05_ENGINEERING/TESTING_STRATEGY.md` §3–6: every formula normal/zero/boundary/invalid/large/precision. Add the standard examples and a regression fixture suite." |
| **A.3 Security** | "Run `security-auditor` on the valuation engine for determinism, no I/O/SQL/AI, no negative EV, full explanation traces." |
| **B.1 Backend** | "Build the backend modular monolith per `05_ENGINEERING/BACKEND_ARCHITECTURE.md`: modules, controller/service/repository/domain, no financial formulas in controllers, PostgreSQL, `GET /health`." |
| **B.2 Auth** | "Implement auth + users + RBAC + businesses + financials modules with multi-tenancy; server-side `businessId` authorization." |
| **B.3 Security** | "Run `security-auditor` on auth, RBAC, multi-tenancy, injection safety." |
| **B.4 Valuation API** | "Implement POST /valuations calling the shared engine and persisting a complete transactional snapshot; completed valuations immutable + reproducible; idempotent." |
| **B.5 Security** | "Run `security-auditor` on the valuation service/API: transactions, immutability, idempotency, tenancy, audit." |
| **C.1 Frontend** | "Build the Next.js/React/Tailwind frontend per `04_DESIGN` + `FRONTEND_ARCHITECTURE.md`: auth, business profile, financial forms, dashboard. Display-only — never reconstruct formulas." |
| **C.2 Results UI** | "Build valuation results UI: range + confidence (no false precision), method breakdown, explanation trace, risk questionnaire, scenario screen. AI explanations visually separated. WCAG 2.1 AA." |
| **C.3 Security+QA** | "Run `security-auditor` and `qa-tester` on the frontend: no secrets/authoritative math client-side, XSS safety, frontend tests + accessibility." |
| **D.1 Health** | "Implement the deterministic Business Health Score per `PRD.md` §6 in the engine; test every sub-score." |
| **D.2 AI layer** | "Implement the AI explanation/report service that consumes ONLY approved structured context, never invents numbers, never has unrestricted DB access, prompt-injection hardened per `TESTING_STRATEGY.md` §12." |
| **D.3 Reports** | "Add basic PDF valuation report generation per `PRD.md` §9 using the engine's authoritative outputs only." |
| **D.4 Release gate** | "Run the full financial release gate (§18) + acceptance testing (§17) + end-to-end security audit." |
| **E.1 Deploy** | "Set up dev→staging→prod deployment, migrations, secrets management (never committed), HTTPS, rate limiting, health check, CI/CD." |
| **E.2 Benchmarks** | "Seed approved industry revenue/EBITDA multiples via the benchmarks module with an approval gate; wire into the engine; every valuation stores its benchmarkSnapshot." |
| **E.3 Final** | "Run final security + penetration-style + load checks, confirm release gate green on staging, then promote to production." |
| **F.2** | "Add DCF, asset valuation, normalization + confidence refinements per `02_METHODOLOGY` + `VALUATION_ENGINE.md` §9–20; full tests; release gate." |
| **F.3** | "Add business health enhancement, value drivers, risk dashboard, historical tracking." |
| **F.4** | "Add value improvement recommendations + impact simulations per `PRD.md` §7." |
| **F.5** | "Add professional reports, accountant/consultant dashboards, multi-client management." |
| **F.6** | "Add industry benchmarking, comparable businesses, SME database, market intelligence." |

## Direct department calls (skip tech-lead when you know who)

| Want to... | Call |
|------------|------|
| Change any math/formula | `valuation-engine` |
| Build backend/API/DB/auth | `backend-dept` |
| Build UI | `frontend-dept` |
| Benchmarks/stats/normalization | `data-methodology` |
| Security review (read-only) | `security-auditor` |
| Write/run tests | `qa-tester` |
| Update docs | `docs-dept` |
| Anything multi-step / unsure | `tech-lead` |

## Gates checklist (all must be true before advancing)

- [ ] Financial formulas have full test coverage and pass (normal/zero/boundary/invalid/large/precision).
- [ ] Regression suite committed; methodology changes explicitly update expected outputs.
- [ ] `security-auditor` reports no Critical/High findings.
- [ ] Multi-tenancy tested: user A cannot reach business B's data.
- [ ] Completed valuations immutable + reproducible; snapshots persist.
- [ ] AI cannot invent numbers, cannot override the engine, cannot leak private data.
- [ ] No secrets in code or logs.
- [ ] Tests actually run and pass (never claim without running).
