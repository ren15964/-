---
name: scaffold-from-zero
description: Fully automated from-scratch scaffolding for graduation projects. Use when user asks to initialize a new project/repo, generate backend+frontend skeletons, base auth, docs, and runnable demo.
---

## 目标（从 0 到 1：可跑、可演示、可扩展）

当用户说“从零搭建项目/初始化毕设/搭底座”时，必须交付：

- **后端工程**：Spring Boot + Maven + MyBatis Plus + MySQL（可选 Redis）可启动
- **前端工程**：Vue3 + Vite + Element Plus + Pinia + Router + Axios 可启动
- **最小闭环**：登录（JWT）→ 访问受保护接口 → 前端鉴权路由
- **统一规范**：Result、异常处理、分页协议、请求封装
- **文档**：README（启动命令）+ 初始 SQL + API 文档骨架

## 后端初始化必须包含

- `common/Result`, `common/PageResult`, `common/ResultCode`（或等价）
- `exception/BusinessException`, `exception/GlobalExceptionHandler`
- `utils/JwtUtil`, `utils/PasswordUtil(BCrypt)`
- `config/CorsConfig`
- `interceptor/AuthInterceptor`（校验 JWT）
- 示例模块：
  - `AuthController`: login、me
  - `UserController`: demo 的受保护接口（可返回当前用户信息）

## 前端初始化必须包含

- `src/utils/request.js`：axios 封装
  - 自动注入 token
  - 401 自动退出并跳转登录
  - 统一错误提示（Element Plus Message）
- `src/store/user.js`：token/userInfo（localStorage 持久化）
- `src/router/index.js`：路由守卫（未登录拦截）
- 页面：
  - `Login.vue`（可登录）
  - `Home.vue`（登录后可见）
  - `Forbidden.vue`、`NotFound.vue`
- 布局：
  - `Layout.vue`（侧边栏/顶部栏/内容区）

## 数据库初始化（通用）

- 输出 `database.sql`：
  - 用户表（至少 admin）
  - 角色表/用户角色表（可选，但推荐预留）
  - 必要字段注释、唯一索引

## README 必须写清楚

- 环境要求（JDK/Node/MySQL）
- 一键启动命令（Windows 也能照抄）
- 默认账号密码

## 验收标准

- 前端 `npm run dev` 能打开登录页
- 后端能启动并连上数据库（或提供本地 mock 兜底方案）
- 登录后能拿到 token，访问需要 token 的接口成功
