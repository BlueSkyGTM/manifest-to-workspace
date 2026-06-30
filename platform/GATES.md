# platform/GATES.md — The Deferral Points (Gates)

A gate is a DEFERRAL POINT, not a performance test. It does not ask "is this good." It asks "is the
context clean enough to commit the next step, and does the output conform to its shape." Quality
lives in the SHAPE of the design schema (the cookie-cutter, cut from logs of proven-good work). The
gate verifies the shape was applied and that committing will not contaminate. The moment a gate
starts judging quality, it reintroduces the early judgment that contaminates — exactly what M2W
defers around.

## Deferral-point types, by stage

### 1. Tooling checks (Excavation, stage 01)
Mechanical checks on the extraction itself, NOT on the content:
- Did the extraction reach its source?
- Did it cover the whole bounded space? (coverage, not choice)
- Is the output addressable (does it land in vault/ with an address)?
Answerable without knowing what is in the material. They never judge worth.

### 2. The Assay (Intake, stage 02) — THREE EXITS, by nature
The seam sort, evaluated only when material arrives. An assay is intrinsically three-way; this is not
a feature, it is what assaying is.
- **Cart** — matched a known on-seam edge with NO conflicting near-miss → carts/
- **Tailings** — clearly off-seam (good material, wrong seam) → tailings/ WITH a reason
- **Bench** — did not cleanly match and did not cleanly fail → bench/ (escalate to human)

Mechanical test for cart-vs-bench: "did it match a known on-seam edge with no conflicting near-miss,
yes/no?" A clean yes carts. ANY ambiguity benches. The agent does NOT resolve ambiguity; it benches
it (defers it to the human). This third exit is how the pipeline runs without committing to an
uncertain call.

The seam is necessarily incomplete and accretes over runs (ARCHITECTURE.md §2.2). On the pilot the
human makes the cart/tailings/bench call and the system records each as training.

### 3. Conformance + Done-gate (Iteration, stage 04)
- **Conformance gate**: did the deliverable apply its design schema, and does the output conform to
  the schema's shape? Pass/fail on shape, not quality.
- **Done-gate**: has marginal utility of another iteration dropped below its cost? Detected as the
  substance-to-surface transition. See stages/04-iteration/done-gate.md. Ship when flat; iterate when
  not. Dismantle permitted only while its marginal utility exceeds its cost.

### 4. The loop boundary / re-entry (after Ship)
After a deliverable ships, the loop is complete and the engine STOPS. Starting another loop is itself a
deferral point: it fires only when the operator supplies new deposits (the *request*), never on the
engine's own initiative. The engine does NOT check for new material and re-enter itself — that would be
a stage running because it *can*, plus a judgment ("is there enough new material to loop?"), both
forbidden (PRINCIPLES #1, #2).

Re-entry is made safe by produced state, not a counter:
- new material enters as new `vault/account.md` rows marked `consumed: false`; each stage processes
  only its `consumed: false` (iteration: `sealed: false`) inputs, so a later loop never re-processes
  already-handled material;
- ids are repo-unique (excavation checks against all existing addresses), so loops never collide;
- `sealed: true` items are terminal and never re-consumed.
**Progress is marked by production, not by numbering** — the boundary between loops is the frontier of
what has been consumed and sealed, visible entirely on disk (glass-box). There is no `run_id` and there
must not be one; an engine-initiated auto-loop (Route A) was considered and REJECTED (changelog
2026-06-30) because it reintroduces cross-loop context contamination — the very thing deferral defends
against.

## What a deferral point NEVER does
- It never judges whether content is "good," "useful to a customer," or "high quality."
- It never resolves ambiguity by committing — ambiguity routes to bench/ or logs/failures.md.
- It never deletes off-seam material — off-seam is tailed with a reason, never destroyed.

## TODO(tools)
The mechanical tests above are wired to specific tools in a LATER pass (after human review). For THIS
pass, the deferral points are specified but not tool-wired. Leave every `TODO(tools)` marker literal.
