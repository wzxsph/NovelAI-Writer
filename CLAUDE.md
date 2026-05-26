# novelai-writer 学习文档

本项目是 novelai-writer 的项目级学习文档，参考 ECC (Everything Claude Code) 架构模式构建。

## 项目概述

novelai-writer 是一个基于 Claude Code 的网文长篇写作助手插件系统。参考 ECC 架构模式和 NovelForge 的设计理念，通过十维一致性校验框架，管理角色、物品、情节线和关系网络，确保数百章的内容连贯一致。

核心能力：
- **状态追踪** — 自动维护角色、物品、事件状态
- **上下文一致性** — 十维一致性校验引擎
- **层级上下文注入** — P0-P3 优先级注入，确保长篇连贯
- **增量状态更新** — 续写后自动提取并更新状态
- **多智能体协作** — 规划/写作/校验分离

## Tech Stack

| Layer | Technology |
|-------|-----------|
| 格式 | Markdown + YAML frontmatter |
| 插件系统 | ECC (Everything Claude Code) |
| 状态存储 | JSON (`state_document.json`) |
| 内容语言 | 中文（网文内容） |

## 目录结构

```
./
├── CLAUDE.md              # 本文件，项目引导
├── agents/                # 5 个智能体
├── commands/              # 6 个命令
├── skills/                # 4 个技能
├── rules/                 # 规则文档
│   ├── common/           # 5 个通用规则
│   └── novelforge/       # NovelForge 特定规则
├── contexts/              # 3 个模式定义
├── docs/                  # 详细指南
├── prompts/               # 7 个中文提示词模板
├── state/                 # state_document.json 模板
└── examples/              # 示例
```

## 核心概念

### P0-P3 层级上下文

生成章节时注入的优先级顺序：

| 优先级 | 名称 | 内容 |
|--------|------|------|
| P0 | Hard Constraints | 核心设定、世界规则 |
| P1 | Current State | 当前位置、情绪、物品 |
| P2 | Near Context | 最近3章事件 |
| P3 | Distant Reference | 背景、伏笔 |

### 十维一致性

1. character_identity - 角色外貌/性格
2. character_location - 位置一致性
3. temporal_sequence - 时间顺序
4. item_possession - 物品归属
5. ability_usage - 能力使用
6. relationship_logic - 关系逻辑
7. plot_thread_progress - 情节推进
8. world_rule_compliance - 世界规则
9. emotional_continuity - 情感过渡
10. factual_contradiction - 事实矛盾

## 关键命令

| 命令 | 说明 |
|------|------|
| `/novelai init` | 初始化工作区 |
| `/novelai continue --chapter N` | 续写章节 |
| `/novelai verify --chapter N` | 校验一致性 |
| `/novelai character add --name "张三"` | 添加角色 |
| `/novelai worldbuild` | 构建世界观 |

## 文件命名约定

- **文件命名**: kebab-case（`novelai-continue.md`、`state-extractor.md`）
- **YAML frontmatter**: 所有 agents 和 skills 必须有 `name` + `description` 字段
- **维度命名**: 英文 snake_case（`character_identity`、`temporal_sequence`）

## 参考文档

- [架构概述](docs/architecture/overview.md)
- [后端架构](docs/backend/overview.md)
- [前端架构](docs/frontend/overview.md)
- [工作流系统](docs/workflow/overview.md)
- [卡牌系统](docs/novel/card-system.md)
- [一致性校验](docs/novel/consistency.md)
- [v0 旧文档](docs/v0_README.md)
- [NovelForge 官方项目](../NovelForge/)

## 参考

- [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) — 架构参考
- [NovelForge](https://github.com/RhythmicWave/NovelForge) — 原始项目
