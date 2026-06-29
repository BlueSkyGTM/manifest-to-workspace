# CONTRACT — Stage 02: Assay / Intake

## Purpose
Sort every piece of material from vault/ three ways by seam-fit. The system's primary deferral point
and the front half of MWP. Seam-fit is evaluated only when material arrives — never speculatively. An
assay is intrinsically THREE-way; this is not a feature, it is what assaying is.

## Inputs
- Raw addressed material in vault/.
- The seam definition (what belongs to this domain). On the pilot the seam is held by the human; the
  seam layer accretes from the human's calls. See ARCHITECTURE.md §2.2.

## Process (per piece)
1. Test seam-fit against known on-seam edges.
2. Route by the THREE-WAY rule below.
3. For tailings, write the curated reason to logs/rejections.md (item + reason, not verdict-only).
4. For bench, write the escalation context (what it is, which edges it was near, why uncertain).
5. Log the firing to logs/gate-checks.md (outcome: carted | tailed | benched).

## The three-way rule (mechanical)
- CART  → carts/    : matched a known on-seam edge with NO conflicting near-miss. Clean yes.
- TAILINGS → tailings/ : clearly off-seam (good material, wrong seam). Tail it WITH a reason.
- BENCH → bench/    : did NOT cleanly match AND did NOT cleanly fail. ANY ambiguity benches.

Test for cart-vs-bench: "matched a known on-seam edge with no conflicting near-miss, yes/no?" A clean
yes carts. ANY ambiguity benches. You do NOT resolve ambiguity — you defer it to the bench. The bench
is the third exit that lets the pipeline run without committing to an uncertain call.

## Outputs
- carts/    : on-seam material, ready for transport.
- tailings/ : off-seam material, each item tailed with a reason (reviewable, never deleted).
- bench/    : uncertain material, escalated to the human.

## Deferral point
The assay IS the deferral point. Three exits. No fourth option. No guessing. No deleting (off-seam is
tailed, not destroyed). Uncertain commitment is deferred to the human via the bench.

## Pilot note (training the seam)
On the pilot the HUMAN makes the cart/tailings/bench call and the system RECORDS each with its reason.
The seam layer is the OUTPUT of the pilot, not the tool it starts with. Tag the highest-value cases
distinctly: looks-transferable-but-off-seam (resembles the seam via a shared principle but sits off
it). These are the ~50%-of-waste cases.

## TODO(tools)
The seam-matching tool (the seam brain) is wired in a LATER pass. For this pass, the assay routes by
HUMAN call and the structure (three exits, three folders, the reason-logging) is what you build.
Leave tool wiring as TODO(tools).

## Do not
- Do not collapse the three exits into two. The bench is non-negotiable.
- Do not delete off-seam material. Tail it with a reason.
- Do not resolve an uncertain call yourself. Bench it.
