---
agent: "tsh-engineering-manager"
description: "Implement feature according to the plan."
---

<goal>
Start implementation delivery for a feature based on a task description, Jira item, standalone research file, or implementation plan. This prompt is a thin trigger that routes execution through the canonical orchestration workflow rather than defining that workflow inline.
</goal>

<required-skills>
<skill name="tsh-orchestrating-implementation">
Required because it contains the canonical implementation-orchestration workflow — Step 0 creates flow-start todos and Step 1 establishes Full Flow as the only implementation route — and the delivery process that this prompt must trigger without duplicating.
</skill>
</required-skills>

<input-requirements>
The four primary inputs are a task description, a Jira ID, a standalone `*.research.md` file, and a `*.plan.md` implementation plan. Missing companion research or plan artifacts trigger preparation through the canonical workflow; they never authorize no-plan implementation.
</input-requirements>

<workflow>
Load `tsh-orchestrating-implementation`, start at Step 0, and follow the canonical workflow defined there for the rest of the implementation delivery. Every route relies on that skill's canonical Human approval gate before the first file-changing delegation.
Delivery begins in a new discussion; an invocation in the authoring discussion follows the canonical `Implementation Discussion Boundary` stop in `tsh-orchestrating-implementation`.
</workflow>

<!-- TSH_COPILOT_COLLECTIONS:prompt:tsh-implement:v5 -->
