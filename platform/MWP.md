# platform/MWP.md — Model Workspace Protocol (Principles, Implemented Natively)

MWP is the engine-principle layer M2W borrows for routing and stage execution. It is the bridge
between data and execution. M2W implements these principles **natively**. There is NO repository to
clone, fork, or vendor to obtain them. If you are about to run `git clone` to get MWP, you have
misread this file — stop.

ICM (Interpreted Context Methodology) is MWP's lineage. ICM is **excluded** from this build. Do not
introduce it. We take principles, not the engine.

## MWP is split in two across the pipeline

- **Intake half → the Assay (stage 02).** Routing-in and the three-way seam sort.
- **Stage-building half → Iteration (stage 04).** The governed build stages.

These two halves sit at opposite ends of the line with Transport/Manifest between them. They were
never adjacent; treating them as one bolted unit was the original error.

## The principles M2W implements

1. **Five-layer routing.** CLAUDE.md (law) → CONTEXT.md (routing) → stage (CONTRACT.md) → references
   (canonical sources) → artifacts (outputs). A cold agent orients from this layout alone, no
   re-briefing.

2. **Stage contracts.** Every stage declares Inputs / Process / Outputs / Gate. The contract is
   authoritative for what the stage does. Nothing happens in a stage its contract does not name.

3. **Numbered-folder sequencing.** Stages are ordered (01, 02, 03, 04). Earlier stages are
   prerequisites for later ones. Order is the dependency graph made visible.

4. **Output-folder handoffs.** A stage hands work to the next by writing to a known output location
   the next stage reads. No hidden channels; the handoff is a folder.

5. **One-way references.** References point downstream-reads-upstream only. No reference cycles.

6. **Canonical sources.** Each concept has exactly one canonical home. Duplicates collapse to the
   canonical source and reference it, never re-state.

7. **Selective routing / context folders.** Each stage sees only what its context folder routes to
   it. The agent never loads the whole repo; it pulls the slice its CONTEXT.md and CONTRACT.md name.
   This is both how the pipeline runs without drowning the agent AND the contamination defense — a
   stage's context cannot be polluted by material it should not see.

8. **Glass-box observability.** All state is on disk and inspectable. No hidden runtime state.

9. **Single-agent execution.** One agent runs the path top to bottom. No subagents. (PRINCIPLES.md #5.)

## What MWP gives M2W that makes the pipeline run without a human

- **Task routing** executes settled cases autonomously (known route in → known stage out).
- **Context folders** hand each stage exactly what it needs (and only that — the contamination
  defense).
- **The three-way assay** routes the unsettled cases to bench/ instead of committing.

Task routing handles the KNOWN. The bench handles the UNKNOWN. Together they let the pipeline run
without a human on everything except the genuinely novel boundary cases.
