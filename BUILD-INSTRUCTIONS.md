# BUILD-INSTRUCTIONS.md — READ THIS FIRST, IN FULL, BEFORE TOUCHING ANYTHING

> If you have not read SETUP.md, read it first. It carries the kickoff prompt and the ONE rule about
> pre-loading material: vault/ is the only door; never place material directly into carts/, tailings/,
> bench/, the manifest, or platform/ — doing so silently invalidates the run. Route ALL material
> through vault/ → excavation → assay.

You are Claude Code. You are building the **M2W** pipeline skeleton. M2W — Manifest to Workspace — is
the name of the whole system: the transform it performs, manifest in, workspace out. Its governing
discipline is deferral. You are an excellent
builder. This document assumes you will interpret every instruction **literally**, so it is
written literally. Where this spec is silent, do **nothing** — do not infer, do not improve, do
not fill gaps. Silence means "not yet," not "use your judgment."

## What M2W is (so you build the right thing)

M2W is not a system that tests work at gates to see if it is good enough. It is a system that
**withholds evaluation and execution until acting is safe** — until the context is clean enough
that committing will not contaminate the result. Gates are **deferral points**, not performance
checkpoints. Nothing is judged, executed, or committed until it is genuinely needed. The whole
design exists to prevent context contamination, not to make material prove its worth.

Build with that emphasis: stages **hold** until the next thing requests them; they do not **run**
to prove themselves.

## What you are building in THIS pass

The **skeleton + logging**. That is all:

1. The folder structure exactly as delivered (already present in this package).
2. The routing files (`CONTEXT.md`) at each level — already present; verify, do not rewrite.
3. The stage contracts (`CONTRACT.md` in each stage) — already present; verify, do not rewrite.
4. The three log files in `logs/` — already present; verify they exist and are writable.
5. Nothing else.

This pass produces a **standing, empty pipeline that runs no work yet**. The frame must stand
correctly before any material enters it or any tool is wired to it.

## What you are NOT building — HARD STOPS

- **DO NOT** populate `vault/`, `carts/`, `tailings/`, `bench/`, or `library/` with any content.
  They start empty. Empty is correct. An empty folder with its `CONTEXT.md` is a finished
  deliverable for this pass.
- **DO NOT** clone, fork, vendor, download, or reference any external repository to obtain MWP or
  any of its principles. MWP is described in `platform/MWP.md` as principles you implement
  natively. There is no repo to fork. If you find yourself running `git clone`, you have misread
  this document. Stop.
- **DO NOT** introduce ICM (Interpreted Context Methodology) in any form. It is deliberately
  excluded so it cannot hijack the infrastructure. If any file appears to reference ICM as
  something to build from, treat it as an error and flag it.
- **DO NOT** wire, install, configure, or invoke any tool (no gstack, no gbrain, no browser, no
  MCP servers, no skills). Tool slots are marked `TODO(tools)`. Leave them literal. Tools are
  wired in a later pass, after human review.
- **DO NOT** add subagents, parallel agents, or any multi-agent orchestration. Single-agent by
  law (see `platform/PRINCIPLES.md`).
- **DO NOT** write code beyond what a `CONTRACT.md` names by exact file. No scripts, no helpers.
- **DO NOT** "improve" the architecture. If something looks wrong, write a note to
  `logs/failures.md` and **stop**. Do not fix it yourself.

## The one principle that governs your whole existence here

**You do not think. You execute.** The thinking already happened; it lives in these files. Your
job is to lay the structure exactly as specified, verify it stands, and log anything that does not
match. When uncertain, you **do not guess** — you route the item to `bench/` (see
`stages/02-assay/CONTRACT.md`) or write to `logs/failures.md` and stop. Guessing is the single
failure mode this whole architecture defers around. A correct "I stopped because X was ambiguous"
is a success. A plausible guess is a failure, even if it happens to be right.

## Order of operations for this pass

1. Read `CLAUDE.md` (the operating manual / the law).
2. Read `platform/PRINCIPLES.md`, `platform/MWP.md`, `platform/GATES.md`, `platform/LOGS.md`,
   `platform/glossary.md`. These are the law. Do not deviate.
3. Read `ARCHITECTURE.md` (full spec, for understanding — contracts are authoritative for building).
4. Walk every folder. Verify each has its `CONTEXT.md` and, where specified, its `CONTRACT.md`.
5. Verify the three log files exist in `logs/` and are writable.
6. Append one line to `logs/gate-checks.md` recording that skeleton verification passed, with a
   timestamp and a list of any files missing or not matching.
7. Stop. Report to the human: what you verified, what (if anything) was missing or ambiguous. Do
   not proceed to wire tools or accept material.

## What "done" means for this pass

The frame stands, every routing and contract file is present and matches the spec, the logs exist
and are writable, and you have logged the verification. Then you stop and wait for human review.
By the system's own law, there is no value in continuing past that point (see
`stages/04-iteration/done-gate.md`).
