# CHANGELOG.md — Machine Change Record (append-only, newest at bottom)

Read CONTEXT.md for the format and the REVERT GUARD. Each entry says what changed, why (design
intent), how it was tested, and what breaks if it is reverted.

## 2026-06-29 — Scrub garbage + LF normalization
- what: removed an accidental nested empty git repo; added `.gitattributes` (`* text=auto eol=lf`).
- why: the nested repo was junk; without `.gitattributes`, Windows CRLF made every file show as
  modified (phantom churn) and risked noisy diffs.
- tested: working tree verified clean (`git status` empty) after renormalize.
- revert-risk: removing `.gitattributes` brings back the 41-file phantom-CRLF churn on Windows.

## 2026-06-29 — Engine/pilot separation (domain-agnostic core)
- what: created `pilots/gtm-systems-curriculum/` (seam, deposits, tooling, iteration-workflow,
  dry-run, answer-key, learned-seam, design-schema) and moved ALL GTM/curriculum specifics out of the
  core law/platform/stage files. Core now uses only the generic `pilots/<name>/` placeholder.
- why: M2W is domain-AGNOSTIC — the operator supplies the domain. The chat-authored draft hard-wired
  the GTM curriculum pilot into the core, which would force every other domain to fight the core.
  ICM's one-way-reference law: pilot may reference core; core must never reference a specific pilot.
- tested: deletion test passes — `grep` confirms no specific-pilot / domain / GBrain / Run-label refs
  in any core file; removing `pilots/` leaves core standing. Cross-model audit (Opus 4.8 + Codex).
- revert-risk: re-contaminates the core with one domain; breaks the ability to run a second domain
  without rewriting core; violates the ICM lineage the system is built on.

## 2026-06-29 — Core stage defects + manifest home
- what: added `manifest/` (`index.md` + `items/`) as the system's namesake home; reframed excavation
  as contamination-defense routing + `vault/account.md`; operationalized the assay off-seam exit;
  split frontmatter field classes (observed/inherited/assigned/lifecycle); replaced domain-specific
  capstone-build.md with a generic iteration-workflow interface; explicit design-schema bootstrap
  rule; vault lifecycle fix (admitted, not earned); single-agent reconciliation (M2W parallelism +
  evaluator as fresh-context pass); canonical read-order in CLAUDE.md; ICM reframed as a methodology
  modification (not a repo fork); ECP demoted to lineage.
- why: close the contradictions a cold CLI agent would hit — a system named "Manifest" had no
  manifest location; "fetch" read as asset-generation instead of contamination defense; the assay
  would bench everything; the read-order was circular; two files contradicted the single-agent law.
- tested: post-refactor structural verify + Codex re-audit (all flagged residuals closed).
- revert-risk: removes the manifest's physical home (stages 03/04 dangle); re-opens the circular
  read-order and the single-agent contradiction; re-hardwires the domain.

## 2026-06-29 — Inter-stage chain-break fixes (FULL-RUN SIMULATION VALIDATED)
- what: defined the assay **cart-record** frontmatter (id/source/assay/seam_match) + a `bench/<id>.md`
  note format; stage 03 inherits the cart record; added `accept|revise|block` to the gate-check log
  enum; declared the design-schema path (`pilots/<name>/design-schema.md`); defined the deliverable
  frontmatter + in-flight working location (`library/`, unsealed) + ship-seal semantics (seal the
  deliverable AND its `built_from` manifest items); fixed the stable-id convention (kebab slug);
  aligned the pilot deposit `type:` enum with the extractor table.
- why: a simulated full run exposed handoffs a cold agent could not complete without GUESSING (which
  the law forbids) — e.g. the assay never persisted `seam_match` but stage 03 requires it, so the run
  would stall. These are the inter-stage CONTRACTS; if guessed, two agents invent incompatible formats
  and the break is invisible until a later stage chokes.
- tested: FULL-RUN SIMULATION on branch `sim/full-run` (4 in → 2 carted/1 tailed/1 benched → 2
  manifested → 1 sealed deliverable) AND an independent Codex cold-agent narration — both agreed on
  the same gaps. Sim artifacts preserved on the `sim/full-run` branch (not merged).
- revert-risk: HIGH. Reverting any of these re-opens a chain-break that makes an autonomous run stall
  mid-pipeline. Do NOT undo without re-running the simulation. See logs/failures.md (2026-06-29 audit
  + sim entries).

## 2026-06-29 — changelog/ + revert guard
- what: added this folder (CONTEXT.md + CHANGELOG.md) and a trigger in CLAUDE.md ("Before you change
  the machine").
- why: stop a future session from reverting a tested change without knowing it was validated. Trigger
  fires on modify/improve/undo.
- tested: trigger wired into CLAUDE.md (the always-read entry); verified the rule is stated there.
- revert-risk: lose the regression guard — future sessions could silently undo tested fixes.

## 2026-06-29 — Self-scanning tool layer
- what: added a Tool scan protocol (platform/TOOLING.md), a machine-readable tool manifest in the
  pilot (pilots/<name>/tooling.md), a per-machine status file (tool-status.md), and a trigger in
  CLAUDE.md + SETUP.md ("Before a run: run the Tool scan").
- why: so a cold session on any machine immediately knows what tools the active pilot needs, installs
  the safe/known-missing ones, and FLAGS the rest (MISSING-ASK) instead of guessing an install or
  failing mid-run. Replaces the bare TODO(tools) markers with a self-describing, detect+install layer.
- tested: scan run on this machine (win32); status recorded in tool-status.md (gstack/codex/gbrain
  present, gbrain repo-pin + back-half tools flagged MISSING-ASK, pip extractors MISSING-safe).
- revert-risk: lose the auto-detect/install capability; a cold session would again discover missing
  tools only by failing mid-run.

## 2026-06-29 — Repo made fully agnostic (GTM pilot removed; universal tools only)
- what: removed `pilots/gtm-systems-curriculum/` (all GTM domain content) and replaced it with
  `pilots/_TEMPLATE/` (the domain-empty pilot contract). Universalized the tool layer: core now
  declares only the UNIVERSAL manifest (gstack, codex, gbrain, the evaluator method, the standing
  skills) in platform/TOOLING.md, with `tool-status.md` at the repo root. Dropped domain tools
  (markitdown, youtube-transcript, article-extractor, docs-site skill, Karpathy wiki,
  Understand-Anything) and ponytail (which does not exist as a gstack skill); the minimalism *policy*
  is kept but de-named.
- why: user directive — this repo is the universal ENGINE; domain features (the GTM pilot and its
  course tools) live in their own projects. "Not universal? dump it." Keeps the engine reusable and
  uncontaminated; the seam mechanism stays in core, the seam CONTENT belongs to a domain project.
- tested: deletion test passes (no GTM / domain-tool refs in core; engine stands with no pilot — its
  normal state). The simulation-tested CORE fixes (cart-record, manifest home, log enum, etc.) are
  unaffected — they are engine, not GTM. Sim artifacts remain on the `sim/full-run` branch.
- revert-risk: re-adding GTM (or any domain) content re-contaminates the agnostic engine. A concrete
  pilot belongs in its own project, copied from `pilots/_TEMPLATE/`.

## 2026-06-29 — Correct the tool dump; pin the raw→manifest conversion model
- what: (1) re-added the universal INTAKE extractors (markitdown, youtube-transcript, article-extractor)
  to the universal manifest — they are engine-level (raw→markdown is universal), not domain tools; the
  prior commit over-dumped them. (2) added a universal token-reduction tool slot (e.g. ponytail) —
  a dedicated token reducer complements a low-context model and is domain-independent (install
  MISSING-ASK pending source). (3) removed the leftover ponytail minimalism-policy section (the policy
  was tool-baggage; iteration's MVP-first/done-gate/design-schema already govern build completeness).
  (4) excavation now states markdown conversion IS the catalogable/assayable form (you cannot manifest
  an unopened PDF), runs on all hauled, and extracts chart DATA as text — NOT rendered visuals.
  (5) iteration now owns deliberate visual rendering (mermaid/excalidraw/table) just-in-time on pulled
  material only, with a `visuals/` subfolder for standalone assets.
- why: resolve the raw→manifest question. markdown conversion can't be deferred (cataloging requires
  it), so it's mechanical extraction at excavation on everything; only the EXPENSIVE visual rendering
  is deferred (a judgment) to iteration, on only what the build uses. This minimizes tokens both ways:
  nothing renders discarded/unused material, and no back-and-forth because extraction made the vault
  assayable in one pass. Intake is a universal engine function; build-chain stays domain.
- tested: not yet (design clarification + manifest correction). Engine chain unchanged structurally.
- revert-risk: re-dumping the intake extractors breaks the engine's ability to ingest non-markdown on
  any domain; collapsing extraction and visual-rendering back together re-introduces the token waste
  (rendering material that gets discarded or never used).

## 2026-06-29 — Identify ponytail (real detect/install + correct scope)
- what: ponytail is a Claude Code PLUGIN (github.com/DietrichGebert/ponytail), a code-minimization
  decision ladder (~22% token cut), not a pip/npm package. Manifest row now has the real detect
  (`[ -d ~/.claude/plugins/marketplaces/ponytail ]`) and install (`/plugin marketplace add
  DietrichGebert/ponytail` then `/plugin install ponytail@ponytail`). Added the scope note: it
  minimizes CODE, so ON for coding/engine-maintenance, OFF during a content deliverable build.
- why: it is the token-reduction tool the user wants, but it is scoped to CODING work — using it during
  a pilot's content build would under-build the deliverable (the design schema governs completeness
  there, not minimalism).
- tested: detected on this machine — marketplace ADDED, plugin not yet enabled (run /plugin install).
- revert-risk: losing the scope note risks a future session running ponytail during a content build
  and under-producing the deliverable.

## 2026-06-29 — Cross-clone tool-scan robustness
- what: added `bin/scan-tools.sh` (the executable form of the Tool scan protocol — detects every
  universal tool, prints status, regenerates tool-status.md, exits non-zero on a missing REQUIRED
  tool). Gitignored `tool-status.md` + `pilots/*/tool-status.md` (per-machine; a fresh clone carries
  NO stale status). CLAUDE.md trigger now names the one-line command.
- why: "no matter where I clone, the model is tipped off if tools are missing." Prose triggers can be
  skipped and a committed status file would show the wrong machine — a runnable script + no-stale-
  status guarantees a fresh clone learns its real state on first scan.
- tested: ran `bash bin/scan-tools.sh` on this machine — correct table, exit 0 (all required present).
- revert-risk: removing the script/gitignore returns to prose-only triggering and stale committed
  status — a clone could trust the wrong machine's tool state.

## 2026-06-29 — gbrain diagnosis (not indexed; blocked on host restart)
- what: attempted to set up gbrain for this repo via /sync-gbrain. Did NOT index it. Diagnosed the
  brain state instead and recorded a durable operational note in platform/TOOLING.md.
- why: gbrain MCP is reachable (18 pages) but has 0 embeddings — the `gbrain serve` MCP server was
  launched without OPENAI_API_KEY in its process env (registration passes `env: []`; the key is set at
  Windows User scope but the running server predates it). Embeddings can't generate, so semantic search
  is dead until the host is restarted so the server relaunches with the key. The CLI/`/sync-gbrain`
  path is also blocked because PGLite is single-connection and the MCP server holds it open.
- tested: get_health via MCP (page_count 18, embed_coverage 0, missing_embeddings 18); confirmed key
  is User-scope SET but MCP `env: []`; CLI sources-list timed out (lock).
- revert-risk: n/a (diagnosis + note only; no engine change). Indexing THIS agnostic 30-file repo is
  low value anyway — gbrain earns its keep on a pilot's content corpus or a large codebase, not the
  engine docs. Revisit after the host restart makes the brain able to embed.

## 2026-06-30 — Add LLMLingua (content-side token reduction; ponytail's counterpart)
- what: added LLMLingua / LLMLingua-2 to the universal manifest (detect `python -c "import llmlingua"`,
  install `pip install llmlingua`) with a scope note. ponytail trims the CODE you write; LLMLingua
  trims the CONTEXT you read (prompt compression, ~up to 20x, lossy but meaning-preserving).
- why: the user's "ponytail equivalent for content." Minimizing content is wrong (under-builds), but
  compressing the INPUT context fed to a low-context model is a genuine universal win — it never
  touches the deliverable, only the prompt/context. LLMLingua-2 recommended (faster, task-agnostic).
- tested: not yet installed (optional, MISSING). Manifest detect/install verified against the repo
  (github.com/microsoft/LLMLingua).
- revert-risk: low — optional tool; removing it just drops a context-compression option.

## 2026-06-30 — CLAUDE.md gbrain session-memory directive (query at start, push at end)
- what: added a "Session memory (gbrain) — optional, only if configured" section to CLAUDE.md: a cold
  session queries gbrain at start to resume prior work and pushes durable decisions at end. Framed as
  optional ("only if `~/.gbrain/config.json` present"), gbrain as a retrieval projection over the
  canonical filesystem (glass-box #4), with a hard "a gbrain miss never blocks work" fallback.
- why: an M2W run spans many sessions between manifest and finished workspace; "pick up from last time"
  is the highest-value gbrain use for this engine. Keeping it optional + files-canonical preserves the
  universal/agnostic and glass-box laws (gbrain is not a hard dependency).
- tested: not yet exercised end-to-end (brain migration to Supabase in progress). Section is pure
  guidance; no engine chain change. Wording reviewed against the non-negotiables (universal, glass-box).
- revert-risk: low — removing it loses the cross-session resume habit, but no stage breaks (gbrain was
  always optional). Do not, however, strip the "files stay canonical / miss never blocks" framing if
  the section is kept — that is what keeps gbrain from becoming a hidden-state dependency.

## 2026-06-30 — Fix gbrain install in TOOLING.md (bun dep + clone-not-registry + 2nd-machine onboarding)
- what: corrected the gbrain row in the universal manifest. (1) Added a `bun` row — the gbrain CLI is a
  Bun program and won't run without it. (2) Replaced the install `npm i -g gbrain` with clone+link from
  `github.com/garrytan/gbrain`: the public-registry `gbrain` is a DIFFERENT, unrelated package (v1.3.1)
  — `npm i -g gbrain` installs the wrong thing. (3) Added a note covering install (bun → git clone →
  bun install → npm/bun link) and onboarding a SECOND machine onto an existing brain via `gbrain init
  --url` (connect, never `migrate`/`--force`), MCP at user scope, restart, and "never commit the
  connection string."
- why: verified on this machine — `npm view gbrain version` returns 1.3.1 (wrong package); the real
  gbrain (0.42.51.0) runs from a source clone. A cold clone following the old row would install the
  wrong tool AND hit the missing-bun wall (which cost a full session to diagnose). This is universal:
  it names only the gbrain tool (already in the universal manifest) + bun, no domain/pilot.
- tested: install method confirmed live — `npm ls -g gbrain` shows a link to `C:\Users\raymo\gbrain`;
  clone remote is `github.com/garrytan/gbrain.git`; bin is `src/cli.ts` (bun). The second-machine
  `init --url` path was exercised end-to-end migrating this machine's brain to a hosted Postgres brain.
- revert-risk: medium — reverting reintroduces the `npm i -g gbrain` defect (wrong package) and drops
  the bun dependency, so a fresh clone's gbrain setup breaks exactly the way it did this session.

## 2026-06-30 — Engine audit (autoplan-lens): scan-tools.sh re-synced + repo hygiene
- what: (1) re-synced `bin/scan-tools.sh` to the universal manifest after the TOOLING.md gbrain fix —
  added a `bun` detect row and an `LLMLingua` row, and corrected the gbrain MISSING install note from
  `npm i -g gbrain` to the clone+link form (the scanner had drifted from the manifest it claims to
  mirror). (2) Removed stray `migrate.log` (gbrain migration cruft) from root and gitignored it.
  (3) Refreshed `session-handoff.md` with a 2026-06-30 clock-out entry.
- why: a lens audit (file placement / tooling / stage routing / leak test) before handoff. The scanner
  is the cold-clone's tip-off; if it disagrees with the manifest a fresh clone installs the wrong thing
  (the npm-i-g-gbrain trap). migrate.log is not engine. Deletion test re-confirmed: no pilot/domain
  refs in core law/spec/stage files (only history files record the past GTM separation).
- tested: deletion-test grep clean across core; scan-tools.sh detect lines now match the manifest rows
  1:1; stage routing left untouched (simulation-validated, revert-guarded).
- revert-risk: low — reverting re-opens manifest/scanner drift (the wrong-package trap) and re-litters
  the root. Stage contracts and core law were not modified by this audit.

## 2026-06-30 — Decision 1: close the 3 codex-sim CAPACITY gaps
- what: (1) resolved the `id`-origin CONTRADICTION — `id` is INHERITED (minted at excavation, reused
  verbatim); fixed stages/03-manifest/frontmatter-schema.md (was mislabeled "ASSIGNED: minted at
  catalogue time") + the field-class example. (2) Added a NATIVE-TEXT no-op rule to
  stages/01-excavation/CONTRACT.md — a `.md`/`.txt` deposit needs no extractor; extractor-matching is
  for non-text only — closing the "no extractor matches a plain note -> false bench" stall.
  (3) Defined the done-gate ITERATION-1 BASELINE (stages/04-iteration/done-gate.md): iteration 1 has no
  previous pass, so its baseline is the empty deliverable — all of it is substance, always iterate,
  earliest ship is iteration 2.
- why: codex full-run sim #2 (logged 2026-06-30 in logs/failures.md) flagged these as the
  CAPACITY-PARTIAL gaps; the user authorized clean execution. Each was a contradiction or a
  silent-contract stall a cold agent would hit.
- tested: localized contract corrections; the inter-stage chain SHAPE is unchanged (the
  simulation-validated handoffs still hold — `id` is the same slug, just correctly labeled INHERITED).
  Not re-simulated; low blast radius.
- revert-risk: low — reverting re-opens the id-origin contradiction, the native-text false-bench, and
  the degenerate iteration-1 ship. CONTINUATION (loop 2 / re-entry) is a SEPARATE, larger decision
  being spec'd next, NOT addressed here.

## 2026-06-30 — CONTINUATION (Decision 2): Route B — one-loop-per-invocation + safe re-entry [tested-design]
- what: closed the CONTINUATION: FAIL gap (codex sim #2) by making loop re-entry a DEFERRAL POINT, not
  an auto-loop. Additive changes only: (1) excavation mints `consumed: false` on each vault/account row
  and ids are now **repo-unique** (checked against all existing addresses, not "within the run"); (2)
  the assay processes only `consumed: false` vault rows and flips them `true` when it routes a piece;
  the cart record gains `consumed: false`; (3) the manifest processes only `consumed: false` carts,
  reuses the inherited id (no re-mint), and flips the cart `consumed: true` on transport; (4) iteration
  pulls only `sealed: false` manifest items — `sealed: true` is TERMINAL, never re-consumed. Added a
  4th deferral-point ("the loop boundary") to platform/GATES.md, CLAUDE.md, and an "After ship" section
  in stages/04-iteration/done-gate.md with an explicit DO-NOT-AUTO-LOOP anti-drift note.
- why: chosen over Route A (engine-defined auto multi-loop with run_id) by the operator's overriding
  criterion — REPO INTEGRITY FIRST, axe anything that risks the whole even if more capable. Route A was
  REJECTED because: post-ship auto-re-entry makes a stage run because it *can* (violates deferral
  PRINCIPLES #2) and makes the engine *judge* whether to loop (violates #1); run-scoped id namespacing
  changes the simulation-tested address convention (tested-chain risk); and auto-continuing in a warm
  context carries loop-1 state into loop-2 judgments — the exact cross-loop contamination M2W defers
  around. Route B makes the loop boundary obey the same law as every other transition (fires on the
  operator's new-deposits *request*), so deferral becomes TOTAL — no engine-initiated action anywhere.
  The "run boundary" is the FRONTIER OF PRODUCED STATE (consumed/sealed on disk), not a counter:
  progress is marked by production, not numbering.
- tested: design validated against the non-negotiables (deferral, glass-box, single-agent, agnostic)
  and the codex sim #2 findings; changes are ADDITIVE (the simulation-validated single-loop chain runs
  identically — `consumed` only gates a SECOND pass; id shape unchanged, uniqueness check additive).
  Not re-simulated end-to-end yet (recommended next: re-run the codex two-loop sim to confirm CONTINUATION → PASS).
- revert-risk: HIGH (conceptual). Do NOT "simplify" this into an engine that auto-loops / adds a
  `run_id` / checks for new material and re-enters itself — that IS Route A, rejected here for breaking
  the deferral law and re-opening cross-loop contamination. The hard stop after ship is load-bearing
  (the context-flush point), and "no run counter" is deliberate. Surface to the human before changing
  any of: the `consumed` flag, the repo-unique id rule, the sealed-terminal rule, or the GATES.md §4
  "loop boundary" deferral point.

## 2026-06-30 — CONTINUATION verified (codex two-loop sim): closed the sealed-routing contradiction
- what: re-ran the codex two-loop simulation against the Route B contracts. Verdict — A1 stop/no-auto-
  loop/no-run_id PASS (unambiguous across CLAUDE.md/GATES.md/done-gate.md), A2 consumed-flag handoff
  excavation->assay->manifest PASS, A4 repo-unique ids PASS. A3 flagged ONE new contradiction Route B
  introduced: stages/03-manifest/frontmatter-schema.md said the `sealed` flag "drives NO routing," but
  stage 04 now routes iteration on `sealed: false`. Narrowed the note: the DESCRIPTIVE fields drive no
  (variance/evolution) routing; the `sealed` LIFECYCLE flag is the one exception — it legitimately
  gates iteration (build only from `sealed: false`; `sealed: true` terminal). With that fix,
  CONTINUATION -> PASS and cold loop-2 execution is no-guess.
- why: the sim is the proof the lock-in holds; it caught exactly what it was meant to — a new inter-
  contract contradiction the additive edits introduced. The retired-variance note over-claimed "no
  routing" and collided with the lifecycle gate Route B relies on.
- tested: codex gpt-5.5 two-loop trace, read-only, exit 0; the only flagged gap is now closed.
- revert-risk: low — reverting re-opens the "sealed drives no routing" contradiction that stalls a cold
  agent at iteration filtering.

## 2026-06-30 — gstack-at-gates locked (deferred to autoplan / codex voice): gate→play map + team reframe
- what: encoded the gstack gate→play map into platform/SKILLS.md (which play fires at which deferral
  point, with the law-keeping constraint per play) and reframed platform/TOOLING.md's gstack section —
  "gstack is the operating TEAM, not a tool; ponytail/LLMLingua/gbrain/memory are the instruments it
  wields." The decision was deferred to /autoplan's cross-model voice ("who knows when gstack should
  fire better than gstack").
- why: the codex autoplan-voice CONFIRMED every gate with constraints and one CHANGE — the loop-
  boundary row is SESSION continuity (persist/recall via files), NOT loop auto-continuation (consistent
  with the Route B loop-boundary lock). Both sub-calls AGREED: (a) assay gets NO review skill (it is
  the seam brain, not a review point); (b) /autoplan pre-build is CONDITIONAL not always (M2W's
  MVP-first + done-gate already govern completeness; wire late, the subset needed).
- tested: codex gpt-5.5 autoplan-voice review, read-only, exit 0 — per-gate CONFIRM/constraint, no law
  violations once the constraints hold (mechanical extraction only at excavation; no review skill at
  assay; sequential fresh-context evaluator, NO subagents, at conformance; ship only after the
  done-gate; loop boundary = session continuity, not auto-loop).
- revert-risk: low — the map is additive guidance; reverting loses the where/when of gstack but breaks
  no chain. Do NOT, however: add a review skill at the assay, make /autoplan always-on, let excavation
  /scrape//browse filter for relevance, let the conformance evaluator spawn subagents, or reframe the
  loop-boundary row as auto-continuation — each re-opens a law violation the voice flagged.

## 2026-06-30 — gstack-at-gates EXPANDED to the full suite (second autoplan/codex voice pass)
- what: the first map was thin (~1/3 of the suite). Expanded the SKILLS.md gate→play map to exploit the
  whole suite at the right deferral points and ran a SECOND codex autoplan-voice over the fuller map.
  Notable additions: the gstack BROWSER (`/browse` + `/open-gstack-browser`) and `/skillify` as the
  EXCAVATION/vaulting engine ("gbrowser for vaulting"); `/diagram` + `/design-html` at iteration build;
  `/qa` `/qa-only` `/cso` `/benchmark` `/design-review` at conformance; `/make-pdf` `/document-*`
  `/canary` at ship; `/investigate` for failure handling; `/learn` `/retro` at the loop boundary;
  `/health` `/gstack-upgrade` `/benchmark-models` as engine-maintenance.
- why: gstack is a suite, not two skills; the engine should use it fully. The decision was again
  deferred to gstack's own voice. The second pass CONFIRMED the additions but TIGHTENED ~12 placements
  and cut one, all toward two invariants — conformance-not-quality and conditional-on-surface:
  setup-* skills are CONDITIONAL (not cold-boot); `/qa`/`/design-review`/`/cso`/`/benchmark` check
  schema conformance, never "is it good"; `/investigate` may root-cause a mechanically-stopped stage
  but NEVER auto-resolve a benched seam call; `/learn` writes readable-first; `/skillify` codifies
  coverage not relevance; `/benchmark-models` moved out of conformance to engine-maintenance.
- tested: codex gpt-5.5 autoplan-voice (2nd pass), read-only, exit 0 — per-play CONFIRM/CHANGE/REMOVE,
  no law violations once the tightened constraints hold.
- revert-risk: low — additive guidance. Preserve the two invariants if editing: every gated play is
  conformance-not-quality and conditional-on-surface; nothing at the assay; no subagents at conformance;
  the loop boundary stays session-continuity, not auto-loop.

## 2026-06-30 — gstack map is OPEN: pilots wake dormant plays on domain need
- what: clarified that the SKILLS.md gate→play map is the universal DEFAULT, not a closed allowlist.
  Added the open-map principle to platform/SKILLS.md and a "Domain-woken gstack plays" slot to
  pilots/_TEMPLATE/tooling.md. A pilot may WAKE any dormant gstack play its domain warrants (e.g. a
  website pilot wakes /qa, /design-html, /design-review, /canary; an iOS pilot wakes the /ios-* suite),
  at the matching gate, under the same invariants. Only the parallel/multi-agent plays are truly
  rejected and can never be woken.
- why: operator directive — dormant skills should wake when the project warrants, not be permanently
  excluded by the core cut. This is the engine/pilot boundary applied to gstack activation: core
  declares the universal default; the pilot owns domain choices (incl. which plays to wake), declared
  in pilots/<name>/tooling.md (consistent with TOOLING.md "concrete tool choices live in the pilot").
- tested: domain-agnostic — the core map still names no specific pilot; website/iOS are illustrative
  examples (like naming OpenAI for embeddings). Deletion test unaffected; not a chain change.
- revert-risk: low — guidance. Keep the invariants on any woken play (single-agent, conformance-not-
  quality, nothing-at-assay, no-subagents, session-continuity loop boundary).
