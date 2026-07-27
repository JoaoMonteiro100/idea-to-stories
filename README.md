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

Skills are plain folders. Clone the repo and copy the skill into your Claude skills directory:

```bash
git clone https://github.com/JoaoMonteiro100/idea-to-stories.git
mkdir -p ~/.claude/skills/idea-to-stories
cp idea-to-stories/SKILL.md ~/.claude/skills/idea-to-stories/
```

Claude Code looks for skills in `~/.claude/skills/` (personal) and `.claude/skills/` (per-project). Once installed, invoke it with `/idea-to-stories`, or just describe your idea and let it trigger.

---

## How it works

It helps to keep three dials separate:

- **The command decides *which phases run*** — how far through the pipeline you go.
- **Parameters decide *how the output looks and behaves*** — they mostly don't change what runs.
- **Depth is automatic** — the skill sizes the work (Simple / Standard / Complex) to the idea.

| Dial | The question it answers | Values |
|---|---|---|
| **command** | How far through the pipeline? | `run_pipeline` (all) · `discover` (0–1) · `specify` (2–3) · `generate_stories` (4) · `quick` (straight to stories) · `review` |
| `depth` | How much analysis? | `auto` (default) · `simple` · `standard` · `complex` |
| `detail` | How much of it is written up? | `standard` · `full` |
| `audience` | Who is it written for? | `product` · `design` · `engineering` · `cross_functional` |
| `strictness` | How hard does it push back on vague input? | `light` · `standard` · `strict` |
| `format` | What is it rendered as? | `markdown` · `json` · `notion` · `jira` |

Two things worth knowing up front:

- **`depth` scales the work; `detail` scales the write-up** — `depth=complex` does more analysis, `detail=full` just explains it more. To skip the analysis entirely and get a rough backlog fast, use the **`quick` command** (stories tagged `[UNVALIDATED]`) — skipping work is a scope choice, not a detail level. And `depth` defaults to `auto`, so you rarely set it unless you want to override the auto-detect.
- **`strictness` never hides a problem** — even `light` surfaces every issue; it only changes tone and whether it blocks.

The short version to hold in your head: *command = how far through the pipeline · `depth` = how much analysis · `detail` = how much of it is written up · `audience` = who it's for · `strictness` = how much it argues with you · `format` = how it's rendered.*

```
#                 how far           who for            how strict       rendered as
/idea-to-stories generate_stories [requirements] audience=engineering strictness=strict format=jira
```

If you don't type a command, the skill infers one from what you give it (a raw idea → full run; pasted requirements → stories; pasted stories + "what's wrong?" → review). An explicit command always wins.

---

## Commands

| Command | What it does |
|---|---|
| `help` | Lists all commands and parameters |
| `discover` | Phases 0–1: problem framing, stakeholders, personas, journey maps |
| `specify` | Phases 2–3: domain model, use cases, typed requirements |
| `generate_stories` | Phase 4: stories with INVEST validation and Gherkin acceptance criteria |
| `run_pipeline` | Full run: Phases 0 → 4 |
| `quick` | Fast path: skips to Phase 4 stories from a raw idea, tagged `[UNVALIDATED]`. The "I know the risks" mode |
| `review` | Critiques existing stories against INVEST, Gherkin quality, and traceability |

If you don't specify a command, the skill infers the right mode from what you provide.

---

## Parameters

Parameters can be appended to any command: `parameter=value`

### `depth` — how much analysis
| Value | Description |
|---|---|
| `auto` | Default. Infers Simple / Standard / Complex from the idea |
| `simple` | A few stakeholders, no journey map, a handful of requirements and stories |
| `standard` | Full phases; one journey map (primary persona) + friction summaries for the rest |
| `complex` | Full depth: journey map per persona, ASRs, integration contracts, phased delivery |

### `detail` — how much is written up
| Value | Description |
|---|---|
| `standard` | Default. The artifacts, cleanly presented |
| `full` | Adds rationale, confidence levels, proactive edge cases, and methodology notes |

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
/idea-to-stories quick [idea]
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
├── LICENSE
├── .gitignore
└── SKILL.md          # The skill definition
```

---

## Related

Part of a broader family of UX & product-design skills for Claude — see the
[**ux-design-skills**](https://github.com/JoaoMonteiro100/ux-design-skills)
collection (usability testing, heuristic & accessibility audits, personas,
competitive analysis, inclusive design, and visual-design fundamentals).
`idea-to-stories` is the standalone, in-depth requirements-engineering member of
that family.

---

## License

MIT
