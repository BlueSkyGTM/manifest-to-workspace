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
| gbrain (binary + global cfg) | retrieval projection (optional; readable files stay canonical) | optional | `command -v gbrain && [ -f ~/.gbrain/config.json ]` | `npm i -g gbrain` |
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
