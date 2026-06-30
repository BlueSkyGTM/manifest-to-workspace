# CONTRACT — Stage 03: Transport / Manifest

## Purpose
Transport carted material to the destination, catalogue it, and stamp each piece with frontmatter —
clean categorical data BORN at this stage, when each piece is individually in hand. The manifest is
the itemized, addressed cargo list.

## Inputs
- On-seam material in carts/, each piece carrying its **cart record** frontmatter (id, source, assay,
  seam_match) written by the assay (stage 02). The INHERITED manifest fields come from this record; if
  a carted piece is missing its cart record (no seam_match), it cannot be typed — bench it or log to
  logs/failures.md and stop. Do not invent seam_match.

## Process (per piece)
0. Process only carts whose cart record is `consumed: false`. A later loop's manifest ignores
   already-transported (`consumed: true`) carts — the boundary is produced state, not a counter.
1. Address the piece by REUSING its inherited `id` (minted at excavation, already repo-unique — do not
   re-mint) and transport it to `manifest/items/<id>.md`. Flip the cart record to `consumed: true`
   (it has been transported).
2. Stamp frontmatter per frontmatter-schema.md. Descriptive fields are OBSERVED, not judged; the rest
   are inherited from the assay or assigned mechanically (see the schema's field classes).
3. Catalogue the piece: append its row to `manifest/index.md` (the cargo list).
4. Keep the human-readable directions in the body; the countable fields live in frontmatter.

## Outputs
- The manifest at `manifest/`: `index.md` (the itemized, addressed cargo list, one row per item) and
  `items/<id>.md` (the transported pieces, each frontmatter-typed). See `manifest/CONTEXT.md`.

## Deferral point (conformance — invariant)
NOTHING UNCATALOGUED AND NOTHING UN-TYPED. Every carted piece has a manifest entry AND every required
frontmatter field is present. A piece missing a required field is NOT on the manifest — route it to
bench/ (the field could not be observed) or logs/failures.md, and stop. Do not invent a field value
to make it pass.
Log the firing to logs/gate-checks.md.

## Do not
- Do not put a data warehouse or external database here. Frontmatter on md files IS the structured
  layer. No new infrastructure.
- Do not judge content quality. Frontmatter is descriptive (what something IS), never prescriptive
  (what it should be). If a field requires judgment rather than observation, it does not belong in the
  schema — flag it to logs/failures.md.
