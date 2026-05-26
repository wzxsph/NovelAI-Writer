# 一致性校验

## 十维框架

1. **character_identity** - 角色外貌/性格
2. **character_location** - 位置一致性
3. **temporal_sequence** - 时间顺序
4. **item_possession** - 物品归属
5. **ability_usage** - 能力使用
6. **relationship_logic** - 关系逻辑
7. **plot_thread_progress** - 情节推进
8. **world_rule_compliance** - 世界规则
9. **emotional_continuity** - 情感过渡
10. **factual_contradiction** - 事实矛盾

## P0-P3 上下文优先级

| 优先级 | 内容 |
|--------|------|
| P0 | 硬约束 (核心设定) |
| P1 | 当前状态 |
| P2 | 近距上下文 (最近3章) |
| P3 | 远距引用 |
