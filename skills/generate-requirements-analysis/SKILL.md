---
name: generate-requirements-analysis
description: |
  用于根据一段需求描述或一个需求文件生成标准化 Markdown 需求分析文档，按日期目录与需求类型自动命名落盘，适用于需要统一模板与章节结构的场景。
metadata:
  authors:
    - "qianbimo"
    - "djhhhhhh"
  version: "1.0.1"
  input_schema:
    type: object
    properties:
      source_input:
        description: 用户提供的需求来源，可以是一段文字，也可以是包含需求内容的本地文件路径
        type: string
      output_base_dir:
        description: 用户期望存放分析文档的根目录（仅目录，不含文件名）；使用正斜杠；AI 在其下创建 yyyy-mm-dd-xxx 子目录
        type: string
      requirement_source:
        description: 用户标识的本需求业务来源，如会议纪要、工单号、PRD 名称、口头沟通记录等
        type: string
    required:
      - source_input
      - output_base_dir
      - requirement_source
---

# generate-requirements-analysis

## 参数处理规则（AI 必须遵守）

1. 从用户输入中提取以下参数：

   | 参数 | 含义 |
   |------|------|
   | `source_input` | 需求内容：一段文字，或本地需求文件路径 |
   | `output_base_dir` | **期望输出根目录**：仅目录路径，不在此参数中指定文件名 |
   | `requirement_source` | 需求业务来源标识（会议、工单、PRD 等） |

   - `output_base_dir` 使用正斜杠，**不得以 `.md` 结尾**；不得包含具体文档文件名。

2. 如果任何必填参数缺失，**必须询问用户**，不要自行猜测。

3. 如果参数不合法（例如 `output_base_dir` 为空、以 `.md` 结尾、`source_input` 指向的文件不存在），提示用户重新输入。

## Instructions

1. 先判断 `source_input` 的来源类型：
   - 如果它是现成的本地文件路径，先读取文件内容，再提取需求信息。
   - 如果它不是有效文件路径，则将其视为用户直接提供的需求描述文本。
2. 处理源内容时，优先提取：背景与目标、范围、功能需求、非功能需求、数据/接口/约束、风险与待确认项，并判断需求类型（见下文「路径与文件名」）。
3. 信息不完整时不要编造事实：可保守整理归类；无法确认的内容写入「待确认事项」或标注「待确认」/「暂无」。
4. 生成文档前，先读取 [`references/requirements-analysis-template.md`](references/requirements-analysis-template.md)，按模板章节顺序输出；某节暂无内容时保留章节并标注「待确认」或「暂无」。
5. **占位符替换**（生成最终文档时不得原样保留 `{{...}}`）：

   | 占位符 | 填写规则 |
   |--------|----------|
   | `{{DOCUMENT_NAME}}` | 与最终 Markdown 文件名一致（不含 `.md`），如 `feature`、`bugfix` 或自定义 `xx` |
   | `{{PROJECT_NAME}}` | 从源内容提取；无法推断时用目录名中的主题 slug 或「待确认」 |
   | `{{VERSION}}` | 默认 `1.0.0`；源内容有版本说明时以其为准 |
   | `{{DATE}}` | 当前日期，格式 `YYYY-MM-DD` |
   | `{{AUTHOR}}` | 执行 `git config --global user.name` 获取；若为空再试 `git config user.name`；仍为空则标注「待确认」 |
   | `{{REQUIREMENT_SOURCE}}` | 使用用户提供的 `requirement_source`；不得根据 `source_input` 自行推断或编造 |

6. 组织正文时：功能需求按模块拆分；非功能需求只写适用项；风险、依赖、约束与功能描述分开。

7. **路径与文件名（AI 必须遵守）**  
   由 AI 确定落盘路径，用户不在输入中指定具体文件名。生成顺序：

   **① 子目录**：在 `output_base_dir` 下创建  
   `{YYYY-MM-DD}-{topic_slug}/`  
   - `YYYY-MM-DD`：生成当天的日期  
   - `topic_slug`：根据需求主题生成的英文短标识，小写、连字符分隔（如 `lost-and-found`），建议 2～4 个英文单词，仅 `[a-z0-9-]`

   **② 文档文件名**：在该子目录内创建 **一个** Markdown 文件，按需求类型命名：

   | 需求类型 | 文件名 | 判定依据（示例） |
   |----------|--------|------------------|
   | 新功能 / 能力增强 | `feature.md` | 新建模块、新业务流程、新能力 |
   | 缺陷修复 | `bugfix.md` | 修复错误、异常、回归、线上故障 |
   | 其他 | `{type}.md` | 小写英文 `type`，如 `refactor`、`improvement`、`tech-debt`；须能从需求中归纳，勿用中文文件名 |

   - 类型不明确时，优先选最贴近的一类；仍无法区分时用 `feature.md`，并在「待确认事项」中注明类型待确认。
   - **最终路径**：`{output_base_dir}/{YYYY-MM-DD}-{topic_slug}/{文件名}`

8. 仅生成 **Markdown**（`.md`）；先创建目录（若不存在）再写入最终路径；不在正文中保留分析过程。
9. 写入前确认已提供 `requirement_source`，并写入文档信息表的「需求来源」。
10. 任务完成后，**仅**按下方「完成回复格式」回复用户。

## 输出规范

### 落盘文档

- 输出为完整 Markdown 需求分析文档，章节顺序与参考模板一致。
- 落盘路径必须符合「路径与文件名」规则，由 AI 生成，不由用户手填文件名。
- 文档信息表中的占位符必须按上表规则替换；「需求来源」必须来自 `requirement_source`。
- 不确定内容写「待确认」或「暂无」，不得伪造细节。

### 完成回复格式（AI 必须遵守）

向用户的回复**只包含**以下结构：

```markdown
## 生成结果

**文档简述**（最多 3 行）：
- …
- …
- …

**文档位置**：`<最终落盘的完整相对路径>`
```

- 「文档位置」填写第 7 步得到的最终路径（含目录与 `feature.md` / `bugfix.md` / `{type}.md`）。

## 示例

**用户输入：**  
`source_input`: `请帮我为一个校园失物招领平台整理需求，用户包括学生、管理员，支持发布、认领、审核、消息通知`  
`output_base_dir`: `docs/requirements`  
`requirement_source`: `2025-05 产品评审会议纪要`

**AI 落盘示例**（假设当天为 2025-05-19）：  
`docs/requirements/2025-05-19-lost-and-found/feature.md`

**预期回复：**

```markdown
## 生成结果

**文档简述**（最多 3 行）：
- 校园失物招领平台，覆盖学生发布/认领与管理员审核、消息通知。
- 本期范围含失物发布、认领流程与审核；非功能与外部系统依赖部分待确认。
- 待确认事项 3 项，含认领规则细节与通知渠道。

**文档位置**：`docs/requirements/2025-05-19-lost-and-found/feature.md`
```
