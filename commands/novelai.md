---
description: 网文长篇写作助手 — 管理角色、物品、事件状态，保证长篇一致性
---

# /novelai

网文长篇写作助手，管理角色、物品、事件状态，保证长篇一致性。

## 子命令

### 初始化与状态
- `/novelai init --template snowflake` — 初始化工作区（可选雪花创作法模板）
- `/novelai status` — 查看项目状态概览

### 角色管理
- `/novelai character add --name "张三" --role protagonist` — 添加角色
- `/novelai character list` — 列出所有角色
- `/novelai character update --name "张三" --field emotional_state --value "开心"` — 更新角色状态
- `/novelai character view --name "张三"` — 查看角色详情

### 章节创作
- `/novelai continue --chapter 5 --words 3000` — 续写章节
- `/novelai continue --chapter 5 --mode polish` — 润色章节
- `/novelai continue --chapter 5 --mode expand` — 扩写大纲为正文
- `/novelai verify --chapter 5 --scope all` — 校验章节一致性
- `/novelai chapters --range 1-30` — 列出章节概览

### 状态查看
- `/novelai timeline --chapter 5` — 查看事件时间线
- `/novelai items --type 法宝` — 列出物品设定
- `/novelai plot --status active` — 列出情节线状态
- `/novelai scenes --character "林小雨"` — 列出场景
- `/novelai organizations --type 门派` — 列出组织势力
- `/novelai concepts --type 功法` — 列出概念设定

### 世界观构建
- `/novelai worldbuild` — 构建世界观设定

## 状态文档

所有状态存储在 `.novelai_writer/state_document.json`，结构：

```json
{
  "version": "2.0",
  "last_updated_chapter": 0,
  "metadata": { "title": "", "genre": [], "style": "", "author": "", "word_count_target": 0, "template": "default" },
  "characters": [],
  "scenes": [],
  "organizations": [],
  "concepts": [],
  "items": [],
  "chapters": [],
  "timeline": [],
  "plot_threads": [],
  "relationships": []
}
```

## 工作流程

1. `/novelai init` 初始化工作区
2. `/novelai worldbuild` 构建世界观
3. `/novelai character add` 添加初始角色
4. `/novelai continue --chapter 1` 生成第一章
5. `/novelai verify` 校验一致性
6. 循环续写直到完本
