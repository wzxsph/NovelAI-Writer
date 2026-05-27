---
description: 启动自动写小说循环，逐章撰写→审阅→修正→更新状态
---

# /loop-start

启动自动写小说循环，按照工作流程逐章自动写章节、审阅、修改，直到完成整本小说或整卷。

**核心原则**：逐章执行，每章必须完成完整流程：撰写→保存→审阅→修正→状态更新，然后才能进入下一章。

## 参数

- `--scope`: 范围（默认 `all`，可选 `all`/`volume`/`chapter_range`）
  - `all`: 整个项目（所有章节）
  - `volume`: 当前分卷（需配合 `--volume-number`）
  - `chapter_range`: 指定范围（需配合 `--from` 和 `--to`）
- `--volume-number`: 分卷编号（当 scope=volume 时必填）
- `--from`: 起始章节（当 scope=chapter_range 时必填）
- `--to`: 结束章节（当 scope=chapter_range 时必填）
- `--mode`: 模式（默认 `safe`，可选 `safe`/`fast`）
  - `safe`: 每章执行 verify + 如有问题执行 revise
  - `fast`: 直接写下一章，跳过审阅

## 执行流程

### 阶段1：初始化

1. **检查前提条件**：
   - 确认 `/plan --type outline` 已完成（有 chapters 目录）
   - 确认 `/worldbuild` 已完成（有 state/ 目录）
   - 确认至少有角色在 state/characters/ 中
   - 确认当前章节进度（读取 state/metadata/project.json 的 last_updated_chapter）

2. **确定目标章节**：
   - scope=all：从 last_updated_chapter+1 到 metadata.word_count_target 对应章节
   - scope=volume：从指定分卷首章到末章
   - scope=chapter_range：从 --from 到 --to

3. **初始化循环状态**：
   创建 `state/metadata/loop_status.json`：
   ```json
   {
     "running": true,
     "started_at": "ISO时间",
     "current_chapter": 起始章节,
     "target_chapter": 结束章节,
     "mode": "safe|fast",
     "scope": "all|volume|chapter_range",
     "completed_chapters": [],
     "failed_chapters": [],
     "stats": {
       "total_words_written": 0,
       "chapters_completed": 0,
       "revisions_made": 0
     },
     "recent_activity": []
   }
   ```

### 阶段2：逐章循环（每章必须完成完整流程）

```
当前章 = last_updated_chapter + 1

循环直到 当前章 > 目标章节：

  ┌─────────────────────────────────────────────────────────────┐
  │ 第 {当前章} 章 工作流程                                        │
  └─────────────────────────────────────────────────────────────┘

  步骤A：撰写章节
  ─────────────
  a1. 执行 /continue --chapter 当前章 --words 3000
  a2. 验证章节文件已保存到 chapters/chapter_{当前章}.txt
  a3. 验证章节记录已保存到 state/chapters/chapter_{当前章}.json
  a4. 字数不足3000字则要求重写

  步骤B：审阅章节
  ─────────────
  b1. 执行 /verify --chapter 当前章 --scope all
  b2. 验证审阅报告已保存到 state/chapters/chapter_{当前章}/review.json

  步骤C：修正问题（仅 mode=safe 且有 CRITICAL问题时执行）
  ─────────────
  c1. 如果 review.json 中有 CRITICAL 问题：
      - 执行 /revise --chapter 当前章
      - 验证修订历史已保存到 state/chapters/chapter_{当前章}/revision_history.json
      - 修订后重新执行 /verify 确认问题已修复
  c2. 如果所有 CRITICAL 问题已修复，记录 revision_made +1

  步骤D：更新循环状态
  ─────────────
  d1. 更新 loop_status.json：
      - current_chapter = 当前章
      - completed_chapters += [当前章]
      - stats.total_words_written += 实际字数
      - stats.chapters_completed += 1
      - recent_activity 添加 "{当前章}章完成"
  d2. 更新 state/metadata/project.json 的 last_updated_chapter

  步骤E：进入下一章
  ─────────────
  e1. 当前章++
  e2. 如果 当前章 > 目标章节，退出循环

  ┌─────────────────────────────────────────────────────────────┐
  │ 流程结束标志                                               │
  └─────────────────────────────────────────────────────────────┘
```

### 阶段3：结束

1. **更新 loop_status.json**：
   ```json
   {
     "running": false,
     "completed_at": "ISO时间",
     "final_stats": { ... }
   }
   ```

2. **输出总结报告**

## 停止条件

- **正常完成**：当前章 > 目标章节
- **用户手动停止**：按 Ctrl+C（会在当前章完成后停止）
- **检测到无法修复的问题**：连续3次 revise 仍有问题，标记 failed_chapters 并询问用户

## 字数要求

| 模式 | 最少字数 | 说明 |
|------|----------|------|
| write | 3000字 | 新写章节必须达到3000字 |
| expand | 2500字 | 扩写大纲为正文 |
| polish | 2000字 | 润色已有章节 |

**字数不足处理**：章节字数不足时，必须扩写直到达标，否则不进入审阅阶段。

## 状态文件要求

| 文件 | 必须存在 | 用途 |
|------|----------|------|
| `chapters/chapter_{N}.txt` | ✅ | 章节正文 |
| `state/chapters/chapter_{N}.json` | ✅ | 章节记录 |
| `state/chapters/chapter_{N}/review.json` | ✅ | 审阅报告 |
| `state/chapters/chapter_{N}/revision_history.json` | ⚠️ | 仅当有修订时 |
| `state/metadata/loop_status.json` | ✅ | 循环状态 |

## 查看状态

使用 `/loop-status` 查看当前循环状态：

```bash
/loop-status
```

## 前提条件检查

执行前请确认：
- [ ] `/init --template snowflake` 已执行
- [ ] `/plan --type outline` 已完成
- [ ] `/worldbuild` 已完成
- [ ] 至少有主角在 `state/characters/` 中

## 示例

```bash
# 自动写所有章节
/loop-start

# 自动写第1-30章
/loop-start --scope chapter_range --from 1 --to 30

# 快速模式（跳过审阅）
/loop-start --mode fast

# 安全模式（每章审阅+修改）
/loop-start --mode safe
```