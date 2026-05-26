# novelai-writer

基于 Claude Code 的网文长篇写作助手插件系统。参考 ECC 架构模式和 NovelForge 设计理念，通过十维一致性校验框架，管理角色、物品、情节线和关系网络，确保数百章内容连贯一致。

> 本项目处于构建初期，尚未经过充分测试，属于即兴创作的产物，在实际使用中可能会遇到各种问题。本项目深度参考了 [ECC](https://github.com/affaan-m/ECC) 和 [NovelForge](https://github.com/RhythmicWave/NovelForge) 的设计理念实现。

## 技术栈

| 层级 | 技术 |
|------|------|
| 格式 | Markdown + YAML frontmatter |
| 插件系统 | ECC (Everything Claude Code) |
| 状态存储 | JSON (`state_document.json` v2.0) |
| 内容语言 | 中文（网文内容） |
| AI 模型 | Claude (Anthropic) |

## 目录结构

```
novelai_writer/
├── agents/                       # 5 个智能体
│   ├── planner.md               # 大纲规划
│   ├── writer.md                 # 写作执行
│   ├── consistency-guardian.md  # 一致性校验
│   ├── state-extractor.md       # 状态提取
│   └── world-builder.md          # 世界观构建
├── commands/                     # 14 个命令
│   ├── novelai.md               # 主入口
│   ├── novelai-init.md          # 初始化
│   ├── novelai-continue.md      # 续写/润色/扩写
│   ├── novelai-verify.md        # 一致性校验
│   ├── novelai-worldbuild.md    # 世界观构建
│   ├── novelai-character.md     # 角色管理
│   ├── novelai-timeline.md      # 时间线查看
│   ├── novelai-items.md          # 物品查看
│   ├── novelai-plot.md           # 情节线查看
│   ├── novelai-scenes.md         # 场景查看
│   ├── novelai-organizations.md # 组织查看
│   ├── novelai-concepts.md      # 概念查看
│   ├── novelai-chapters.md       # 章节查看
│   └── novelai-status.md         # 状态概览
├── skills/                       # 4 个技能
│   ├── novelai-writer.md        # 核心写作
│   ├── novelai-extract.md       # 状态提取
│   ├── novelai-consistency.md   # 一致性校验
│   └── novelai-worldbuild.md    # 世界观生成
├── rules/
│   ├── common/                  # 5 个通用规则
│   └── novelforge/              # 6 个写作规则
│       ├── p0-p3-context.md     # 层级上下文
│       ├── novelai-rules.md     # 文风规范
│       ├── scene-tracking.md   # 场景追踪
│       ├── timeline-rules.md   # 时间线规则
│       ├── plot-thread-rules.md # 情节线状态机
│       └── item-chain-rules.md  # 物品所有权链
├── prompts/                     # 16 个提示词模板
├── knowledge/                   # 4 个知识库
├── workflows/                   # 3 个工作流
│   ├── 雪花创作法.md            # 项目启动
│   ├── 章节生成.md             # 章节生成
│   └── 拆书工作流.md            # 导入现有小说
├── contexts/                    # 3 个模式定义
├── state/                       # state_document.json v2.0 模板
└── examples/                    # 示例项目
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

### State Document v2.0

```json
{
  "version": "2.0",
  "metadata": { "title": "", "genre": [], "style": "", "author": "", "word_count_target": 0, "template": "" },
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

## 快速开始

```bash
# 1. 初始化工作区（支持雪花创作法模板）
/novelai init --template snowflake

# 2. 构建世界观
/novelai worldbuild

# 3. 添加角色
/novelai character add --name "林小雨" --role protagonist

# 4. 开始写第一章
/novelai continue --chapter 1

# 5. 校验一致性
/novelai verify --chapter 1

# 6. 查看状态
/novelai status
```

## 命令参考

### 初始化与状态
| 命令 | 说明 |
|------|------|
| `/novelai init --template snowflake` | 初始化工作区 |
| `/novelai status` | 查看项目状态概览 |

### 角色管理
| 命令 | 说明 |
|------|------|
| `/novelai character add --name "张三" --role protagonist` | 添加角色 |
| `/novelai character list` | 列出所有角色 |
| `/novelai character update --name "张三" --field emotional_state --value "开心"` | 更新角色状态 |

### 章节创作
| 命令 | 说明 |
|------|------|
| `/novelai continue --chapter 5 --words 3000` | 续写章节 |
| `/novelai continue --chapter 5 --mode polish` | 润色章节 |
| `/novelai continue --chapter 5 --mode expand` | 扩写大纲为正文 |
| `/novelai verify --chapter 5 --scope all` | 校验一致性 |
| `/novelai chapters --range 1-30` | 列出章节概览 |

### 状态查看
| 命令 | 说明 |
|------|------|
| `/novelai timeline --chapter 5` | 事件时间线 |
| `/novelai items --type 法宝` | 物品设定 |
| `/novelai plot --status active` | 情节线状态 |
| `/novelai scenes --character "林小雨"` | 场景列表 |
| `/novelai organizations --type 门派` | 组织势力 |
| `/novelai concepts --type 功法` | 概念设定 |

## 组件详解

### 智能体 (agents/)

| 智能体 | 职责 |
|--------|------|
| **planner** | 大纲规划：雪花创作法、起承转合 |
| **writer** | 写作执行：P0-P3 上下文、文风统一 |
| **world-builder** | 世界观构建：修炼体系、势力架构 |
| **state-extractor** | 状态提取：角色、物品、场景变化 |
| **consistency-guardian** | 一致性校验：十维框架 |

### 技能 (skills/)

| 技能 | 职责 |
|------|------|
| **novelai-writer** | 核心写作技能 |
| **novelai-extract** | 状态提取 |
| **novelai-consistency** | 一致性校验 |
| **novelai-worldbuild** | 世界观生成 |

### 工作流 (workflows/)

| 工作流 | 说明 |
|--------|------|
| **雪花创作法** | 7步项目启动法 |
| **章节生成** | 标准章节生成流程 |
| **拆书工作流** | 导入现有小说 |

## 安装

### 方式一：安装为插件（推荐）

```bash
# 添加市场
/plugin marketplace add https://github.com/wzxsph/novelai-writer

# 安装插件
/plugin install novelai-writer
```

### 方式二：复制到项目

```bash
# 克隆仓库
git clone https://github.com/wzxsph/novelai-writer.git

# 进入目录
cd novelai-writer

# 复制命令到项目（不推荐，命令文件应通过插件加载）
mkdir -p .claude/commands
cp -r commands/* .claude/commands/
```

> **注意**：插件安装方式为推荐方式。复制命令文件会导致命令无法正确加载插件元数据。

## 参考

- [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC)
- [NovelForge](https://github.com/RhythmicWave/NovelForge)
- [CLAUDE.md](CLAUDE.md) — 项目引导
- [examples/](examples/) — 示例项目
