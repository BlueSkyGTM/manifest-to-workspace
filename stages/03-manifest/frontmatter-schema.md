# Frontmatter Schema — Stage 03 (Observed, Not Judged)

Every manifested item carries this frontmatter block. Every field must be OBSERVABLE at catalogue
time without judgment. If a field would require the agent to judge quality or seam-fit rather than
observe a fact, it does NOT belong here — flag it to logs/failures.md.

```yaml
---
id: <stable unique address>               # observed: assigned at catalogue time
source: <deposit / cluster it came from>  # observed: known from excavation
format: <md | csv | code | doc | pdf | ...>  # observed: the file type
assay: cart                               # observed: it is here because it was carted
seam_match: <the on-seam edge it matched>  # observed: recorded by the assay (human on pilot)
stage: manifest                           # observed: current stage
sealed: false                             # observed: true once it clears its final gate (quotable by siblings)
---
```

## Rules
- Every field is observed, not judged. `seam_match` is recorded BY the assay (the human on the pilot),
  not re-judged here.
- `sealed` flips to true only when an item clears its final gate downstream. A sealed item is quotable
  by sibling work; an unsealed (in-flight) item is treated as not-yet-existing by siblings.
- The body of the file stays human-readable directions/prose. Frontmatter is the countable layer.
- The invariant from the contract: nothing uncatalogued, nothing un-typed. Missing a required field =
  not on the manifest = bench or logs/failures.md.

## TODO(tools)
Any tool that auto-populates or validates frontmatter is wired in a LATER pass. For this pass, the
SCHEMA is what you build (this file), not its automation.

## Note: frontmatter is DESCRIPTIVE, not a routing stat (retired idea)
An earlier design considered frontmatter as a COUNTABLE VARIANCE STAT feeding an evolution trigger —
a Tyrogue-style fork that would route low-variance material to one method and higher-variance
material to another (the ICM-vs-ECP fork). That fork was RETIRED when M2W collapsed to a SINGLE
method: there is no fork to evolve into. Frontmatter is therefore purely DESCRIPTIVE cataloguing plus
the `sealed` flag (which makes an item quotable by siblings). It drives NO routing. Do not build an
evolution trigger reading these fields — there is nothing for it to route to. The variance idea is
recorded here as considered-and-dropped so its absence is intentional, not an oversight.
