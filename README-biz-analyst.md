# BA Orchestration Overview

This workspace now uses `tsh-business-analyst` as the BA orchestrator for discovery workshops, backlog extraction, Jira iteration, and post-push verification. The orchestrator keeps the user-facing workflow, approval gates, synthesis, Jira access, and Jira mutations, while specialized internal workers handle the phase-specific reasoning in memory only.

## What The BA System Does

The BA flow turns workshop inputs into business-facing Jira artifacts. It supports transcript cleanup, business-context exploration, intent-brief approval, epic and story extraction, quality review, Jira-ready formatting, Jira push, post-push verification, and archive/baseline refresh.

The output stays Jira-first and stakeholder-readable. Technical detail is included only when it was explicitly discussed in the source materials.

## OpenSpec-Inspired Changes Already In Place

1. Gate 0 intent brief before extraction.
	The BA flow now creates and reviews an intent brief before any backlog extraction begins, so the scope and business intent are aligned first. `tsh-business-analyst` owns the Gate 0 approval conversation with the user, while `tsh-ba-extraction-worker` drafts the brief in memory as part of the supporting extraction flow. Extraction does not start until that brief is explicitly approved, which keeps the downstream backlog focused and reviewable. Explore Mode context can inform the brief when helpful, but it does not replace the Gate 0 approval step.

2. Source traceability plus GIVEN/WHEN/THEN acceptance scenarios.
	Every extracted epic or story now keeps a short source reference back to the workshop transcript, workshop summary, or baseline context that informed it. Acceptance criteria are written mainly as concise GIVEN/WHEN/THEN scenarios so stakeholders can quickly understand expected outcomes and edge conditions. That traceability is preserved into Jira formatting as source context, which makes later review and refinement easier. The extraction worker is explicitly designed to carry those references forward rather than drop them during synthesis.

3. Post-push verification after Jira sync.
	After Jira create or update actions, the orchestrator reads the issues back and checks that the key fields match what was approved before sync. This includes practical fields such as the title or summary, parent linkage, acceptance criteria, relevant description sections, and status. `tsh-ba-formatting-worker` supports that comparison in memory, but `tsh-business-analyst` owns the Jira read-back and verification step because it controls the final user-facing workflow. Session archiving only moves ahead after verification succeeds or any discrepancies are explicitly reviewed and handled.

4. Lite and Full quality-review modes.
	Quality review now runs automatically after Gate 1 approval, so the extracted backlog is checked before it is turned into final Jira-ready output. Lite mode is the default for smaller workshops, while Full mode provides a deeper pass for broader or higher-risk scopes where more scrutiny is warranted. `tsh-ba-quality-worker` applies the review logic defined in the quality-review skill and returns structured suggestions for improvement rather than rewriting the backlog on its own. Gate 1.5 is where the user accepts or rejects those suggestions item by item instead of rerunning extraction blindly.

5. Session archiving plus project backlog baseline refresh.
	After a verified Jira sync, the session can be archived into the project's session history so the team has a durable record of what was reviewed and pushed. At the same time, `task-baseline.md` is refreshed so the next workshop starts from the latest known backlog state instead of an outdated snapshot. `tsh-ba-formatting-worker` prepares the archive and baseline content in memory, while `tsh-business-analyst` writes the final files as part of the controlled workflow. That refreshed baseline is then reused by later analysis and exploration flows to detect overlap, preserve continuity, and reduce duplicate work.

6. Optional exploration mode before commitment.
	Users can now explore workshop materials without committing immediately to backlog extraction, which is useful when the source material is still ambiguous or incomplete. This path is triggered through `tsh-explore-materials`, or chosen by the orchestrator when the material clearly needs clarification before the standard backlog flow begins. `tsh-ba-analysis-worker` synthesizes the available context into `workshop-context-summary.md`, including likely epics, overlap with prior backlog items, ambiguities, and overall readiness. No backlog items are created in this mode until the user explicitly moves forward into the standard Gate 0 flow.

## New Multi-Model Strategy

The BA workflow is now split across a small set of model-specialized workers:

- `tsh-ba-transcript-worker` uses `GPT-5.4 mini` for transcript cleanup.
- `tsh-ba-analysis-worker` uses `Gemini 3.1 Pro (Preview)` for multi-source context synthesis.
- `tsh-ba-extraction-worker` uses `Claude Sonnet 4.6` for intent brief drafting and epic/story extraction.
- `tsh-ba-quality-worker` uses `GPT-5.4` for Lite or Full quality-review passes.
- `tsh-ba-formatting-worker` uses `GPT-5.4 mini` for Jira formatting, verification support, and baseline-refresh preparation.

`tsh-business-analyst` stays in charge of user interaction, review gates, merge/synthesis, Jira create/update operations after Gate 2, and final file writing.

## Architecture Change

The previous single-agent BA setup is now an orchestrator plus internal workers.

- The orchestrator handles the conversation, approval gates, Jira operations, and final artifact writing.
- The workers handle focused phases and return in-memory results only.
- Workers never write files and never ask the user questions directly.

## Routing By Activity

| Activity | Worker |
|---|---|
| Transcript cleanup and structure | `tsh-ba-transcript-worker` |
| Multi-source context analysis and overlap checks | `tsh-ba-analysis-worker` |
| Intent brief drafting and epic/story extraction | `tsh-ba-extraction-worker` |
| Lite or Full quality review | `tsh-ba-quality-worker` |
| Jira formatting, post-push verification, baseline refresh | `tsh-ba-formatting-worker` |

## Produced Artifacts

The BA workflow produces these files under the workshop folder in `specifications/`:

- `cleaned-transcript.md`
- `workshop-context-summary.md`
- `intent-brief.md`
- `extracted-tasks.md`
- `quality-review.md`
- `jira-tasks.md`
- `specifications/projects/<project-name>/task-baseline.md`

## How To Use It

Start with `tsh-explore-materials` when you want business-context discovery before backlog commitment. Use `tsh-analyze-materials` when you are ready to process workshop materials into a Gate 0 intent brief, then proceed through extraction, quality review, Jira formatting, and Jira push.

The review gates remain mandatory:

- Gate 0 approves the intent brief before extraction.
- Gate 1 approves the extracted tasks.
- Gate 1.5 accepts or rejects quality-review suggestions.
- Gate 2 approves the final Jira-ready output before Jira sync.

For Jira iteration, the orchestrator can import existing backlog items, refine them locally, and push updates back after approval. Post-push verification confirms the Jira result, and the archive/baseline step preserves continuity for later workshops.

## Internal Workers In The Flow

The workers only support the orchestrator. They clean transcripts, synthesize context, extract backlog content, run quality passes, and prepare Jira formatting in memory. Their output is merged by `tsh-business-analyst`, which owns all user-facing decisions and Jira mutations. Workers remain Jira-tool-free; when Jira context or read-back verification is needed, the orchestrator fetches it and passes the relevant context into the worker phase.

## What Changed From The Previous BA Setup

The previous BA agent was a single model-driven flow. This version keeps the same business-facing workflow but moves execution into a multi-model orchestrator pattern, so each phase uses a specialized worker while the top-level BA agent retains control of the gates, Jira operations, and final artifact writing.
