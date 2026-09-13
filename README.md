# qianchang-skills
My skill repository.

## Skills

| Skill | 用途 |
|-------|------|
| [writing-skills](skills/writing-skills/README.md) | 创建、编辑、验证 Cursor Agent Skill（**仅 Cursor**；修改时靠用户级规则强制加载，见该 README） |
| [writing-conventional-commits](skills/writing-conventional-commits/README.md) | 按 Conventional Commits 写 git commit，确认后再提交 |

## Rules

复制到 `~/.cursor/rules/`，并保持 `alwaysApply: true`。两份都要拷：

- `rules/unslop.mdc`
- `skills/writing-skills/require-writing-skills.mdc`（源文件跟 skill 走，不在 `rules/`）

| Rule | 用途 |
|------|------|
| [unslop](rules/unslop.mdc) | 用户可见回复用字面陈述：说机制、文件或数字；没查过就说没查；不写比喻和套话 |
| [require-writing-skills](skills/writing-skills/require-writing-skills.mdc) | 创建或修改任何 skill 时，先完整读取并遵循 `writing-skills`；仅靠 skill 的 description 不够 |

## 提交规范

本仓库使用 [Conventional Commits](https://www.conventionalcommits.org/)：

- 前缀用英文 type（`feat` / `fix` / `docs` / `refactor` / `chore` 等）
- 说明用中文，简洁明了，写清做什么
- 不要只写 skill 名（反例：`feat: 添加writing-skills`）
- 格式：`type: 说明`

```
feat: 添加规范撰写skill的skill
feat: 添加commit规范skill
```

不同类型拆成多次提交。先出示消息，确认后再 `git commit`。
