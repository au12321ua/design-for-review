# Design for Review

[English](README.md)

一个轻量的 agent skill：让你不用细看实现，也能判断设计里**准备改什么、为什么值得改、有什么代价和风险**。

例如，把“新增重试管理器”改写成：“在同一个导出请求里重试短暂的服务故障，限制等待时间，并避免生成重复报表。”代码细节放在最后。参见[完整示例（英文）](examples/reviewable-design.md)。

## 它做什么

- 分开写意图与决策、可观察行为、实现细节。
- 把重要理由、兼容性影响和风险保留在正文里。
- 将代码细节放在可选附录，提供设计决策到实现的映射。
- 根据问题规模组织内容，避免机械填写长模板。
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
我主要 review 要改什么、为什么这样改、行为有什么变化。
先写设计，不要实现。
```

```text
用 $design-for-review 改写这份设计，让我不看实现附录也能判断方案。
保留原有需求和决策，缺失的理由请明确标出来。
```

skill 会跟随你的语言和要求控制详略。可以要求“只写概念设计，不写实现”，也可以指定保存到 `DESIGN.md`。如果你已经要求“先设计再实现”，它不会额外增加一次审批。

## 参与改进

欢迎提交 issue 或 PR。请提供一个简短输入，说明输出哪里增加了 review 成本，例如理由缺失、行为不清楚、范围扩大，或必须读代码细节才能理解决策。请去掉私人项目信息。

优先做有实际案例支持的小幅改进。提交前检查 YAML 元数据和本地链接，再用真实设计任务验证：隐藏实现附录后，正文是否仍足够支持判断？格式检查不能替代这个判断。

## 许可证

[MIT](LICENSE)。
