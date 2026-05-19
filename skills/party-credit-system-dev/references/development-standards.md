# 开发规范

## 命名与分层规范

- Java 源文件名必须与顶层类名完全一致。
- 类与枚举使用 `UpperCamelCase` 命名。
- 接口名应体现能力或业务语义，禁止使用 `I` 前缀。
- 实现类统一追加 `Impl` 后缀。
- 配置类统一追加 `Config` 或 `Configuration` 后缀。
- 异常类统一追加 `Exception` 后缀。
- 工具类统一追加 `Util` 或 `Utils` 后缀。
- 测试类统一追加 `Test` 后缀。
- Controller 类必须以 `Controller` 结尾。
- Service 接口名称应体现业务领域，实现类追加 `Impl`。
- 数据访问层类名必须以 `Mapper` 或 `Dao` 结尾。
- DTO 命名必须体现使用场景，例如 `CreateDTO`、`UpdateDTO`、`Query`。
- VO 命名必须体现展示场景，例如 `ListVO`、`DetailVO`。
- 包名必须全小写，并遵循反向域名风格。

## 文件与方法规模限制

- 一个 Java 文件只应包含一个顶层类或接口。
- 单个 Java 文件应控制在 500 行以内。
- 单个方法应控制在 80 行以内。
- 单个方法参数不应超过 7 个，超过时应封装为 DTO。

## 分层职责

- Controller 仅负责 HTTP 请求接收与响应返回。
- Service 承担业务逻辑，并应面向接口编程。
- Mapper 或 DAO 仅负责数据访问。
- Entity 类仅负责数据库结构映射，应保持为简单 POJO。

## 注释与文档规范

- 注释应解释设计意图，而不是重复显而易见的代码行为。
- 公共类、公共方法、公共字段必须补充 Javadoc。
- 行内注释应解释业务原因或特殊边界。
- `TODO` 与 `FIXME` 注释必须包含作者和日期。
- 枚举值必须说明各自的业务含义。

## 编码实践规范

- 缩进统一使用 4 个空格。
- 每行长度控制在 120 个字符以内。
- 即使只有一条语句，也必须使用大括号。
- 方法参数与成员变量使用 `lowerCamelCase`。
- 常量使用 `CONSTANT_CASE`。
- 日志统一使用 SLF4J 与占位符写法。
- 避免魔法字符串与魔法数字，优先提取为常量或枚举。
- 返回集合时使用空集合，不返回 `null`。
- 适合表达“可能不存在”结果时，优先使用 `Optional`。

## 数据库与接口规范

- 数据记录默认采用软删除。
- API 路径应尽量遵循 RESTful 规范。

## 协作与 Git 规范

- `main` 是受保护分支。
- 所有变更应先提交到功能分支，再通过 Pull Request 合并。
- 每个 Pull Request 至少需要 1 名审核人通过。
- 分支命名应遵循 `type/owner/description`，例如 `feat/cgh/add-auth`。
- Commit 信息应遵循 Conventional Commits，例如 `feat: 新增PDF导出功能`。
