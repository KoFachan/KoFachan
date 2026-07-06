---
name: codex-architecture-governance
description: Audit and govern local Codex architecture. Use when the user asks where to store or configure AGENTS.md rules, project knowledge, memories, skills, SKILL.md files, references, hooks, subagents, plugins, MCP servers, connectors, ECC assets, context management, or Codex architecture diagrams; when creating or updating global/project Codex configuration; or when deciding whether information belongs in AGENTS.md, a skill, a reference file, project knowledge, a hook, an agent config, or a plugin.
---

# Codex Architecture Governance

Use this skill to keep Codex configuration layered, reusable, and context-light.
Its job is to decide where instructions and knowledge should live, then make
small, verifiable configuration changes when the user asks for them.

## Workflow

1. Define the requested change: architecture audit, placement decision, new
   skill, AGENTS.md update, hook, agent, plugin, MCP, or project knowledge.
2. Inspect source truth before editing. For local audits, check real files and
   redact config values, auth data, tokens, passwords, and environment secrets.
3. Read the smallest reference set needed:
   - `references/layer-model.md` for the local layer model.
   - `references/placement-rules.md` for deciding where content belongs.
   - `references/local-path-map.md` for this machine's known Codex paths and
     audit commands.
4. Keep global `AGENTS.md` short. Add only stable routing or safety rules there.
   Put detailed methods in skills, `references/`, project knowledge, or source
   docs loaded on demand.
5. When creating or updating a skill, keep `SKILL.md` lean and move detailed
   background into `references/`. Validate with the system skill validation
   script when practical.
6. If the task crosses a phase boundary, update the active project
   `current-state.md` with paths, decisions, verification, and next actions.

## Output

For advice-only tasks, return:

- Recommended location.
- Reasoning by layer.
- Exact file path if known.
- Risks or context-window tradeoffs.

For implementation tasks, also return:

- Files changed.
- Validation performed.
- Any restart or new-conversation requirement.

## Boundaries

- Do not store secrets in AGENTS.md, skills, references, memories, knowledge
  files, reports, or generated artifacts.
- Do not treat social-media architecture diagrams as authoritative. Use them as
  mental models, then map them to the real local Codex runtime.
- Do not create broad catch-all `memory.md` files for unrelated business,
  machine-learning, and engineering knowledge. Create focused skills or project
  wiki pages instead.
