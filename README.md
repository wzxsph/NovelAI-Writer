# NovelAI Writer

基于 Claude Code 的网文长篇写作助手插件，参考 [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) 系统设计。

## 功能

- **状态追踪** — 自动维护角色、物品、事件状态
- **上下文一致性** — 十维一致性校验引擎
- **层级上下文注入** — P0-P3 优先级注入，确保长篇连贯
- **增量状态更新** — 续写后自动提取并更新状态
- **多智能体协作** — 规划/写作/校验分离
- **甜文法则** — 专为甜蜜恋爱题材优化

## 安装

### 方式一：复制到项目（推荐）

```bash
# 在你的小说项目目录下
mkdir -p .claude/commands/novelai_writer
cp -r /path/to/novelai_writer/commands/* .claude/commands/novelai_writer/
```

重启 Claude Code 或运行 `/reload-plugins`。

### 方式二：安装为插件

```bash
/plugin marketplace add https://github.com/你的用户名/novelai-writer
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

## 架构

```
novelai_writer/
├── agents/                      # 5个智能体
│   ├── planner.md              # 大纲规划
│   ├── world-builder.md       # 世界观构建
│   ├── writer.md              # 写作执行
│   ├── state-extractor.md     # 状态提取
│   └── consistency-guardian.md # 一致性校验
├── commands/                    # 6个命令
│   ├── novelai.md             # 主入口
│   ├── novelai-init.md        # 初始化
│   ├── novelai-character.md    # 角色管理
│   ├── novelai-continue.md    # 章节续写
│   ├── novelai-verify.md      # 一致性校验
│   └── novelai-worldbuild.md   # 世界观构建
├── skills/                      # 4个技能
├── rules/                       # 文风/禁词规则
│   ├── novelai-rules.md        # 文风规范与禁词
│   └── p0-p3-context.md        # P0-P3 层级上下文定义
├── prompts/                     # 7个 prompt 模板
└── state/                       # 状态文档模板
```

## 核心概念

### 状态文档 (State Document)

`.novelai_writer/state_document.json` 是所有状态的单一真相来源：

```json
{
  "version": "1.0",
  "last_updated_chapter": 2,
  "characters": [...],
  "timeline": [...],
  "items": [...],
  "plot_threads": [...],
  "relationships": [...]
}
```

### P0-P3 上下文

| 优先级 | 名称 | 内容 |
|--------|------|------|
| P0 | Hard Constraints | 核心角色定义、世界规则 |
| P1 | Current State | 当前状态（位置、情绪、物品） |
| P2 | Near Context | 最近3章事件 |
| P3 | Distant Reference | 背景设定、伏笔 |

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

## 命令参考

| 命令 | 说明 |
|------|------|
| `/novelai init` | 初始化工作区，创建状态文档 |
| `/novelai worldbuild` | 构建世界观设定 |
| `/novelai character add --name "张三" --role protagonist` | 添加角色 |
| `/novelai character list` | 列出所有角色 |
| `/novelai character update --name "张三" --field emotional_state --value "开心"` | 更新角色状态 |
| `/novelai continue --chapter 1` | 续写章节 |
| `/novelai verify --chapter 1` | 校验章节一致性 |
| `/novelai status` | 查看当前状态概览 |

## 示例小说

《我真的可以拥有甜甜的恋爱吗》— 都市+玄幻+穿越，幽默风趣

- 主角：林小雨（穿越者）、陆尘（天元宗首席弟子）
- 红线绳绑定，命中注定的相遇
- 修炼加成与恋爱甜蜜值挂钩

## 参考

- [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) — 系统架构参考
- [NovelForge](https://github.com/你的用户名/NovelForge) — prompt 模板参考

## 许可证

MIT
