---
sidebar_position: 2
title: Architect
---

# Architect Agent

**File:** `.github/agents/tsh-architect.agent.md`

The Architect agent designs technical solutions, system architecture, and detailed implementation plans. It translates business requirements into structured, executable specifications that are validated by the Architect Reviewer before implementation begins.

The Architect owns plan revisions and records only the user's literal Human approval response; it never infers consent from context or from Reviewer approval. `tsh-plan-reviewer` `APPROVED` is Reviewer approval only and leaves Human approval pending. Both **Start Implementation** and **Start Infrastructure Implementation** pass through the Engineering Manager and its Human approval gate; neither handoff directly authorizes file-changing work.

The Architect presents and records Human Approval when delegated to update the plan. Execution owners, not the Architect, validate the persisted Human Approval record from disk before editing.

## Responsibilities

- Designing the overall architecture of the solution (components, interactions, data flow).
- Creating detailed implementation plans broken into phases and tasks with checklist-style tracking.
- Ensuring solutions align with project requirements, best practices, and quality standards.
- Defining security considerations and quality assurance guidelines.
- Collaborating with Business Analysts to clarify ambiguities.

## What It Produces

Each technical specification includes:

1. **Solution Architecture** — High-level overview of components, interactions, and data flow.
2. **Implementation Plan** — Phases and tasks with clear definition of done for each task, ready for Architect Reviewer validation.
3. **Test Plan** — Automated testing strategies (no manual QA steps).
4. **Security Considerations** — Security aspects to address during implementation.
5. **Quality Assurance** — Guidelines for ensuring implementation quality.

## Tool Access

| Tool                      | Usage                                                                  |
| ------------------------- | ---------------------------------------------------------------------- |
| **Atlassian**             | Gather requirements from Jira issues and Confluence pages              |
| **Context7**              | Evaluate libraries, verify compatibility, search integration patterns  |
| **Figma**                 | Translate visual requirements into technical specifications            |
| **PDF Reader**            | Read and extract content from PDF requirement documents                |
| **Sequential Thinking**   | Design complex architectures, evaluate trade-offs, break down features |
| **Terminal**              | Run build tools, scripts, and validation commands                      |
| **File Read/Edit/Search** | Read, modify, and search workspace files                               |
| **VS Code Commands**      | Execute VS Code commands and run tasks                                 |
| **Sub-agents**            | Delegate subtasks to specialized agents                                |
| **Todo**                  | Track task progress with structured checklists                         |

## Skills Loaded

- `tsh-architecture-designing` — Solution design, components, data flows.
- `tsh-creating-implementation-plans` — Implementation plan template, structure, and DoD rules.
- `tsh-codebase-analysing` — Analyze current architecture, components, and patterns.
- `tsh-implementation-gap-analysing` — Focus the plan on necessary changes without duplicating existing work.
- `tsh-technical-context-discovering` — Establish project conventions and patterns before designing.
- `tsh-sql-and-database-understanding` — Database schema design, indexing strategies, transaction patterns.
- `tsh-designing-multi-cloud-architecture` — Cross-provider infrastructure design and service selection across AWS, Azure, and GCP.
- `tsh-optimizing-cloud-cost` — Cost implications of architectural decisions, pricing model comparison.
- `tsh-implementing-ci-cd` — CI/CD pipeline design, deployment strategies, delivery workflows.
- `tsh-implementing-terraform-modules` — IaC structure, Terraform module hierarchy, Terragrunt patterns.
- `tsh-managing-secrets` — Secrets management, credential rotation, vault integration.
- `tsh-implementing-kubernetes` — K8s workload configurations, scaling strategies, Helm chart structure.
- `tsh-implementing-observability` — Monitoring architecture, SLO frameworks, alerting, distributed tracing.
- `tsh-engineering-prompts` — LLM prompt architecture: prompt template strategy, system prompt design, few-shot vs zero-shot decisions.

## Pre-Submission Self-Check and Review Loop

Before submitting a plan, the Architect runs a pre-submission self-check against the same six `BLOCKER` categories used by the Architect Reviewer: materially invalid architecture; security, privacy, or authentication risk; unsupported irreversible or high-cost decisions; critical integration, data, migration, rollout, or rollback failure; an execution-critical unresolved decision; and material contradiction with research or an omitted requirement.

After creating or revising a plan, the Architect invokes `tsh-plan-reviewer` for at most two automatic review passes. Every `BLOCKER` finding must be closed by an architect correction or a recorded explicit evidence-based resolution or justification — never by omission or an unexplained downgrade — and the Architect never approves a plan while a blocker remains. `.plan-review.md` is append-only and never overwritten.

If a `BLOCKER` survives the second automatic pass, the Architect asks the user via `vscode/askQuestions` with exactly these choices: `try one more iteration`, `stop here`, or `custom guidance`. `try one more iteration` grants exactly one user-authorized extra pass and repeats the same question if blockers remain; `stop here` ends the loop with a closing `.plan-review.md` entry; `custom guidance` follows the supplied instruction. The Architect never silently continues or approves while a blocker remains.

## Handoffs

After creating the plan, the Architect can hand off to:

- **Internal plan review loop** → the Architect invokes `tsh-plan-reviewer` as a nested subagent after creating or revising a plan, following the self-check and review loop described above. This loop only ever settles Reviewer approval; it never grants or implies Human approval.
- **Engineering Manager** → `/tsh-implement` (`Start Implementation`) once the plan is Reviewer-ready, for the manager's Human approval gate
- **Engineering Manager** → `Start Infrastructure Implementation` for infrastructure work, through the same Human approval gate
