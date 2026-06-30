# seam.md — TEMPLATE (fill with YOUR domain; no content here is real)

The engine defines the seam GENERICALLY (operator-supplied boundary; on/off/uncertain; necessarily
incomplete; accretes via logged human calls — see `stages/02-assay/CONTRACT.md`). This file supplies
the CONCRETE boundary for one domain. Replace every <...> below.

## The domain
On-seam = <what belongs to your domain — be specific about its shape>.
Off-seam = <everything else, including material that shares a principle but does not belong>.

## The boundary is discovered, not authored
The seam starts incomplete. On the first run the HUMAN makes each cart/tailings/bench call and the
system records it with its reason (core `platform/LOGS.md`); the seam layer accretes into
`learned-seam/`. It is "load-bearing" once escalations to the human are rare.

## Positive tests (the assay needs both — see the assay contract)
- on-seam test: <what, observably, makes a piece a clean on-seam match>
- off-seam test: <what, observably, is positive evidence a piece is off-seam (a known off-seam pattern,
  or membership in another identified domain)>
Neither passes cleanly -> bench.

## Highest-value training data: looks-transferable-but-off-seam
<List your domain's near-miss patterns — material that resembles the seam via a shared principle but
sits off it. These are the cases the seam layer exists to catch. Tag them distinctly in
logs/rejections.md.>

## Negative-space control (for blind grading)
<Name the off-seam material you will quarantine in answer-key/ to grade the blind run. The model never
sees it.>
