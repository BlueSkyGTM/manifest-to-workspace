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
