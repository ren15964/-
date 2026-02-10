---
name: db-design
description: Database design conventions for reusable graduation projects. Use when designing tables, fields, indexes, relationships, soft delete, and migration SQL.
---

## 目标（通用且好扩展）

- 表结构适配多题材：管理系统/商城/预约/社区/教务
- 统一命名与通用字段，便于代码生成与复用
- 核心查询有索引，列表页性能不崩

## 命名规范

- **库/表/字段**：全小写 + 下划线
  - 表：`user`, `role`, `user_role`, `order_item`
  - 字段：`create_time`, `update_time`, `is_deleted`
- **枚举/状态**：用 `tinyint` 或 `varchar`（推荐 `tinyint` + 注释）

## 每张业务表建议包含（通用字段）

- `id` BIGINT 主键，自增或雪花（按项目选）
- `create_time` DATETIME 创建时间
- `update_time` DATETIME 更新时间
- `is_deleted` TINYINT 逻辑删除（0/1）

可选通用字段：

- `create_by` BIGINT（创建人）
- `update_by` BIGINT（更新人）
- `remark` VARCHAR(255)

## 字段类型建议

- 主键：BIGINT
- 金额：DECIMAL(10,2)（禁止 FLOAT/DOUBLE 表金额）
- 时间：DATETIME（统一时区 Asia/Shanghai）
- 文本：VARCHAR（长度按业务），长文本用 TEXT

## 索引策略（最低要求）

- 列表页常用筛选条件必须建索引（如 `status`, `type`, `create_time`）
- 外键字段（逻辑关联）建议建索引（如 `user_id`, `order_id`）
- 唯一约束：账号/手机号/学号/工号等用 UNIQUE

## 关系建模（建议）

- 1:N：子表持有 `parent_id`
- N:M：中间表 `a_b`，至少包含 `a_id`, `b_id` + 通用字段（可选）
- 尽量少用物理外键（便于迁移与本地导入），业务一致性在 Service 层保证

## SQL 输出要求（可复用）

- 输出 `database.sql` 或 `xxx-init.sql`：
  - 建库（可选）+ 建表 + 索引 + 初始化数据（至少 admin 用户/基础角色）
- 每个字段写清楚 `COMMENT`，便于答辩讲解
