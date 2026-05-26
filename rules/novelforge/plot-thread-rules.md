---
name: plot-thread-rules
description: 情节线状态机规则
---

# 情节线状态机规则

## Plot Thread 定义

每个情节线（plot_thread）代表一条独立的故事线：

```json
{
  "thread_id": "pt_001",
  "name": "主角复仇线",
  "description": "描述",
  "status": "active|dormant|completed|abandoned",
  "started_at_chapter": 1,
  "resolved_at_chapter": null,
  "key_events": [],
  "foreshadowing": []
}
```

## 状态定义

| Status | Meaning | Transition |
|--------|---------|------------|
| active | 正在推进 | → dormant（暂时搁置）或 → completed（解决） |
| dormant | 暂时搁置 | → active（重新唤醒） |
| completed | 已解决 | 不可逆 |
| abandoned | 已放弃 | 不可逆 |

## 状态转换规则

1. **新情节线** → 自动设为 `active`
2. **情节线完成** → 满足结局条件后标记 `completed`
3. **情节线搁置** → 超过3章无进展 → 设为 `dormant`
4. **情节线唤醒** → 新事件触发 dormant 线程 → 设为 `active`
5. **情节线放弃** → 明确放弃时标记（需用户确认）

## 追踪原则

1. 每章至少更新一次情节线状态
2. 情节线状态变化必须有事件支持
3. Active 情节线必须有明确进展
4. Dormant 情节线必须有明确的唤醒条件

## 一致性检查

情节线检查维度：
- plot_thread_progress（情节推进）
- 状态转换合理性
- 伏笔回收检查
- 情节线交叉引用
