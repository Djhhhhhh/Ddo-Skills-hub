---
name: party-credit-system-dev
description: |
  用于党员信用系统相关的开发、评审与交付工作，适用于需要遵循项目既定角色边界、MVP范围、技术选型、编码规范与协作流程的场景。
metadata:
  authors:
    - "qianbimo"
  version: "1.0.0"
---

# party-credit-system-dev

## Instructions

1. 先识别任务类型：功能开发、缺陷修复、接口设计、前端页面实现、代码评审，或文档更新。
2. 所有工作默认都应限制在说明书定义的 MVP 范围内。除非用户明确要求扩展，否则不要擅自增加新角色、新流程、额外评分规则或便捷功能。
3. 根据任务类型按需阅读参考资料：
   - 处理业务范围、角色职责、模块边界时，先看 [`references/project-overview.md`](references/project-overview.md)。
   - 处理技术选型、模块实现方式、框架约束时，先看 [`references/tech-stack.md`](references/tech-stack.md)。
   - 编码、重构、评审、接口设计前，先看 [`references/development-standards.md`](references/development-standards.md)。
4. 每一项改动都要先映射到三类既定角色之一：普通党员、党支部、党委办公室。如果请求跨角色，必须明确区分各角色权限与行为边界。
5. 默认遵循说明书中的整体架构：
   - 后端采用 Spring Boot 分层结构，按 Controller、Service、Mapper 或 DAO、Entity、DTO、VO 分责实现。
   - 前端采用 Vue 3、TypeScript、Vite、Vue Router、Pinia、Axios、Vue Query 组合。
   - 接口设计尽量遵循 RESTful 风格，请求和响应格式保持一致。
   - 数据库记录默认采用软删除，不使用硬删除，除非用户明确要求偏离该规范。
6. 编写或评审代码时，严格执行项目规范：
   - 使用说明书要求的命名规则，覆盖类名、方法名、DTO、VO、配置类、异常类、分支名等。
   - Controller 只负责请求与响应，业务逻辑放在 Service，数据访问放在 Mapper 或 DAO。
   - 优先按场景拆分 DTO 与 VO，不创建万能传输对象。
   - 公共类与公共方法补充有意义的 Javadoc。
   - 日志统一使用 SLF4J 占位符写法，禁止使用 `System.out.println`。
7. 如果用户需求表述不完整，优先采用最保守、最贴近说明书边界的理解方式，并在最终结果中说明假设。
8. 进行代码评审时，优先检查这些高风险问题：分层越界、命名不合规、缺少校验、误用硬删除、接口不符合 RESTful、类或方法过大，以及违反协作流程。

## 输出规范

- 先给出完成结果或主要发现。
- 明确说明本次工作对应的业务角色、功能模块或技术层。
- 如果实现方案直接受说明书约束，应点明对应的规则来源，例如 MVP 范围、分层规范、命名规范、软删除、RESTful API 或 Git 流程。
- 如果用户需求与说明书存在冲突，先指出冲突点，再给出最小偏离方案。

## 示例

**用户输入：**  
给党员信用系统增加党支部的一键审批接口，并检查是否符合项目规范。

**预期输出：**  
先将该需求归类为党支部角色下的审核模块，并确认其仍属于 MVP 范围。实现时保持 Controller、Service、Mapper 分层，接口路径遵循 RESTful 风格，审批相关数据流不使用硬删除。最后总结本次改动内容、受影响模块，以及方案分别受哪些说明书规则约束，并补充仍需确认的边界假设。
