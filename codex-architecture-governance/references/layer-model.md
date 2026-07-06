# Local Codex layer model

This reference maps the common five-layer Codex architecture diagram to this
local Windows Codex setup. The diagram is useful as a mental model, but local
source truth comes from actual files under `%USERPROFILE%\.codex`,
`%USERPROFILE%\.agents`, project folders, and connected tools.

## Layer 1: Rules and memory

Purpose: stable operating agreements, safety boundaries, high-level routing, and
project recovery rules.

Typical local files:

- `%USERPROFILE%\.codex\AGENTS.md`
- project-level `AGENTS.md`
- `%USERPROFILE%\.codex\config.toml` for model, plugins, MCP, and trusted
  projects
- `%USERPROFILE%\.codex\knowledge\projects\<project>\index.md`
- `%USERPROFILE%\.codex\knowledge\projects\<project>\current-state.md`
- `%USERPROFILE%\.codex\memories\`

Use this layer for short rules that should affect nearly every relevant
conversation. Avoid large background dumps.

## Layer 2: Skills and reusable workflows

Purpose: reusable procedures, domain knowledge, API/tool usage, business SOPs,
templates, and scripts that should be loaded only when relevant.

Typical local roots:

- `%USERPROFILE%\.codex\skills`
- `%USERPROFILE%\.codex\skills\.system`
- `%USERPROFILE%\.agents\skills`
- plugin-provided skill cache folders

Recommended skill anatomy:

```text
skill-name/
├── SKILL.md        # trigger metadata and short workflow
├── references/     # detailed knowledge loaded only when needed
├── scripts/        # deterministic helpers, if useful
├── assets/         # templates or output resources, if useful
└── agents/         # UI metadata such as openai.yaml
```

Use this layer for architecture frameworks, business background that is not
always needed, ML model-selection methods, VOC SOPs, Lark workflows, and other
repeatable work.

## Layer 3: Hooks and guardrails

Purpose: lifecycle automation around Codex sessions and tool use.

Typical local files:

- `%USERPROFILE%\.codex\hooks.json`
- hook scripts under a skill, for example
  `%USERPROFILE%\.codex\skills\knowledge-context-manager\scripts\`

Use hooks for deterministic, low-risk automation such as session start,
pre-compact checkpoints, post-compact recovery, and stop-time state capture.
Do not use hooks for broad research, model reasoning, publishing, or risky
third-party actions.

## Layer 4: Delegation and subagents

Purpose: bounded exploration, review, documentation verification, and parallel
work when the user explicitly wants delegation or the task benefits from
independent passes.

Typical local files:

- `%USERPROFILE%\.codex\agents\explorer.toml`
- `%USERPROFILE%\.codex\agents\reviewer.toml`
- `%USERPROFILE%\.codex\agents\docs-researcher.toml`
- ECC prompt templates under `%USERPROFILE%\.codex\prompts`

Use this layer for noisy exploration and independent review. Keep final
synthesis in the main thread.

## Layer 5: Plugins, connectors, MCP, and distribution

Purpose: tool access and package distribution.

Typical local roots:

- `%USERPROFILE%\.codex\plugins\cache\openai-bundled`
- `%USERPROFILE%\.codex\plugins\cache\openai-curated`
- `%USERPROFILE%\.codex\plugins\cache\openai-curated-remote`
- `%USERPROFILE%\.codex\plugins\cache\openai-primary-runtime`
- `[mcp_servers.*]` sections in `config.toml`
- external CLIs such as `gh`, `lark-cli.cmd`, and `npx.cmd`

Skills describe how to work. Plugins, MCP servers, connectors, and CLIs provide
tool access.

## Local extension: Project knowledge

The local setup uses a durable project knowledge layer in addition to the five
diagram layers. This is where long-running project state belongs:

- source map
- current state
- decisions
- wiki pages
- checkpoint summaries

Treat source files, databases, reports, and connected systems as source truth.
Treat project knowledge as maintained synthesis.

## Local extension: External capability layer

MCP servers, Feishu/Lark CLI, GitHub CLI, TiDB MCP, node_repl, and app
connectors are capabilities. They are not automatically safe or authoritative.
Check permissions, scopes, and current access before promising results.
