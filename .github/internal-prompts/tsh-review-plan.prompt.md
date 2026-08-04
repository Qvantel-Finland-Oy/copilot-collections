Stress-test the implementation plan for the provided task. Assume the architect likely produced a basically correct plan, then look for the strongest reasons it could still fail in implementation, create expensive rework, or give false confidence. The primary deliverable is the full structured review document saved as `specifications/{task-name-or-id}/{task-name}.plan-review.md`, alongside the plan in the same directory.

## Required Skills

Before starting, load and follow these skills:

- `tsh-codebase-analysing` - for verifying the plan against the existing codebase
- `tsh-creating-implementation-plans` - for understanding the canonical plan structure and Definition of Done rules
- `tsh-implementation-gap-analysing` - for identifying what the plan must address versus what already exists
- `tsh-technical-context-discovering` - for understanding repository conventions and applicable instructions

## Workflow

1. **Read the research file** (`.research.md`) — understand the full set of requirements, acceptance criteria, and constraints that the plan must address.

2. **Read the plan file** (`.plan.md`) — understand the proposed architecture, phases, tasks, and definitions of done. Stop immediately if any row in `## Open Questions` has Status = `❓ Open`; this is a blocker and review cannot proceed until the architect resolves it.

3. **High-level gate** — Review architecture, security, and execution risk against the research and codebase. A `BLOCKER` is eligible only for one of these six categories: "materially invalid architecture"; "security, privacy, or authentication risk"; "unsupported irreversible or high-cost decisions"; "critical integration, data, migration, rollout, or rollback failure"; "an execution-critical unresolved decision"; or "material contradiction with research or an omitted requirement". Do not redesign the plan.

4. **Failure-modes pass** — Find the strongest reasons the plan may fail during implementation or cause major rework. Prioritize substantive risks such as integration mismatches, unsafe migrations, coordination traps, weak rollout strategies, and brittle task breakdowns.

5. **Hidden-assumptions pass** — Identify assumptions that are unproven in this repository. Flag beliefs about files, abstractions, contracts, environment behavior, team coordination, or data shape that the plan depends on but does not verify.

6. **Codebase-reality pass** — For every critical file, component, function, class, abstraction, or dependency the plan relies on:
   - Search the codebase to verify it exists
   - Read the file to verify it has the expected interface/behavior
   - Flag any reference that doesn't match reality or is weaker/more constrained than the plan assumes

7. **Sequencing-and-feasibility pass** — Identify order-of-operations traps, risky migrations, rollback gaps, coordination issues, and test or rollout blind spots. Focus on how the plan could break when executed step by step.

8. **Execution-critical decision gate** — Before final verdict, explicitly check for unresolved provider, vendor, stack, framework, auth, privacy, security, integration-contract, or migration-prerequisite decisions that sit on the critical path or lock in downstream work. These cannot be waved through as harmless notes.

9. **Produce the report and binary verdict** — Save the concise review report with final verdict (`APPROVED` or `REVISIONS NEEDED`) as `specifications/{task-name-or-id}/{task-name}.plan-review.md`, in the same directory as the plan. Bind the verdict to the exact `Plan Revision` you read verbatim from the plan's `## Human Approval` table, and report that same value in the returned assessment. The artifact is never overwritten: append any explicitly user-directed new review event to the existing report. For every eligible `BLOCKER`, provide a violated category, evidence, consequence, and minimum correction. Notes and suggestions are advisory and never independently drive the verdict or another review. Never state, infer, evaluate, remind, or ask about human approval, user consent, or execution authorization, and never record a human decision in `.plan-review.md`.


## Review Requirements

- Do not pad the report with cosmetic, wording, or style-only notes.
- Do not fall back to generic quality audits or pattern-consistency checks, and do not require narration of unrelated domains or no-issue results.
- Flag any `❓ Open` item in the plan's `## Open Questions` table as a blocker requiring resolution before approval.
- Consolidate duplicate findings and do not escalate `WARNING` or `SUGGESTION` merely because an issue repeats.
- Every consolidated finding must state the violated criterion, evidence, consequence, and the action needed. Do not redesign the plan; identify only the minimum correction required to address the finding.
- Never comment on human approval, user consent, or execution authorization. Those are outside reviewer scope and belong to the plan's `## Human Approval` record and the engineering manager's gate.
- Blocker eligibility is limited to the six listed categories. `grep` and shell-command syntax, diff-hunk counts and cumulative-diff mechanics, style and formatting preferences, plan-template conformance, task `Files` bookkeeping, minor wording consistency, optional documentation synchronization, and report verbosity or completeness are ineligible process-heavy concerns. A verification defect is eligible only when it removes the only meaningful safety proof for a change, which is category 4 rather than a command-syntax complaint.

<output-specification>
Save `specifications/{task-name-or-id}/{task-name}.plan-review.md` alongside the plan. The report contains exactly three required content items: a summary line or table carrying the reviewed plan path, reviewed `Plan Revision` read verbatim from the plan's `## Human Approval` table (or `unknown` when it cannot be read), review date, and verdict; material blockers each with violated category, evidence, consequence, and minimum correction; and concise explicitly advisory notes. Content formerly carried in larger report sections may appear only when it materially supports a blocker. Preserve the exact returned `<plan-review-report>` schema.
</output-specification>

## Key Principles

- **Failure orientation** — look first for why the plan may break, stall, or trigger major rework.
- **Verify, don't assume** — always search the codebase before flagging phantom references. The architect may have found something you haven't.
- **Advisory notes** — notes and suggestions are advisory, never independently produce `REVISIONS NEEDED`, never trigger another review, and never block implementation.
- **Pragmatism over permissiveness** — issues can exist and the verdict can still be `APPROVED`, but not when execution-critical open decisions remain unresolved.
- **Scope discipline** — never suggest adding features or requirements not in the research file.
- **Blocker accountability** — every eligible `BLOCKER` must be supported by evidence and a minimum correction; it cannot be silently omitted or downgraded without explanation.

<!-- TSH_COPILOT_COLLECTIONS:prompt:tsh-review-plan:v4 -->
