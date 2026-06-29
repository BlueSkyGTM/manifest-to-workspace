# session-handoff.md — Clock-Out Note (cross-session continuity)

Updated at the end of every working session. The next session reads this FIRST. Keeps the agent from
losing the thread across long runs.

## Format
```
## <ISO-8601 timestamp> — session N
- verified: <what is confirmed working>
- changed: <what changed this session>
- broken: <what is known broken / blocked>
- next: <the single next-best step>
```

## Current state
## (skeleton built + review changes applied; awaiting curriculum-maze loop)
- verified: M2W skeleton stands (stages, gates, logs, holding folders). Tool set validated and now
  declared in platform/TOOLING.md with the gstack fence and ponytail scoping.
- changed: DRY-RUN.md recast Pass 1 as a LOOP (1a human-calls/train, 1b seam-proposes/correct, 1c
  seam-acts-blind/grade) with required audits between sub-runs, blind-discovery scheduled in 1c, and
  a reversibility asterisk (deliverable reversible, learning sticky-but-inspectable). Added
  platform/TOOLING.md (gstack fence incl. /autoplan; ponytail coding-only/off-during-curriculum).
  Buried the variance/Tyrogue thread in frontmatter-schema.md. Added /ponytail-off note to
  capstone-build.md. Pointed PRINCIPLES.md #5 at the fence. Updated glossary + platform/CONTEXT.md.
- broken: nothing broken; many items BLOCKED-ON-RUN (see DRY-RUN.md) — they are the questions the
  loop exists to answer, not unfinished work.
- next: wire maze-pass tools only (skeleton + logs + ONE extractor + evaluator + the two skills),
  then run Run 1a of the curriculum-maze loop. Fold gtm-starter-kit into the corpus BEFORE 1a only if
  it is answer-key material to grade 1c against; otherwise keep it deferred.
