# Build Playbook

This folder is the step-by-step algorithm for building Valuora — from the
`01_PRODUCT` vision through to a running, complete product.

## Files

| File | What it is |
|------|------------|
| `ALGORITHM.md` | The full sequential algorithm: phases, gates, and which departments get triggered. The high-level map. |
| `ONE_PAGE_TRIGGERS.md` | **The complete working script.** Has the exact copy-paste prompt to send `tech-lead` at every step, from Global Setup to a fully running product. |
| `README.md` | This file — how to start and trigger the build. |

## How to start (the trigger, step by step)

1. **Open a terminal** and move into the Valuora folder:
   ```
   cd C:\Users\USER\Documents\Valuora
   ```
2. **Start opencode** from that folder:
   ```
   opencode
   ```
   *(opencode loads the agents from `.opencode/` when started here. You must restart
   opencode from this folder any time agent/config files change.)*
3. **You are already talking to `tech-lead`** — it is your default agent. There is
   **no command** to type to activate it; just start the session.
4. **Paste the prompt** for the step you want. Copy the exact text from the
   "Copy-paste prompt" column in `ONE_PAGE_TRIGGERS.md` (or say *"do the global setup
   from BUILD_PLAYBOOK/ONE_PAGE_TRIGGERS.md"*).
5. **Let tech-lead work** — it reads the specs and delegates to the departments
   (`valuation-engine`, `backend-dept`, `frontend-dept`, `data-methodology`,
   `qa-tester`, `security-auditor`, `docs-dept`) by itself.
6. **Verify the gate** for that step before starting the next one. See
   `ONE_PAGE_TRIGGERS.md` → "Gates to verify" for each step.

> **Note on changes:** if you edit any file under `.opencode/` or `opencode.json`,
> you must quit opencode and start it again from this folder for changes to apply.

## Phase order (work top to bottom)

```
0. Global Setup
A. Shared Valuation Engine   ← the honest core, never skip
B. Backend (modular monolith)
C. Frontend (display-only UI)
D. Health score, AI explanation, Reports
E. Deploy → LIVE and RUNNING
F. Post-launch roadmap (phases 2–6)
```

Do not jump ahead — each phase depends on the previous one (e.g. the backend's
valuation API must call the shared engine from Phase A, so A must exist first).

## The golden rule (applies to every step)

> **The deterministic valuation engine is the source of truth for financial
> numbers. AI explains, it never computes. The frontend is display-only.
> Financial correctness is a release requirement, not a nice-to-have.**
