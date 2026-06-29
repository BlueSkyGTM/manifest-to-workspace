# DRY-RUN.md — The Fidelity Ladder (curriculum loop → GTM)

The only way to know whether the open items work is to run the pipeline. The run is staged by
FIDELITY and CONSEQUENCE: each pass carries more consequence than the last. This is M2W applied to
its own validation — defer consequence, not just execution.

## Why Pass 1 is a LOOP, not a single run

The apprenticeship is watch → propose → act. ONE run cannot contain it: you cannot watch the seam
learn, then have it propose, then have it act, all in a single pass. So Pass 1 is the curriculum
RE-RUN — the same reversible domain, the seam taking over progressively across sub-runs. The loop IS
the apprenticeship. The curriculum is reversible, which is exactly what makes iterating it safe.

The critical property this fixes: without the loop, the FIRST time the seam acts autonomously is on
live, irreversible GTM (Pass 2). That is backwards. The seam must act autonomously on REVERSIBLE
ground first, where a wrong call costs a thrown-out dig. The loop provides that ground.

## PASS 1 — CURRICULUM-MAZE (a loop)

### Pass 1 scoping (do not over-load)
Wire ONLY:
- the skeleton + logs (built)
- ONE excavation extractor (match to the material's deposit type)
- the manifest + frontmatter
- iteration with the capstone workflow + evaluator (Accept/Revise/Block)
- context-compressor + memory-manager writing to READABLE files
Do NOT wire the full tool set. If everything is loaded and something breaks, the failure could be the
maze OR any of several tools — an undiagnosable haystack. Test the walls first.

### Run 1a — HUMAN CALLS (train)
- The HUMAN makes every cart/tailings/bench call; the system logs each WITH its reason.
- This tests the walls (flow excavation→assay→manifest→iteration; the three assay exits; the gates;
  the logs) AND produces the seam's training data.
- The seam does NOT act here. It watches and records.
- **AUDIT (required before advancing):** read the learned-seam files. Are the human's calls
  consistent? Is the reasoning capturable and generalizable (not a per-item blacklist)? Correct bad
  training now — it is sticky and carries forward.

### Run 1b — SEAM PROPOSES (correct)
- The seam (now holding 1a's training) PROPOSES a verdict per piece; the human confirms or corrects.
- Corrections are the highest-value training — they mark exactly the boundary the seam got wrong.
- **AUDIT (required):** did corrections drop vs 1a? If the seam is still wrong on SETTLED cases, it
  has not learned the boundary — stay in 1b, re-run, do not advance.

### Run 1c — SEAM ACTS, BLIND (grade)
- The seam acts AUTONOMOUSLY. The human spot-checks; does NOT pre-approve each call.
- **This is where the BLIND-DISCOVERY experiment runs** (the one designed and, until now, never
  scheduled): feed material the seam has not seen — including the AI-engineering curriculum fed as if
  it were GTM — and require the seam to discover the negative space by reasoning. Grade against the
  QUARANTINED seam corpus (the answer key the human holds and the model does NOT).
- Still reversible — a wrong call costs a thrown-out dig.
- **PASS CONDITION:** the seam sorts blind material correctly against the corpus, with rare
  escalation to the bench. Only then is the seam trusted. If it carts an off-seam item or benches
  everything, it has not learned — return to 1b.

## Reversibility, precisely (the asterisk)
- The DELIVERABLE (the course) is reversible — throw it away, no cost.
- The LEARNING is STICKY-BUT-INSPECTABLE — it persists into Pass 2. The between-sub-run AUDITS are
  REQUIRED, not optional, because bad training carries forward into live work.
- "Reversible" means the DIG is throwable. It does NOT mean nothing is at stake — the seam's training
  QUALITY is at stake, and that is what the audits protect.

## PASS 2 — GTM SYSTEM (live, gated on 1c)
Live, irreversible (touches real leads/systems). Details deferred entirely for now. Runs ONLY after
1c holds. Because the seam already acted autonomously on reversible ground in 1c, Pass 2 is NOT the
seam's first autonomous act — which was the entire point of the loop. This is where operational-GTM
(currently tailed off-seam) becomes on-seam.

## BLOCKED-ON-RUN (the questions the run exists to answer — not to-do items)
- gtm-starter-kit fold into the seam corpus (curriculum-pass material; fold before 1a ONLY if it is
  answer-key material you will grade 1c against, else deferred)
- GBrain vs Understand-Anything final split (needs a wiki produced in the loop to test GBrain ingest)
- the seam layer itself (it is the OUTPUT of the 1a→1c loop, not an input)
- whether conformance-by-shape holds (first tested when material flows the gates, 1a)
- whether the done-gate's substance-to-surface signal fires correctly (first tested across iterations)
- whether the seam discovers the boundary BLIND (the 1c experiment)
