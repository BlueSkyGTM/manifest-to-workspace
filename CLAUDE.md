# CLAUDE.md — M2W Pipeline Operating Manual (THE LAW)

This file is the law. It is present from the first moment and is never earned. It orients a cold
agent — you — to the pipeline itself: how the stages run, where the deferral points are, what you
are permitted and forbidden to do. It is not the *product's* instruction set. The product's
instruction set is **produced** by the pipeline, downstream, as an output. Do not confuse the two.
This file governs the machine; the machine produces the deliverable's own CLAUDE.md later.

## What M2W is, in one paragraph

M2W — **Manifest to Workspace** — is the name of this system: the transform it performs, manifest in, workspace out. Its governing discipline is deferral (see M2W.md). It
is a mining operation for turning raw, located material into finished deliverables while
**withholding every evaluation and execution until acting is safe** — until the context is clean
enough that committing will not contaminate the result. Raw material is excavated, assayed (sorted
on-seam / off-seam / uncertain) only when it arrives, transported and catalogued into a manifest,
then iterated into a deliverable that ships when another iteration would add nothing. The structure
is the system: routing files and contracts encode where work goes and what each stage must produce,
so the pipeline runs without a human steering it — escalating to the human only what it genuinely
cannot decide. Nothing is judged or run to prove itself; everything is deferred until needed.

## Canonical read order (this is the one; other files defer to it)

A cold agent reads in THIS order. Any other file that states a "read first" defers to this list:
1. `CLAUDE.md` (this file) — the law / orientation. The entry point.
2. `platform/CONTEXT.md` and every law file it lists (PRINCIPLES, MWP, GATES, LOGS, TOOLING, SKILLS, glossary).
3. `M2W.md` — the governing discipline, explained.
4. `ARCHITECTURE.md` — the full spec (understanding; contracts are authoritative for building).
5. `stages/` in order: 01-excavation → 02-assay → 03-manifest → 04-iteration.
6. For a BUILD pass only: `BUILD-INSTRUCTIONS.md` — what to build this pass + the hard DO-NOT list.
7. For a RUN: the instantiation under `pilots/<name>/` (the operator's supplied domain).

## Core vs instantiation (the operator supplies the domain)

This file and everything under `platform/`, `stages/`, the holding folders, `manifest/`, and `logs/`
is the **domain-agnostic core**. It never names a domain. A **pilot** under `pilots/` is one supplied
domain (the current instantiations live there; core does not name them). The law of the boundary:
**a pilot may reference core; core must never reference a specific pilot.** If a core file names a specific domain, tool, or
deliverable shape, that is a leak — flag it to `logs/failures.md`. Deletion test: remove `pilots/` and
the core must still stand. (This is ICM's one-way-reference and canonical-source discipline.)

## Before a run (the TOOL SCAN)

On SETUP, and before a RUN actually fires excavation, run the **Tool scan** (`platform/TOOLING.md`)
against the active pilot's manifest (`pilots/<name>/tooling.md`). It detects what is installed,
installs the safe/known-missing ones, and FLAGS the rest (`MISSING-ASK`) for the human — it never
guesses an install. The result is written to `pilots/<name>/tool-status.md`. A missing REQUIRED tool
is a blocker: stop and report. This is why a cold session on a new machine knows immediately what to
install.

## Before you change the machine (the REVERT GUARD)

If the user asks you to **modify** the repo, **improve/refactor** it, or **undo/revert** a change,
read `changelog/CHANGELOG.md` FIRST. Hard rule: **do not revert or overwrite a change whose entry is
marked `tested:` without surfacing it to the human** — name the entry, what it was designed to do, and
how it was validated, then let the human decide. A tested change that looks odd is usually the fix;
the oddness is load-bearing. See `changelog/CONTEXT.md`.

## The overarching law: M2W

See `M2W.md` and `platform/PRINCIPLES.md`. The short form: defer every evaluation, execution, and
judgment until the moment it is actually required. A stage does not run because it exists; it runs
because the stage before it produced something that **requests** it. This is why an empty folder is
a valid state, why nothing is loaded into context speculatively, why the pipeline can stop cleanly
at any point, and — most importantly — why context cannot be contaminated by work that was never
needed. Deferral is the contamination defense.

## The stages (mining-ops model)

```
EXCAVATION  →  ASSAY/INTAKE  →  TRANSPORT/MANIFEST  →  ITERATION  →  (SHIP)
  mine          three-way sort      catalogue +           build
  raw material  cart/tailings/      frontmatter,          MVP-first,
                bench               address               done-gate
```

- **Excavation** (`stages/01-excavation/`) — pull raw material from located deposits into `vault/`.
  No judgment (deferred). Just extraction. Tooling-checks only. See its `CONTRACT.md`.
- **Assay / Intake** (`stages/02-assay/`) — the seam deferral point. Sort each piece three ways
  only when it arrives: **cart** (on-seam → `carts/`), **tailings** (off-seam → `tailings/`,
  reviewable), **bench** (uncertain → `bench/`, escalate). Front half of MWP. See its `CONTRACT.md`.
- **Transport / Manifest** (`stages/03-manifest/`) — load carted material, catalogue it, stamp it
  with frontmatter (clean categorical data, born here), address it. The manifest lives in `manifest/`
  (`index.md` cargo list + `items/`). See its `CONTRACT.md` and `frontmatter-schema.md`.
- **Iteration** (`stages/04-iteration/`) — build the deliverable from the manifest, MVP-first,
  stopping when marginal utility of another pass drops below its cost. The concrete build chain is
  supplied by the instantiation, not core. Back half of MWP. See its `CONTRACT.md`,
  `iteration-workflow.md`, `evaluator-rubric.md`, and `done-gate.md`.

## The non-negotiables (full text in platform/PRINCIPLES.md)

1. **You do not think; you execute.** Judgment lives in the files. When uncertain, bench it or log
   it and stop. You never guess.
2. **Defer, do not commit.** Nothing is evaluated, executed, or loaded into context until it is
   needed. Deferral keeps the context clean — it is the contamination defense, not a delay tactic.
3. **Earned, not given.** The repo starts empty except this law. Every piece of content earned its
   place by clearing a deferral point. Nothing is entitled to exist.
4. **Glass-box.** State is the filesystem. No hidden state. If you must remember, write it down.
5. **Single-agent.** No subagents, no parallel orchestration. One agent, one path, depth-first.

## The deferral points (gates) — full text in platform/GATES.md

A gate is a **deferral point**, not a performance test. It does not ask "is this good." It asks
"is the context clean enough to commit the next step, and does the output conform to its shape."
Quality lives in the shape of the design schema, not in a gate's opinion. Three kinds:

- **Tooling checks** (excavation) — did the extraction reach and cover its bounded space.
- **The assay** (intake) — three-way seam sort, evaluated only when material arrives. Three exits
  by nature: cart, tailings, bench. The bench exit is how the pipeline runs without a human while
  never committing to an uncertain call.
- **Conformance + done-gate** (iteration) — did the deliverable conform to its design schema, and
  has marginal utility dropped below cost (ship) or not (iterate).

## Logging is mandatory and continuous (platform/LOGS.md)

- `logs/failures.md` — anything that broke, did not match the spec, or that you stopped on.
- `logs/gate-checks.md` — every deferral point firing: what passed, what was carted/tailed/benched.
- `logs/rejections.md` — every tailings decision with its reason (the reviewable recycle bin index).
  Off-seam is never deleted; it is tailed with a reason so it can be reclaimed if scope shifts.

## Your standing orders

Execute the spec literally. Verify, do not improve. Log everything. Defer, do not commit early.
When uncertain, bench or log and stop. Never guess. Never fork or clone anything. Never wire a tool
until a later pass says to. The frame before the material; the material before the tools.
