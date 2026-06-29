# CONTRACT — Stage 03: Transport / Manifest

## Purpose
Transport carted material to the destination, catalogue it, and stamp each piece with frontmatter —
clean categorical data BORN at this stage, when each piece is individually in hand. The manifest is
the itemized, addressed cargo list.

## Inputs
- On-seam material in carts/.

## Process (per piece)
1. Catalogue the piece (give it a manifest entry).
2. Stamp frontmatter per frontmatter-schema.md. Fields are OBSERVED, not judged.
3. Address the piece (a stable, unique address).
4. Keep the human-readable directions in the body; the countable fields live in frontmatter.

## Outputs
- A manifest: the itemized, addressed list of everything carted, each entry typed by frontmatter.

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
