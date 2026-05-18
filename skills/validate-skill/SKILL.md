---
name: validate-skill
description: |
  Validates SKILL.md against DDO Skills Hub templates in skills/init-skill/_templates/.
  Use when authoring or reviewing a skill, or before opening a pull request.
metadata:
  authors:
    - "ddo-pulse"
  version: "1.0.0"
---

# Validate Skill

对照 [`skills/init-skill/_templates/`](skills/init-skill/_templates/) 中的官方模板，检查目标 Skill 是否合规。本技能为 **simple** 格式，仅由 Agent 执行检查，**不运行任何脚本**。

## 何时使用

- 编写或修改 `skills/<skill-name>/SKILL.md` 之后
- 提交 Pull Request 之前
- Review 他人 Skill 变更时

## 对照模板

| 类型 | 模板文件 | 判定条件 |
|------|----------|----------|
| **parameterized**（带参数） | `SKILL.template-parameterized.md` | frontmatter 含 `metadata.input_schema` |
| **simple**（无参数） | `SKILL.template-simple.md` | frontmatter **无** `input_schema` |

校验前请先阅读对应模板全文，再逐项比对目标 `SKILL.md`。

## 共有项（两种模板均需满足）

### Frontmatter

- [ ] 以 `---` 包裹的 YAML frontmatter
- [ ] `name` 与父目录名一致，符合 `^[a-z0-9-]{1,64}$`，且不是占位符 `<SKILL_NAME>`
- [ ] `description` 使用 `|` 多行块；内容非空、第三人称、说明 WHAT + WHEN，≤1024 字符
- [ ] `metadata.authors` 为非空列表；每项为 **GitHub username**（非 `@` 前缀、非中文姓名、非邮箱）
- [ ] `metadata.version` 为非空字符串（建议 SemVer，如 `1.0.0`）
- [ ] 无未替换的模板占位符：`your-github-username`、`<SKILL_NAME>`、`<parameter_name>` 等

### 正文结构

- [ ] 一级标题 `#` 与 `name` 一致（人类可读标题，可与目录名相同）
- [ ] 包含 `## Instructions` 章节
- [ ] 包含 `## 输出规范` 章节
- [ ] 包含 `## 示例` 章节
- [ ] 全文 ≤500 行；路径使用正斜杠，无 Windows 反斜杠路径

## parameterized 专项（对照 `SKILL.template-parameterized.md`）

- [ ] `metadata.input_schema.type` 为 `object`
- [ ] `properties` 中每个参数含 `description` 与 `type`（`number` | `string` | `integer` | `boolean`）
- [ ] `required` 列表中的字段均定义在 `properties` 中
- [ ] 存在 `## 参数处理规则（AI 必须遵守）`，且包含三条规则：
  1. 从用户输入中提取参数（列出各 `required` 字段）
  2. 必填缺失时**必须询问用户**，不得猜测
  3. 类型错误时提示用户重新输入
- [ ] **不得**同时出现 simple 模板才应有的结构冲突（例如无 `input_schema` 却写参数规则）

## simple 专项（对照 `SKILL.template-simple.md`）

- [ ] frontmatter **无** `metadata.input_schema`
- [ ] **无** `## 参数处理规则（AI 必须遵守）` 章节
- [ ] 结构与模板章节顺序一致：`#` 标题 → Instructions → 输出规范 → 示例

## 本仓库 Hub 规则

- `skills/` 为提交入口；`.agent/skills/` 为本仓启用集
- 若存在 `.agent/skills/<name>/`，须与 `skills/<name>/` **文件树与内容完全一致**
- 仅存在于 `skills/`、未在 `.agent/skills/` 出现的 Skill 允许

## 执行步骤

1. 确认目标路径 `skills/<skill-name>/SKILL.md`（或 PR 中变更的文件）
2. 判定为 **parameterized** 或 **simple**
3. 打开 [`skills/init-skill/_templates/`](skills/init-skill/_templates/) 中对应模板，逐节比对
4. 输出校验报告（见下），列出所有未通过项及修改建议

## 输出格式

```markdown
## Validation Report: <skill-name>

- format: parameterized | simple
- template: SKILL.template-parameterized.md | SKILL.template-simple.md
- result: PASS | FAIL

### 未通过项
- [Must] ...
- [Should] ...

### 建议
- ...
```

**Must** 未全部通过时，`result` 为 `FAIL`。
