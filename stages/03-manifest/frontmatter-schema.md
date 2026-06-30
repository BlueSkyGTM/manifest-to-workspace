# Frontmatter Schema — Stage 03 (Descriptive, Not Judged)

Every manifested item carries this frontmatter block. No field may require the agent to JUDGE quality
or seam-fit — frontmatter is descriptive (what something IS), never prescriptive. If a field would
require a quality/seam judgment, it does NOT belong here — flag it to logs/failures.md. Fields fall
into four CLASSES (the chat draft wrongly called all of them "observed"; stamping the stage is not
observing one):

```yaml
---
id: <stable unique address>               # INHERITED:  minted at excavation (the vault id), reused verbatim
source: <deposit / cluster it came from>  # OBSERVED:    a fact carried from excavation
format: <md | csv | code | doc | pdf | ...>  # OBSERVED:    the file type
assay: cart                               # INHERITED:   carried from the assay verdict (it was carted)
seam_match: <the on-seam edge it matched>  # INHERITED:   recorded BY the assay (human on pilot)
stage: manifest                           # ASSIGNED:    the current stage
sealed: false                             # LIFECYCLE:   flips true when it clears its final gate downstream
---
```

Field classes: **OBSERVED** = a fact read off the material. **INHERITED** = carried verbatim from an
upstream stage's record (not re-judged here). **ASSIGNED** = minted mechanically at this stage.
**LIFECYCLE** = state that changes as the item moves downstream. None of the four is a judgment.

## Rules
- No field is judged. `id` is INHERITED from excavation (the vault address, minted at haul time and
  reused verbatim — never re-minted here); `seam_match` and `assay` are INHERITED from the assay (the
  human on the pilot), never re-judged here.
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
method: there is no fork to evolve into. The DESCRIPTIVE fields (source, format, seam_match, etc.)
therefore drive NO routing — do not build an evolution/variance trigger reading them; there is nothing
for it to route to. The ONE exception is the `sealed` LIFECYCLE flag: it is not a variance stat but a
lifecycle GATE — iteration builds only from `sealed: false` items and treats `sealed: true` as terminal
(and quotable by siblings). That is lifecycle filtering, not the retired variance routing. The variance
idea is recorded here as considered-and-dropped so its absence is intentional, not an oversight.
