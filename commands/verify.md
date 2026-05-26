---
description: 校验章节一致性并保存审阅报告
---

# /verify

校验章节一致性，检测角色状态、物品归属、时间线等矛盾。

## 参数

- `--chapter`: 章节编号（必填）
- `--file`: 章节文件路径（若不提供则使用 chapters/chapter_{N}.txt）
- `--scope`: 校验范围（默认 `all`，可选 `basic`/`extended`/`all`）
  - `basic`: 基础十维校验
  - `extended`: 扩展校验（包含场景、组织、概念一致性）
  - `all`: 完整校验

## 执行流程

1. **读取正文**：从 `chapters/chapter_{N}.txt` 读取
2. **读取状态**：从 `state/` 目录读取相关状态文件
3. **执行校验**：按照十维框架逐项检查
4. **生成报告**：输出格式化的审阅报告
5. **保存报告**：⚠️ 必须保存到 `state/chapters/chapter_{N}/review.json`

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
| 11 | scene_consistency | 场景描述一致性 |
| 12 | organization_logic | 组织架构逻辑 |
| 13 | concept_definition | 概念定义一致性 |

## 输出格式

```
## 一致性校验报告

### 第{N}章 通过 ❌/✅

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

## 保存要求 ⚠️

**必须将审阅报告保存到文件**：
- 报告路径：`state/chapters/chapter_{N}/review.json`
- 使用 Write 工具保存，不能仅显示在对话中
- 如果目录不存在，先创建 `state/chapters/chapter_{N}/` 目录

## 严重级别

| 级别 | 说明 | 处理方式 |
|------|------|----------|
| **critical** | 必须修复 | 使用 `/revise --chapter N` 修复 |
| **warning** | 建议检查 | 使用 `/revise --chapter N --fix dimension` 修复 |
| **info** | 仅供参考 | 可忽略 |

## 下一步

发现问题后，使用 `/revise --chapter N` 修复：

```bash
/revise --chapter 5
/revise --chapter 5 --fix character_identity
```

## 示例

```
/verify --chapter 5
/verify --chapter 5 --scope extended
/verify --chapter 5 --file chapters/chapter_5.txt
```
