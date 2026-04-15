# Changelog

All notable changes to `idea-to-stories` are documented here.

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
