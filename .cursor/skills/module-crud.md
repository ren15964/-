---
name: module-crud
description: End-to-end module CRUD generation for graduation projects. Use when the user asks to add a new module: SQL -> backend CRUD (pagination/search/validation/auth) -> frontend admin page (search+table+pagination+dialogs) with optional frontend-design polishing.
---

## 目标（新增模块一条龙）

当用户说“新增 XX 模块”时，必须按固定顺序交付可运行产物：

1) **数据库**：表设计 + SQL（含通用字段、索引、初始化数据如需要）  
2) **后端**：CRUD + 分页/排序/条件搜索 + 校验 + 最小鉴权  
3) **前端**：标准管理页（搜索区 + 表格 + 分页 + 新增/编辑弹窗 + 删除确认 + loading）  
4) **文档**：同步更新 `API接口文档.md`（接口路径、入参出参、鉴权、示例）

## 交付清单（后端）

- Entity（含注释/MP 注解）
- DTO：Create / Update / Query
- VO：ListVO / DetailVO（按需）
- Controller：遵循 `Result<T>` 统一返回
- Service：业务校验、事务（如多表）
- Mapper：MP + 必要自定义 SQL
- 分页：`pageNum/pageSize` + `sortBy/sortOrder` + 条件筛选

## 交付清单（前端）

- `src/api/xxx.js`：接口封装
- `src/views/admin/XxxManage.vue`（或按路由规划放置）
- Element Plus：
  - 搜索表单（回车搜索、重置）
  - 表格（操作列：编辑/删除）
  - 分页组件
  - 弹窗表单（新增/编辑复用，校验完整）
  - 删除二次确认
  - loading / empty 状态

## 与 `frontend-design` 的关系

- 管理页默认先保证“可用、清晰、好讲”
- 用户说“美化/更有设计感”或页面属于展示/看板类时，再额外套用 `frontend-design`
