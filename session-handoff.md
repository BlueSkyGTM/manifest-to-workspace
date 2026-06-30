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
## 2026-06-29 — engine/pilot separation refactor (domain-agnostic core)
- verified: core skeleton stands and is now domain-agnostic. Cross-model audit (Opus 4.8 + Codex)
  recorded in logs/failures.md; findings resolved this pass.
- changed: separated the generic ENGINE from the GTM-curriculum PILOT.
    - Added pilots/gtm-systems-curriculum/ (pilot.md, seam.md, deposits.md, tooling.md,
      iteration-workflow.md, dry-run.md, answer-key/, learned-seam/) under a one-way-reference law
      (pilot→core only; core never references pilot; deletion test must leave core runnable).
    - Stripped GTM/curriculum specifics from core: ARCHITECTURE §2.2/§6, glossary, TOOLING, SKILLS,
      LOGS, DRY-RUN (now a generic validation-ladder interface), SETUP, README, CLAUDE.md.
    - Added manifest/ (index.md + items/) — the system's namesake now has a physical home; wired into
      stage 03 outputs + stage 04 inputs.
    - Reframed excavation as contamination-defense routing; defined vault/account.md.
    - Fixed contradictions: single-agent (M2W §Parallelism subordinated to PRINCIPLES #5; evaluator =
      fresh-context sequential pass, not a second agent); design-schema bootstrap (explicit ungated
      iteration-1 rule); frontmatter field classes (observed/inherited/assigned/lifecycle); assay
      off-seam positive-evidence rule; done-gate iteration-log path; vault lifecycle (admitted, not
      earned); circular read-order (canonical order now in CLAUDE.md); ICM framing (modification of
      methodology, not a repo fork); ECP demoted to retired lineage.
    - Replaced core capstone-build.md with a generic iteration-workflow interface (chain → pilot).
- broken: nothing in the CORE is broken.
- 2026-06-29 (later): made the repo FULLY agnostic per user direction. Removed the GTM pilot; this
  repo is now the universal ENGINE only. Added pilots/_TEMPLATE/ (the pilot contract). Universalized
  the tool layer (universal manifest in platform/TOOLING.md; tool-status.md at root); dropped all
  domain tools + ponytail. Added changelog/ + revert guard, and the self-scanning tool layer.
  Full-run simulation (sim/full-run branch) validated the engine's stage chain end to end.
- next: to RUN, create a domain pilot in ITS OWN project by copying pilots/_TEMPLATE/ and filling the
  extension points (seam, deposits, domain tooling, iteration-workflow, dry-run, answer-key). The
  engine here needs nothing further to be reused; it is domain- and (beyond universal harness)
  tool-agnostic.
