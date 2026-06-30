# account.md — Vault-Level Index (the routable account, append-only)

Every piece excavation hauls gets one row here, so vault/ is a routable account, not a pile. Stage 01
appends; stage 02 (assay) reads. Starts EMPTY (nothing hauled yet). Exists as ready infrastructure,
like the logs.

# Row format (one per hauled piece):
# - id: <stable address> | source: <deposit> | format: <md|csv|code|doc|pdf|...> | bounded_space: <coverage target> | consumed: false
#
# `consumed: false` is minted by excavation and flipped true by the assay when it routes the piece
# (cart/tailings/bench). The assay processes only `consumed: false` rows — this is what lets a LATER
# loop ignore already-routed material without a run counter (progress marked by production; GATES.md §4).

