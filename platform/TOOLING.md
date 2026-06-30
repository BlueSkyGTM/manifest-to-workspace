# platform/TOOLING.md — Tool-Surface POLICY (not tool choices)

This file is the core POLICY for HOW tools are admitted: which surface area of a multi-feature tool
M2W adopts vs fences out, and why. Two categories, and the line between them matters:
- **Operating-environment / harness tools** (the agent's own runtime — e.g. gstack, a minimalism
  ladder) are CONSTANT across domains, so their fence is core policy and named here.
- **Domain / deliverable tool CHOICES** (extractors, build-chain tools, a retrieval store) vary by
  domain and are picked by the instantiation, NOT here — see `pilots/<name>/tooling.md`.

So core names no DOMAIN tool. A tool's reputation is not a license to import all of it. Where a tool's
core conflicts with an M2W law, the conflicting part is fenced out EXPLICITLY, so a later reader does
not pull it in because "that's how the tool works."

## The admission rule (applies to every tool a pilot chooses)
1. **Adopt the surface, not the engine.** Take the part that fits the laws; fence the rest by name.
2. **Single-agent law wins (PRINCIPLES #5).** Any tool whose value is parallel/multi-agent
   orchestration is fenced to its single-agent-compatible surface. Concurrent workers are rejected.
3. **Readable-canonical (SKILLS.md).** Any retrieval/memory tool is a PROJECTION; the canonical state
   stays in readable files. Never let a tool become the only home of a decision.
4. **Wire late, wire the subset.** Tools are wired in a later pass, after human review, and only the
   subset a given run needs — wiring everything at once makes a failure undiagnosable. Until then,
   slots stay literal `TODO(tools)`.

## gstack — the fence (the canonical worked example of rule 2)
**Framing: gstack is the operating TEAM, not a tool in the box.** It is the set of colleagues (plays)
that run this engine; the actual *tools* the team wields are ponytail, LLMLingua, gbrain, and the
memory skills. This section is the FENCE — which of the team's plays are adopted (single-agent surface)
vs rejected (parallel). WHEN each adopted play fires is the gate→play map in `platform/SKILLS.md`.

gstack's headline value is PARALLEL multi-agent orchestration (Conductor running concurrent sprints,
maker/checker sub-agents, /pair-agent). M2W's law is SINGLE-AGENT, depth-first. Parallel silos under
loose contracts were the original source of drift. So the gstack surface is split:

ADOPT (single-agent-compatible):
- Confusion Protocol — gags the agent from guessing on architecture (= "you do not think; you execute")
- /freeze, /guard — edit-locks so the agent cannot "fix" unrelated code
- sprint-stage discipline — the Think → Plan → Build → Review → Test → Ship structure
- /spec — the spec gate (craftsmanship floor; NOT the seam gate — the seam gate is the seam brain)
- /autoplan — the reviewed-plan pipeline; surfaces only taste decisions, spawns NO concurrent workers
- a read-write / read-only / deny trust triad for any retrieval tool — read-only solves re-entry
  contamination

REJECT (violates single-agent law):
- Conductor parallel sprints
- sub-agents / maker-checker parallelism
- /pair-agent and any cross-agent coordination
- anything that spawns concurrent workers

NOTE TO A LATER READER: do NOT enable Conductor or sub-agents because a tool's README centers them.
They are fenced OUT here by law. Using such a tool means using the adopted surface, not its parallel
core.

## Model-tier policy (single agent, multiple tiers — self-directed)
Single-agent (PRINCIPLES #5) means one agent on one depth-first path — NOT one model tier for the whole
run. The agent switches its OWN tier to match each stage's cognitive demand: a cheaper/faster tier
where the work is mechanical, the top tier where judgment lives. This cuts token cost and keeps the
agent focused (a light tier on fetch/catalogue; the heavy tier reserved for the build). The switch is
self-directed (the agent recognizes the stage and switches — not the operator's to drive) and mechanical
(read the tier here and execute; do not deliberate the choice). Tiers are by ROLE — substitute the
current light / mid / top models (e.g. Haiku / Sonnet / Opus); the engine stays version-agnostic.

| Stage / work | tier | why |
|---|---|---|
| Excavation (fetch, extract, address) | LIGHT | mechanical reach/coverage/addressing; no judgment (deferred) — focus on fetching |
| Assay (three-way seam routing) | MID | the cart/tailings/bench test is near-mechanical and ANY ambiguity benches — the agent never RESOLVES an uncertain call, so it never needs the top tier to "think harder" |
| Manifest (catalogue + frontmatter) | LIGHT | mechanical cataloguing + descriptive frontmatter |
| Iteration (build the deliverable) | TOP | the judgment-heavy core — authoring schema-conforming content, design-schema work, the substance-vs-surface read. (The conformance GATE checks shape, not quality; TOP is for the BUILD that produces conforming content, where craftsmanship lives — not for the gate to "judge harder.") |
| Loop boundary / session continuity (persist, recall, handoff) | LIGHT–MID | mechanical persist/recall |

Switching is DETERMINISTIC, not deliberated: at each stage boundary, identify the active contract and
use the tier declared here (or in the pilot's predeclared `pilots/<name>/tooling.md` policy). Do NOT
deliberate the tier per item.

**Within-stage step-up** is allowed ONLY for capability failures that do not decide a deferred call —
context capacity, mechanical parsing/formatting, implementation complexity, or ambiguity-DETECTION that
can only bench/log/stop. A step-up may NEVER convert uncertainty into a cart/tailings/pass. Stepping up
buys capability, never permission to judge where the contract says stop.

**The assay guard (the perception loophole).** "Any ambiguity benches" only protects against ambiguity
the model PERCEIVES — a lighter tier can falsely see a clean match, stamp `seam_match`, and contaminate
`carts/` (then `manifest/`, which inherits the cart record and may not invent it later). So at the
assay: MID may propose routes ONLY against an explicit seam layer (on-seam edges, off-seam patterns,
near-miss examples). Before committing any CART or TAILINGS, record the positive evidence AND an
explicit near-miss check; if the evidence is absent, uncited, or weak — or the near-miss inventory is
missing — BENCH. On an immature or high-risk seam, the assay commit is TOP or human-cleared. A TOP
step-up at the assay may only DETECT ambiguity and force bench/log/stop; it may never convert
uncertainty into a cart/tailings.

**Glass-box.** Every gate firing in `logs/gate-checks.md` records the `tier:` it ran on, and any
within-stage step-up records its reason and allowed effect. Tier is not hidden state.

(Harness note: the agent self-switches wherever the runtime supports mid-run model switching; where it
does not, these are the recommended per-stage tiers. A pilot may override a stage's tier ONLY as
predeclared file policy in `pilots/<name>/tooling.md` — never operator-driven per item, and never below
the assay guard.)

## Concrete tool choices live in the pilot
The adopted set for a given run — which extractor matches which deposit, which retrieval store, which
build-chain tools — is domain-specific. It does NOT belong here. See `pilots/<name>/tooling.md`. Core
declares the policy; the pilot makes the picks.

## Tool scan protocol (self-bootstrapping — so a cold session knows what to install)
A cold session must not discover missing tools by failing mid-run. There are TWO manifests:
- the **universal manifest** below (domain-independent tools — the harness, cross-model review,
  retrieval, the evaluator method, the standing skills). It lives in core because these apply to every
  domain.
- a **domain manifest** (per pilot, in `pilots/<name>/tooling.md`) — extractors and build-chain tools
  specific to one domain. Present only when a pilot is active.

The scan reads BOTH (the domain one only if a pilot is present) and acts on each.

Manifest row schema:
```
| tool | role / stage slot | required|optional | detect (shell test) | install (shell cmd) |
```

### Universal manifest (this repo's tools — domain-independent)
The engine's INTAKE (raw → markdown) and its HARNESS are universal — every domain uses them. Only the
deliverable BUILD-CHAIN is domain-specific. So intake extractors live here; build-chain tools do not.

Harness + state/memory:
| tool | role | required | detect (shell test) | install (shell cmd) |
|---|---|---|---|---|
| gstack skills | harness (Confusion/freeze/guard/spec/autoplan) | yes | `[ -d ~/.claude/skills/gstack ]` | MISSING-ASK (ships with the Claude Code env) |
| context-compressor / memory-manager | session-boundary compression + durable memory | yes | `[ -d ~/.claude/skills/gstack ]` | (part of gstack) |
| ponytail (Claude Code plugin) | CODE/token minimization for CODING work (decision ladder, ~22% token cut); see scope note below | optional | `[ -d ~/.claude/plugins/marketplaces/ponytail ]` (marketplace added) | `/plugin marketplace add DietrichGebert/ponytail` then `/plugin install ponytail@ponytail` (Claude Code slash cmds, run interactively) |
| LLMLingua / LLMLingua-2 | CONTENT-side token reduction: compress the CONTEXT/prompt fed to the model (~up to 20x, lossy but meaning-preserving); see scope note below | optional | `python -c "import llmlingua"` | `pip install llmlingua` (pulls torch + a small model; CPU-ok) |
| codex | cross-model review/audit | optional | `command -v codex` | `npm i -g @openai/codex` |
| bun | JS runtime the gbrain CLI runs on (required IF using gbrain) | if gbrain | `command -v bun` | `irm bun.sh/install.ps1 \| iex` (Win) / `curl -fsSL https://bun.sh/install \| bash` (unix); then reopen the shell |
| gbrain (CLI + global cfg) | retrieval projection (optional; readable files stay canonical) | optional | `command -v gbrain && [ -f ~/.gbrain/config.json ]` | clone+link — see note (do NOT `npm i -g gbrain`: the registry `gbrain` is a DIFFERENT package) |
| gbrain repo-pin | this repo indexed for gbrain | optional | `[ -f .gbrain-source ]` | MISSING-ASK (`/sync-gbrain --full`; needs human) |
| evaluator | iteration Accept/Revise/Block | yes | n/a (rubric + fresh-context pass; no external tool) | n/a |

**LLMLingua scope (the content-side counterpart to ponytail):** LLMLingua compresses the INPUT
context/prompt you feed the model — useful when a stage pulls a lot of source material into context
(e.g. iteration over a large manifest, or assaying many items). It NEVER compresses the deliverable
you produce — you ship full content, never a compressed one. LLMLingua-2 is recommended (faster,
task-agnostic). The clean split: **ponytail trims the CODE you write; LLMLingua trims the CONTEXT you
read.** Both are universal token-reduction tools for a low-context model. (Repo:
github.com/microsoft/LLMLingua.)

**gbrain note (embeddings need the key at MCP-server start):** gbrain embeds via OpenAI
(`text-embedding-3-large`). The gbrain MCP server (`gbrain serve`) captures its environment at START,
so if `OPENAI_API_KEY` wasn't present when Claude Code launched it, embeddings silently produce 0
coverage (pages exist, search returns nothing). Fix: ensure `OPENAI_API_KEY` is a persistent env var
(Windows User scope / shell profile), then RESTART the host so the server relaunches with it; then
backfill embeddings. Also: PGLite is single-connection, so the `gbrain` CLI (and `/sync-gbrain`) cannot
run while the MCP server holds the brain open — use the gbrain MCP tools, or stop the server first.
And: `gbrain serve` exposes the API but does NOT process the job queue — submitted jobs (embed, sync)
sit `waiting` until a worker runs. Run `gbrain autopilot --install` (a per-machine background daemon)
to actually process embeds/syncs. Until the daemon runs, embeddings stay at 0 even with the key set.

**gbrain install (clone, NOT the registry) + onboarding a second machine:** gbrain's CLI is a Bun
program published from `github.com/garrytan/gbrain` — it is NOT the unrelated `gbrain` on the public
npm registry (that is a different package; `npm i -g gbrain` installs the wrong thing). Install:
ensure `bun` is present, then `git clone https://github.com/garrytan/gbrain && cd gbrain && bun
install`, then put it on PATH with `npm link` (or `bun link`). If the CLI errors right after linking,
run `bun run build` in the clone. To point a SECOND machine at an EXISTING brain (e.g. a shared
hosted-Postgres brain), do NOT `migrate` or `--force` anything — install as above, then `gbrain init
--url '<the brain's connection string>'`, which CONNECTS to the existing brain without copying or
wiping. Then register the MCP at user scope (`claude mcp add --scope user gbrain -- <abs-path>/gbrain
serve`) and restart the host so `mcp__gbrain__*` tools load. NEVER commit the connection string — it
carries the DB password; fetch it per-machine from the provider (e.g. Supabase → Connect → Session
pooler) and keep it out of git. (PGLite is single-machine/local; a hosted-Postgres brain is what
enables multi-machine sharing — many connections, no single-connection lock.)

**ponytail scope (important):** ponytail minimizes CODE. Use it ON coding / tooling / engine-
maintenance work (where over-engineering wastes tokens). Keep it OFF during a pilot's CONTENT
deliverable build (iteration), where a minimalism ladder UNDER-builds the deliverable — completeness
there is governed by the design schema, not by minimalism. (Repo: github.com/DietrichGebert/ponytail.)

Intake extractors (universal; which are REQUIRED depends on the deposit types a run actually has):
| tool | role / deposit type | required | detect (shell test) | install (shell cmd) |
|---|---|---|---|---|
| markitdown | excavation: pdf/office/doc → markdown | if that deposit type | `python -c "import markitdown"` | `pip install markitdown` |
| youtube-transcript | excavation: video → transcript | if that deposit type | `python -c "import youtube_transcript_api"` | `pip install youtube-transcript-api` |
| article-extractor | excavation: web article → markdown | if that deposit type | `python -c "import trafilatura"` | MISSING-ASK (candidate `pip install trafilatura`; confirm) |

NOT here (domain-specific, belongs in a pilot's `tooling.md`): the deliverable BUILD-CHAIN — e.g. a
wiki compiler, a graph/comprehension tool, a course builder. Repo policy: intake + harness are
universal; the build-chain is the domain's.

WHEN the scan runs (the trigger — wired into CLAUDE.md and SETUP.md):
- on SETUP (first orientation in a repo), and
- before a RUN (before excavation actually fires).

The scan algorithm (single-agent, glass-box):
1. For each manifest row, run `detect`. Record PRESENT / MISSING.
2. For a MISSING **required** tool with a known, side-effect-safe `install`: install it, then re-detect.
3. For a MISSING tool whose `install` is **UNKNOWN, needs secrets/auth, needs a clone, or changes the
   machine in a way the human should approve**: do NOT guess or run it. FLAG it (status `MISSING-ASK`)
   and surface it to the human. Guessing an install violates "you do not think; you execute."
4. Write the result to `tool-status.md` at the repo root (universal tools) and, if a pilot is active,
   to `pilots/<name>/tool-status.md` (domain tools). Glass-box: state on disk, regenerated each scan,
   per-machine. Never hold tool status only in your head.
5. If a required tool is MISSING after the scan, the run cannot start that stage — STOP and report,
   per DRY-RUN.md scoping (wire only the subset a run needs; a missing required tool is a blocker, a
   missing optional tool degrades gracefully).

This makes the repo self-describing about its tools: open it on any machine, the scan tells you (and
installs, where safe) what the active pilot needs.
