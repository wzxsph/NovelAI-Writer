---
description: 校验章节一致性，检测角色/物品/时间线矛盾
---

# /novelai verify

校验章节一致性，检测角色状态、物品归属、时间线等矛盾。

## 参数

- `--chapter`: 章节编号（必填）
- `--file`: 章节文件路径（若不提供则使用 chapters/chapter_{N}.txt）

## 校验维度

| # | Dimension | Check |
|---|-----------|-------|
| 1 | character_identity | 角色外貌/性格一致性 |
| 2 | character_location | 角色不能同时在两地 |
| 3 | temporal_sequence | 事件时间顺序正确 |
| 4 | item_possession | 物品持有链连续 |
| 5 | ability_usage | 能力使用正确 |
| 6 | relationship_logic | 关系与事件匹配 |
| 7 | plot_thread_progress | 情节线状态推进 |
| 8 | world_rule_compliance | 世界规则遵守 |
| 9 | emotional_continuity | 情感状态过渡自然 |
| 10 | factual_contradiction | 无事实矛盾 |

## 输出格式

```
## 一致性校验报告

### 通过 ❌/✅

#### Violations

- **[CRITICAL]** dimension: description
  - Location: 章节位置
  - Suggestion: 修复建议

- **[WARNING]** dimension: description
  - Location: 章节位置
  - Suggestion: 修复建议

#### 通过项 ✅
- dimension1
- dimension2
```

## 严重级别

- **critical**: 必须修复
- **warning**: 建议检查
- **info**: 仅供参考
