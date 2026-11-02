# Design for Review

[English](README.md)

一个轻量的 agent skill：降低软件设计的审阅负担，让你的注意力集中在**方案为什么对症、会引入什么代价**。直接使用你熟悉的技术术语，补上判断方案所需的因果与取舍。

例如：“短暂的服务故障会让导出任务直接失败，所以建议在 worker 中加入有上限的指数退避重试；代价是最终失败确认更慢、服务请求增加。”参见[示例（英文）](examples/reviewable-design.md)。

## 它做什么

- 把问题的原因或约束、推荐的机制、引入的副作用联系起来。
- 保留熟悉的技术表达，补充缺失的理由和取舍。
- 突出需要你判断的事，在约定范围内自行处理常规选择。
- 保留重要风险和可能改变建议的假设。
- 按是否有助于判断来取舍技术细节，其余按需展开。
- 按任务决定详略和结构，不要求固定章节、附录或映射表。
- 尊重“先设计，等我 review”的要求，也保留已经授权的设计加实现流程。

适合编码前写方案，或改写混杂了想法与实现的已有设计。它不负责视觉 UI 设计，也不是完整架构审计工具。

## 安装

下载或克隆本仓库。仓库根目录就是 skill 目录；请保留 `SKILL.md`、`agents/` 和 `examples/` 的相对位置。

Codex 可以安装到：

- `~/.agents/skills/design-for-review/`：个人使用，跨项目生效。
- `<项目目录>/.agents/skills/design-for-review/`：只供该项目使用。

例如，在 macOS 或 Linux 的仓库根目录运行：

```bash
mkdir -p ~/.agents/skills/design-for-review
cp -R SKILL.md agents examples ~/.agents/skills/design-for-review/
```

如果已安装，这些命令会覆盖同名文件。Codex 会自动发现 skill；如果未出现，可重启后查看。安装路径依据 [Codex 官方文档](https://developers.openai.com/codex/skills)。

其他支持 `SKILL.md` 的 agent 可以使用相同的核心指令，请按各自文档选择安装路径和调用方式。`agents/openai.yaml` 是可选的 Codex 界面元数据。这个 skill 不需要运行时依赖、API key 或外部工具。

## 使用

```text
用 $design-for-review 设计报表导出失败后的自动重试。
解释这个机制为什么对症，以及会引入什么副作用。
先写设计，不要实现。
```

```text
用 $design-for-review 改写这份设计，让关键决策更容易判断。
保留已经确定的需求和决策，缺失的理由请明确标出来。
```

实现细节可以按需展开，附录是可选的。也可以指定保存到 `DESIGN.md`。如果你已经要求“先设计再实现”，它不会额外增加一次审批。

## 参与改进

欢迎提交 issue 或 PR。请提供一个简短输入，说明输出哪里增加了 review 成本，例如理由缺失、行为不清楚、范围扩大，或必须读代码细节才能理解决策。请去掉私人项目信息。

优先做有实际案例支持的小幅改进。检查 YAML 元数据和本地链接，在日常使用中留意：哪些地方仍需要追问，哪些细节增加了判断负担。

## 许可证

[MIT](LICENSE)。
