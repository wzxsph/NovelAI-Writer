# 状态约束规则

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

- `current_holder` 必须存在于 `characters` 目录中
- 物品转移时必须同时更新旧持有者的 `possessions`
- `significant_events` 最多保留3条，超出时删除最早的

### 事件状态约束

- `chapter` 编号必须单调递增
- `plot_thread_id` 必须关联已存在的情节线

## ID 命名规则

| 类型 | 格式 | 示例 |
|------|------|------|
| 角色 | `char_NNN` | `char_001` |
| 物品 | `item_NNN` | `item_002` |
| 事件 | `event_NNN` | `event_003` |
| 场景 | `scene_NNN` | `scene_004` |
| 组织 | `org_NNN` | `org_001` |
| 情节线 | `plot_NNN` | `plot_001` |
| 关系 | `rel_NNN` | `rel_001` |

## 时间戳规范

- 所有时间字段使用 ISO 8601 格式
- 写作时使用模拟时间（如 "修仙纪元第XXX年"）
- `updated_at` 由系统自动更新，不可手动修改

## 状态文件路径规范

```
state/
├── characters/{id}.json       # 角色状态
├── items/{id}.json           # 物品状态
├── timeline/event_{id}.json  # 时间线事件
├── scenes/{id}.json          # 场景状态
├── organizations/{id}.json   # 组织状态
├── concepts/{id}.json        # 概念状态
├── plot_threads/{id}.json    # 情节线状态
├── relationships/{id}.json   # 关系状态
├── chapters/chapter_{N}.json # 章节记录
├── outline/outline.json      # 整体大纲
├── outline/volume_{N}.json   # 分卷大纲
└── outline/chapter_{N}.json  # 章节大纲
```

## Schema 版本管理

- 每个状态文件包含 `schema_version` 字段
- 大版本升级时创建新的 Schema 文件（如 `character.schema.v2.json`）
- 迁移时提供数据迁移脚本

## 一致性检查

每次状态更新后自动执行以下检查：

1. **角色存在性**：引用 `current_holder` 的物品，其 holder 必须在 `characters/` 中存在
2. **ID 格式合规性**：所有 ID 必须符合命名规则
3. **章节连续性**：事件和章节记录的 `chapter` 必须单调递增
4. **时间戳顺序**：`updated_at` 不可早于 `created_at`
