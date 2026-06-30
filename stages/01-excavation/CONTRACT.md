# CONTRACT — Stage 01: Excavation

## Purpose
Pull raw material from located deposits into vault/ and give each piece a stable ADDRESS. No quality
judgment (deferred to the assay). Coverage, not choice.

## Why excavation exists (contamination defense, not asset generation)
Excavation is not "go fetch the assets." Its job is to convert located material into **addressed
routes** in vault/ so every downstream stage can pull the exact slice it needs JUST-IN-TIME and
nothing else. The whole point is that the agent NEVER absorbs the whole corpus into context — it
follows an address to one piece. Hauling without addressing would defeat this: an unaddressed pile
forces whole-corpus loading, which is the contamination excavation exists to prevent. Address-on-haul
is the contamination defense; the assets are a side effect of building the routes.

## Inputs
- A located deposit (a source to extract from) and the bounded space to cover (what counts as "the
  whole deposit"). These are domain-specific and supplied by the instantiation, NOT by core — see the
  pilot's deposit register and extractor-matching rule. On the pilot the human supplies the deposits.

## Process
1. Match the deposit to its extractor (the universal intake extractors are in `platform/TOOLING.md`;
   which one is keyed to the deposit type). **Native text is a no-op:** a deposit already in markdown
   or plaintext (`.md` / `.txt`) needs NO extractor — pass it through unchanged (it lands in vault at
   step 4 as-is). Extractor-matching applies only to NON-text deposits (pdf/office, video, web). If a
   non-text deposit has no matching extractor, do NOT guess — bench it and log to failures.
2. Extract the material to **markdown** (e.g. markitdown for pdf/office). The markdown conversion is
   not cosmetic — it is what makes a piece **catalogable and assayable at all** (you cannot sort or
   manifest an unopened PDF). This is mechanical, not a judgment: get the substance into markdown.
   For a chart/diagram, extract its UNDERLYING DATA as text (a markdown table or a described relation),
   NOT a screenshot or a blind copy-paste. Do NOT render visuals here — choosing the best visual form
   (mermaid / excalidraw / table) is a judgment, deferred to iteration (stage 04) on the material the
   build actually pulls.
3. Haul EVERYTHING that meets the extraction criteria. Do not select, filter, or judge worth —
   selection is deferred to the assay (stage 02). Coverage is the goal. (The markdown conversion runs
   on all hauled material because all of it must be assayable; the EXPENSIVE visual rendering does
   not — that waits for iteration.)
4. Land each piece in vault/ with a stable address. **Address convention:** the `id` is a kebab-case
   slug of the piece's title/source, **unique within the repo** — before assigning it, check the
   candidate against every existing address (`vault/account.md`, `manifest/index.md`, and `library/`);
   on collision, suffix `-2`, `-3`. The vault filename is `vault/<id>.md`. This convention is fixed so
   addresses are stable and inferable, not guessed per-run; every downstream stage reuses the same
   `id`. Repo-wide uniqueness (not merely per-run) is what lets a LATER LOOP add new material beside
   the old without collision — there is no run counter.
5. Record each piece in the vault account (`vault/account.md`): id + source + format + the
   bounded-space it came from, plus `consumed: false`. The account is the index that makes vault/
   routable instead of a pile.

## Outputs
- Raw material in vault/, each piece addressed.
- `vault/account.md`: the vault-level index. Every hauled piece has one row
  (`- id: <address> | source: <deposit> | format: <type> | bounded_space: <coverage target> | consumed: false`).
  `consumed:` is minted `false` here and flipped `true` by the assay when it routes the piece; the
  assay processes only `consumed: false` rows, so a later loop never re-assays old material. Nothing
  unaddressed, nothing unaccounted.

## Deferral point (tooling checks only — NO quality judgment)
- Reach: did the extraction reach the source? (pass/fail)
- Coverage: did it cover the whole bounded space? Report anything that came back empty as explicitly
  as anything that hit — an empty result is DATA, not a reason to move on.
- Addressability: did every hauled piece land in vault/ with an address?
Log the firing to logs/gate-checks.md.

## TODO(tools)
The extraction tool (e.g. an authenticated browser/extractor) is wired in a LATER pass. For this
pass, leave this as TODO(tools). Do NOT install or invoke any extraction tool now.

## Do not
- Do not judge whether any piece is "good" or "relevant." That is deferred to the assay.
- Do not delete or skip material to be efficient. Haul the whole bounded space.
- Do not load vault/ contents into context. They sit addressed on disk until pulled (M2W).
