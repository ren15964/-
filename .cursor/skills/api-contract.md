---
name: api-contract
description: API contract conventions for graduation projects. Use when designing REST endpoints, request/response DTO/VO, pagination/search params, error codes, auth semantics, and API documentation.
---

## 目标（接口“讲得清、对得上、好联调”）

- 前后端字段一致、语义一致
- 一套分页/排序/筛选的通用约定
- 错误码/鉴权语义明确（401/403/400/500）
- 方便沉淀到 `API接口文档.md`

## REST 资源命名（建议）

- 资源复数：`/api/users`、`/api/books`、`/api/orders`
- 详情：`GET /api/books/{id}`
- 新增：`POST /api/books`
- 修改：`PUT /api/books/{id}`（或 `POST /api/books/update`，但优先 REST）
- 删除：`DELETE /api/books/{id}`（逻辑删除）
- 批量：`POST /api/books/batch-delete`

## 请求/响应对象（强制分离）

- **入参**：DTO（禁止直接用 Entity 作为入参）
- **出参**：VO（禁止直接返回 Entity）
- **返回包装**：统一 `Result<T>`（分页用 `Result<PageResult<T>>`）

## 分页/排序/筛选（统一参数名）

- `pageNum`：从 1 开始
- `pageSize`：每页条数（建议默认 10/20）
- `sortBy`：排序字段（后端需白名单映射，禁止直接拼 SQL）
- `sortOrder`：`asc` | `desc`
- 筛选字段：按业务命名（如 `status`、`keyword`、`startTime`、`endTime`）

分页响应建议：

- `list`: 当前页数据
- `total`: 总条数
- `pageNum`, `pageSize`: 回传便于前端状态一致

## 错误码与 HTTP 状态（建议）

- **200**：业务成功（`Result.code=200`）
- **400**：参数校验失败（`Result.code=400xx`）
- **401**：未登录/Token 失效（前端需跳转登录）
- **403**：无权限（前端提示无权限）
- **500**：未知异常（统一兜底）

约定：后端返回 `Result` 的同时，也尽量设置合适的 HTTP Status（可选，但推荐）。

## 鉴权语义

- 需要登录：接口注释标记“需要 Token”
- 需要角色：标记“仅管理员/教师/学生”
- Header 统一：
  - `Authorization: Bearer <JWT>`

## 文件上传（通用约定）

- 上传：`POST /api/files/upload`（multipart/form-data）
- 返回：`url`、`fileName`、`size`、`contentType`
- 下载：优先返回可访问的 URL；或 `GET /api/files/{id}/download`

## 文档同步（强制）

任何新增/修改接口后：

- 更新 `API接口文档.md`：路径、方法、入参、响应示例、错误码、鉴权说明
