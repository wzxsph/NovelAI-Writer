# 入门指南：novelai-writer

> ⚠️ **本项目仍处于测试开发期**，存在诸多不完善之处，欢迎批评指正。

## 概述

novelai-writer 是一个基于 Claude Code 的网文长篇写作助手插件系统。参考 ECC (Everything Claude Code) 架构模式和 NovelForge 的设计理念，通过十维一致性校验框架，管理角色、物品、情节线和关系网络，确保数百章的内容连贯一致。

## 技术栈

| 层级 | 技术 |
|------|------|
| 格式 | Markdown + YAML frontmatter |
| 插件系统 | ECC (Everything Claude Code) |
| 状态存储 | JSON (state_document.json) |
| 内容语言 | 中文（网文内容） |
| AI 模型 | Claude (Anthropic) |

## 架构

本项目采用 ECC 的 6 类组件结构：

- **智能体** (agents/*.md) — 自主角色（planner, writer, consistency-guardian, state-extractor, world-builder）
- **命令** (commands/*.md) — 斜杠命令（/novelai init、/novelai continue 等）
- **技能** (skills/*.md) — 智能体引用的可复用技能定义
- **规则** (rules/*.md) — 写作约束和 P0-P3 上下文
- **提示词** (prompts/*.txt) — LLM 提示词模板
- **状态** (state_document.json) — 所有故事状态的单一真相来源

## 目录结构

```
./
├── CLAUDE.md                    # 项目引导文件
├── agents/                       # 5 个智能体
├── commands/                     # 6 个命令
├── skills/                       # 4 个技能
├── rules/                        # 规则文档
│   ├── common/                  # 5 个通用规则
│   └── novelforge/              # NovelForge 特定规则
├── contexts/                     # 3 个模式定义
├── docs/                         # 详细指南
│   ├── architecture/           # 架构文档
│   ├── backend/                 # 后端开发指南
│   ├── frontend/                # 前端开发指南
│   ├── workflow/                # 工作流系统指南
│   └── novel/                   # 网文创作指南
├── prompts/                      # 7 个中文提示词模板
├── state/                        # state_document.json 模板
└── examples/                    # 示例
```

## 核心概念

### P0-P3 层级上下文

| 优先级 | 名称 | 内容 |
|--------|------|------|
| P0 | Hard Constraints | 核心设定、世界规则 |
| P1 | Current State | 当前位置、情绪、物品 |
| P2 | Near Context | 最近3章事件 |
| P3 | Distant Reference | 背景、伏笔 |

### 十维一致性

1. **character_identity** — 角色外貌/性格
2. **character_location** — 位置一致性
3. **temporal_sequence** — 时间顺序
4. **item_possession** — 物品归属
5. **ability_usage** — 能力使用
6. **relationship_logic** — 关系与事件
7. **plot_thread_progress** — 情节推进
8. **world_rule_compliance** — 世界规则
9. **emotional_continuity** — 情感过渡
10. **factual_contradiction** — 事实矛盾

## 约定

**文件命名**: kebab-case（novelai-continue.md、state-extractor.md）

**YAML frontmatter**: 所有 agents 和 skills 必须有 name + description 字段

**P0-P3 上下文**: 定义在 rules/novelforge/p0-p3-context.md

**状态文档**: .novelai_writer/state_document.json — 不可手动编辑

**维度命名**: 英文 snake_case（character_identity、temporal_sequence）

**提交风格**: 约定式提交（feat:、refactor:、fix:）

## 常见任务

- **初始化项目**: /novelai init
- **撰写章节**: /novelai continue --chapter 1 --words 3000
- **校验一致性**: /novelai verify --chapter 1
- **添加角色**: /novelai character add --name "张三" --role protagonist
- **构建世界观**: /novelai worldbuild

## 查找位置

| 我想... | 查看... |
|---------|---------|
| 了解项目结构 | CLAUDE.md |
| 添加新命令 | commands/novelai-*.md |
| 修改写作规则 | rules/novelforge/novelai-rules.md |
| 修改 P0-P3 上下文 | rules/novelforge/p0-p3-context.md |
| 了解状态结构 | state/state_document.json |
| 修改提示词模板 | prompts/*.txt |
| 查看架构文档 | docs/architecture/overview.md |
| 查看后端指南 | docs/backend/overview.md |
| 查看前端指南 | docs/frontend/overview.md |
| 查看工作流文档 | docs/workflow/overview.md |

## 功能

- **状态追踪** — 自动维护角色、物品、事件状态
- **上下文一致性** — 十维一致性校验引擎
- **层级上下文注入** — P0-P3 优先级注入，确保长篇连贯
- **增量状态更新** — 续写后自动提取并更新状态
- **多智能体协作** — 规划/写作/校验分离
- **分层规则系统** — common/ 通用规则 + novelforge/ 特定规则
- **模式定义** — dev/review/novel-writing 三种工作模式

## 安装

### 方式一：复制到项目（推荐）

```bash
# 在你的小说项目目录下
mkdir -p .claude/commands/novelai_writer
cp -r /path/to/novelai_writer/commands/* .claude/commands/novelai_writer/
```

重启 Claude Code 或运行 /reload-plugins。

### 方式二：安装为插件

```bash
/plugin marketplace add https://github.com/wzxsph/novelai-writer
/plugin install novelai-writer
```

## 快速开始

```bash
# 1. 初始化工作区
/novelai init

# 2. 构建世界观
/novelai worldbuild

# 3. 添加角色
/novelai character add --name "林小雨" --role protagonist

# 4. 开始写第一章
/novelai continue --chapter 1

# 5. 校验一致性
/novelai verify --chapter 1

# 6. 查看状态
/novelai character list
```

## 命令参考

| 命令 | 说明 |
|------|------|
| /novelai init | 初始化工作区，创建状态文档 |
| /novelai worldbuild | 构建世界观设定 |
| /novelai character add --name "张三" --role protagonist | 添加角色 |
| /novelai character list | 列出所有角色 |
| /novelai character update --name "张三" --field emotional_state --value "开心" | 更新角色状态 |
| /novelai continue --chapter 1 | 续写章节 |
| /novelai verify --chapter 1 | 校验章节一致性 |
| /novelai status | 查看当前状态概览 |

## 组件详解

### 智能体 (agents/)

| 智能体 | 文件 | 职责 |
|--------|------|------|
| **planner** | agents/planner.md | 网文大纲规划：根据用户意图创建章节/卷/整体大纲。擅长三幕结构、起承转合、人物弧设计。 |
| **writer** | agents/writer.md | 写作执行：基于上下文和状态文档生成章节内容。遵守 P0-P3 层级上下文，保持文风统一。 |
| **world-builder** | agents/world-builder.md | 世界观构建：从用户描述生成完整世界设定，包括修炼体系、势力架构、地理设定。 |
| **state-extractor** | agents/state-extractor.md | 状态提取：从章节文本提取角色、物品、关系变化，更新状态文档。 |
| **consistency-guardian** | agents/consistency-guardian.md | 一致性校验：十维框架校验角色、物品、时间线、关系的连贯性。 |

### 命令 (commands/)

| 命令 | 文件 | 职责 |
|------|------|------|
| **novelai** | commands/novelai.md | 主入口，列出所有子命令 |
| **novelai init** | commands/novelai-init.md | 初始化工作区，创建 .novelai_writer/state_document.json |
| **novelai character** | commands/novelai-character.md | 角色管理：添加、查看、更新、删除角色 |
| **novelai continue** | commands/novelai-continue.md | 续写章节，参考 P0-P3 上下文生成内容 |
| **novelai verify** | commands/novelai-verify.md | 校验章节一致性，十维框架检测矛盾 |
| **novelai worldbuild** | commands/novelai-worldbuild.md | 根据用户描述生成世界观设定 |

### 技能 (skills/)

| 技能 | 文件 | 职责 |
|------|------|------|
| **novelai-writer** | skills/novelai-writer.md | 核心写作技能，激活于用户撰写/续写小说章节时。强制状态优先、增量更新、P0-P3 上下文注入。 |
| **novelai-extract** | skills/novelai-extract.md | 状态提取技能，从章节文本提取角色动态信息、物品状态、场景状态、关系变化。 |
| **novelai-consistency** | skills/novelai-consistency.md | 一致性校验技能，十维维度校验框架。 |
| **novelai-worldbuild** | skills/novelai-worldbuild.md | 世界观生成技能，参考 prompt 模板生成完整世界设定。 |

### 规则 (rules/)

| 规则 | 目录 | 职责 |
|------|------|------|
| **通用规则** | rules/common/ | 编码规范、Git 工作流、测试要求、安全规范、通用模式 |
| **NovelForge 规则** | rules/novelforge/ | 写作文风规范、P0-P3 上下文、NovelForge 特定模式 |

### 上下文 (contexts/)

| 模式 | 文件 | 用途 |
|------|------|------|
| **开发模式** | contexts/dev.md | 实现、编码、构建功能 |
| **审查模式** | contexts/review.md | 分析、评审、质量保证 |
| **网文写作模式** | contexts/novel-writing.md | 创意写作、故事生成 |

## 参考

- [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) — 架构参考
- [NovelForge](https://github.com/RhythmicWave/NovelForge) — 原始项目
- [架构概述](docs/architecture/overview.md)
- [后端架构](docs/backend/overview.md)
- [前端架构](docs/frontend/overview.md)
- [工作流系统](docs/workflow/overview.md)
- [v0 旧文档](docs/v0_README.md)

## 许可证

MIT
