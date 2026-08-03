---
sidebar_position: 7
title: Architect Reviewer
---

# Architect Reviewer Agent

**File:** `.github/agents/tsh-plan-reviewer.agent.md`

The Architect Reviewer is an internal sub-agent that stress-tests implementation plans before code is written. It challenges the plan for likely failure modes, hidden assumptions, sequencing traps, integration mismatches, migration and data risks, and false confidence in testing.

Its `APPROVED` result is **Reviewer approval** only. It reports automated readiness and never grants Human approval or permission to implement; the Engineering Manager must still obtain Human approval of the exact current plan revision before the first file-changing delegation.

The reviewer is non-implementing and does not validate or record the execution precondition. Execution owners validate the persisted Human Approval record before editing; Reviewer approval remains distinct and never authorizes implementation.

## Responsibilities

- Stress-testing the plan against the research context to expose likely failure modes.
- Checking that referenced files, functions, classes, integrations, and patterns actually exist in the codebase.
- Surfacing hidden assumptions, sequencing traps, dependency order issues, and migration or data risks.
- Challenging integration boundaries, rework risk, and false confidence in test coverage.
- Producing a structured approval or revision report for the Architect.

## What It Produces

- A failure-oriented review report with a binary verdict, top risks, assumptions, rework triggers, and any blocking gaps.
- The report is saved as `{task-name}.plan-review.md` alongside the plan in `specifications/<task-name-or-id>/`.

## Blocker Criteria

`BLOCKER` eligibility is limited to exactly six high-level categories: materially invalid architecture; security, privacy, or authentication risk; unsupported irreversible or high-cost decisions; critical integration, data, migration, rollout, or rollback failure; an execution-critical unresolved decision; and material contradiction with research or an omitted requirement. No other category is a `BLOCKER`. `WARNING` and `SUGGESTION` findings are non-blocking and never escalate merely because they repeat.

Each finding states the violated criterion, evidence, consequence, and minimum correction — the reviewer never redesigns the plan. An unresolved `BLOCKER` carries forward across review iterations and closes only through an architect correction or a recorded explicit evidence-based resolution or justification, never through omission or an unexplained downgrade. The reviewer never approves a plan while a `BLOCKER` remains, and each review is appended to `{task-name}.plan-review.md` as part of its append-only history.

## Tool Access

| Tool | Usage |
| --- | --- |
| Context7 | Verify framework or library assumptions when the plan references them |
| Sequential Thinking | Evaluate trade-offs, phase ordering, and over-engineering risk |
| File Read/Search | Inspect the plan, research file, instructions, and referenced code |

## Skills Loaded

- `tsh-architecture-designing` — Evaluate architectural shape, phase coherence, and trade-offs.
- `tsh-creating-implementation-plans` — Verify plan template, structure, and definition-of-done compliance.
- `tsh-codebase-analysing` — Verify plan references against the actual codebase.
- `tsh-technical-context-discovering` — Check pattern consistency against established conventions.
- `tsh-implementation-gap-analysing` — Compare what exists with what the plan proposes.
- `tsh-sql-and-database-understanding` — When the plan includes database schema, migration, or query changes.

## How It Is Used

- It is not invoked directly by users.
- The Architect directly invokes the Plan Reviewer as a nested subagent after creating or revising a plan, for at most two automatic passes; the Engineering Manager is not part of the review loop.
- If the reviewer returns revisions, the plan goes back to the Architect and is re-reviewed until the reviewer returns `APPROVED` (Reviewer approval only, never Human approval) or, if a `BLOCKER` survives both automatic passes, the Architect escalates to the user with an explicit choice to continue, stop, or give custom guidance.
