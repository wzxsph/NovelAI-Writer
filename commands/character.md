---
description: 管理角色卡片 — 添加、查看、更新、删除角色
---

# /novelai character

管理角色卡片。

## 子命令

### `/novelai character add`

添加新角色。

**参数：**
- `--name`: 角色名（必填）
- `--role`: 角色定位（protagonist/antagonist/supporting/minor）
- `--description`: 角色描述（可选）
- `--first-appearance`: 首次出现章节（默认第1章）

**示例：**
```
/novelai character add --name "张三" --role protagonist --description "天剑宗大师兄"
```

### `/novelai character list`

列出所有角色及状态。

### `/novelai character update`

更新角色状态。

**参数：**
- `--name`: 角色名（必填）
- `--field`: 字段名（必填）
- `--value`: 新值（必填）

**可更新字段：**
- `status`: alive/deceased/disappeared
- `current_location`: 当前位置
- `emotional_state`: 情绪状态
- `relationships`: 关系（JSON格式）

**示例：**
```
/novelai character update --name "张三" --field emotional_state --value "悲伤"
```

### `/novelai character view`

查看单个角色详情。

## 角色数据结构

```json
{
  "id": "char_001",
  "name": "张三",
  "role": "protagonist",
  "status": "alive",
  "current_location": "天剑宗",
  "emotional_state": "平静",
  "relationships": {"李四": "挚友"},
  "first_appearance": 1,
  "updated_chapter": 1
}
```
