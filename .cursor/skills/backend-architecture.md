---
name: backend-architecture
description: Backend architecture conventions for Spring Boot + MyBatis Plus graduation projects. Use when building backend modules, controllers/services/mappers/entities, auth(JWT), unified result, exception handling, pagination/search, and code organization.
---

## 目标（可复用“毕设后端底座”）

- **分层清晰**：Controller / Service / Mapper / Entity + DTO(入参) + VO(出参)
- **统一返回**：`Result<T>` + 分页 `PageResult<T>`
- **统一异常**：全局异常处理（业务/参数/未知）
- **鉴权可讲清**：JWT 登录 + 拦截器/过滤器校验，预留角色/权限扩展点
- **列表必可用**：分页、排序、条件搜索一套标准化

## 目录与命名约定

- **包结构**（建议）：`common`, `config`, `controller`, `service`, `service.impl`, `mapper`, `entity`, `dto`, `vo`, `exception`, `interceptor`, `utils`
- **命名**：
  - Controller：`XxxController`
  - Service：`XxxService` / `XxxServiceImpl`
  - Mapper：`XxxMapper`
  - DTO：`XxxCreateDTO` / `XxxUpdateDTO` / `XxxQueryDTO`
  - VO：`XxxVO` / `XxxDetailVO`

## 接口实现规范（必须）

- **Controller 只做**：参数接收/校验、鉴权注解或拦截器依赖、调用 Service、返回 Result
- **Service 只做**：业务规则、事务、组合查询、权限判断（最小可用）
- **Mapper 只做**：数据访问（MyBatis Plus + 必要的 XML/自定义 SQL）

## 分页/搜索（统一模式）

- 列表接口统一支持：
  - `pageNum`（从 1 开始）、`pageSize`
  - `sortBy`、`sortOrder`（asc/desc）
  - 业务字段条件（like/eq/range）
- Service 使用 MyBatis Plus 的 `Page<>` + `LambdaQueryWrapper`（优先）

## 校验与错误处理

- DTO 使用 `javax.validation`（`@NotNull`、`@NotBlank`、`@Size`…）
- Controller 方法入参配合 `@Validated`
- 业务错误抛 `BusinessException(code, message)`，由 `GlobalExceptionHandler` 统一转为 `Result.fail`

## 认证鉴权（最小可用且可扩展）

- **登录**：账号密码校验 → 生成 JWT（包含 userId、role 等最小信息）
- **拦截**：请求 Header `Authorization: Bearer <token>`，校验通过后把 userId/role 写入上下文（ThreadLocal 或 request attribute）
- **权限**：
  - 最小：按角色限制路由
  - 进阶：资源权限（菜单/按钮）可后续加表扩展

## 数据库通用字段建议

- `id`（BIGINT 主键）
- `create_time`、`update_time`（DATETIME）
- `is_deleted`（TINYINT 逻辑删除）

## 事务与一致性

- 涉及多表写入、库存/容量扣减等逻辑必须 `@Transactional`
- 并发热点（如抢课/抢购）先给出最小可运行实现，再逐步加锁/乐观锁/Redis

## 禁止事项（踩坑清单）

- 禁止 Controller 里写复杂业务/SQL
- 禁止接口返回裸对象/Map（统一 `Result<T>`）
- 禁止无分页的大列表接口（除非明确限制返回条数）
- 禁止把密码明文存储（必须 BCrypt）
