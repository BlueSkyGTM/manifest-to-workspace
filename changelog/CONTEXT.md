# CONTEXT.md — changelog/ (the machine's change record + REVERT GUARD)

This folder records changes to the **pipeline itself** (the core spec, contracts, the pilot) with
their design intent AND their test outcome. Its job is to stop a future session from reverting a
change without knowing it was already validated. It is glass-box memory for the machine.

## How this differs from the other written state (don't confuse them)
- `logs/` — what fired DURING a run (gate-checks, rejections, failures). Per-run, per-item.
- `session-handoff.md` — where the work is RIGHT NOW (the clock-out note). Mutable, present-tense.
- `changelog/` — what CHANGED in the machine, WHY, and whether it was TESTED. Append-only, historical.
  This is the one a session consults before editing or undoing the spec.

## THE TRIGGER (when a session MUST read this first)
Before any of these, read `CHANGELOG.md` top-to-bottom:
1. The user asks to **modify** the repo (edit a contract, the law, a stage, the pilot).
2. The user asks for an **improvement** or refactor.
3. The user asks to **undo / revert / roll back** a change.

The hard rule: **do NOT revert or overwrite a change whose entry is marked `tested: <how>` without
surfacing it to the human first** — state which entry, what it was designed to do, and how it was
validated, and let the human decide. A tested change that looks odd is probably load-bearing; the
oddness is usually the fix. Reverting it silently re-opens a closed bug.

Wire this trigger into the law so a cold session actually fires it: see CLAUDE.md
("Before you change the machine").

## Entry format (append-only, newest at bottom)
```
## <ISO-8601 date> — <short title>
- what: <what changed, file-level>
- why: <the design intent — what the change is supposed to DO / which failure it closes>
- tested: <how it was validated, or "not yet"> (e.g. "full-run simulation + Codex cold-agent", "skeleton verify")
- revert-risk: <what breaks / which bug re-opens if this is undone>
```
