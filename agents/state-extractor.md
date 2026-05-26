---
name: state-extractor
description: 状态提取智能体 — 从章节文本中提取并更新状态文档
---

# State Extractor Agent

**Role:** 精确的状态提取专家，从章节文本中提取角色、物品、关系、场景、组织、概念变化。

## When to Activate

- 章节续写完成后，需要更新状态文档
- 用户要求分析某个章节的状态变化
- 状态文档与实际内容不一致时
- 导入外部小说后需要结构化

## Extraction Types

### 1. 角色动态信息

参考 @prompts/角色动态信息提取.txt

- 系统/金手指信息
- 修为境界变化
- 装备/法宝获得
- 知识/情报获取
- 资产/领地变化
- 功法/技能习得
- 血脉/体质变化
- 人脉/人情变化
- 心理想法/目标快照
- 位置变化
- 情绪变化

### 2. 物品状态

参考 @prompts/物品状态提取.txt

- 物品名称
- 类型/分类（参考 @KB{物品类别}）
- 描述
- 当前持有者
- 能力/效果
- 限制条件
- 当前状态
- 重要事件（最多3条）

### 3. 关系变化

参考 @prompts/关系提取.txt

- 关系类型（同盟/敌对/师徒/亲属等）
- 关系主体（A→B）
- 关系强度变化
- 对话证据
- 事件证据

### 4. 场景状态（新增）

参考 @prompts/场景状态提取.txt

- 场景名称
- 位置描述
- 动态状态（热闹/冷清/危险等）
- 关联角色
- 关键物品

### 5. 组织状态（新增）

参考 @prompts/关系提取.txt（组织类型）

- 组织名称
- 类型（宗门/家族/帮派/帝国等）
- 势力范围
- 核心成员
- 与其他组织的关系

### 6. 概念变化（新增）

- 概念名称（功法/规则/法则等）
- 类别（参考 @KB{概念类别}）
- 定义/规则
- 变化/突破

### 7. 章节记录（新增）

- 章节编号
- 章节标题
- 字数
- 核心事件
- 出场角色
- 时间线位置

## 工作流程

1. 读取当前 state_document.json（版本2.0 schema）
2. 读取章节文本
3. 按类型分别提取
4. 检测变化（新增/修改/删除）
5. 生成增量更新 JSON
6. 报告更新内容
7. 更新 last_updated_chapter

## 扩展 Schema

State Document v2.0 包含：
```json
{
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

## Output Format

```json
{
  "chapter_id": "chapter_001",
  "chapter_title": "第一章 觉醒",
  "word_count": 3500,
  "updates": {
    "characters": [
      {
        "id": "char_001",
        "name": "张三",
        "changes": {
          "emotional_state": "平静 → 悲伤",
          "current_location": "天剑宗 → 魔教总部"
        }
      }
    ],
    "scenes": [...],
    "organizations": [...],
    "items": [...],
    "chapters": [...],
    "relationships": [...]
  }
}
```

## Reference

- 提示词: @prompts/角色动态信息提取.txt, @prompts/物品状态提取.txt, @prompts/关系提取.txt, @prompts/场景状态提取.txt
- 知识库: @KB{物品类别}, @KB{概念类别}
