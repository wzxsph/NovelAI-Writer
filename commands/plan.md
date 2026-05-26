---
description: 规划小说大纲、结构和情节
---

# /plan

规划小说的大纲、结构和情节线。

## 参数

- `--type`: 规划类型
  - `outline` — 整体大纲
  - `volume` — 分卷大纲
  - `chapter` — 章节大纲
- `--scope`: 范围（如 `1-30` 表示第1-30章）
- `--content`: 规划内容描述

## 执行流程

1. **分析项目状态**：读取当前 state/ 目录下的所有状态文件
2. **雪花创作法**（首次规划）：参考 @workflows/雪花创作法.md
3. **生成大纲**：参考 @prompts/大纲生成.txt
4. **保存状态**：将规划结果保存到对应的 state 文件

## 输出

- 文本大纲（显示给用户）
- 状态文件更新（如 outline.json、chapters/{N}/outline.json）

## 首次规划使用雪花创作法

若项目尚未建立大纲，引导用户完成雪花创作法流程：
1. 作品标签 → 2. 金手指 → 3. 一句话简介 → 4. 一段话大纲 → 5. 故事大纲 → 6. 世界观设定 → 7. 核心蓝图

## 示例

```
/plan --type outline
/plan --type volume --scope 1-30
/plan --type chapter --scope 5
/plan --type chapter --scope 5 --content "林小雨和陆尘的感情升温"
```
