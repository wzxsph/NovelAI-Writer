---
description: 状态数据结构规范 — 定义角色、物品、事件等状态的数据结构模板
---

# 状态数据结构规范

## 概述

状态数据存储在用户工作目录的 `state/` 目录下，按类型分散存储。本规范定义各状态实体的数据结构模板。

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

## 通用字段

所有状态实体都包含以下字段：

```json
{
  "id": "char_001",
  "name": "角色名称",
  "created_at": "2026-05-27T10:00:00Z",
  "updated_at": "2026-05-27T12:00:00Z",
  "last_updated_chapter": 5,
  "schema_version": "1.0"
}
```

## 角色状态 (Character)

```json
{
  "id": "char_001",
  "name": "张三",
  "role": "protagonist",
  "core_identity": {
    "appearance": "外貌描述",
    "personality": "性格特点",
    "background": "背景故事",
    "goals": ["目标1", "目标2"],
    "secrets": ["秘密1"]
  },
  "current_state": {
    "location": {
      "current": "当前位置",
      "previous": "之前位置",
      "changed_at_chapter": 3
    },
    "emotional_state": "开心",
    "physical_state": "良好",
    "possessions": ["item_001", "item_002"],
    "abilities": ["技能1", "技能2"],
    "cultivation_realm": "筑基期"
  },
  "relationship_map": {
    "char_002": {
      "target_id": "char_002",
      "relationship_type": "师徒",
      "strength": 8,
      "updated_at_chapter": 5
    }
  },
  "timeline": [
    {
      "chapter": 1,
      "event": "出场",
      "timestamp": "2026-05-27T10:00:00Z"
    }
  ]
}
```

## 物品状态 (Item)

```json
{
  "id": "item_001",
  "name": "玄天剑",
  "type": "法宝",
  "description": "一把锋利的宝剑",
  "current_holder": "char_001",
  "previous_holders": ["char_003"],
  "abilities": ["剑气", "破甲"],
  "limitations": ["每天只能用3次"],
  "current_state": "完好",
  "significant_events": [
    {"chapter": 1, "event": "获得"},
    {"chapter": 5, "event": "升级"}
  ]
}
```

## 事件状态 (Event)

```json
{
  "id": "event_001",
  "title": "宗门大比",
  "chapter": 5,
  "timestamp": "2026-05-27T10:00:00Z",
  "location": "天剑宗比武场",
  "participants": ["char_001", "char_002"],
  "related_items": ["item_001"],
  "plot_thread_id": "plot_001",
  "description": "描述",
  "consequences": ["主角获胜", "获得法宝"]
}
```

## 状态文件路径

```
state/
├── characters/{id}.json       # 角色状态
├── items/{id}.json           # 物品状态
├── timeline/event_{id}.json   # 时间线事件
├── scenes/{id}.json          # 场景状态
├── organizations/{id}.json   # 组织状态
├── concepts/{id}.json        # 概念状态（功法、规则等）
├── plot_threads/{id}.json    # 情节线状态
├── relationships/{id}.json   # 关系状态
├── chapters/chapter_{N}.json # 章节记录
├── outline/outline.json      # 整体大纲
├── outline/volume_{N}.json   # 分卷大纲
└── outline/chapter_{N}.json  # 章节大纲
```

## Schema 版本管理

- 每个状态文件包含 `schema_version` 字段
- 大版本升级时创建新的 Schema 文件
- 迁移时提供数据迁移脚本

## 关联

- 参考: `@rules/novelforge/state-constraints.md` (状态更新约束)
- 使用: `@agents/state-extractor.md` (状态提取)
- 校验: `@agents/consistency-guardian.md` (一致性校验)
