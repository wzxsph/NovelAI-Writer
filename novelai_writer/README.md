# NovelAI Writer

基于 Claude Code 的网文长篇写作助手插件，参考 [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) 系统设计。

## 功能

- **状态追踪**：自动维护角色、物品、事件状态
- **上下文一致性**：十维一致性校验
- **层级上下文注入**：P0-P3 优先级注入
- **增量状态更新**：续写后自动提取并更新状态
- **多智能体协作**：规划/写作/校验分离

## 安装

### 方式一：作为插件安装

```bash
/plugin marketplace add https://github.com/你的用户名/novelai-writer
/plugin install novelai-writer
```

### 方式二：手动安装

```bash
# 复制到项目目录
cp -r novelai_writer /path/to/your/project/

# 或复制到用户目录
mkdir -p ~/.claude/skills/novelai_writer
cp -r novelai_writer/* ~/.claude/skills/novelai_writer/
```

## 使用

### 命令方式

```bash
/novelai init                           # 初始化工作区
/novelai worldbuild                     # 构建世界观
/novelai character add --name "张三"     # 添加角色
/novelai continue --chapter 1           # 续写章节
/novelai verify --chapter 1              # 校验一致性
/novelai status                         # 查看状态
```

### Agent 方式

```bash
/novelai agent planner                 # 规划大纲
/novelai agent world-builder           # 构建世界观
/novelai agent writer --chapter 1      # 写作
/novelai agent consistency-guardian    # 一致性校验
```

## 架构

- **agents/** — 5个智能体（planner, world-builder, writer, state-extractor, consistency-guardian）
- **commands/** — 6个命令文件
- **skills/** — 4个技能定义
- **rules/** — 写作规则
- **prompts/** — 7个详细 prompt 模板（参考 NovelForge）
- **state/** — 状态文档模板

## 核心概念

### 状态文档

`.novelai_writer/state_document.json` 是所有状态的单一真相来源。

### P0-P3 上下文

| 优先级 | 名称 | 内容 |
|--------|------|------|
| P0 | Hard Constraints | 核心角色定义、世界规则 |
| P1 | Current State | 当前状态 |
| P2 | Near Context | 最近3章 |
| P3 | Distant Reference | 背景设定 |

### 十维一致性

1. character_identity
2. character_location
3. temporal_sequence
4. item_possession
5. ability_usage
6. relationship_logic
7. plot_thread_progress
8. world_rule_compliance
9. emotional_continuity
10. factual_contradiction

## 参考

- [Everything Claude Code](https://github.com/affaan-m/ECC) — ECC 系统
- [NovelForge](../NovelForge/backend/app/bootstrap/prompts/) — prompt 模板
