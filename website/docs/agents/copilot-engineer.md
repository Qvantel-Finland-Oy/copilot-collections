---
sidebar_position: 8
title: Copilot Engineer
---

# Copilot Engineer Agent

**File:** `.github/agents/tsh-copilot-engineer.agent.md`

The Copilot Engineer agent specializes in designing, creating, reviewing, and improving all GitHub Copilot customization artifacts — custom agents, skills, prompts, and instructions.

## Responsibilities

- Creating, reviewing, and improving custom agents (`.agent.md`), skills (`SKILL.md`), prompt files (`.prompt.md`), and instruction files (`.instructions.md`).
- Applying prompt engineering best practices: clarity, structure, token efficiency, progressive disclosure.
- Designing context architecture: what information flows where, at which layer, and with what priority.
- Enforcing strict separation of concerns between customization types.
- Advising on tool and MCP server configuration for agents and prompts.
- Optimizing the signal-to-noise ratio within context windows.

## Separation of Concerns

The Copilot Engineer enforces a strict boundary model:

| Artifact | Role | Contains |
|---|---|---|
| **Agent** (`.agent.md`) | WHO | Persona, behavior, responsibilities, tool access |
| **Skill** (`SKILL.md`) | HOW | Reusable workflows, domain knowledge, step-by-step processes |
| **Prompt** (`.prompt.md`) | WHAT | Workflow trigger, task starter, routes to an agent; model selection is inferred from that agent |
| **Instructions** (`.instructions.md`) | RULES | Coding standards, project conventions, always-applied |

When any artifact crosses these boundaries, the Copilot Engineer identifies and corrects the violation.

## Key Design Principles

- **Token efficiency** — Every token competes for context window space. Only add context the LLM doesn't already have.
- **Progressive disclosure** — Discovery (~100 tokens): name + description. Activation (&lt;5000 tokens): full body. Resource (on demand): templates, examples.
- **Structural parsing reliability** — XML-like tags for content with explicit boundaries; Markdown for sequential content.

## Tool Access

| Tool | Usage |
|---|---|
| **Context7** | Research VS Code Copilot customization API, agent file format, MCP server docs |
| **Sequential Thinking** | Design agent architecture, analyze multi-artifact interactions, evaluate trade-offs |
| **Web Fetch** | Fetch external documentation and reference materials |
| **Mermaid Diagrams** | Render architecture and workflow diagrams |
| **File Read/Edit/Search** | Read, modify, and search workspace files |
| **VS Code Commands** | Execute VS Code commands and run tasks |
| **Sub-agents** | Delegate subtasks to specialized agents |
| **Todo** | Track task progress with structured checklists |

## Skills Loaded

- `tsh-creating-agents` — Agent file creation workflow, templates, and validation checklist.
- `tsh-creating-skills` — Naming conventions, body structure, progressive disclosure patterns.
- `tsh-creating-prompts` — Prompt file creation workflow, templates, and workflow focus guidelines.
- `tsh-creating-instructions` — Templates, decision framework for instruction vs. skill placement.
- `tsh-technical-context-discovering` — Understand existing customization patterns in the project.
- `tsh-codebase-analysing` — Analyze existing customization files and identify patterns.
