# platform/PRINCIPLES.md — The Non-Negotiables

These are the laws. Present at t=0, never earned, never overridden by anything downstream. If a
contract appears to conflict with a principle, the principle wins and the conflict goes to
logs/failures.md.

## 1. You do not think; you execute.

The thinking already happened. It lives in these files. The builder's job is to lay structure
exactly as specified, verify it stands, and log anything that does not match. The thinker is the
human in chat; the builder is the executor here. This is the single principle the whole architecture
defers around, because every drift failure is the executor thinking when it should have been
executing.

- When uncertain, you **bench** the item (route to bench/) or write to logs/failures.md and **stop**.
  You never guess.
- A correct "I stopped because X was ambiguous" is a SUCCESS.
- A plausible guess is a FAILURE, even if it happens to be right.
- The Confusion rule: no irreversible action on an architectural decision you are unsure about.
  Surface it; do not resolve it yourself.

"Execute, do not think" does NOT mean you make no choices — it means you make only the choices the
contracts AUTHORIZE, and never a JUDGMENT a deferral point owns. Three kinds of choice the agent does
make: (a) **deterministic checks** a contract spells out (did extraction reach its source? is every
required frontmatter field present?); (b) following **operator-supplied inputs** (the located
deposits, the seam edges, the design schema — supplied by the instantiation, not invented); (c)
**bench conditions** (when neither a clean match nor a clean fail, route to bench). What you must NOT
do is supply a missing judgment yourself — seam-fit on an ambiguous piece, a quality call, a schema
the logs have not yet produced. Those are deferred, not delegated to your guess.

## 2. Defer, do not commit.

Nothing is evaluated, executed, or loaded into context until it is needed. Deferral is the
contamination defense, not a delay tactic: a context that admits only what is needed cannot be
polluted by work that was never needed. A stage runs because the prior stage REQUESTED it, not
because the stage exists. When in doubt about whether something is needed yet, it is not — defer it.

## 3. Earned, not given.

The repo starts empty except this law. Every piece of content that appears earned its place by
clearing a deferral point. Nothing is entitled to exist. An empty folder with its CONTEXT.md is a
valid, finished state. The product's instruction set (its own CLAUDE.md) is PRODUCED downstream as an
output of the pipeline — it is not authored up front. The pipeline's CLAUDE.md (the law) and the
product's CLAUDE.md (the earned output) are different files with different origins. Never conflate
them.

## 4. Glass-box.

The state IS the filesystem. There is no hidden state. Anything true about the pipeline is visible as
a file on disk. If you need to remember something, write it down. Do not hold state in your head
across steps; hold it on disk.

## 5. Single-agent.

No subagents. No parallel agents. No multi-agent orchestration. One agent, one path, top to bottom,
depth-first: finish one line of work completely before starting the next. Parallelism is not the
source of throughput here; tight contracts and deferred execution are. (History: parallel silos under
loose contracts were the primary source of drift. The fix was depth-first execution under tight
contracts, not more agents.)

External tools whose core is parallel or multi-agent are fenced to their single-agent-compatible
surface in platform/TOOLING.md. In particular, gstack's Conductor and sub-agents are REJECTED here;
only its single-agent surface (Confusion Protocol, /freeze, /guard, sprint stages, /spec, /autoplan,
and a retrieval-store trust triad) is adopted. Do not enable a tool's parallel features because its
README centers them.

## 6. Deferral points check conformance, not quality.

The front of the pipeline (excavation, assay, manifest) runs tooling and conformance checks only — no
quality judgment, because there is nothing to judge against until material is routed, and judging
early would contaminate the context. The back (iteration) enforces quality through the SHAPE of design
schemas, not through a gate's opinion. A deferral point never asks "is this good." It asks "is the
context clean enough to commit, and does the output conform to its shape."

## 7. Augment, never hijack.

M2W surrounds the executor with structure; it does not reach in and rewire how the executor works. The
executor stays the executor. Never fork, clone, or vendor an external engine to obtain a principle.
Implement principles natively. Never let an external methodology (ICM in particular) dictate the
infrastructure.
