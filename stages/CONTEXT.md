# CONTEXT.md — stages/ (The Work)

The pipeline runs these four stages in numbered order. Each is a prerequisite for the next.
Depth-first: finish one line of work through all stages before starting the next.

01-excavation → mine raw material into vault/ (tooling checks only)
02-assay      → three-way seam sort: carts/ | tailings/ | bench/
03-manifest   → catalogue carted material, stamp frontmatter, address it
04-iteration  → build the deliverable, MVP-first, ship at the done-gate

Each stage has a CONTRACT.md authoritative for what the stage does. Nothing happens in a stage that
its contract does not name. Handoffs between stages are folders, not hidden channels. A stage runs
only when the prior stage produces something that requests it (M2W).
