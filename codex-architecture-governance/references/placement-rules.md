# Placement rules

Use this reference when deciding whether guidance belongs in global AGENTS.md, a
project AGENTS.md, a skill, a reference file, a project knowledge wiki, a hook,
an agent config, a plugin, or an external connector.

## Fast decision table

| Content type | Put it here | Why |
|---|---|---|
| Always-on safety rule | Global `AGENTS.md` | Must affect most work |
| Project-specific engineering rule | Project `AGENTS.md` | Applies only inside that repo |
| Reusable workflow or SOP | Skill `SKILL.md` + `references/` | Triggered only when relevant |
| Large business background | Focused skill reference or project wiki | Too big for global context |
| Current project status | `knowledge/projects/<project>/current-state.md` | Drift-prone active state |
| Source inventory or durable synthesis | Project knowledge wiki | Maintained context, not rules |
| Deterministic repeated command | Skill `scripts/` | Less error-prone than retyping |
| Session lifecycle action | `hooks.json` + hook script | Runs before/after known events |
| Independent exploration/review role | Agent config or subagent prompt | Keeps noisy work out of main thread |
| Team-wide installable bundle | Plugin/package | Use only when multiple machines need sync |
| External data/API capability | MCP, connector, or CLI | Tool access, not instruction storage |

## Global AGENTS.md

Use global `AGENTS.md` for:

- context management rules
- safety boundaries
- skill routing principles
- default writing style
- compact, stable business priorities
- one-line pointers to focused skills

Do not put these in global `AGENTS.md`:

- full company background
- full ML textbook notes
- architecture diagrams and examples
- large prompt libraries
- current machine inventory
- volatile access status or permissions

Good global rule:

```markdown
- For Codex architecture, Skill/Agent/Hook/Plugin/MCP placement, or local setup
  audits, use `codex-architecture-governance`.
```

Bad global rule:

```markdown
Paste the entire five-layer architecture handbook and every local path here.
```

## Skills

Use a skill when the same reasoning or workflow will repeat. Keep `SKILL.md`
short:

- frontmatter says what the skill does and when to use it
- body says the workflow and which reference to read
- detailed knowledge goes in `references/`
- deterministic helpers go in `scripts/`

Avoid catch-all skills. A skill named `memory` or a generic
`skills/memory.md` that contains company strategy, ML theory, architecture
rules, and project state will become hard to route and expensive to load.

## References inside skills

Use `references/` for:

- detailed layer definitions
- path maps
- API docs
- business background
- model-selection rules
- examples and decision tables

Keep references one hop from `SKILL.md`. The main skill must clearly say when to
read each reference.

## Project knowledge

Use project knowledge for durable but project-scoped state:

- current objectives
- decisions
- validated findings
- source maps
- open questions
- checkpoint summaries

Do not put secrets or raw credentials here. Do not treat knowledge files as
source truth when source files or systems are available.

## Hooks

Use hooks only when the action is:

- deterministic
- low risk
- tied to a lifecycle event
- useful even without model reasoning

Good hook candidates:

- initialize project knowledge
- checkpoint before compaction
- record stop-time state

Poor hook candidates:

- choose an ML model
- search the web
- publish docs
- modify third-party systems

## Agents and subagents

Use agents or subagents when work is separable:

- exploration
- review
- documentation verification
- long log analysis
- parallel research

Do not use a subagent just because a topic is important. Main-thread synthesis
remains responsible for final judgment.

## Plugins and packages

Use plugins/packages when a whole team needs the same capabilities installed and
versioned. For a single-machine personal workflow, a global skill is usually
lighter.

## Business background

Company strategy and product background should not live as a giant global
AGENTS block. Use one of these patterns:

1. Project wiki page for project-specific background.
2. A focused business-context skill with references by domain.
3. A short AGENTS pointer if the context must influence many tasks.

Example:

```markdown
- For Petlibro strategy work, consider the 2026 H2 priorities: Product and R&D
  Capability, UX/User Experience, and Net Profit; load project knowledge or
  the relevant business-context skill when details are needed.
```

## Machine learning knowledge

ML model-selection knowledge should be a focused skill, not a section inside
this architecture skill and not a giant global AGENTS block.

Recommended future shape:

```text
ml-model-selection/
├── SKILL.md
├── references/
│   ├── data-diagnosis.md
│   ├── supervised-models.md
│   ├── unsupervised-models.md
│   ├── evaluation-metrics.md
│   └── petlibro-business-use-cases.md
└── scripts/
    └── optional data profiling helpers
```

Global AGENTS should contain only a short routing rule, for example:

```markdown
- For ML, prediction, classification, clustering, causal modeling, or model
  selection tasks, use the focused ML skill and start by defining metric,
  numerator, denominator, time window, labels, sample size, leakage risk, and
  business objective.
```

The workflow should compare candidate models, but it should not claim a model is
"optimal" before validating data quality, labels, leakage, metric choice, and
test performance.
