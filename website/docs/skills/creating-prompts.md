---
sidebar_position: 18
title: Creating Prompts
---

# Creating Prompts

**Folder:** `.github/skills/tsh-creating-prompts/`
**Used by:** Copilot Engineer

Provides templates, guidelines, and a structured process for building custom prompt files (`.prompt.md`) that trigger specific workflows routed to the right custom agent, with model selection inferred from that agent.

## Core Design Principles

- **Separation of concerns** — A prompt file defines WHAT workflow to execute. It must NOT define WHO the agent is (agent territory) or embed reusable domain knowledge (skill territory).
- **Workflow focus** — A prompt routes work to a specific agent, which determines the model, and describes workflow steps for a specific task.
- **Minimal duplication** — Prompts reference skills and agents; they don't duplicate their content.

## Prompt File Structure

| Section | Purpose |
|---|---|
| **YAML Frontmatter** | `agent`, `description` fields (model is inferred from `agent`, not declared) |
| **Workflow Steps** | Ordered steps the agent should follow |
| **Input/Output** | What the prompt expects and what it produces |
| **Required Skills** | Skills the agent must load for this workflow |

## Key Guidelines

- Every prompt must specify an `agent` field routing to a `tsh-` prefixed agent name
- Public prompts must omit the `model` field; model selection is inferred from the bound agent's own `model` array
- Prompt filenames follow `tsh-<action>.prompt.md` convention
- Prompts should not redefine, override, or contradict the agent's identity

## Validation Checklist

- YAML frontmatter includes `agent`, `description`, and omits a prompt-level `model` field
- Agent reference uses `tsh-` prefix
- No personality or behavioral content (agent territory)
- No reusable workflow steps (skill territory)
- No coding standards (instructions territory)

## Connected Skills

- `tsh-creating-agents` — For creating the agent a prompt routes to.
- `tsh-creating-skills` — For creating skills referenced by the prompt.
