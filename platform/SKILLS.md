# platform/SKILLS.md — Standing Skills (State / Memory Harness Layer)

These skills run periodically across the pipeline's life. They are the State/Memory harness layer:
what keeps the agent from losing the thread across sessions. CRITICAL RULE: they write durable
output to READABLE FILES (logs, learned-seam files, session-handoff), never silently into a blackbox
store. The pilot depends on the seam learning being inspectable so the human can grade it. A skill
that buries decisions in an unreadable index defeats the answer-key check.

## context-compressor (periodic)
When context grows long or a session is wrapping, compress history into a structured summary WITHOUT
losing critical state. Writes the summary to a readable session file, not into hidden memory. Used
to hand off between sessions during long runs (a first run can span sessions).

## memory-manager (on durable decisions)
When a standing decision is made — a seam call with its reason, a schema cut, a routing rule —
memory-manager records it. RULE: it writes to the readable learned-seam files and the logs, which are
THEN optionally indexed into a retrieval store IF the instantiation wires one (the concrete store is a
domain choice, named in the pilot). The file is canonical; the store is only a projection. Never write a seam decision only into a store — the
human must be able to read and grade it from the files alone. (If no store is configured, the readable
files ARE the system and nothing is lost.)

## session-handoff.md (the clock-out note)
At the end of each working session, write/update session-handoff.md at root: what is verified, what
changed, what is broken, the single next-best step. The next session reads this first. This is the
cross-session thread the agent would otherwise lose.

## Why readable-file-canonical matters here specifically
A domain's first run IS the seam layer's training run. Every seam call the human makes is training
data. If memory-manager compresses those calls into a blackbox, the human cannot check whether the
seam layer learned the right boundary. Readable files preserve the answer-key check. This is not a
style preference; it is what makes a run gradeable.
