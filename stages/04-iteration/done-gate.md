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

Record the classification each iteration in the iteration log so the curve is visible.

## Why this works
- It compares THIS pass to the LAST pass (a delta), not absolute quality (a judgment). The gate never
  needs to decide "is this good enough" — only "did this pass add substance or just surface."
- It caps both failure modes: stopping too early (the efficiency bias declaring premature victory)
  AND never stopping (the perfectionism/dismantle loop). Substance present = not done; substance
  absent = done.

## Ship condition
When the done-gate fires: write the deliverable to library/, set frontmatter sealed: true, log the
ship to logs/gate-checks.md (outcome: ship). The sealed deliverable is now quotable by sibling work.

## TODO(tools)
Any tool that helps classify substance-vs-surface is wired in a LATER pass. For this pass, the RULE is
what you build (this file), not its automation.
