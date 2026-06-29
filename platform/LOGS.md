# platform/LOGS.md — The Three Log Streams (Built In, Continuous, Mandatory)

Logging is not optional and not a postmortem. The logs are the system learning its own shape and,
during the pilot, the training corpus for the seam layer. Three streams, always written.

## 1. logs/failures.md — what broke or stopped
Append an entry whenever:
- something did not match the spec,
- the agent stopped on an ambiguity it could not resolve,
- a tooling check or conformance check failed,
- anything looked wrong that the agent was told not to fix itself.

Entry format (append-only, newest at bottom):
```
## <ISO-8601 timestamp>
- stage: <stage or "skeleton-verify">
- what: <what broke or stopped>
- expected: <what the spec said>
- found: <what was actually there>
- action: <stopped / benched / flagged>   (NEVER "fixed it myself" unless a contract named the fix)
```

## 2. logs/gate-checks.md — every deferral point firing
Append an entry every time a gate/deferral point fires (tooling check, assay, conformance, done-gate).

Entry format:
```
## <ISO-8601 timestamp>
- gate: <tooling | assay | conformance | done-gate>
- stage: <stage>
- item: <address of the item, or "n/a">
- outcome: <passed | failed | carted | tailed | benched | ship | iterate>
- note: <one line>
```

## 3. logs/rejections.md — the tailings index (reviewable recycle bin)
Append an entry for every tailings decision. Off-seam material is NEVER deleted; it is tailed with a
reason so it can be reclaimed if scope shifts. This file is the queryable index of the tailings/ pile.

Entry format — CAPTURE THE FULL ITEM POINTER PLUS THE CURATED REASON, not just a verdict:
```
## <ISO-8601 timestamp>
- item: <address in tailings/>
- verdict: tailings
- reason: <the curated reason it is off-seam — written by the human on the pilot>
- category: <off-seam | looks-transferable-but-off-seam>   # the second is the highest-value tag
- reclaimable-if: <what scope shift would make this material on-seam again>
```

The `looks-transferable-but-off-seam` category is the highest-value training data in the system. It
captures material that resembles the seam because it shares a principle (e.g. a DevOps or
platform-engineering pattern) but sits off it. This is the ~50% of wasted effort the seam layer
exists to prevent. Tag it distinctly; never let it file as an ordinary cut.

## Why item-plus-reason, not verdict-only
A verdict-only log teaches a brittle blacklist (memorizes specific items). An item-plus-reason log
teaches the principle that generalizes (recognizes new off-seam items it has never seen). The reason
is the actual curriculum the seam layer learns from. On the pilot, the human writes every reason.
This is non-negotiable and cannot be automated for convenience — automating the reason collapses the
seam layer back into a blacklist.
