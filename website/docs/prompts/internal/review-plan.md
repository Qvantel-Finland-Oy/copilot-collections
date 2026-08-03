---
sidebar_position: 10
title: /tsh-review-plan
---

:::info
Not invoked directly by users. To trigger plan validation, use [`/tsh-implement`](../public/implement) — the Architect automatically delegates to the Architect Reviewer after producing or updating a plan.
:::

**Agent:** Architect Reviewer  
**File:** `.github/internal-prompts/tsh-review-plan.prompt.md`

Stress-tests architect implementation plans before implementation begins and persists the review report alongside the plan.

## How It's Triggered

```text
/tsh-implement <JIRA_ID or task description>
```

The Architect uses this internal prompt after creating or updating a `.plan.md` file. The only skip is the Architect's low-risk exemption, which applies solely to initial plan preparation before any Human approval has ever been recorded and is unavailable once any Human approval exists.

## What It Does

1. Reads the research file first so the review is grounded in the original requirements.
2. Reads the plan file and checks every task, phase, and definition of done.
3. Runs failure-mode, assumption, codebase-reality, and sequencing-and-feasibility passes.
4. Consolidates duplicate findings and reports only actionable, well-evidenced risks, with no finding quota.
5. Produces a failure-oriented review report with a binary verdict and the highest-risk issues, assumptions, rework triggers, and blocking gaps. The returned `short summary` is fenced to the verdict, the single highest-signal reason, and the blocker/warning/suggestion counts — never human approval or execution authorization.
6. Reads the `Plan Revision` verbatim from the plan's `## Human Approval` table and returns it as `reviewed-plan-revision`; if it cannot be read, returns `REVISIONS NEEDED` with `reviewed-plan-revision="unknown"`.
7. Saves the report as `{task-name}.plan-review.md` in the same `specifications/<task-name-or-id>/` directory as the plan, with the reviewed `Plan Revision` recorded in the report header.
8. If the verdict is `REVISIONS NEEDED`, the Architect addresses the findings and re-invokes the reviewer, for at most two automatic passes; if a blocker survives both, the Architect escalates via `vscode/askQuestions` with exactly `try one more iteration`, `stop here`, and `custom guidance`.

## Skills Loaded

- `tsh-architecture-designing` — Evaluate architectural shape, phase coherence, and trade-offs.
- `tsh-creating-implementation-plans` — Verify plan template, structure, and definition-of-done rules.
- `tsh-codebase-analysing` — Verify the plan's references against actual codebase state.
- `tsh-technical-context-discovering` — Check pattern consistency against established conventions.
- `tsh-implementation-gap-analysing` — Validate what exists vs. what the plan proposes to build.
- `tsh-sql-and-database-understanding` — When the plan includes database schema, migration, indexing, or query changes.

## Output

A `.plan-review.md` file placed in `specifications/<task-name-or-id>/` alongside the plan, containing the failure-oriented review report, the reviewed `Plan Revision` in the report header, and the binary verdict.
