---
description: 初始化网文写作工作区
---

# /init

初始化网文写作工作区，创建分布式状态文件。

## 参数

- `--template`: 模板类型（默认 `default`，可选 `snowflake`）
  - `default`: 空白项目，从零开始
  - `snowflake`: 雪花创作法模板，带有引导问题

## 执行步骤

1. 创建 `state/` 目录结构
2. 创建 `state/metadata/project.json` 元数据文件
3. 询问用户项目基本信息：
   - 小说标题
   - 题材类型（都市/玄幻/穿越/科幻/其他）
   - 文风偏好
   - 作者名（可选）
4. 更新 `state/metadata/project.json`
5. **若使用 `--template snowflake`**: 参考 `@workflows/雪花创作法.md` 引导用户完成创作流程

## 状态目录结构

```
state/
├── metadata/
│   └── project.json          # 项目元数据
├── outline/                  # 大纲目录
│   └── outline.json         # 整体大纲
├── characters/              # 角色状态（每角色一个文件）
├── items/                    # 物品状态（每物品一个文件）
├── scenes/                  # 场景状态（每场景一个文件）
├── organizations/           # 组织状态（每组织一个文件）
├── concepts/                # 概念状态（每概念一个文件）
├── timeline/                 # 时间线事件（每事件一个文件）
├── plot_threads/            # 情节线状态（每情节线一个文件）
├── relationships/           # 关系状态（每关系一个文件）
└── chapters/                # 章节记录（每章节一个文件）
```

## 初始化后的下一步

1. `/plan --type outline` — 创建整体大纲
2. `/worldbuild` — 构建世界观
3. `/character add` — 添加主要角色
4. `/continue --chapter 1` — 开始撰写第一章
