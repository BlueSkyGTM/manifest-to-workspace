# CONTEXT.md — _TEMPLATE (the pilot contract — copy this into your domain project)

This repo is the domain-AGNOSTIC engine. A **pilot** is one domain's instantiation of it, and pilots
are domain-specific, so they live in **their own projects**, not here. This `_TEMPLATE/` is the
CONTRACT: the extension points your domain must fill. Copy it into your project, rename it to your
domain, and fill each file. The engine never references your pilot; your pilot references the engine.

## The one law (ICM one-way references)
- Your pilot MAY reference core (its contracts, schemas, gates, glossary).
- Core MUST NEVER reference your pilot. Deletion test: the engine must stand with no pilot present.

## Extension points your pilot must supply
| You provide | Fills the core interface |
|---|---|
| `seam.md` | the assay's seam (`stages/02-assay/CONTRACT.md`) — your domain's on-seam/off-seam edges |
| `deposits.md` | the excavation inputs (`stages/01-excavation/CONTRACT.md`) — your sources + extractor matching |
| `tooling.md` | your DOMAIN tools (the universal tools are core; see `platform/TOOLING.md`) |
| `iteration-workflow.md` | the concrete build chain (`stages/04-iteration/iteration-workflow.md`) |
| `dry-run.md` | your validation ladder (`DRY-RUN.md`) — the reversible first run + grading |
| `design-schema.md` | the quality cookie-cutter (cut during schema-discovery iteration 1) |
| `answer-key/` | the quarantined grading key for blind-discovery (the model never reads it) |
| `learned-seam/` | where the seam's training accretes (the OUTPUT of your first run) |

## Required shapes (so the handoffs match the engine)
- **seam.md** — on-seam definition; off-seam patterns; the highest-value `looks-transferable-but-off-seam`
  cases. The assay needs BOTH a positive on-seam test and a positive off-seam test (see the assay
  contract); supply both.
- **deposits.md** — a register row per source: `deposit_id | source | type | bounded_space | extractor
  | status`, and a deposit-type → extractor table whose left column matches your `type:` enum exactly.
- **tooling.md** — a manifest table the core Tool scan reads:
  `| tool | role/stage slot | required | detect (shell test) | install (shell cmd) |`. Flag anything
  whose install you can't state safely as `MISSING-ASK` (never guess an install).
- **design-schema.md** — declare its path here; the schema-discovery iteration writes it.

Read `pilots/CONTEXT.md` for how this layer relates to the engine.
