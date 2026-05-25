---
description: 网文长篇写作助手 — 管理角色、物品、事件状态，保证长篇一致性
---

# /novelai

网文长篇写作助手，管理角色、物品、事件状态，保证长篇一致性。

## 子命令

- `/novelai init` — 初始化工作区，创建状态文档 `.novelai_writer/state_document.json`
- `/novelai status` — 查看当前状态概览（角色数/物品数/时间线长度）
- `/novelai character add --name "张三" --role protagonist` — 添加新角色
- `/novelai character list` — 列出所有角色及状态
- `/novelai character update --name "张三" --emotional_state "悲伤"` — 更新角色状态
- `/novelai timeline` — 查看事件时间线
- `/novelai items` — 列出物品/设定
- `/novelai plot` — 列出情节线状态
- `/novelai continue --chapter 5 --previous "chapters/chapter_04.txt"` — 续写章节
- `/novelai verify --chapter 5` — 校验章节一致性
- `/novelai worldbuild` — 根据用户描述的世界观生成设定文档

## 状态文档

所有状态存储在 `.novelai_writer/state_document.json`，结构：

```json
{
  "version": "1.0",
  "last_updated_chapter": 0,
  "metadata": { "title": "", "genre": "", "style": "" },
  "characters": [...],
  "timeline": [...],
  "items": [...],
  "plot_threads": [...],
  "relationships": [...]
}
```

## 工作流程

1. `/novelai init` 初始化工作区
2. `/novelai worldbuild` 构建世界观
3. `/novelai character add` 添加初始角色
4. `/novelai continue --chapter 1` 生成第一章
5. `/novelai verify` 校验一致性
6. 循环续写直到完本
