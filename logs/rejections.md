# rejections.md — Tailings Index (append-only, reviewable recycle bin)

# Off-seam material is NEVER deleted. It is tailed here WITH a reason so it can be reclaimed if scope
# shifts. Capture item + curated reason, never verdict-only. The looks-transferable-but-off-seam
# category is the highest-value training data — tag it distinctly.

# Entry format (see platform/LOGS.md):
# ## <ISO-8601 timestamp>
# - item: <address in tailings/>
# - verdict: tailings
# - reason: <curated reason it is off-seam — human-written on the pilot>
# - category: <off-seam | looks-transferable-but-off-seam>
# - reclaimable-if: <what scope shift would make this material on-seam again>

