# Evaluator Rubric — The Independent Checker (Accept / Revise / Block)

The worker does not grade itself. A separate evaluator pass checks each iteration's output and
returns ONE of three verdicts. This is the harness "feedback" layer — the highest-ROI part and the
one most often skipped. The builder cannot mark its own work done.

## The three verdicts (a three-exit gate, like the assay)
- **ACCEPT** — the output applied its design schema and conforms. It advances (toward the done-gate).
- **REVISE** — it does not yet conform, but the gap is fixable in place. Return it to iteration with
  the specific non-conformance named. NOT a failure — a targeted re-run.
- **BLOCK** — it is wrong in a way that in-place revision will not fix (off-schema, off-seam content
  surfaced, structural break). Stop. Route to bench/ or logs/failures.md. Do not revise; escalate.

This mirrors the assay's three exits (cart / tailings / bench) so the back half is structurally
consistent with the front half: no gate collapses to pass/fail; every gate has a middle path and an
escalation path.

## What the evaluator checks (conformance, NOT quality)
- Was the design schema applied?
- Does the output conform to the schema's shape?
- For the curriculum capstone specifically: does every fact trace to the wiki/manifest source? (the
  "do not add facts not in my material" constraint — source-pinning is a conformance check)
Verdict is on conformance and source-fidelity, never on "is this good." Quality lives in the schema.

## Worker / checker separation (non-negotiable)
The agent that built the iteration does NOT run the evaluator on its own output. Either a separate
pass with fresh context, or a second checker. "It looks done" is not a verdict; "it conforms and
every fact traces to source" is. Log every verdict to logs/gate-checks.md (outcome: accept | revise | block).

## TODO(tools)
A second-agent checker or gstack's review apparatus may run the evaluator in a later pass. For now
the rubric (this file) is what you build; the verdict is rendered against it.
