# platform/SKILLS.md — Standing Skills (State / Memory Harness Layer)

These skills run periodically across the pipeline's life. They are the State/Memory harness layer:
what keeps the agent from losing the thread across sessions. CRITICAL RULE: they write durable
output to READABLE FILES (logs, learned-seam files, session-handoff), never silently into a blackbox
store. The pilot depends on the seam learning being inspectable so the human can grade it. A skill
that buries decisions in an unreadable index defeats the answer-key check.

## context-compressor (periodic)
When context grows long or a session is wrapping, compress history into a structured summary WITHOUT
losing critical state. Writes the summary to a readable session file, not into hidden memory. Used
to hand off between sessions during long runs (a first run can span sessions).

## memory-manager (on durable decisions)
When a standing decision is made — a seam call with its reason, a schema cut, a routing rule —
memory-manager records it. RULE: it writes to the readable learned-seam files and the logs, which are
THEN optionally indexed into a retrieval store IF the instantiation wires one (the concrete store is a
domain choice, named in the pilot). The file is canonical; the store is only a projection. Never write a seam decision only into a store — the
human must be able to read and grade it from the files alone. (If no store is configured, the readable
files ARE the system and nothing is lost.)

## session-handoff.md (the clock-out note)
At the end of each working session, write/update session-handoff.md at root: what is verified, what
changed, what is broken, the single next-best step. The next session reads this first. This is the
cross-session thread the agent would otherwise lose.

## Why readable-file-canonical matters here specifically
A domain's first run IS the seam layer's training run. Every seam call the human makes is training
data. If memory-manager compresses those calls into a blackbox, the human cannot check whether the
seam layer learned the right boundary. Readable files preserve the answer-key check. This is not a
style preference; it is what makes a run gradeable.

## gstack at the gates — where the team's plays fire (the gate→play map)
gstack is the **operating TEAM** that runs this engine, not an item in the toolbox; the actual *tools*
(ponytail, LLMLingua, gbrain, the memory skills) are instruments the team wields. This map says WHICH
of the team's plays fire at WHICH deferral point. Two laws bind every row: a play fires ONLY in its
adopted single-agent surface (the fence in `platform/TOOLING.md`), spawning NO subagents (PRINCIPLES
#5); and a play at a gate checks **conformance, not quality** (GATES.md) — it never judges "is this
good." Plays marked *conditional* fire only when the deposit/deliverable actually has that surface
(wire late, the subset needed). The map names only the engine's own deferral points and the team's
plays — never a pilot or domain.

| Deferral point | gstack play | constraint (so it stays inside the law) |
|---|---|---|
| **SETUP** (cold boot) | tool scan; `/guard` `/freeze` `/careful` | edit-lock / destructive-action safety harness only — never a decision gate, never approval to touch unrelated files |
| **Conditional setup** (only when the run needs it, NOT cold-boot) | `/setup-browser-cookies` (just before excavation, for auth'd deposits); `/setup-gbrain` `/sync-gbrain` (only if memory is configured/approved — gbrain is a projection, must never block); `/setup-deploy` (only if the deliverable is deployable) | wired by actual need (a located auth'd deposit, configured memory, a deployable deliverable), not speculatively |
| **Excavation** (vaulting web/video deposits) | `/browse` + `/open-gstack-browser` (reach & haul into addressable markdown); `/scrape` (structured page data); `/skillify` (codify a PROVEN recurring scrape) | MECHANICAL reach/coverage only — NO relevance filtering (selection is the assay's, deferred); `/skillify` codifies coverage/extraction, never relevance logic; ambiguous route → bench / logs/failures.md |
| **Assay** (seam sort) | Confusion Protocol → bench ONLY | NO `/spec` / `/review` / `/qa` / `/autoplan` — the seam is the engine's own brain; a review/plan skill here contaminates the routing call. Any ambiguity benches |
| **Manifest** (catalogue) | none | mechanical cataloguing + frontmatter; no gstack play (unless a conformance check fails → logs/failures.md and stop) |
| **Iteration — pre-build** | `/spec` ALWAYS; `/autoplan` *conditional*; `/design-consultation` `/design-shotgun` *conditional* | `/spec` is the craftsmanship floor, NOT the seam gate; `/autoplan` fires only on real eng/design/DX surface (never a trivial/pure-content build — MVP-first + done-gate already govern completeness); the design skills only when the deliverable needs a design SYSTEM, and only to produce/refine schema inputs, not to judge quality |
| **Iteration — build** | `/diagram` (mermaid/excalidraw); `/design-html` *(if HTML/web)* | visual rendering is deferred here from excavation and must be SOURCE-PINNED (only data that traces to the manifest) |
| **Iteration — conformance** (sequential fresh-context evaluator, worker ≠ checker, NO subagents; checks CONFORMANCE + source-fidelity, never "is it good") | `/review` + `/codex`; `/design-review` *(visual — schema fit, not "is it beautiful")*; `/qa` `/qa-only` *(interactive/web — schema-derived acceptance; report broken behavior/source-drift, not "good UX")*; `/cso` *(only with a security surface — secrets/auth/deploy/untrusted input/deps; explicit obligations, no speculative scope)*; `/benchmark` *(only if performance is in the schema/contract)* | never spawns concurrent workers; never judges quality where the schema's shape owns it |
| **Done-gate → ship** | `/ship` or `/land-and-deploy` *(land only if deployable)*; `/make-pdf` *(PDF deliverable)*; `/document-generate` `/document-release`; `/canary` *(post-deploy)* | only AFTER the done-gate fires (substance→surface = ship); `/document-generate` may run before the gate only if docs are part of the deliverable schema; release docs document what shipped, never reopen substance |
| **Loop boundary** (after ship) | `session-handoff.md` + save-memory (context-compressor + memory-manager) + gbrain push/query; `/context-save` `/context-restore`; `/learn`; `/retro` | SESSION continuity ONLY — write/recall via READABLE files first (then optionally index); this is NOT loop auto-continuation, the loop restarts solely on operator deposits (GATES.md §4); `/learn` never buries a seam call in blackbox memory; `/retro` must not trigger a new loop |
| **Failure handling** (cross-cutting) | `/investigate` | only when a stage MECHANICALLY stops/breaks — root-cause and log to `logs/failures.md`; must NEVER auto-resolve a benched seam ambiguity or "fix" an uncertain call (uncertainty benches/stops, PRINCIPLES #1) |
| **Engine-maintenance** (working on M2W itself, NOT a pilot run) | `/health`; `/gstack-upgrade` *(human-aware if it mutates the toolchain)*; `/benchmark-models` *(model/prompt selection only)*; ponytail | not pilot-run gates; ponytail is OFF during a content deliverable build |

**Rejected (the fence, PRINCIPLES #5):** `/pair-agent`, Conductor parallel sprints, sub-agents /
maker-checker parallelism — all violate single-agent. Cross-model review is the SEQUENTIAL fresh-context
evaluator, not a parallel worker.

**The map is OPEN, not a closed allowlist — dormant plays wake on domain need.** The rows above are the
universal DEFAULT: the prebuilt triggers that fire on ANY domain. They do NOT exhaust the suite. The
active pilot may WAKE any other gstack play its domain warrants, at the matching deferral point, under
the same invariants (single-agent surface; conformance-not-quality; the hard edges — nothing at the
assay, no subagents at conformance, the loop boundary stays session-continuity). A play that did not
make the core cut is DORMANT, not rejected; the ONLY truly rejected plays are the parallel/multi-agent
ones above. Examples: a pilot whose deliverable is a **website** wakes `/qa`, `/design-html`,
`/design-review`, `/canary`; an **iOS-app** pilot wakes the `/ios-*` suite (`/ios-qa`, `/ios-fix`,
`/ios-design-review`, …) at build/conformance. These are absent from the core map precisely because
they are domain-specific — the kind of choice the pilot owns. The pilot declares the plays it wakes
(and at which gate) in `pilots/<name>/tooling.md`; core declares the open default, the pilot wakes what
its domain needs.

**Tools the team wields freely (NOT gated):** **ponytail** (code minimization — engine-maintenance
only, OFF during a content build), **LLMLingua** (compress INPUT context only — never the deliverable,
never replacing readable canonical files), **gbrain** (the optional recall projection), the **memory
skills**. These are instruments, not gates; the team reaches for them whenever useful within any
deferral point.

(This map was settled by deferring to gstack's own reviewed-plan voice — "who knows when gstack should
fire better than gstack" — and validated cross-model twice, the second pass tightening every play to
conformance-not-quality and conditional-on-surface. See changelog 2026-06-30.)
