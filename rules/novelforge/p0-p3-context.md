---
name: p0-p3-context
description: P0-P3 层级上下文优先级定义，供所有 agent 和 command 引用
---

# P0-P3 层级上下文

生成章节时注入的优先级顺序，确保长篇写作的一致性。

| Priority | Name | Content |
|----------|------|---------|
| P0 | Hard Constraints | 核心角色定义、世界规则、关键物品属性 |
| P1 | Current State | 角色位置、情绪状态、持有物品、活跃情节线 |
| P2 | Near Context | 最近3章事件、最近关系变化 |
| P3 | Distant Reference | 背景设定、伏笔线、Dormant 情节 |

## Usage

引用方式：`@rules/p0-p3-context.md`

所有续写、校验、状态提取操作必须遵循此层级顺序。
