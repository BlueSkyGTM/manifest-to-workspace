# SETUP.md — How to Start M2W (read this first)

This file exists so the invocation and the pre-load rules live IN the repo, not in your head.

## The kickoff prompt (paste this into Claude Code, pointed at this repo)

```
Read CLAUDE.md first. Then read platform/CONTEXT.md and every file it lists. Then read
BUILD-INSTRUCTIONS.md. Follow the build instructions EXACTLY. Do not improvise, do not add
anything not specified, and do not skip the read-order. If anything is ambiguous, STOP and ask —
do not guess. Confirm you have read all of it before taking any action.
```

The first act is READING, never building. CLAUDE.md routes to the law layer; platform/CONTEXT.md
lists all eight law files (PRINCIPLES, MWP, GATES, LOGS, TOOLING, SKILLS, glossary, + itself);
BUILD-INSTRUCTIONS.md carries the hard DO-NOT list. There is no magic phrase — the discipline is
that reading comes first and improvisation is forbidden.

## Pre-loading material (IMPORTANT — one door only)

You MAY place raw material into the pipeline before setup. There is exactly ONE correct door:

- **vault/ — SAFE.** vault/ is the excavation target. Raw, unsorted material is SUPPOSED to land
  here. Pre-loading it just means excavation has material waiting. No harm. This is the only door.

Everything else is a TRAP. Do NOT pre-place material into:

- **carts/ , tailings/ , bench/ — NEVER.** These are DOWNSTREAM of the assay. Material is only
  supposed to arrive there by passing THROUGH a logged seam call. On the curriculum loop the assay
  is the thing being TRAINED and TESTED. Hand-placing material in carts/ asserts "this is on-seam"
  without the assay running — which contaminates the seam's training data (Run 1a) and corrupts the
  blind-discovery grade (Run 1c), where the seam would appear to have sorted material it never saw.
  This does not crash anything. It SILENTLY INVALIDATES the run, which is worse than a crash because
  you will not know it happened. The empty-downstream-folders property exists precisely so that
  everything in a downstream folder EARNED its way there through a logged call.

- **the manifest — NEVER.** Same reason: manifest entries are born at stage 03 from carted,
  assayed material. Pre-placing them fakes the catalogue.

- **platform/ — NEVER.** The law layer. Hand-editing it mid-design is how drift starts.

### The rule, in one line
Pre-load **vault/** freely. Never pre-load anything downstream of the assay. vault/ is the only door.

This rule applies to the AGENT too: if a human might wrongly pre-place assets in carts/, so might
Claude Code. The agent must route ALL material through vault/ → excavation → assay, never directly
into a downstream folder.
