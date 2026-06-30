# CONTRACT — Stage 04: Iteration

## Purpose
Build the deliverable from the manifest. The destination yard and back half of MWP. MVP-first,
conformance-gated, stopped by diminishing marginal utility.

## Inputs
- The manifest at `manifest/` (stage 03 output): `index.md` (the cargo list) + `items/<id>.md`.
  Pull from it just-in-time; do NOT load all of `items/` into context. Pull only `sealed: false`
  items — a `sealed: true` item is TERMINAL (already consumed into a shipped deliverable) and is never
  re-consumed; a later loop's iteration sees only the unsealed items (sealed material stays quotable by
  siblings but is never rebuilt).
- The deliverable's design schema (the cookie-cutter that holds the quality judgment) — supplied by
  the instantiation, or cut from logs of proven-good work.
- The concrete build workflow — supplied by the instantiation (see the iteration-workflow interface
  below). Core declares the interface; a pilot supplies the chain.

## Design-schema bootstrap (the first-run rule — not a hole)
The schema is cut from logs of proven-good work, so on a domain's FIRST run it does not exist yet.
This is handled by an explicit rule, NOT by "flag and proceed":
- **Iteration 1 runs in SCHEMA-DISCOVERY mode: conformance is explicitly UNGATED.** Its job is to
  produce the MVP and the logs from which the schema is cut. Record what the schema should capture.
- **Where the schema lives:** the instantiation declares the schema's path (recommended:
  `pilots/<name>/design-schema.md`). Schema-discovery WRITES the cut schema there; iterations 2+ READ
  it. Core names the slot; the pilot owns the file. If the path is undeclared, that is a stop-and-ask,
  not a guess.
- **Conformance gating begins at iteration 2**, once the schema file exists. From then on the
  conformance gate applies normally.
This is a deliberate, bounded exception declared up front — not the agent guessing past a missing
input. Log the schema-discovery iteration to logs/gate-checks.md (outcome: passed, note: ungated
schema-discovery).

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

## Visual rendering (deferred here from excavation)
Excavation extracted chart/diagram DATA as text (markdown tables). Rendering that data into the BEST
visual form is a judgment, so it happens HERE, just-in-time, only on material the build actually pulls
(never on discarded or unused material — that is the token saving):
- For each visual the deliverable needs, choose the representation that fits — a markdown table, a
  **mermaid** diagram, or an **excalidraw** file — rather than blind-pasting the source chart.
- Inline-renderable visuals (tables, mermaid) live in the deliverable body. Standalone visual assets
  (e.g. `.excalidraw`) go in a `visuals/` subfolder beside the deliverable in `library/`, referenced
  from the body. Keep it light: render what the deliverable uses, nothing speculative.
- Source-pinning still applies: a rendered visual may only show data that traces to the manifest.

## Working location and the deliverable's frontmatter
- The in-progress deliverable is built IN `library/` from iteration 1, carrying `sealed: false` while
  it iterates. `library/` therefore holds both in-flight (unsealed) and shipped (sealed) deliverables;
  the `sealed` flag is the difference. There is no separate draft folder.
- Every deliverable carries minimal frontmatter:
  ```yaml
  ---
  id: <stable address>
  kind: <deliverable kind, e.g. course | report | dataset>   # named by the instantiation
  built_from: [<manifest item ids it consumed>]
  sealed: false   # flips true at the done-gate (ship)
  ---
  ```

## Outputs
- A shipped deliverable in library/, sealed (frontmatter sealed: true), quotable by siblings.
- On ship, the manifest items listed in `built_from` flip their own `sealed: true` (they have cleared
  their final gate — consumed into a shipped deliverable). Update `manifest/index.md` rows to match.

## Do not
- Do not perfect one piece before the whole stands (MVP-first, not piece-perfect).
- Do not iterate past the done-gate. Flat curve = ship. Polishing the fifth slice is forbidden.
- Do not judge quality at the gate. The schema's shape carries quality; the gate checks conformance.
