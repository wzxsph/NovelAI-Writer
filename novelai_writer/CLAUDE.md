# NovelAI Writer

网文长篇写作助手，基于 Claude Code 的 ECC skill/command/agent 系统。

## 项目结构

```
novelai_writer/
├── .claude-plugin/              # 插件元数据
│   └── plugin.json              # 插件配置
├── agents/                      # 智能体（5个）
│   ├── planner.md              # 大纲规划
│   ├── world-builder.md       # 世界观构建
│   ├── consistency-guardian.md # 一致性校验
│   ├── state-extractor.md     # 状态提取
│   └── writer.md              # 写作执行
├── commands/                    # 命令（6个）
│   ├── novelai.md             # 主命令入口
│   ├── novelai-init.md       # 初始化
│   ├── novelai-character.md   # 角色管理
│   ├── novelai-continue.md    # 章节续写
│   ├── novelai-verify.md      # 一致性校验
│   └── novelai-worldbuild.md  # 世界观构建
├── skills/                      # 技能（4个）
│   ├── novelai-writer.md     # 核心写作技能
│   ├── novelai-extract.md    # 状态提取技能
│   ├── novelai-consistency.md # 一致性校验
│   └── novelai-worldbuild.md # 世界观生成
├── rules/                       # 写作规则
│   └── novelai-rules.md       # 文风/禁词规则
├── prompts/                      # Prompt 模板
│   ├── 内容生成.txt
│   ├── 角色动态信息提取.txt
│   ├── 物品状态提取.txt
│   ├── 场景状态提取.txt
│   ├── 关系提取.txt
│   ├── 文风约束.txt
│   └── 一致性校验.txt
├── state/                       # 状态文档模板
│   └── state_document.json
├── examples/                    # 示例配置
│   └── example-project/
└── .claude/                     # 项目级配置
```

## 核心概念

### 状态文档 (State Document)

`.novelai_writer/state_document.json` 是所有状态的单一真相来源：
- characters: 角色状态（位置、情绪、关系、生死）
- timeline: 事件时间线
- items: 物品归属
- plot_threads: 情节线状态
- relationships: 关系网络

### P0-P3 层级上下文

生成章节时注入的优先级顺序：
- **P0**: 硬约束（核心设定、世界规则）
- **P1**: 当前状态（位置、情绪、物品）
- **P2**: 近距上下文（最近3章）
- **P3**: 远距引用（背景、伏笔）

### 一致性十维校验

1. character_identity — 角色外貌/性格
2. character_location — 位置一致性
3. temporal_sequence — 时间顺序
4. item_possession — 物品归属
5. ability_usage — 能力使用
6. relationship_logic — 关系与事件
7. plot_thread_progress — 情节推进
8. world_rule_compliance — 世界规则
9. emotional_continuity — 情感过渡
10. factual_contradiction — 事实矛盾

## 使用流程

### 1. 初始化
```
/novelai init
```

### 2. 构建世界观
```
/novelai worldbuild
```
或使用 agent：
```
/novelai agent world-builder
```

### 3. 规划大纲
```
/novelai plan
```
或使用 agent：
```
/novelai agent planner
```

### 4. 添加角色
```
/novelai character add --name "张三" --role protagonist
```

### 5. 续写章节
```
/novelai continue --chapter 1 --words 3000
```
或使用 agent：
```
/novelai agent writer --chapter 1 --words 3000
```

### 6. 校验一致性
```
/novelai verify --chapter 1
```
或使用 agent：
```
/novelai agent consistency-guardian --chapter 1
```

## AGENTS.md 约定

项目根目录的 `AGENTS.md` 定义了可用智能体。每个智能体是一个 Markdown 文件，包含 YAML frontmatter：
- name: 智能体名称
- description: 描述
- tools: 可用工具
- model: 推荐模型

## 参考

- [Everything Claude Code](https://github.com/affaan-m/ECC) — ECC 系统参考
- [NovelForge](../NovelForge/backend/app/bootstrap/prompts/) — prompt 模板参考
