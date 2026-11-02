# Design for Review

[中文说明](README.zh-CN.md)

A lightweight agent skill that reduces the effort of reviewing software designs.
Focus on **why the proposed mechanism addresses the problem and what it costs**.
Use familiar technical terms directly; add the reasoning needed to judge the
recommendation.

For example: “Transient service failures currently end the export immediately,
so add bounded exponential backoff to the worker. This can avoid manual restarts,
but delays final failure and increases service load.”
See the [example](examples/reviewable-design.md).

## What it does

- Connects the cause or constraint to a recommended mechanism and its side effects.
- Keeps familiar technical shorthand and adds missing reasons and tradeoffs.
- Highlights what needs your judgment and handles routine choices within scope.
- Keeps important risks and assumptions visible.
- Includes technical detail where it helps you judge the design; defers the rest.
- Adapts length and structure without required sections, appendices, or tables.
- Respects design-only requests and existing authorization to implement.

Use it to draft a proposal before coding or rewrite a design that mixes rationale
with mechanics. It is not a visual UI design skill or a full architecture audit.

## Install

Download or clone this repository. The repository root is the skill folder:
`SKILL.md`, `agents/`, and `examples/` should stay together.

For Codex, copy the folder to either:

- `~/.agents/skills/design-for-review/` for personal use across projects.
- `<your-project>/.agents/skills/design-for-review/` for one project.

For example, from the downloaded repository on macOS or Linux:

```bash
mkdir -p ~/.agents/skills/design-for-review
cp -R SKILL.md agents examples ~/.agents/skills/design-for-review/
```

These commands replace matching files if already installed. Codex detects skills
automatically; restart it if the skill does not appear. See the
[official skill documentation](https://developers.openai.com/codex/skills).

Other agents that support `SKILL.md` can use the same core instructions; follow
their documented installation location and invocation syntax. `agents/openai.yaml`
is optional Codex UI metadata. No runtime dependencies, API keys, or external tools
are required by this skill.

## Use

```text
Use $design-for-review to design automatic retries for failed report exports.
Explain why the mechanism fits the problem and what side effects it introduces.
Do not implement yet.
```

```text
Use $design-for-review to rewrite this proposal so its key decisions are easy to
judge. Preserve the agreed requirements and decisions, and flag missing rationale.
```

Implementation details can be expanded when useful; an appendix is optional.
You can request a file such as `DESIGN.md` to save the result. When you explicitly
authorize both design and implementation, the skill does not insert an additional
approval step.

## Contributing

Issues and pull requests are welcome. Include a small input and explain how the
result made review harder: missing rationale, hidden behavior, unnecessary scope,
or implementation details needed to understand a decision. Remove private project
information. Prefer focused changes over adding mandatory sections for every case.

Check the YAML frontmatter and local links. Iterate from daily use: notice what
still requires follow-up questions or unnecessary reading to judge the proposal.

## License

[MIT](LICENSE).
