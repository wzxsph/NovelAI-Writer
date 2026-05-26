# 卡牌系统

## Schema-Driven

卡牌具有类型定义，包含 JSON Schema：
```json
{
  "type": "chapter",
  "schema": {
    "title": "string",
    "content": "string",
    "word_count": "number"
  }
}
```

## 卡牌操作

- 创建卡牌
- 更新卡牌内容
- 卡牌树结构 (parent/children)
- Schema 验证

## 上下文注入

@DSL 语法引用卡牌：
- `@card[id]` - 引用特定卡牌
- `@type[chapter]` - 引用类型定义
- `@relation[人物]` - 引用关系
