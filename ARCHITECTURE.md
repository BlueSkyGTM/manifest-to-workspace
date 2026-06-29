# ARCHITECTURE.md — M2W (Manifest to Workspace)

The consolidated specification. This is the single document that carries the full design out of
the conversation that produced it. The per-stage `CONTRACT.md` files are authoritative for
*building*; this document is authoritative for *understanding why*. Where they appear to conflict,
the contracts win for execution and this document is updated to match.

---

## 0. The system and its components

- **M2W — Manifest to Workspace** is the system: the transform from catalogued material to built workspace. Its governing discipline is deferral. The whole
  pipeline withholds evaluation and execution until acting is safe — until the context is clean
  enough that committing will not contaminate the result. M2W is not a performance model where work
  proves itself at each gate; it is a contamination-defense model where judgment and execution are
  deferred until genuinely needed.
- **ECP — Earned Contract Plumbing + Pipelines** is the component layer beneath M2W: the plumbing
  that moves material and the pipelines that process it.
- **MWP — Model Workspace Protocol** is the engine-principle layer M2W borrows for routing and
  stage execution. M2W splits MWP in two: its **intake** half becomes the assay; its
  **stage-building** half becomes iteration. MWP is the bridge between data and execution, not the
  destination.

ICM (Interpreted Context Methodology) is the lineage MWP descends from. It is **deliberately
excluded** from this build to keep it from hijacking the infrastructure. We borrow principles, not
the engine, and we never fork it.

(Historical note: an earlier name for the system was "ECA — Earned Contract Architecture." It is
retired. ECA emphasized work proving itself at every gate — a performance framing. M2W replaces it
because the system is the opposite: a carefully deferred pipeline that avoids context contamination
by withholding commitment, not one that demands performance at each checkpoint.)

---

## 1. The governing discipline: M2W

Defer every evaluation, execution, and judgment until the moment it is actually required.

Consequences that are load-bearing, not stylistic:
- A stage runs because the prior stage produced something that **requests** it, not because the
  stage exists. Nothing fires speculatively.
- Nothing is loaded into context speculatively. Material sits addressed on disk until pulled. This
  is the contamination defense: context stays clean because only what is needed is ever present.
- The pipeline can stop cleanly at any point, because nothing downstream has been committed.
- A bad input never triggers the expensive downstream work, because that work was deferred until a
  request the bad input never produces. Dead-end material dies cheap, and it dies without
  contaminating anything downstream.

M2W is why the front of the pipeline is allowed to be slow and judgment-free, why the cost of a
mistake is pushed to the front where it is cheapest, and why the context never fills with work that
was never needed.

---

## 2. The pipeline (mining-ops model)

```
EXCAVATION → ASSAY/INTAKE → TRANSPORT/MANIFEST → ITERATION → SHIP
```

The mining analogy is exact, not decorative: prospect and mine the material, **assay** it at the
dig site before spending a cart on it, transport and catalogue what passes, process it at the
destination yard into a finished product. The hard part is not navigation (there is no maze to
solve); the hard part is deciding what material is worth processing — and deferring that decision
until the material is actually in hand. That is an assay problem.

### 2.1 Excavation — mine the material
Pull raw material from located deposits into `vault/`. No quality judgment occurs here; it is
deferred to the assay, because you do not yet know what is in the material. The only checks are
**tooling checks**: did the extraction reach the source, and did it cover the whole bounded space
(coverage, not choice — haul everything that meets the extraction criteria; selection happens
downstream). Output: raw material addressed in `vault/`, accounted for.

### 2.2 Assay / Intake — the three-way seam sort
This is the front half of MWP and the system's primary deferral point. Seam-fit is evaluated only
when material arrives — never speculatively. Every piece is sorted three ways:

- **Cart** (on-seam) → `carts/`. Matched a known on-seam edge with no conflicting near-miss. Earns
  transport.
- **Tailings** (off-seam) → `tailings/`. Good material, wrong seam. **Not deleted** — tailed with a
  reason so it can be reclaimed if scope shifts. A reviewable recycle bin, not a landfill.
- **Bench** (uncertain) → `bench/`. Did not cleanly match and did not cleanly fail. Escalated to the
  human. The bench is what lets the pipeline run without a human on the settled cases while never
  committing to an uncertain call.

The three-way sort is **intrinsic to assaying**, not a feature bolted onto a binary. A real assay
always returns ore, tailings, and "needs further testing." This dissolves the "how does the router
express uncertainty" problem: the uncertain pile exists by definition, and the test is mechanical —
"did it match a known edge with no conflict, yes/no." A "no" benches; it does not guess. Deferring
the uncertain call to the bench is the contamination defense at the seam: an uncertain piece never
enters the manifest to corrupt it.

**The seam.** The seam is the boundary defining what belongs to the domain in play (the operator supplies the domain; the system is agnostic
engineering, which is **civil-engineering-shaped** — assembling proven components into load-bearing
infrastructure others run on — **not** AI-systems/software-engineering-shaped, even though the
vocabulary overlaps). The seam layer is **necessarily incomplete** at the start; boundaries are
discovered by hitting them, not authored up front. It **accretes**: the human teaches edges during
the pilot (every assay call captured with its reason), and the seam layer grows more complete every
run. It is never "done"; it is "load-bearing" once escalations to the human are rare enough that the
human is not the bottleneck. The highest-value training data is the
**looks-transferable-but-off-seam** case — material that resembles the in-domain set because it shares a
DevOps/platform principle but sits off the seam. Tag these distinctly; they are the ~50% of wasted
effort the seam layer exists to prevent.

### 2.3 Transport / Manifest — catalogue and address
Carted material is loaded, catalogued, and stamped with **frontmatter** — clean categorical data
**born here**, when each piece is individually in hand. The manifest is the cargo list: an
itemized, addressed account of everything aboard. Frontmatter fields are observed, not judged (see
`stages/03-manifest/frontmatter-schema.md`). The invariant: **nothing uncatalogued and nothing
un-typed** — every item carries its categorical fields or it is not on the manifest.

This is where clean data is born in the purest sense — born structured at the moment of observation,
not parsed out of prose later. Human-readable directions stay in the body; the countable fields live
in frontmatter. No external database, no warehouse — frontmatter on `md` files is the structured
layer, globbed and counted when needed.

### 2.4 Iteration — build the deliverable
Back half of MWP and the destination yard. Build the deliverable from the manifest, governed by:

- **MVP-first.** The first iteration gets the thing off the ground — viable, unfinished, not
  perfect. Subsequent iterations either finish what stands or dismantle it for the next MVP.
- **Conformance, not quality.** Each iteration is checked for whether it applied its **design
  schema** (the cookie-cutter that holds the quality judgment, cut from logs) — not whether it is
  "good." Quality lives in the schema's shape; the gate checks conformance.
- **The done-gate is diminishing marginal utility.** Ship when another iteration would add less than
  it costs. Detected mechanically as the **substance-to-surface transition**: when a pass stops
  adding content that survives the schema and starts only reordering or rewording, marginal utility
  has dropped below cost — ship. See `done-gate.md`.
- **Dismantle is capped by the same economics.** A teardown is permitted only while its marginal
  utility exceeds its cost. Once a rebuild would add less than it costs, the gate refuses the
  teardown and ships what stands. This prevents the infinite-rebuild (perfectionism) loop.

---

## 3. MWP, split in two

MWP was never one tool; it was intake bolted to stage-building. M2W un-bolts them and places them at
**opposite ends** of the line, with transport between:

- **MWP intake half → the Assay (stage 02).** Routing-in, the three-way seam sort, context folders
  that decide what each downstream stage sees.
- **MWP stage-building half → Iteration (stage 04).** The governed build stages, stage contracts
  (Inputs / Process / Outputs), output-folder handoffs.

The principles M2W implements **natively** (no external repo): five-layer routing
(CLAUDE → CONTEXT → stage → refs → artifacts), stage contracts, numbered-folder sequencing,
output-folder handoffs, one-way references, canonical sources, selective routing, glass-box
observability, single-agent execution. See `platform/MWP.md`.

---

## 4. What makes the pipeline run without a human

Three things, all built into MWP's routing and context folders:

1. **Task routing** runs the settled cases autonomously — known route in, known stage out.
2. **Context folders** hand each stage exactly what it needs and nothing else (and nothing else is
   the contamination defense — the stage's context cannot be polluted by material it should not see).
3. **The third exit (bench).** The assay routes the *unsettled* cases to `bench/` instead of
   committing. The human empties the bench; the bench shrinks every run as the seam layer learns.

The human is the **source** of seam judgment, not the permanent **gate**. On the pilot the human
makes every assay call manually and the system learns from each. Over runs, the system handles
settled edges autonomously and escalates only novel boundaries. The human never fully leaves the
loop — they become an **escalation target** for genuinely hard cases, which is exactly where human
judgment is worth spending.

---

## 5. Logging (built in, continuous, mandatory)

Three streams (`platform/LOGS.md`):
- `logs/failures.md` — anything that broke or that the agent stopped on.
- `logs/gate-checks.md` — every deferral point firing and its outcome (passed / carted / tailed /
  benched / ship / iterate).
- `logs/rejections.md` — every tailings decision with its **reason** (the reviewable index of the
  recycle bin). Off-seam is never deleted; it is tailed with a reason so it can be reclaimed if
  scope shifts.

The logs are not a postmortem. They are the system learning its own shape and — during the pilot —
the training corpus for the seam layer. The discipline that makes them training-grade: each
rejection captures the **full item plus the curated reason**, not just a verdict. Verdict-only
teaches a brittle blacklist; item-plus-reason teaches the principle that generalizes.

---

## 6. The pilot (context for why the skeleton is shaped this way)

The first real run is the **GTM systems curriculum**, chosen because it is reversible (no live
leads, nothing that cannot be taken back). The model is kept **blind** to the fact that some source
material (e.g. an AI-engineering curriculum fed in as if it were GTM) does not fit — forcing it to
discover the negative space by reasoning rather than confirming a told answer. The pilot is the seam
layer's **training run**, not its first deployment: the human makes the seam calls, the system
records them, and the seam layer is the **output** of the pilot, not the tool it starts with.

Tools (extraction, the seam brain, etc.) are wired in a **later pass**, after the human reviews this
skeleton. This pass builds only the frame and the logs.

---

## 7. Glossary pointer

All terms used literally are defined in `platform/glossary.md`. If any term in any contract is
ambiguous, the glossary is authoritative; if the glossary is silent, bench the question and stop.
