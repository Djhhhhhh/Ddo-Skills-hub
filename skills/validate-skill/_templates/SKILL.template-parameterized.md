---
name: <SKILL_NAME>
description: |
  [Third-person, WHAT + WHEN]
metadata:
  authors:
    - "your-github-username"
  version: "1.0.0"
  input_schema:
    type: object
    properties:
      <parameter_name>:
        description: <What this parameter means>
        type: number | string | integer | boolean
    required:
      - <parameter_name>
---

# <SKILL_NAME>

## 参数处理规则（AI 必须遵守）

1. 从用户输入中提取以下参数：
   - `<parameter_name>`: <description>

2. 如果任何必填参数缺失，**必须询问用户**，不要自行猜测。

3. 如果参数类型错误（例如 `principal` 不是数字），提示用户重新输入。

## Instructions

（在此按步骤编写业务逻辑）

### 步骤示例
1. 接收提取到的参数。
2. 执行核心计算或操作。
3. 按指定格式输出结果。

## 输出规范
- （例如：使用 Markdown 表格、代码块或纯文本）

## 示例
**用户输入：**  
<示例输入>

**预期输出：**  
<示例输出>
