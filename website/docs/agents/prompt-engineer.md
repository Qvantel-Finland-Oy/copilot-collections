---
sidebar_position: 14
title: Prompt Engineer
---

# Prompt Engineer Agent

**File:** `.github/agents/tsh-prompt-engineer.agent.md`

The Prompt Engineer agent designs, writes, optimizes, and secures LLM application prompts — system prompts, user prompt templates, RAG context injection templates, agent tool-calling instructions, and classification/extraction prompts.

Before any file change, validate from disk a plan whose current Human Approval record satisfies exactly `Human Decision=APPROVED`, `Approved Revision=current Plan Revision`, and a valid ISO 8601 UTC `Decision Timestamp` ending in `Z`. Fail closed when a field is missing, stale, mismatched, inferred, based only on Reviewer approval, or when the plan cannot be located or read; retry an unreadable or ambiguous reference once and resolve relative paths against the workspace root. Name the exact failed field, condition, or file, then use `vscode/askQuestions` for guided recovery on every entry path, including direct selection as the primary chat agent. Offer pointing to the correct plan, obtaining Human approval for an existing plan, starting plan preparation, or, when delegated, handing back to `tsh-engineering-manager`; continue only from the user's explicit choice, which is never Human approval.

This agent handles **LLM application prompts** (prompts consumed by LLM APIs at runtime). It does NOT handle Copilot customization files (`.prompt.md`, `.agent.md`, `SKILL.md`) — that is `tsh-copilot-engineer`.

## Responsibilities

- Designing prompt structure with clear role separation, delimiters, and output format specification.
- Optimizing existing prompts for clarity, token efficiency, output quality, and consistency.
- Creating new prompts from business requirements with appropriate constraints and examples.
- Securing prompts against injection attacks with layered defenses (delimiter separation, input sanitization, output validation).
- Evaluating prompts through A/B testing, metric-based comparison, and edge case testing.

## Key Behaviors

- **Prompt-focused** — Handles only the prompt engineering aspects, returns to the software engineer for integration.
- **Security-first** — Every prompt includes injection defense as a non-negotiable default.
- **Technology-agnostic** — Patterns apply to any LLM provider (OpenAI, Anthropic, etc.).
- **Strictly follows the plan** — Does not deviate unless explicitly instructed.

## Tool Access

| Tool | Usage |
|---|---|
| **Context7** | Search LLM provider API docs, framework-specific prompt template syntax |
| **Sequential Thinking** | Design complex prompt chains, analyze injection vectors, evaluate trade-offs |
| **File Read/Edit/Search** | Read and modify workspace files containing prompts |
| **Ask Questions** | Clarify ambiguous prompt requirements, domain-specific terminology |
| **Todo** | Track prompt engineering progress |

## Skills Loaded

- `tsh-engineering-prompts` — Primary skill: prompt structure, optimization, security, templates, evaluation, anti-patterns.
- `tsh-technical-context-discovering` — Project conventions and existing prompt patterns.
- `tsh-code-reviewing` — When reviewing prompt code quality as part of broader review.

## Delegation Model

The Prompt Engineer is invoked in two ways:

- **Direct:** User invokes `@tsh-prompt-engineer` for standalone prompt tasks.
- **Orchestrated:** The Engineering Manager delegates LLM prompt tasks directly to the Prompt Engineer during feature implementation. After completing prompt work, control returns to the manager for the next task in the plan.
