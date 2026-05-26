---
description: 续写小说章节，基于上下文状态文档
---

# /novelai continue

续写小说章节，保持角色、物品、事件一致性。

## 参数

- `--chapter`: 章节编号（必填）
- `--previous`: 前一章文件路径（可选，若不提供则从状态文档推断）
- `--words`: 目标字数（默认 3000）
- `--instructions`: 额外写作指示（可选）
- `--mode`: 模式（默认 `write`，可选 `polish`/`expand`）
  - `write`: 新写章节
  - `polish`: 润色现有章节（参考 `@prompts/章节润色.txt`）
  - `expand`: 扩写大纲为正文（参考 `@prompts/章节扩写.txt`）

## 执行流程

1. **读取状态文档** `.novelai_writer/state_document.json`
2. **构建 P0-P3 上下文**：参考 `@rules/novelforge/p0-p3-context.md`
3. **加载写作规则**：参考 `@rules/novelforge/novelai-rules.md`
4. **生成章节内容**：
   - `write` 模式：参考 `@workflows/章节生成.md` 和 `@prompts/内容生成.txt`
   - `polish` 模式：参考 `@prompts/章节润色.txt`
   - `expand` 模式：参考 `@prompts/章节扩写.txt`
5. **提取状态变化**：参考 `@prompts/角色动态信息提取.txt`
6. **更新状态文档**
7. **保存章节文件**

## 状态更新要求

生成章节后，必须更新状态文档中的：
- 新增角色（若有）
- 新增事件到时间线
- 物品所有权变化
- 角色位置/情绪变化
- 情节线状态变化
- 场景变化（若有）

## 输出

- 章节文本保存到 `chapters/chapter_{N}.txt`
- 状态文档更新
- 向用户报告更新了什么内容

## 章节审核

完成章节后，可选执行 `/novelai verify --chapter N` 进行一致性校验。
