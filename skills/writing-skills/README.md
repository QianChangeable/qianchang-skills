# writing-skills

**Runtime: Cursor only.** Do not load this skill as-is in Claude Code, Codex, Gemini CLI, or other agents. Paths, deploy steps, and pressure-test harness are Cursor-specific.

这是 Cursor 适配版。方法论（怎么写 skill、怎么测）可复用；运行时约定不能直接给其他 agent 用。

## 当前绑定

| 项 | 本仓库这份 |
|----|------------|
| 目标环境 | Cursor Agent |
| 个人 skill | `~/.cursor/skills/<name>/SKILL.md` |
| 项目 skill | `<repo>/.cursor/skills/<name>/SKILL.md` |
| 禁止写入 | `~/.cursor/skills-cursor/`（Cursor 内置，编辑器托管） |
| 部署 | 写到上面两个目录即部署，不要求推 Superpowers fork |
| 压测 | 用 Cursor subagent；不依赖 Superpowers TDD skill / harness |
| 与内置 skill 的关系 | 若 Cursor 自带 `create-skill` 同时命中，写约束以本 skill 为准，路径用 Cursor 路径 |

Claude Code 默认路径是 `~/.claude/skills/`。本 skill 已写明：**不要用 Claude Code 路径，除非那台机器上真的有这个目录。**

## 其他 agent 不能直接用的原因

1. **落盘路径写死了 Cursor。** 按本文部署会写到 `~/.cursor/skills/`，其他运行时找不到，或写进错误位置。
2. **部署定义不同。** Cursor 写文件即生效；Claude Code / Superpowers 可能还要求装到指定 skills 仓库或走另一套安装流程。
3. **压测手段不同。** 文中的 subagent 是 Cursor 的；其他产品没有同名工具，不能照抄「dispatch Cursor subagent」。
4. **依赖声明是负向的。** 「Superpowers `test-driven-development` 未安装、缺了也不许 stall」只对这份 Cursor 安装成立。换环境后，该依赖可能存在、也可能要用别的测试方式。

## 可跨环境复用的部分

这些不绑 Cursor，换 agent 仍可当规范用：

- `SKILL.md` 里的结构、SDO（description 只写 when）、Iron Law、checklist
- `testing-skills-with-subagents.md` 的 RED-GREEN-REFACTOR 与压力场景设计（需换成该环境的 agent/subagent）
- `persuasion-principles.md`
- `anthropic-best-practices.md`（Anthropic 原文；文末仍提到 Claude Code）
- `graphviz-conventions.dot`

## 换环境时最少要改什么

1. 把所有 `~/.cursor/skills/`、`.cursor/skills/`、`~/.cursor/skills-cursor/` 换成目标运行时的 skill 目录。
2. 改掉「Cursor runtime / Cursor subagent / 不要推 Superpowers fork」整段，写成该环境的部署与测试步骤。
3. 改 `SKILL.md` 的 `description`：现在写的是 `authoring Cursor agent skills`，否则其他 agent 可能根本不会选中它，或选中后仍按 Cursor 路径执行。
4. 同步改 `examples/CLAUDE_MD_TESTING.md` 里的路径和「check skills 目录」文案。
5. 改完后按该环境重新做一次压测，不要假设 Cursor 上验证过就够。

未完成上述替换前，其他 agent 应拒绝按本 skill 落盘或部署。

## 文件

| 文件 | 作用 |
|------|------|
| [SKILL.md](SKILL.md) | 主指令（Cursor 运行时） |
| [testing-skills-with-subagents.md](testing-skills-with-subagents.md) | 用 agent 压测 skill |
| [anthropic-best-practices.md](anthropic-best-practices.md) | Anthropic 官方写作建议 |
| [persuasion-principles.md](persuasion-principles.md) | 纪律类 skill 的措辞原则 |
| [graphviz-conventions.dot](graphviz-conventions.dot) | flowchart 风格 |
| [examples/CLAUDE_MD_TESTING.md](examples/CLAUDE_MD_TESTING.md) | 文档变体压测示例 |
