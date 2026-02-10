# skills 目录使用说明（本仓库约定）

这里存放可复用的“技能规范”（skills），用于让 AI 在特定类型任务中自动采用对应的质量标准。

## 目录结构

- `.cursor/skills/frontend-design.md`
  - 用于：前端页面/组件/UI 美化与风格重构
  - 目标：避免模板化“AI 审美”，做出更有设计感、可上线的界面

## 如何触发（无需你每次说明）

本仓库的 `.cursorrules` 已约定：

- 只要需求属于“做页面/写组件/美化 UI/调整样式/做 landing/dashboard”，就自动启用 `frontend-design`。

如果你希望强制指定某个 skill，也可以在需求里写：

- “请按 `frontend-design` skill 的规范来做”

