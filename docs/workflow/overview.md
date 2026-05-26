# 工作流系统

## DSL v2

代码式工作流定义，存储在 `Workflow.definition_code`。

```python
# 示例工作流
trigger:
  type: card.saved
  
nodes:
  - type: ai.generate
    input: "{{card.content}}"
    output: "generated"
    
  - type: card.update
    id: "{{card.id}}"
    content: "{{generated}}"
```

## 节点类型

| 类型 | 说明 |
|------|------|
| `ai.generate` | AI 生成节点 |
| `card.create` | 创建卡牌 |
| `card.update` | 更新卡牌 |
| `data.transform` | 数据转换 |
| `logic.if` | 条件分支 |
| `trigger.event` | 事件触发 |

## 执行引擎

AsyncIterator-based 执行，支持：
- 暂停/恢复
- 节点级进度
- SSE 实时更新

## 内置模板

- 项目创建·雪花创作法
- 世界观·转组织
- 核心蓝图·落子卡
