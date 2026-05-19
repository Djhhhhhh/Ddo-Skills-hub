---
name: generate-requirements-analysis
description: |
  用于根据一段需求描述或一个需求文件生成标准化需求分析文档，适用于需要统一模板、统一章节结构，并由用户指定输出路径与输出格式的场景。
metadata:
  authors:
    - "qianbimo"
  version: "1.0.0"
  input_schema:
    type: object
    properties:
      source_input:
        description: 用户提供的需求来源，可以是一段文字，也可以是包含需求内容的本地文件路径
        type: string
      output_path:
        description: 输出文档目标路径，由用户明确指定，使用正斜杠路径
        type: string
      output_format:
        description: 输出文档格式，由用户指定，例如 md、txt、docx
        type: string
    required:
      - source_input
      - output_path
      - output_format
---

# generate-requirements-analysis

## 参数处理规则（AI 必须遵守）

1. 从用户输入中提取以下参数：
   - `source_input`: 用户提供的需求来源，可以是一段文字，也可以是包含需求内容的本地文件路径
   - `output_path`: 输出文档目标路径，由用户明确指定，使用正斜杠路径
   - `output_format`: 输出文档格式，由用户指定，例如 md、txt、docx

2. 如果任何必填参数缺失，**必须询问用户**，不要自行猜测。

3. 如果参数类型错误或内容不合法（例如 `output_path` 为空、`output_format` 不是文本格式标识、`source_input` 指向的文件不存在），提示用户重新输入。

## Instructions

1. 先判断 `source_input` 的来源类型：
   - 如果它是现成的本地文件路径，先读取文件内容，再提取需求信息。
   - 如果它不是有效文件路径，则将其视为用户直接提供的需求描述文本。
2. 处理源内容时，优先提取这些信息：
   - 项目背景或问题背景
   - 建设目标
   - 使用角色或相关干系人
   - 业务场景与核心流程
   - 功能需求
   - 非功能需求
   - 数据、接口、依赖、约束、风险、待确认项
3. 如果源内容信息不完整，不要编造事实：
   - 可以根据上下文做最保守的整理与归类
   - 对无法确认的内容，统一写入“待确认事项”或“未明确项”
4. 生成文档时，必须严格使用固定模板：
   - 先读取 [`references/requirements-analysis-template.md`](references/requirements-analysis-template.md)
   - 所有需求分析文档默认按该模板章节顺序输出
   - 若某章节暂无内容，也应保留章节，并明确标注“待确认”或“暂无”
5. 组织内容时遵循这些规则：
   - 区分“已明确需求”和“推断出的整理项”
   - 功能需求按模块拆分，并尽量写出输入、处理、输出或结果
   - 非功能需求至少覆盖性能、安全、可用性、兼容性、可维护性中的适用项
   - 风险、依赖、假设、边界条件单独列出，避免混入功能描述
6. 输出文档时遵循用户指定的 `output_format`：
   - `md`：输出 Markdown 文档
   - `txt`：输出纯文本文档，并保持模板层级可读
   - `docx`：若环境支持生成 Word 文档，则按模板写出 `.docx`；若当前环境无法可靠生成，必须先告知用户再调整方案
7. 将最终结果写入 `output_path`，并确保：
   - 输出文件内容与模板一致
   - 文档标题、章节标题、编号与列表结构清晰
   - 不在正文中保留与任务无关的分析过程
8. 完成后，向用户说明：
   - 已使用的输入来源类型
   - 输出路径与输出格式
   - 文档中哪些部分是明确需求，哪些部分仍需确认

## 输出规范

- 输出结果必须是一份完整的需求分析文档，而不是零散摘要。
- 文档章节顺序必须与参考模板保持一致。
- 对不确定内容，统一写为“待确认”或“暂无”，不得伪造细节。
- 若输入内容同时包含显性需求和隐性约束，应在文档中分别归类。
- 若用户指定了输出路径与格式，最终结果应以写入文件为主，并在回复中简要说明落盘情况。

## 示例
**用户输入：**  
`source_input`: `请帮我为一个校园失物招领平台整理需求，用户包括学生、管理员，支持发布、认领、审核、消息通知`  
`output_path`: `docs/lost-and-found-requirements.md`  
`output_format`: `md`

**预期输出：**  
先将 `source_input` 识别为文本需求，再按照固定模板生成需求分析文档，内容至少覆盖项目背景、建设目标、角色、业务流程、功能需求、非功能需求、数据与接口关注点、风险与待确认事项，最后将文档写入 `docs/lost-and-found-requirements.md`。
