---
name: novelai-writer
description: 网文长篇写作助手，激活于用户撰写/续写小说章节时
---

# NovelAI Writer

## When to Activate
- 用户要求续写章节
- 用户要求生成新章节
- 用户要求校验一致性
- 用户要求查看/编辑状态文档

## Core Rules

1. **状态优先**：每次续写前必须读取 `.novelai_writer/state_document.json`
2. **增量更新**：续写完成后，必须更新状态文档（新增角色、事件、物品变化）
3. **一致性前置**：生成前注入 P0-P3 层级上下文
4. **用户确认**：情节重大转折前必须确认用户意图
5. **文风统一**：严格按照 `@rules/novelai-rules.md` 执行

## P0-P3 层级上下文

| Priority | Name | Content |
|----------|------|---------|
| P0 | Hard Constraints | 核心角色定义、世界规则、关键物品属性 |
| P1 | Current State | 角色位置、情绪状态、持有物品、活跃情节线 |
| P2 | Near Context | 最近3章事件、最近关系变化 |
| P3 | Distant Reference | 背景设定、伏笔线、Dormant 情节 |

## State Update Workflow

After generating chapter content:
1. Analyze new text for:
   - New characters introduced
   - New events occurred
   - Items acquired/lost/transferred
   - Plot thread state changes
   - Character location/emotion changes
2. Update `.novelai_writer/state_document.json` via Edit tool
3. Report what was updated to user

## Chapter Generation Prompt Template

参考 `@prompts/内容生成.txt` 的 prompt 模板生成章节内容。

## Writing Rules

严格遵守 `@rules/novelai-rules.md` 中的规范。