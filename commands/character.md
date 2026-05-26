---
description: 管理角色卡片 — 添加、查看、更新、删除角色
---

# /character

管理角色卡片。

## 子命令

### `/character add`

添加新角色。

**参数：**
- `--name`: 角色名（必填）
- `--role`: 角色定位（protagonist/antagonist/supporting/minor）
- `--description`: 角色描述（可选）
- `--first-appearance`: 首次出现章节（默认第1章）

**示例：**
```
/character add --name "张三" --role protagonist --description "天剑宗大师兄"
```

**保存要求**：⚠️ 必须保存到 `state/characters/{char_id}.json`

### `/character list`

列出所有角色及状态。

### `/character update`

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
/character update --name "张三" --field emotional_state --value "开心"
```

**保存要求**：⚠️ 必须更新 `state/characters/{char_id}.json`

### `/character view`

查看单个角色详情。

## 保存规则 ⚠️

所有角色操作**必须保存到文件**：
- 新增角色：`state/characters/{char_id}.json`
- 更新角色：`state/characters/{char_id}.json`
- 删除角色：从文件系统删除对应文件

**使用 Write 或 Edit 工具保存，不能仅停留在内存中显示。**

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
