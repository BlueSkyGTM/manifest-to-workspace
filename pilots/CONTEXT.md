# CONTEXT.md — pilots/ (the instantiation CONTRACT, not instantiations)

This repo is the domain-AGNOSTIC engine. A **pilot** is one domain's instantiation of the engine.
Pilots are domain-specific, so they live in **their own projects** — not in this engine repo. What
ships here is `_TEMPLATE/`: the CONTRACT a pilot must satisfy. A domain project copies the template,
renames it, and fills it.

## The one law of this layer (ICM one-way references)
- A pilot MAY reference core (its contracts, schemas, gates, glossary).
- The core MUST NEVER reference a specific pilot. Deletion test: this engine stands with NO pilot
  present — that is the normal state of this repo.

## The extension points a pilot fills (declared by core)
| Core declares (interface) | Pilot supplies (in its own project) |
|---|---|
| Seam contract (`stages/02-assay/CONTRACT.md`) | the domain boundary + its near-miss edges |
| Deposit/extractor contract (`stages/01-excavation/CONTRACT.md`) | where material lives + which extractor matches |
| Universal tool policy + scan (`platform/TOOLING.md`) | the DOMAIN tools (extractors, build-chain) |
| Iteration-workflow interface (`stages/04-iteration/`) | the concrete build chain |
| Validation ladder (`DRY-RUN.md`) | the domain's reversible run + grading |
| Logs / manifest / gates | (used as-is; nothing to supply) |

Read `_TEMPLATE/CONTEXT.md` for the full contract and the required shapes. There is intentionally no
concrete pilot in this repo; the engine is what you are looking at.
