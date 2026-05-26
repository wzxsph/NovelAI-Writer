---
name: item-chain-rules
description: 物品所有权链规则
---

# 物品所有权链规则

## 物品记录

```json
{
  "item_id": "item_001",
  "name": "玄铁剑",
  "category": "武器",
  "description": "描述",
  "current_owner": "char_001",
  "previous_owners": ["char_002", "char_003"],
  "origin": "天剑宗传承",
  "abilities": ["锋利", "坚韧"],
  "limitations": ["需真气驱动"],
  "events": [
    {"chapter": 1, "event": "char_002 获得"},
    {"chapter": 5, "event": "char_003 继承"},
    {"chapter": 10, "event": "char_001 获得"}
  ]
}
```

## 所有权链规则

1. **物品获取**：记录 `current_owner` 变化
2. **物品转移**：从 `previous_owners` 移除旧拥有者
3. **物品丢失**：记录丢失事件和原因
4. **物品销毁**：标记为 `destroyed: true`

## 追踪原则

1. 每章物品所有权变化必须记录
2. 物品转移必须有明确事件
3. 物品当前持有者必须与场景匹配
4. 物品存在性必须在合理范围内

## 一致性检查

物品检查维度：
- item_possession（当前持有者）
- 物品转移链完整性
- 物品能力使用合理性
- 物品状态变化

## 特殊物品

关键物品（主角相关、核心剧情相关）：
- 额外记录 `significance: high`
- 追踪更多细节
- 一致性检查优先级更高
