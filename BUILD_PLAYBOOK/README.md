# Build Playbook

This folder is the step-by-step algorithm for building Valuora — from the
`01_PRODUCT` vision through to a running product.

## Files

| File | What it is |
|------|------------|
| `ALGORITHM.md` | The full sequential algorithm: phases, what to *say* to `tech-lead`, which departments get triggered, and the **gate** that must pass before advancing. |
| `ONE_PAGE_TRIGGERS.md` | Quick-reference cheat sheet for day-to-day triggering. |

## How to use

1. Open an opencode session **in this folder** (agents load from `.opencode/`).
2. Start with the **GLOBAL SETUP** phase, then follow **Phase A → B → C → D → E**.
3. At each step, paste the text from the *"You say to tech-lead"* column into the
   session, then let `tech-lead` delegate to the departments.
4. Do not advance until the listed **gate** passes (tests run, security reviewed,
   no Critical/High findings).

The core rule above all else: **the deterministic valuation engine is the source
of truth for financial numbers; AI explains, it never computes; and financial
correctness is a release requirement.**
