# platform/TOOLING.md — Tool Surface Declaration

The validated tool set, each tool's role, and — critically — which surface area of multi-feature
tools M2W ADOPTS vs REJECTS. A tool's reputation is not a license to import all of it. Where a tool's
core conflicts with a M2W law, the conflicting part is fenced out EXPLICITLY, so a later reader does
not pull it in because "that's how the tool works."

## The adopted set (knowledge + tools, housed by role)

EXCAVATION extractors (match to deposit type):
- docs-site → skill (from awesome-claude-skills) — documentation websites
- article-extractor — web articles
- youtube-transcript — video
- markitdown — PDF / Office → markdown

ASSAY: human calls on the curriculum loop; the seam brain (GBrain) is the OUTPUT, wired after 1c.

MANIFEST: native frontmatter (no external store).

ITERATION (back half):
- Karpathy LLM wiki — compile manifest → linked articles + ordered index. Principles native,
  STRUCTURE NOT FORKED.
- Understand-Anything /understand-knowledge — graph + capstone. HUMAN comprehension + course build.
- the capstone workflow — source-pinned course (see stages/04-iteration/capstone-build.md).
- the evaluator — Accept / Revise / Block (see stages/04-iteration/evaluator-rubric.md).

RETRIEVAL: GBrain — BLACKBOX machine retrieval for the pipeline. Canonical lives in READABLE files;
GBrain is the projection (never write a seam decision only into GBrain).

GLOBAL: gstack (fenced below); ponytail (scoped below).

## gstack — the fence (USE these, REJECT the rest)

gstack's headline value is PARALLEL multi-agent orchestration (Conductor running many concurrent
sprints, maker/checker sub-agents, /pair-agent). M2W's law is SINGLE-AGENT, depth-first
(PRINCIPLES.md #5). Parallel silos under loose contracts were the original source of drift.
Therefore the gstack surface is split:

ADOPT (single-agent-compatible):
- Confusion Protocol — gags the agent from guessing on architecture (= "you do not think; you execute")
- /freeze, /guard — edit-locks so the agent cannot "fix" unrelated code
- sprint-stage discipline — the Think → Plan → Build → Review → Test → Ship structure
- /spec — the spec gate (craftsmanship floor; NOT the seam gate — the seam gate is the seam brain)
- /autoplan — the reviewed-plan pipeline; surfaces only taste decisions, spawns NO concurrent workers
- GBrain + the read-write / read-only / deny trust triad — read-only solves re-entry contamination

REJECT (violates single-agent law):
- Conductor parallel sprints
- sub-agents / maker-checker parallelism
- /pair-agent and any cross-agent coordination
- anything that spawns concurrent workers

NOTE TO A LATER READER: do NOT enable Conductor or sub-agents because the gstack README centers them.
They are fenced OUT here by law. Using gstack means using the surface above, not its parallel core.

## ponytail — the scope (coding tasks ONLY)

ponytail's ladder (does this need to exist → skip; already covered → reuse; the minimum that works)
is DISCIPLINE for code and CORROSIVE for curriculum. Applied to lessons, it skips "redundant" content
and writes minimal explanations — fighting pedagogical completeness. Spaced reinforcement, worked
examples, and restating a concept in a new frame are NOT waste; they are how people learn.

- USE ponytail on coding / repo / tooling tasks (where over-engineering is the risk).
- /ponytail off during curriculum back-half iteration (where UNDER-building is the risk).
- It is NOT a global minimalism rule. It is a CODING minimalism rule.

## Parked (decided after the curriculum loop)
- GBrain vs Understand-Anything: DIFFERENT LAYERS (blackbox machine retrieval vs human comprehension)
  — both kept; treating them as competitors was an error. Final ingest details settle when 1c
  produces a wiki to point GBrain at.

## Dropped (recorded so they are not reconsidered by accident)
- headroom-desktop — operating-environment token optimizer; no architectural role. (markitdown, which
  it bundled, was taken on its own.)
- loop-engineering — its orchestration is sub-agent / parallel, contradicting single-agent law;
  gstack + MWP cover the loop. Its phased-rollout discipline informed the 1a→1c ladder; the repo was
  not forked.
- ICM — referenced as MWP's lineage only; never forked (would hijack the infra).
- awesome-claude-skills wholesale — shopped for the one extractor; the rest overlaps gstack or is
  off-domain or operational material.
- tapestry — overlaps the wiki layer.
