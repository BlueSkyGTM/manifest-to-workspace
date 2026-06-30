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

## Minimalism scope (a CODING rule, not a global one)
A minimalism "does this need to exist → skip; already covered → reuse; the minimum that works" ladder
is DISCIPLINE for code and CORROSIVE for content deliverables. Applied to a content build it skips
"redundant" material and writes minimal explanations — fighting the completeness such content needs. So:
- USE a minimalism rule on coding / repo / tooling tasks (where over-engineering is the risk).
- Turn it OFF during content/deliverable building (where UNDER-building is the risk).
- It is NOT a global minimalism rule. It is a CODING minimalism rule.
(If a domain wires a concrete minimalism tool to enforce this, that is a pilot tooling detail, declared
and verified there — not a core tool.)

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
| tool | role | required | detect (shell test) | install (shell cmd) |
|---|---|---|---|---|
| gstack skills | harness (Confusion/freeze/guard/spec/autoplan) | yes | `[ -d ~/.claude/skills/gstack ]` | MISSING-ASK (ships with the Claude Code env) |
| context-compressor / memory-manager | standing skills (state/memory harness) | yes | `[ -d ~/.claude/skills/gstack ]` | (part of gstack) |
| codex | cross-model review/audit | optional | `command -v codex` | `npm i -g @openai/codex` |
| gbrain (binary + global cfg) | retrieval projection (optional; readable files stay canonical) | optional | `command -v gbrain && [ -f ~/.gbrain/config.json ]` | `npm i -g gbrain` |
| gbrain repo-pin | this repo indexed for gbrain | optional | `[ -f .gbrain-source ]` | MISSING-ASK (`/sync-gbrain --full`; needs human) |
| evaluator | iteration Accept/Revise/Block | yes | n/a (rubric + fresh-context pass; no external tool) | n/a |

DOMAIN tools (extractors, wiki/graph/build-chain) are NOT here — they belong to a pilot's
`tooling.md`, because which one you need depends on the domain. (Repo policy: not universal? it is not
a core tool.)

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
