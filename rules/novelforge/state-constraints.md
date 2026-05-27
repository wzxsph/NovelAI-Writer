---
description: 状态更新约束规则 — 定义状态读写、一致性保证、版本管理
---

# 状态更新约束规则

## 状态更新原则

1. **增量更新**：只更新变化的字段，保留未变化的字段
2. **版本检查**：更新前检查 `updated_at` 时间戳，避免旧数据覆盖新数据
3. **章节顺序**：状态更新的 `last_updated_chapter` 必须 >= 当前章节编号

## 约束规则

### 角色状态约束

- `emotional_state` 变化必须有 `chapter` 记录
- `location` 变化必须记录 `previous` 和 `current`
- 物品获得/失去必须同步更新物品的 `current_holder`
- `role` 字段一旦设定不可更改（如需更改需新建角色）

### 物品状态约束

- `current_holder` 必须存在于 `characters/` 中
- 物品转移时必须同时更新旧持有者的 `possessions`
- `significant_events` 最多保留3条，超出时删除最早的

### 事件状态约束

- `chapter` 编号必须单调递增
- `plot_thread_id` 必须关联已存在的情节线

## 一致性检查

每次状态更新后自动执行以下检查：

1. **角色存在性**：引用 `current_holder` 的物品，其 holder 必须在 `characters/` 中存在
2. **ID 格式合规性**：所有 ID 必须符合命名规则
3. **章节连续性**：事件和章节记录的 `chapter` 必须单调递增
4. **时间戳顺序**：`updated_at` 不可早于 `created_at`

## 读取规范

- 读取状态时优先使用最新的 `updated_at` 版本
- 如有版本冲突，保留 `last_updated_chapter` 较大的版本
- 读取失败时返回错误，不使用过期的缓存数据

## 写入规范

- 写入前备份原文件（保持 .bak 后缀）
- 写入后立即验证 JSON 格式正确性
- 写入失败时回滚到备份版本

## 关联

- 定义: `@rules/novelforge/state-schema.md` (状态数据结构)
- 提取: `@agents/state-extractor.md` (状态提取)
- 校验: `@agents/consistency-guardian.md` (一致性校验)
