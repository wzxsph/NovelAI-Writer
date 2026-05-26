---
name: scene-tracking
description: 场景状态追踪规则
---

# 场景状态追踪规则

## 场景定义

场景是故事发生的特定地点，每个场景有其：
- 物理位置
- 环境特征
- 动态状态
- 关联角色
- 关键物品

## 场景状态字段

```json
{
  "scene_id": "scene_001",
  "name": "天剑宗大殿",
  "location": "天剑山",
  "environment": "庄严、肃穆、大殿中央有祭台",
  "dynamic_state": "热闹/冷清/危险/平静",
  "associated_characters": ["char_001", "char_002"],
  "key_items": ["item_001"],
  "important_events": []
}
```

## 状态变化规则

### 场景切换
- 角色进入新场景 → 更新 `current_scene` 字段
- 记录时间线事件

### 场景动态状态变化
- 事件影响场景 → 更新 `dynamic_state`
- 例：战斗发生 → `dynamic_state: "危险"`

### 场景关联变化
- 角色进入/离开 → 更新 `associated_characters`
- 物品放置/取走 → 更新 `key_items`

## 追踪原则

1. 每章至少记录一次场景状态
2. 场景切换必须有时间戳
3. 场景变化必须有事件原因
4. 场景状态必须与角色位置一致

## 一致性检查

场景一致性检查维度：
- 角色当前位置是否与场景匹配
- 场景动态状态是否合理
- 场景物品是否存在
