---
name: save
description: One-shot session "clock-out" that chains the memory + context + token-saving skills in the correct order. Trigger when the user says "/save", "save", "save the session", "clock out", "wrap up and save", "save memory and compact", or when the context window is getting full and needs compressing before continuing. Runs memory-manager → consolidate-memory → context-compressor, then hands off to /compact. Do NOT trigger for saving a single file, a one-off memory edit, or niche project-learning/task-tracking work.
---

# /save — clock out: save memory, consolidate, compress, compact

One command to persist everything durable and shrink the context safely, in one pass. The order is
load-bearing: **memory is saved and tidied BEFORE the history is compressed**, so nothing durable is
lost when detail gets dropped.

## The chain — run in THIS order

1. **Save memory** — invoke `anthropic-skills:memory-manager`.
   Scan the session for durable decisions, rules, and preferences; apply its save test + schema;
   write them to the memory files and update `MEMORY.md`. Runs FIRST so durable state is captured
   before any compression touches the history.

2. **Consolidate memory** — invoke `anthropic-skills:consolidate-memory`.
   Reflective pass over the memory files: merge duplicates, fix stale facts, prune the index. Keeping
   the store lean is itself token savings on every future recall. Skip only if memory is fresh/tiny
   with nothing to merge.

3. **Compress context** — invoke `anthropic-skills:context-compressor`.
   Compress the conversation into a structured summary that preserves the critical state — decisions,
   open threads, file/repo state — for a fresh window. Safe now because step 1 already persisted the
   durable parts to disk.

4. **Compact** — the built-in. Once the compressor has produced the summary, run `/compact` to
   actually reclaim the window (or tell the user to run it, if this harness requires `/compact` to be
   user-invoked). The compressed summary carries forward into the next window.

## Rules

- **Order is non-negotiable: save → consolidate → compress → compact.** Never compress or compact
  before memory is saved, or durable state can be lost.
- If the user asks for only part of it ("just save memory", "just compact"), run only that step —
  don't force the whole chain.
- Report one terse line per step: what was saved, what was consolidated/pruned, that context was
  compressed, and that compaction is ready/done.
- **Scope: memory + context + token hygiene only.** Leave the niche ones out — no project-learnings
  (`learn`), task tracking (`taskmaster`), scheduling, or gstack `context-save`/`context-restore`.
