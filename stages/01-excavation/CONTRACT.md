# CONTRACT — Stage 01: Excavation

## Purpose
Pull raw material from located deposits into vault/. No quality judgment (deferred to the assay).
Coverage, not choice.

## Inputs
- A located deposit (a source to extract from). On the pilot this is provided by the human.
- The bounded space to cover (what counts as "the whole deposit").

## Process
1. Extract the material from the source.
2. Haul EVERYTHING that meets the extraction criteria. Do not select, filter, or judge worth —
   selection is deferred to the assay (stage 02). Coverage is the goal.
3. Land each piece in vault/ with an address.
4. Record each piece in the vault account (address + source + format).

## Outputs
- Raw material in vault/, addressed.
- A vault-level account: every hauled piece has an address. Nothing unaddressed.

## Deferral point (tooling checks only — NO quality judgment)
- Reach: did the extraction reach the source? (pass/fail)
- Coverage: did it cover the whole bounded space? Report anything that came back empty as explicitly
  as anything that hit — an empty result is DATA, not a reason to move on.
- Addressability: did every hauled piece land in vault/ with an address?
Log the firing to logs/gate-checks.md.

## TODO(tools)
The extraction tool (e.g. an authenticated browser/extractor) is wired in a LATER pass. For this
pass, leave this as TODO(tools). Do NOT install or invoke any extraction tool now.

## Do not
- Do not judge whether any piece is "good" or "relevant." That is deferred to the assay.
- Do not delete or skip material to be efficient. Haul the whole bounded space.
- Do not load vault/ contents into context. They sit addressed on disk until pulled (M2W).
