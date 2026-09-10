# Design for Review

[中文说明](README.zh-CN.md)

A lightweight agent skill for software designs you can review without reading the
implementation details. Understand **what changes, why it matters, and what the
tradeoffs are** before looking at files, classes, or APIs.

Instead of “add a retry manager,” get “retry temporary interruptions within the
same export request, with a bounded wait and protection against duplicate reports.”
See the [complete example](examples/reviewable-design.md).

## What it does

- Separates intent and decisions, observable behavior, and implementation.
- Keeps important rationale, compatibility effects, and risks in the main body.
- Places code details in an optional appendix with a decision-to-implementation map.
- Scales to the change instead of requiring a long template.
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
Explain the behavior and tradeoffs first. Do not implement yet.
```

```text
Use $design-for-review to rewrite this proposal so I can review its decisions
without reading the implementation appendix. Preserve the existing requirements
and flag missing rationale.
```

The skill follows your language and requested depth. Ask for a conceptual design
only to omit implementation details, or request a file such as `DESIGN.md` to save
the result. When you explicitly authorize both design and implementation, the
skill does not insert an additional approval step.

## Contributing

Issues and pull requests are welcome. Include a small input and explain how the
result made review harder: missing rationale, hidden behavior, unnecessary scope,
or implementation details needed to understand a decision. Remove private project
information. Prefer focused changes over adding mandatory sections for every case.

Before submitting, check the YAML frontmatter and local links, then try a realistic
design request. Judge whether the main body stands on its own with the appendix
hidden; formatting checks alone cannot establish design quality.

## License

[MIT](LICENSE).
