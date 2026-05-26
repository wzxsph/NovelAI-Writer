---
description: 启动自动写小说循环
---

# /loop-start

启动自动写小说循环，按照工作流程逐章自动写章节、审阅、修改，直到完成整本小说或整卷。

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

1. **检查前提条件**：
   - 确认 `/plan --type outline` 已完成
   - 确认 `/worldbuild` 已完成
   - 确认至少有角色在 state 中
   - 确认当前章节进度（last_updated_chapter）

2. **循环执行**（每章）：
   ```
   当前章 = last_updated_chapter + 1
   
   循环直到 当前章 > 目标章：
       a. /continue --chapter 当前章
       b. 保存章节到 chapters/chapter_{当前章}.txt
       c. /verify --chapter 当前章
       d. 保存审阅报告到 state/chapters/chapter_{当前章}/review.json
       e. 如果 mode=safe 且有 CRITICAL 问题：
          /revise --chapter 当前章
       f. 更新 last_updated_chapter
       g. 当前章++
   ```

3. **停止条件**：
   - 达到目标章节（`--to` 或 metadata 中的目标字数）
   - 用户手动停止（按 Ctrl+C）
   - 检测到无法修复的问题

## 状态存储

循环状态保存在 `state/metadata/loop_status.json`：

```json
{
  "running": true,
  "started_at": "2026-05-27T00:00:00Z",
  "current_chapter": 5,
  "target_chapter": 30,
  "mode": "safe",
  "completed_chapters": [1, 2, 3, 4],
  "failed_chapters": [],
  "stats": {
    "total_words_written": 12000,
    "chapters_completed": 4,
    "revisions_made": 2
  }
}
```

## 查看状态

使用 `/loop-status` 查看当前循环状态：

```bash
/loop-status
```

## 停止循环

当前章节完成后，循环会自动停止。若需提前停止，请使用 `/loop-stop`（如支持）。

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

## 前提条件检查

执行前请确认：
- [ ] `/init` 已执行
- [ ] `/plan --type outline` 已完成
- [ ] `/worldbuild` 已完成
- [ ] 至少有主角在 state 中
