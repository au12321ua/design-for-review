---
name: design-for-review
description: >-
  Help reviewers judge software proposals by connecting the problem's cause or
  constraint, the recommended mechanism, and its tradeoffs. Use when drafting or
  revising software designs, not routine code edits or visual UI design alone.
---

# Design for Review

Help the reviewer judge whether a proposal addresses the problem and whether its
benefits are worth its costs. Focus attention on the reasoning needed for that
judgment.

- Connect the relevant cause or constraint to the recommendation: why this
  mechanism addresses it, what changes as a result, and what material costs or
  side effects it introduces. For a new capability, start from the unmet need.
- Use technical terms the reviewer knows directly. Add the reasons and tradeoffs
  that a mechanism's name leaves unstated; expand its mechanics only when that
  helps judge the proposal. Supporting implementation detail can be omitted or
  deferred to an optional sketch.
- Ground causal claims in available evidence. Mark unconfirmed causes as
  assumptions and explain when they would change the recommendation. Keep
  material risks and compatibility effects visible even when mechanics are
  deferred.
- Recommend an approach and make clear what, if anything, needs the user's
  judgment. Handle routine choices within the agreed scope yourself. Compare
  alternatives when their differences could change the decision; do not
  manufacture decisions for the reviewer.
- Let the task determine length and structure. Use sections, examples, and tables
  when they reduce the reader's work. The causal link is a reasoning guide, not a
  required template; avoid repeating the same rationale in several forms.

When revising an existing proposal, distinguish clearer explanation from changes
to agreed decisions. Respect the user's existing boundary between design and
implementation; this skill adds no approval step.

For an optional example of where to focus attention, see
[examples/reviewable-design.md](examples/reviewable-design.md). Its format is not
required.
