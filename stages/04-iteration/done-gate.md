# Done-Gate — Diminishing Marginal Utility

The done-gate is the system's stop condition. It fires when the marginal utility of another iteration
drops below its cost. Borrowed directly from the economic law of diminishing marginal utility: each
additional pass adds less than the last, and at some point another pass is not worth its cost.

## The mechanical signal: substance-to-surface transition
You do NOT count abstract "utils." You watch for a CATEGORY SHIFT in what each iteration adds:

- SUBSTANCE = new content that survives the design schema (new material, new structure, a real
  addition that conforms and stays).
- SURFACE = reordering, rewording, relabeling, polishing — changes to how existing content is
  presented, not what it contains.

The done-gate fires when iterations STOP adding substance and START only adding surface. When a pass's
delta is surface-only, marginal utility has dropped below cost → SHIP.

## Per-iteration measurement (mechanical)
After each iteration, classify its delta versus the previous iteration:
- Did this pass add content that survived the schema (substance)? → keep iterating.
- Did this pass only reorder/reword existing content (surface)? → the curve has flattened → ship.

**Iteration-1 baseline (no previous pass):** iteration 1 has nothing to compare against — its baseline
is the EMPTY deliverable, so all of iteration 1 is substance by definition → always `iterate`, never
`ship`. The earliest the done-gate can fire `ship` is iteration 2 (the first pass that CAN be
surface-only). This forecloses a degenerate iteration-1 ship on a deliverable never measured against a
prior pass.

Record the classification each iteration in `logs/gate-checks.md` (gate: done-gate, outcome: iterate
or ship, note: `delta=substance` or `delta=surface` + one line on what was added) so the curve is
visible across passes. The "iteration log" is this stream filtered to `gate: done-gate` — no separate
file. The substance-vs-surface call is a delta comparison (this pass vs last), not an absolute
quality judgment; when in doubt whether a delta is substance, it is surface (the conservative call
ships rather than perfecting).

## Why this works
- It compares THIS pass to the LAST pass (a delta), not absolute quality (a judgment). The gate never
  needs to decide "is this good enough" — only "did this pass add substance or just surface."
- It caps both failure modes: stopping too early (the efficiency bias declaring premature victory)
  AND never stopping (the perfectionism/dismantle loop). Substance present = not done; substance
  absent = done.

## Ship condition
When the done-gate fires: the deliverable is already in library/ (built there, unsealed). Flip its
frontmatter to sealed: true, flip the `built_from` manifest items to sealed: true (update
manifest/index.md), and log the ship to logs/gate-checks.md (outcome: ship). The sealed deliverable is
now quotable by sibling work. See stage 04 CONTRACT (Working location + Outputs) for the deliverable
frontmatter and seal semantics.

## After ship — the loop boundary is a deferral point (do NOT auto-loop)
When the done-gate ships, the LOOP IS COMPLETE and the engine STOPS. It does NOT check for more work,
and it does NOT start another loop on its own. A new loop begins only when the operator supplies new
deposits to excavation — that supply is the *request* that re-fires the pipeline (PRINCIPLES #2: a
stage runs because something requested it, never because it exists). The loop boundary is the system's
final deferral point, handled like every other: by waiting for a request, not by self-triggering.

Re-entry is SAFE without any run counter, because the boundary is carried by PRODUCED STATE on disk:
- new deposits enter as new `vault/account.md` rows (`consumed: false`); the assay processes only
  those, ignoring loop-1's `consumed: true` material;
- ids are repo-unique (excavation checks against every existing address), so a new loop cannot collide
  with the old;
- `sealed: true` manifest items are terminal — iteration never re-consumes them.
The run boundary is the FRONTIER OF PRODUCED STATE — what has been consumed and sealed. **Progress is
marked by production, not by numbering.** There is no `run_id`, and there must not be one.

**Do NOT drift this into an auto-loop.** An engine that checks for new material and re-enters on its
own was specified as Route A and REJECTED (changelog 2026-06-30): it makes a stage run because it
*can* (violates deferral #2), it makes the engine *judge* whether to loop (violates #1 "execute, don't
think"), and it carries loop-1's warm context into loop-2's judgments — the exact contamination M2W
defers around. The hard stop is load-bearing: it is the context-flush point that keeps each loop's
context clean. Removing it does not add a feature; it punches a hole in the contamination defense.
(Revert guard: tested design decision — surface before changing.)

## TODO(tools)
Any tool that helps classify substance-vs-surface is wired in a LATER pass. For this pass, the RULE is
what you build (this file), not its automation.
