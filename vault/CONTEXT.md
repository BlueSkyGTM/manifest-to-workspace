# CONTEXT.md — vault/
This folder starts EMPTY and is correct when empty. Do not populate it in the skeleton pass.

Role: INGEST holding — the ONE door into the pipeline. Raw excavated material lands here, addressed
and accounted, BEFORE any assay. Filled by stage 01 (excavation); read by stage 02 (assay).

Lifecycle note (vault is the exception to "earned, not given"): material here is **admitted and
accounted**, NOT seam-earned. "Earned by clearing a deferral point" governs the DOWNSTREAM folders
(carts/, tailings/, bench/, manifest/, library/) — a piece reaches those only by passing a gate.
Vault is upstream of the first gate, so nothing here has earned anything yet; it has only been hauled
and addressed. What vault requires is not a verdict but an ADDRESS: every piece is in `vault/account.md`
(address + source + format + bounded-space + `consumed: false`). Unaddressed material in vault is the
only error state.

Nothing is loaded into context from here speculatively (M2W) — downstream stages follow an address to
one piece, never absorb the pile.
