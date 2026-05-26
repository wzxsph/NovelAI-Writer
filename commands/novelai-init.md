---
description: 初始化网文写作工作区，创建状态文档
---

# /novelai init

初始化网文写作工作区，创建 `.novelai_writer/state_document.json`。

## 执行步骤

1. 创建 `.novelai_writer/` 目录
2. 从 `novelai_writer/state/state_document.json` 复制模板到 `.novelai_writer/state_document.json`
3. 询问用户项目基本信息：
   - 小说标题
   - 题材类型（都市/玄幻/穿越/科幻/其他）
   - 文风偏好
   - 作者名（可选）
4. 更新状态文档的 `metadata` 字段

## 状态文档结构

```json
{
  "version": "1.0",
  "last_updated_chapter": 0,
  "metadata": {
    "title": "小说标题",
    "genre": "题材类型",
    "style": "文风偏好",
    "author": "作者名"
  },
  "characters": [],
  "timeline": [],
  "items": [],
  "plot_threads": [],
  "relationships": []
}
```

## 初始化后的下一步

1. `/novelai worldbuild` — 构建世界观
2. `/novelai character add` — 添加主要角色
3. `/novelai continue --chapter 1` — 开始撰写第一章
