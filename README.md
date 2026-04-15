# idea-to-stories

> Turn product ideas into rigorous, traceable user stories through structured requirements engineering.

A Claude skill that takes you from raw idea to a production-ready backlog — via problem framing, stakeholder analysis, personas, journey mapping, domain modeling, use cases, and typed requirements. Every story traces back to a use case, which traces back to a requirement, which traces back to a real stakeholder concern.

---

## Why this exists

Most story-writing tools take what you type and reformat it. This one does the opposite — it extracts what you haven't articulated yet. Requirements you didn't know were missing. Stakeholder conflicts you hadn't named. Domain terms that mean different things to different people. Edge cases that only appear when you model the flows properly.

The output isn't just stories. It's a traceable chain from problem to backlog that a team can actually build from.

---

## Pipeline

The skill runs five sequential phases:

| Phase | Name | Output |
|---|---|---|
| 0 | Problem Framing | Problem Statement with explicit out-of-scope |
| 1 | Stakeholder & Elicitation | Stakeholder Map, Personas, Journey Maps, Elicitation Gap List |
| 2 | Domain Modeling | Glossary, Core Entities, System Boundary, Use Cases |
| 3 | Requirements Engineering | BR, UR, FR, QR, CON — typed, numbered, prioritised (MoSCoW) |
| 4 | User Story Generation | Stories with INVEST validation, Gherkin ACs, coverage report |

Phases can be run individually or as a full pipeline. The skill detects what you've already done and picks up from there.

---

## Installation

Download `idea-to-stories-skill.skill` from [Releases](../../releases) and install it in your Claude workspace.

---

## Commands

| Command | What it does |
|---|---|
| `help` | Lists all commands and parameters |
| `discover` | Phases 0–1: problem framing, stakeholders, personas, journey maps |
| `specify` | Phases 2–3: domain model, use cases, typed requirements |
| `generate_stories` | Phase 4: stories with INVEST validation and Gherkin acceptance criteria |
| `run_pipeline` | Full run: Phases 0 → 4 |
| `review` | Critiques existing stories against INVEST, Gherkin quality, and traceability |

If you don't specify a command, the skill infers the right mode from what you provide.

---

## Parameters

Parameters can be appended to any command: `parameter=value`

### `output_mode`
| Value | Description |
|---|---|
| `summary` | Fast, condensed. Stories tagged `[UNVALIDATED]`. Alias: `--fast` |
| `standard` | Default |
| `full` | Extended output with rationale, confidence levels, and methodology notes |

### `audience`
| Value | Description |
|---|---|
| `product` | Leads with business requirements and MoSCoW. Outcome-focused language |
| `design` | Leads with flows and personas. UX Considerations callout per story |
| `engineering` | Leads with FRs, constraints, and ASRs. Implementation Notes callout per story |
| `cross_functional` | Balanced. Default |

### `strictness`
| Value | Description |
|---|---|
| `light` | Suggestions, not blockers. Documents assumptions and proceeds |
| `standard` | Default. Flags vague language, blocks on unresolvable gaps |
| `strict` | Maximum challenge. Refuses vague language. For handoff-ready specs |

### `format`
| Value | Description |
|---|---|
| `markdown` | Default |
| `json` | Structured JSON output with full schema |
| `notion` | Optimised for pasting into Notion |
| `jira` | Epics and stories with Jira-compatible fields and size estimates |

---

## Usage examples

```
# Full pipeline from a rough idea
/idea-to-stories I want to build a tool that helps restaurant owners manage reservations

# Discovery only — explore the problem before writing requirements
/idea-to-stories discover We're seeing high churn in our onboarding flow

# Turn existing requirements into stories for an engineering audience
/idea-to-stories generate_stories [requirements] audience=engineering strictness=strict

# Review stories before a sprint
/idea-to-stories review [stories] format=notion

# Fast backlog when you know the risks
/idea-to-stories run_pipeline [idea] output_mode=summary
```

---

## What the output covers

**Phase 0 — Problem Statement**
Current situation, affected parties, failure mode, desired outcome, why now, explicit out-of-scope.

**Phase 1 — Discovery**
Stakeholder map with influence levels. Behavioral personas with current workflow, frustrations, workarounds. Current-state journey maps per persona. Elicitation gap list with flagged assumptions.

**Phase 2 — Domain Model**
Glossary of ambiguous terms. Core domain entities with relationships. System boundary (inside / outside / integration points). Full use cases with alternative and exception flows.

**Phase 3 — Requirements**
Five typed requirement categories (BR, UR, FR, QR, CON). Quality requirements as structured attribute scenarios — measurable, not prose. Architecturally significant requirements flagged with implications. Conflict and tension documentation. Volatility assessment. MoSCoW prioritisation including Won't.

**Phase 4 — Stories**
Stories grounded in specific stakeholder roles from Phase 1. INVEST validation per story with split recommendations when needed. Gherkin acceptance criteria — happy path, unhappy path, boundary values. Edge cases assigned to specific stories. Requirement coverage report.

---

## Traceability chain

Every story output connects back through:

```
Problem Statement
  └── Stakeholder concern
        └── Requirement (typed, numbered)
              └── Use case
                    └── User story
                          └── Acceptance criterion (Gherkin)
```

Nothing in Phase 4 exists without a path back to Phase 0.

---

## Structure

```
/
├── README.md
├── CHANGELOG.md
├── .gitignore
├── idea-to-stories-skill/
│   └── SKILL.md          # The skill definition
└── evals/
    └── evals.json        # Eval test cases and assertions
```

---

## License

MIT
