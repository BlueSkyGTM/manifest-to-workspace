# tooling.md — Concrete Tool Choices for This Pilot

The core declares a tool-surface POLICY (the gstack fence, ponytail-as-a-coding-rule, the
single-agent law that fences out parallel/multi-agent surfaces — see `platform/TOOLING.md`). This
file makes the CONCRETE choices for the GTM-curriculum domain. Tools are wired in a later pass, after
human review; for now these are declared selections, not installed wiring (`TODO(tools)`).

## Excavation extractors (matched to this pilot's deposit types — see deposits.md)
- docs-site skill (from awesome-claude-skills) — documentation websites
- article-extractor — web articles
- youtube-transcript — video
- markitdown — PDF / Office → markdown

## Assay
Human calls on the curriculum loop (`dry-run.md`). The seam brain is the OUTPUT of the loop, wired
after Run 1c — not an input.

## Iteration (back half) — see iteration-workflow.md
- Karpathy LLM wiki — compile manifest → linked articles + ordered index. Principles native,
  STRUCTURE NOT FORKED.
- Understand-Anything `/understand-knowledge` — graph + capstone (human comprehension + course build).
- the evaluator — Accept / Revise / Block, with source-fidelity ("no facts not in the material").

## Retrieval
- GBrain — BLACKBOX machine retrieval for the pipeline. **Canonical lives in READABLE files**
  (logs, learned-seam, session-handoff); GBrain is the projection, never the only home of a seam
  decision. NOTE: GBrain is not yet configured in this environment; until it is, the readable files
  ARE the system and GBrain is simply absent (the deletion test still passes).

## Standing skills (state/memory harness — these are core-generic but their outputs land here)
- context-compressor, memory-manager — write durable decisions to readable files first, then index
  into GBrain. See core `platform/SKILLS.md`.

## ponytail scoping (applied)
ponytail's minimalism ladder is tuned for code and UNDER-builds pedagogy. Run `/ponytail off` during
the curriculum back-half build (it would skip "redundant" lessons and write minimal explanations,
fighting the spaced reinforcement and worked examples a course needs). Keep it ON for coding/tooling
work. NOTE: verify `/ponytail` exists in the environment before relying on it (it was named in the
chat-authored spec but is not in the current gstack skill list).

## Tool manifest (machine-readable — drives the core Tool scan protocol)
The scan (platform/TOOLING.md) reads this table, detects each, installs the safe/known ones, and FLAGS
the rest as MISSING-ASK (never guesses an install). `required` extractors are conditional on the
deposit types actually in `deposits.md`.

| tool | role / stage slot | required | detect (shell test) | install (shell cmd) |
|---|---|---|---|---|
| gstack skills | harness (Confusion/freeze/guard/spec/autoplan) | yes | `[ -d ~/.claude/skills/gstack ]` | MISSING-ASK (ships with the Claude Code env) |
| codex | cross-model review (optional) | no | `command -v codex` | `npm i -g @openai/codex` |
| gbrain (binary+global cfg) | retrieval projection (optional) | no | `command -v gbrain && [ -f ~/.gbrain/config.json ]` | `npm i -g gbrain` |
| gbrain repo-pin | this repo indexed for gbrain | no | `[ -f .gbrain-source ]` | MISSING-ASK (`/sync-gbrain --full`; needs human) |
| markitdown | excavation: pdf-or-office | if that deposit type | `python -c "import markitdown"` | `pip install markitdown` |
| youtube-transcript | excavation: video | if that deposit type | `python -c "import youtube_transcript_api"` | `pip install youtube-transcript-api` |
| article-extractor | excavation: web-article | if that deposit type | `python -c "import trafilatura"` | MISSING-ASK (candidate `pip install trafilatura`; spec name ambiguous — confirm) |
| docs-site skill | excavation: docs-site | if that deposit type | `ls ~/.claude/skills 2>/dev/null \| grep -i docs-site` | MISSING-ASK (from awesome-claude-skills; manual copy) |
| Karpathy LLM wiki | iteration: wiki compile | yes (back half) | (no package) | MISSING-ASK (an approach/repo, not a package — confirm how to wire) |
| Understand-Anything | iteration: graph + capstone | yes (back half) | (no package) | MISSING-ASK (confirm source/command) |
| evaluator | iteration: Accept/Revise/Block | yes (back half) | n/a (rubric + fresh-context pass; no external tool) | n/a |
| ponytail | coding minimalism (optional) | no | `ls ~/.claude/skills/gstack \| grep -i ponytail` | MISSING-ASK (NOT FOUND in gstack — likely misnamed; verify before relying) |

The latest scan result for THIS machine is written to `tool-status.md` (regenerated each scan).

## Parked / dropped (decided for this pilot)
- GBrain vs Understand-Anything: DIFFERENT LAYERS (blackbox machine retrieval vs human
  comprehension) — both kept. Final ingest details settle once Run 1c produces a wiki to point GBrain
  at.
- Dropped: headroom-desktop (no role; markitdown taken on its own), loop-engineering (parallel,
  violates single-agent), awesome-claude-skills wholesale (shopped for the one extractor), tapestry
  (overlaps the wiki layer).
