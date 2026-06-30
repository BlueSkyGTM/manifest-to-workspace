# tooling.md — TEMPLATE (your DOMAIN tools only; the universal tools are core)

The UNIVERSAL tools (harness, cross-model review, retrieval, the evaluator method, the standing
skills) are domain-independent and live in the engine: `platform/TOOLING.md` (universal manifest). Do
NOT re-declare them here. This file declares the tools that are specific to YOUR domain — typically
the extractors that match your deposit types and any build-chain tools your deliverable needs.

The core Tool scan (`platform/TOOLING.md` → "Tool scan protocol") reads this table, detects each,
installs the safe/known-missing ones, and FLAGS the rest `MISSING-ASK` (never guesses an install).

## Domain tool manifest (fill in; delete the example row)
| tool | role / stage slot | required | detect (shell test) | install (shell cmd) |
|---|---|---|---|---|
| <e.g. a PDF extractor> | excavation: <deposit type> | if that deposit type | `<detect>` | `<install, or MISSING-ASK>` |

## Rules
- `required` extractors are conditional on the deposit types in your `deposits.md` — only the ones a
  run actually needs are required for that run (engine policy: wire the subset a run needs).
- If you cannot state a safe install command, write `MISSING-ASK` — do not guess.
- The scan writes the per-machine result to your pilot's `tool-status.md` (regenerated each scan).
