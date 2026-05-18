---
name: init-skill
description: |
  Scaffolds a new skill directory under skills/ from bundled templates (simple or parameterized).
  Use when creating a new skill folder in ddo_skills_hub before filling in business logic.
metadata:
  authors:
    - "djhhhhhh"
  version: "1.0.0"
  input_schema:
    type: object
    properties:
      skill_name:
        description: Directory and frontmatter name in kebab-case
        type: string
      format:
        description: Template type — simple (no params) or parameterized (with input_schema)
        type: string
    required:
      - skill_name
      - format
---

# Init Skill

在 `skills/` 下初始化一个新 Skill 的**目录与骨架文件**。模板位于本技能目录下的 [`_templates/`](_templates/)，**不编写业务逻辑**。

## 参数处理规则（AI 必须遵守）

1. 从用户输入中提取以下参数：
   - `skill_name`: 新 Skill 目录名，须符合 `^[a-z0-9-]{1,64}$`（kebab-case）
   - `format`: 模板类型，仅允许 `simple` 或 `parameterized`（`parameterized` 表示带参数模板）

2. 如果任何必填参数缺失，**必须询问用户**，不要自行猜测。

3. 如果 `skill_name` 不符合命名规则，或 `format` 不是上述二者之一，提示用户重新输入。

## Instructions

1. **定位本技能根目录**：当前 Skill 所在文件夹（即包含本 `SKILL.md` 与 `_templates/` 的目录）。以下模板路径均**相对于该目录**。
2. **检查冲突**：若仓库根下 `skills/<skill_name>/` 已存在，停止并告知用户，不得覆盖。
3. **读取模板**（用 Read 工具打开文件内容，勿猜测）：
   - `format: simple` → `_templates/SKILL.template-simple.md`
   - `format: parameterized` → `_templates/SKILL.template-parameterized.md`
4. **创建目录**：`skills/<skill_name>/`（相对于仓库根）
5. **写入文件**：将模板内容保存为 `skills/<skill_name>/SKILL.md`，且**仅**做以下替换：
   - 将所有 `<SKILL_NAME>` 替换为 `skill_name` 的实际值（含 frontmatter 的 `name` 与正文一级标题）
   - **保留**其余占位符（如 `your-github-username`、`<parameter_name>` 等），由作者后续填写
6. **不要**：
   - 填写 Instructions、输出规范、示例中的业务内容
   - 创建 `scripts/` 或其他子目录（除非用户明确要求）
   - 自动写入 `.agent/skills/`（本仓启用需作者自行同步）
7. 完成后提示用户：用 [`validate-skill`](../validate-skill/SKILL.md) 对照 `init-skill/_templates/` 检查，并替换 `your-github-username` 等占位符。

## 输出规范

- 说明已创建的路径、选用的 `format` 及使用的模板相对路径
- 列出作者仍需手动完成的占位符清单

## 示例

**用户输入：**  
`skill_name: compound-interest`，`format: parameterized`

**预期结果：**  
从 `_templates/SKILL.template-parameterized.md` 读取内容，创建 `skills/compound-interest/SKILL.md`，`<SKILL_NAME>` 已替换为 `compound-interest`，其余占位符保留。
