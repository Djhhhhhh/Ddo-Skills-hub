# 技术栈

## 后端技术栈

- Spring Boot
- Spring Web，用于 RESTful API 开发
- Spring Security，用于身份认证与授权
- JWT，基于 `jjwt` 实现 Token 生成与校验
- Spring Data JPA 与 Hibernate
- MySQL Driver
- Flyway，用于 MySQL 版本控制
- Druid，用于连接池与 SQL 防火墙
- Springdoc OpenAPI，用于 API 文档生成
- Lombok，用于简化样板代码
- Jackson，用于 JSON 处理
- MapStruct，用于分层对象转换
- Apache Commons，用于常用工具能力
- Logback，用于日志管理

## 前端技术栈

- Vue 3
- TypeScript
- Vite
- ESLint
- Prettier
- Axios
- Axios Retry
- Vue Query
- Vue Router
- Pinia
- Pinia Plugin Persistedstate
- Vue Devtools

## 默认架构要求

- 后端应保持清晰的分层结构。
- 前端应拆分路由、状态管理、接口访问与界面展示职责。
- API 契约应明确，并与生成的接口文档保持一致。
- 身份认证与权限控制应作为整体设计的一部分，而不是事后补丁式接入。
