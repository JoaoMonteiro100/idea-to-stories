# Changelog

All notable changes to `idea-to-stories` are documented here.

---

## v3.0.0 — July 2026

**Breaking:** parameter redesign for clarity. v2 parameter names are not accepted (clean break).

### Why
`output_mode` (summary/standard/full) and the automatic depth calibration (Simple/Standard/Complex) were two overlapping "how much" concepts, which made the API confusing. They're now cleanly separated.

### Changed
- **`output_mode` split into two dials:**
  - **`depth`** (`auto` default · `simple` · `standard` · `complex`) — how much *analysis* is done (personas, use cases, ASRs, integration contracts). Now **user-overridable**; previously depth was automatic-only.
  - **`detail`** (`standard` · `full`) — how much of that analysis is *written up* (rationale, confidence, methodology notes).
- **`output_mode=summary` / `--fast` removed**, replaced by a new **`quick` command** — skipping the pipeline is a scope choice, so it belongs with the commands, not a detail level.
- **Commands renamed for consistency:** `run_pipeline` → `run`, `generate_stories` → `stories`. All commands are now single tokens: `discover`, `specify`, `stories`, `run`, `quick`, `review`, `help`.

### Added
- **Interactive vs. batch execution** documented as a first-class concept (previously only referenced in passing).
- The skill now **infers parameters from the user's phrasing** (e.g. "for my engineering team" → `audience=engineering`) and surfaces the tunable levers in one short line on first output, instead of interrogating up front or forcing a config questionnaire.
- **`quick` named as the sole sanctioned exception to Hard Rule 1** — its stories carry `[UNVALIDATED]` in place of use-case/requirement traceability.

### Migration from v2
- `output_mode=full` → `detail=full`
- `output_mode=summary` (or `--fast`) → the `quick` command
- `run_pipeline` → `run`; `generate_stories` → `stories`
- Depth is now settable directly: `depth=simple|standard|complex` (or leave it `auto`)

---

## v2.1.0 — July 2026

Refinements from a full evaluation run of the pipeline.

### Changed
- **Lighter Phase 1 at Standard depth**: journey-map the single most important persona (with a one- or two-line friction summary for the rest) instead of a full five-stage map per persona. Complex depth still maps every primary persona; Simple skips journey maps entirely. Removes upfront ceremony where it wasn't earning its keep.

### Fixed
- Corrected the `format=json` metadata `mode` enum to match the actual command names (`run_pipeline`, `generate_stories` — was `full_run`, `stories`).
- Resolved a tension between `output_mode=full` and `audience=product`: edge cases are still surfaced under `full`, phrased as user impact rather than dropped.
- De-duplicated the "strictness only changes tone, never suppresses a flag" rule — stated once as canonical, cross-referenced elsewhere.
- Fixed the README: accurate repo structure, and working clone-and-copy install instructions (previously pointed to a release that doesn't exist).

---

## v2.0.0 — April 2026

Complete rewrite from the ground up. v1 was a structured template with requirements vocabulary. v2 is a requirements engineering process.

### Added
- **5-phase pipeline**: Problem Framing → Stakeholder & Elicitation → Domain Modeling → Requirements Engineering → Story Generation
- **Personas**: Behavioral archetypes with current workflow, frustrations, and workarounds — not just stakeholder roles
- **Journey maps**: Current-state journey mapping per primary persona before requirements are written
- **Elicitation techniques**: Guided prompts for interviews, observation, prototype walkthroughs, artifact analysis, and JTBD framing
- **Structured use cases**: Full format with preconditions, main success scenario, alternative flows, exception flows, and postconditions
- **Quality attribute scenarios**: Six-element structured format (Given / when / the artifact / must / measured by). Prose NFRs rejected
- **Architecturally significant requirements (ASRs)**: Flagged with architectural implications and trade-offs
- **Volatility assessment**: Stable / Evolving / Uncertain per requirement
- **MoSCoW prioritisation**: All four categories including Won't
- **INVEST validation**: Per story, with split recommendations for violations
- **Gherkin acceptance criteria**: Mandatory. Happy path + unhappy path minimum. Boundary value scenarios for numeric/threshold stories
- **Requirement coverage report**: Fully covered / partially covered / not covered
- **Mode detection**: Infers discover / specify / generate / review from input
- **Commands**: `help`, `discover`, `specify`, `generate_stories`, `run_pipeline`, `review`
- **Parameters**: `output_mode`, `audience`, `strictness`, `format`
- **Format outputs**: `markdown`, `json` (with full schema), `notion`, `jira`
- **Depth calibration**: Simple / Standard / Complex tiers with different phase depths
- **15 hard rules**: Non-negotiable constraints on output quality

### Removed
- Single-pass template approach
- Generic "target users" input
- Prose NFRs
- Unstructured acceptance criteria

---

## v1.0.0 — Initial release

Single-pass plugin. Took idea input, structured it into five requirement categories, generated stories. No elicitation, no stakeholder management, no use cases, no INVEST validation.
