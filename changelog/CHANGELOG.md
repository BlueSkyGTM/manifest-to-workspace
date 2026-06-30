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
