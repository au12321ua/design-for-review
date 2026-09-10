---
name: design-for-review
description: >-
  Write or revise software design proposals for human review, separating intent,
  observable behavior, and rationale from implementation details. Use when a user
  asks for a design before coding, a decision-focused proposal, or a more readable
  existing design. Not for routine code edits or visual UI design alone.
---

# Design for Review

Help the reviewer decide what should change and why, without reading code-level
details. Assume they will stop reading before the implementation appendix.

## Establish the task

- Follow the user's language, scope, requested depth, and document conventions.
- For an existing system, inspect enough relevant code and documentation to ground
  claims about current behavior. Distinguish observed facts from assumptions;
  never invent files, interfaces, measurements, or repository findings.
- Ask only about missing information that materially changes the design. Make
  reasonable, explicit assumptions for details that do not block useful work.
- If rewriting an existing proposal, preserve its requirements and decisions.
  Flag missing rationale or contradictions rather than silently inventing intent.
- Deliver in chat unless the user requests a file or the project has an established
  design-document location. Preserve the requested template while separating layers.

## Keep three layers distinct

**Intent and decisions:** the problem, desired outcome, important choices, and why
those choices are justified.

**Behavior:** what users, callers, and other components will experience; conceptual
responsibilities, boundaries, flow, and meaningful failure behavior.

**Implementation:** files, classes, functions, signatures, dependencies, data
structures, migration mechanics, and test locations. Put these in the appendix.

In the main body, explain a mechanism through its effect and purpose, not through
a list of code components. Include a technical constraint there only when it
changes the decision or an observable contract; explain its consequence in plain
language. Compatibility, data loss, rollout disruption, and other material risks
must remain visible even when their mechanics belong in the appendix.

## Write the reviewable design

Lead with a short statement of the proposed change and its value. Cover the topics
below, combining sections for small proposals instead of filling a rigid template.
Spend attention on consequential decisions; summarize routine details.

1. **Problem.** Describe the current limitation and who or what it affects.
2. **Goal.** State the observable capability or outcome the change should enable.
3. **Proposed approach.** Explain the conceptual mechanism, responsibilities,
   boundaries, and flow. Include an ordinary case and relevant failure behavior.
4. **Decisions and rationale.** For each consequential choice, connect the decision
   to its reason, the problem it solves, and its tradeoff. Compare plausible
   alternatives, including keeping the current behavior where useful. Do not
   invent alternatives just to fill space or elevate routine coding choices into
   architectural decisions.
5. **Observable changes.** Describe concrete before/after behavior and how success
   can be checked. Mention preserved behavior when it matters for compatibility
   or prevents a likely misunderstanding.
6. **Scope.** Identify relevant boundaries and non-goals that prevent scope creep.
   Do not introduce speculative abstractions or unrelated cleanup.
7. **Risks and open questions.** Separate unresolved design choices from details
   that implementation can validate. Explain the consequence of uncertain
   assumptions and how they can be checked. Highlight questions requiring the
   reviewer's decision; do not manufacture a question when none is needed.

The main body must contain enough information to judge the direction, scope,
tradeoffs, and risk without opening the appendix. A test filename is not an
acceptance criterion, and a class name is not an explanation of responsibility.

## Add the implementation appendix

After the main body stands on its own, add a concise **Implementation sketch**
when requested or useful to establish feasibility. Describe likely code locations,
interfaces, dependencies, migration steps, and verification. Label tentative
mechanics as tentative; they are not additional requirements.

When including a sketch, end with a compact mapping:

| Decision | Why it exists | Observable effect | Likely implementation |
|---|---|---|---|

Map significant implementation components back to a requirement or justified
decision. Question components without such a connection. Keep decision rationale
in the main body as well, so the table is not required reading. If the user asks
for a conceptual design only, omit the appendix and implementation mapping.

## Check the review boundary

Before delivering, mentally hide the appendix and check:

- Can the reviewer explain what changes, why, and who is affected?
- Does each major mechanism have a reason beyond naming a code structure?
- Are observable outcomes and important failure cases clear?
- Are requirements, proposed decisions, and assumptions distinguishable?
- Are material costs, compatibility effects, and uncertainties visible?
- Does the scope stay tied to the problem?

Revise any missing explanation before presenting the proposal. For a worked
example of separating behavior from mechanics, read
[examples/reviewable-design.md](examples/reviewable-design.md) only when helpful.

## Respect the requested workflow

If the user asked for design only or explicitly wants to review before coding,
deliver the design and stop before implementation. If they already authorized
design and implementation, continue within that authorization after producing
the design; this skill does not add a new approval gate. A self-check is not
human approval. When implementation exposes a material design change, explain
its effect and update the design, following the user's existing review boundary.
