---
description: 网文长篇写作助手 — 管理角色、物品、事件状态，保证长篇一致性
---

# /novelai

网文长篇写作助手，管理角色、物品、事件状态，保证长篇一致性。

## 子命令

### 初始化与状态
- `/init --template snowflake` — 初始化工作区
- `/status` — 查看项目状态概览

### 规划
- `/plan --type outline` — 整体大纲规划
- `/plan --type volume --scope 1-30` — 分卷大纲
- `/plan --type chapter --scope 5` — 章节大纲

### 角色管理
- `/character add --name "张三" --role protagonist` — 添加角色
- `/character list` — 列出所有角色
- `/character update --name "张三" --field emotional_state --value "开心"` — 更新角色状态

### 章节创作
- `/continue --chapter 5 --words 3000` — 续写章节
- `/continue --chapter 5 --mode polish` — 润色章节
- `/continue --chapter 5 --mode expand` — 扩写大纲为正文
- `/verify --chapter 5 --scope all` — 校验章节一致性
- `/chapters --range 1-30` — 列出章节概览

### 状态查看
- `/timeline --chapter 5` — 事件时间线
- `/items --type 法宝` — 物品设定
- `/plot --status active` — 情节线状态
- `/scenes --character "林小雨"` — 场景列表
- `/organizations --type 门派` — 组织势力
- `/concepts --type 功法` — 概念设定

### 世界观构建
- `/worldbuild` — 构建世界观设定

## 状态存储

状态文件分布在 `state/` 目录下：
- `metadata/project.json` — 项目元数据
- `characters/` — 角色状态文件
- `items/` — 物品状态文件
- `scenes/` — 场景状态文件
- `organizations/` — 组织状态文件
- `concepts/` — 概念状态文件
- `timeline/` — 时间线事件文件
- `plot_threads/` — 情节线状态文件
- `relationships/` — 关系状态文件
- `chapters/` — 章节记录文件

## 工作流程

1. `/init` 初始化工作区
2. `/plan --type outline` 创建整体大纲
3. `/worldbuild` 构建世界观
4. `/character add` 添加初始角色
5. `/continue --chapter 1` 生成第一章
6. `/verify` 校验一致性
7. 循环续写直到完本
