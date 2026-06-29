# CONTRACT — Stage 04: Iteration

## Purpose
Build the deliverable from the manifest. The destination yard and back half of MWP. MVP-first,
conformance-gated, stopped by diminishing marginal utility.

## Inputs
- The manifest (stage 03 output): catalogued, frontmatter-typed, addressed on-seam material.
- The deliverable's design schema (the cookie-cutter that holds the quality judgment).
  NOTE: on the pilot the design schema is cut FROM LOGS of the run; it may not exist in final form on
  iteration one. Where it is absent, flag to logs/failures.md and proceed MVP-first, recording what
  the schema should capture.

## Process
1. MVP-first: the first iteration gets the deliverable off the ground — viable, unfinished, not
   perfect. Pull manifest material just-in-time (M2W); do not load the whole manifest into context.
2. Each subsequent iteration either FINISHES what stands or DISMANTLES it for a better MVP.
3. After each iteration, run the conformance gate and the done-gate (below).
4. Log every iteration: what it added (substance vs surface), gate outcomes.

## Conformance gate (shape, not quality)
- Did this iteration apply its design schema, and does the output conform to the schema's shape?
- Pass/fail on SHAPE. Never judge "is this good." Quality lives in the schema's shape.
- Log to logs/gate-checks.md.

## Done-gate (diminishing marginal utility) — see done-gate.md
- Fires when another iteration would add less than it costs.
- Detected as the SUBSTANCE-TO-SURFACE TRANSITION: when a pass stops adding content that survives the
  schema and starts only reordering/rewording what is already there → marginal utility has dropped
  below cost → SHIP (write to library/).
- Until then → iterate again.

## Dismantle rule (caps the infinite-rebuild loop)
A teardown is permitted ONLY while its marginal utility exceeds its cost. Once a rebuild would add
less than it costs, the gate REFUSES the teardown and ships what stands. Perfectionism (endless
rebuild for marginal gain) is forbidden by the same economics that govern the done-gate.

## Outputs
- A shipped deliverable in library/, sealed (frontmatter sealed: true), quotable by siblings.

## Do not
- Do not perfect one piece before the whole stands (MVP-first, not piece-perfect).
- Do not iterate past the done-gate. Flat curve = ship. Polishing the fifth slice is forbidden.
- Do not judge quality at the gate. The schema's shape carries quality; the gate checks conformance.
