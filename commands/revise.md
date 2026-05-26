---
description: 根据审阅报告修改章节正文
---

# /revise

根据 `/verify` 命令生成的审阅报告，修改章节正文内容。

## 参数

- `--chapter`: 章节编号（必填）
- `--report`: 审阅报告文件路径（可选，默认使用上次 verify 的结果）
- `--fix`: 只修复指定类型的问题（可选）
  - `character_identity` — 角色一致性
  - `character_location` — 位置一致性
  - `temporal_sequence` — 时间顺序
  - `item_possession` — 物品归属
  - `ability_usage` — 能力使用
  - `relationship_logic` — 关系逻辑
  - `plot_thread_progress` — 情节推进
  - `world_rule_compliance` — 世界规则
  - `emotional_continuity` — 情感过渡
  - `factual_contradiction` — 事实矛盾

## 执行流程

1. **读取审阅报告**：从 `state/chapters/chapter_{N}/review.json` 读取
2. **分析问题**：识别 CRITICAL 和 WARNING 级别的问题
3. **修改正文**：直接修改 `chapters/chapter_{N}.txt`
4. **更新状态**：如有状态变化，更新对应 state 文件
5. **保存报告**：将修复结果保存到 `state/chapters/chapter_{N}/revision_history.json`

## 示例

```
/revise --chapter 5
/revise --chapter 5 --fix character_identity
/revise --chapter 5 --report state/chapters/chapter_5/review.json
```

## 保存要求

⚠️ **强制保存规则**：
- 修改正文后必须保存到 `chapters/chapter_{N}.txt`
- 状态更新后必须保存到对应 state 文件
- 审阅报告必须保存到 `state/chapters/chapter_{N}/review.json`
- 所有保存操作使用 Edit 或 Write 工具，不能仅停留在内存
