# Capstone Build — The Back-Half Workflow (sources → wiki → graph → course)

The iteration stage's concrete build workflow for a curriculum deliverable. Adapted from the
sources→wiki→graph→course chain. Each step is source-pinned; the deliverable cannot contain facts
absent from the manifest.

## The chain
1. MANIFEST is the source of truth (stage 03 output): catalogued, frontmatter-typed material.
2. WIKI COMPILE (Karpathy LLM wiki, native principles): compile the manifest into linked articles,
   one per concept, cross-linked, with an index ordered beginner → advanced — the learning order.
   Immutable raw preserved; wiki is the compiled layer.
3. GRAPH (Understand-Anything /understand-knowledge): turn the wiki into a navigable graph for
   HUMAN comprehension and inspection. This is the inspectable layer (GBrain is the blackbox
   machine-retrieval layer; they are different jobs, both used).
4. COURSE (capstone): build ONE self-contained deliverable from the wiki + index as the SINGLE
   SOURCE OF TRUTH. Constraint, enforced by the evaluator: do NOT add facts not in the material.
   Lessons in the index's learning order; per-lesson objectives, explanation, worked example, and a
   short comprehension check; a final assessment; review-pointers for misses.

## MVP-first applies
Iteration one produces a viable, unfinished course (the structure stands, some lessons stubbed).
Later iterations finish or dismantle, gated by the done-gate (substance-to-surface) and the
evaluator (Accept/Revise/Block). Ship when another pass adds only surface.

## Source-pinning is the conformance discipline
The "no facts not in my material" rule IS the back-half's conformance check: the deliverable is
pinned to the manifest, so it cannot drift off-seam by inventing content. The evaluator verifies
every fact traces to source. This is how the curriculum stays on-seam at GENERATION time, not just
at intake.

## TODO(tools)
Wire Karpathy wiki (compile), Understand-Anything (graph + capstone), and the evaluator (source-
fidelity check) in the curriculum pass — NOT the maze pass. The maze tests the walls first.

## ponytail OFF for curriculum iteration
Run `/ponytail off` during the curriculum back-half build (see platform/TOOLING.md). ponytail's
minimalism ladder is tuned for code and UNDER-builds pedagogical content — it would skip "redundant"
lessons and write minimal explanations, which fights the spaced reinforcement and worked examples a
course needs. ponytail stays ON for coding/tooling work; it comes OFF here.
