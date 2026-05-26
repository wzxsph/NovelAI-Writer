---
description: 校验章节一致性，检测角色/物品/时间线矛盾
---

# /novelai verify

校验章节一致性，检测角色状态、物品归属、时间线等矛盾。

## 参数

- `--chapter`: 章节编号（必填）
- `--file`: 章节文件路径（若不提供则使用 chapters/chapter_{N}.txt）
- `--scope`: 校验范围（默认 `all`，可选 `basic`/`extended`）
  - `basic`: 基础十维校验
  - `extended`: 扩展校验（包含场景、组织、概念一致性）
  - `all`: 完整校验

## 校验维度

### 基础十维（basic/extended/all）

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

### 扩展维度（extended/all）

| # | Dimension | Check |
|---|-----------|-------|
| 11 | scene_consistency | 场景描述一致性（时间、天气、氛围） |
| 12 | organization_logic | 组织架构逻辑（成员关系、等级制度） |
| 13 | concept_definition | 概念定义一致性（功法原理、世界规则） |

## 规则参考

- 场景追踪：`@rules/novelforge/scene-tracking.md`
- 时间线规则：`@rules/novelforge/timeline-rules.md`
- 情节线状态机：`@rules/novelforge/plot-thread-rules.md`
- 物品所有权链：`@rules/novelforge/item-chain-rules.md`

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
