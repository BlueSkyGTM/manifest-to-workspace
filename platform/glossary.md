# platform/glossary.md — Authoritative Term Definitions

If a term in any contract is ambiguous, this glossary is authoritative. If the glossary is silent on
a term you need, BENCH the question and stop — do not guess a definition.

- **M2W** — Manifest to Workspace. The SYSTEM: the transform from manifest (catalogued material) to workspace (built deliverable). Governed by deferral. Defer every
  evaluation, execution, and judgment until needed; deferral is the context-contamination defense.
- **ECP** — Earned Contract Plumbing + Pipelines. RETIRED lineage name (see the lineage note at the
  bottom); never operationalized as a component layer. Do not build from it.
- **MWP** — Model Workspace Protocol. Engine principles for routing and stage execution, implemented
  natively. Bridge between data and execution. Split into Assay (intake) and Iteration (stage-building).
- **ICM** — Interpreted Context Methodology. MWP's lineage. EXCLUDED from this build. Do not use.
- **ECA** — Earned Contract Architecture. RETIRED earlier name for the system; superseded by M2W
  (which emphasizes deferral/contamination-avoidance over performance-at-gates). Do not use.
- **Material / ore** — raw excavated content before it has been assayed.
- **Excavation** — stage 01. Mining raw material from located deposits into vault/. No judgment.
- **Assay / Intake** — stage 02. The three-way seam sort: cart / tailings / bench.
- **Seam** — the boundary defining what belongs to the domain in play. Operator-supplied; the system
  is domain-agnostic. Necessarily incomplete; accretes over runs via logged human calls. The concrete
  domain shape and near-miss edges are an instantiation detail (`pilots/<name>/seam.md`), never core.
- **Cart** — on-seam material that earned transport. Lands in carts/.
- **Tailings** — off-seam material. Good but wrong seam. Tailed WITH a reason. Reviewable, never
  deleted. Lands in tailings/.
- **Bench** — uncertain material that did not cleanly match or fail the seam. Escalated to human.
  Lands in bench/. The third exit of the assay.
- **Transport / Manifest** — stage 03. Cataloguing carted material, stamping frontmatter, addressing.
- **Manifest** — the itemized, addressed cargo list of everything carted. Invariant: nothing
  uncatalogued, nothing un-typed.
- **Frontmatter** — clean categorical data stamped on each item at manifest time. Observed, not
  judged. The structured layer; body stays human-readable prose.
- **Iteration** — stage 04. Building the deliverable from the manifest. MVP-first.
- **Design schema** — the cookie-cutter that holds the quality judgment, cut from logs of proven-good
  work. Quality lives in its SHAPE. The conformance gate checks it was applied.
- **Conformance gate** — checks the design schema was applied and output conforms to its shape. Never
  checks quality.
- **Done-gate** — fires when marginal utility of another iteration drops below its cost. Detected as
  the substance-to-surface transition. Ship when flat.
- **MVP-first** — first iteration produces a viable, unfinished deliverable; later iterations finish
  or dismantle it.
- **Substance-to-surface transition** — the mechanical signal for the done-gate: when iterations stop
  adding content that survives the schema and start only reordering/rewording.
- **Deferral point / gate** — a checkpoint that defers commitment until the context is clean and the
  output conforms. Not a performance test.
- **Glass-box** — all state is on disk; no hidden state.
- **Tooling check** — a mechanical check on extraction (reach, coverage, addressability), not on
  content worth.
- **TODO(tools)** — a literal marker for where a tool is wired in a LATER pass. Leave it literal.

# --- Added: harness/dry-run terms ---
- **Evaluator / Accept-Revise-Block** — the independent checker (worker != checker). Three verdicts:
  accept (conforms, advances), revise (fixable, re-run), block (wrong, escalate). Conformance + source-
  fidelity, never quality.
- **Pilot vocabulary** — a domain's own terms (the name of its concrete iteration chain, the labels
  for its validation sub-runs, its deliverable shape) are defined by the instantiation, NOT core. See
  `pilots/<name>/`. Core declares only the generic interfaces: the iteration-workflow interface
  (`stages/04-iteration/iteration-workflow.md`) and the validation ladder (`DRY-RUN.md`).
- **context-compressor / memory-manager** — standing skills (State/Memory harness). Write durable
  decisions to READABLE files (canonical), then optionally index into a retrieval store (projection).
  Never blackbox-only.
- **session-handoff.md** — the clock-out note for cross-session continuity.
- **Blocked-on-run** — an item that cannot be decided until a run produces the information. Not
  unfinished; waiting on evidence the run will generate.

# --- Added: tooling-policy terms ---
- **gstack fence** — the split of gstack's surface into adopted (single-agent-compatible: Confusion
  Protocol, /freeze, /guard, sprint stages, /spec, /autoplan, a retrieval-store trust-triad) vs
  rejected (Conductor, sub-agents, /pair-agent — parallel, violates single-agent law). See TOOLING.md.
- **Minimalism scope** — a CODING minimalism rule, not a global one. ON for coding/tooling, OFF during
  content/deliverable iteration, where a minimalism ladder under-builds the deliverable. See TOOLING.md.
- **Variance thread (retired)** — the dropped idea of frontmatter as a variance stat driving an
  evolution-trigger fork. Moot once M2W went single-method; frontmatter is descriptive + sealed-flag,
  drives no routing.

# --- Lineage note (retirements, recorded not silent) ---
- **D2E (retired → M2W)** — the system was named D2E, "Deferred Evaluation and Execution," which
  named the LAW. Renamed to M2W, "Manifest to Workspace," to name the TRANSFORM instead. The deferral
  law did not change — it remains the governing discipline (see M2W.md, PRINCIPLES.md #2); it simply
  stopped being the headline. The center of gravity moved from the defensive property (don't
  contaminate) to the generative one (manifest → workspace). Prior names in the lineage: PAFA →
  Signal Foundry → ECP → CEA → ECA → D2E → M2W. Each rename recorded what the system had become.
- **ECA (retired → D2E, then M2W)** — earlier name emphasizing work "proving itself at gates"
  (performance framing). Retired because the system withholds commitment until context is clean, not
  to make work prove its worth.
