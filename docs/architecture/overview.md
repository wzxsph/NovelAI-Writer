# NovelForge 系统架构概述

## 技术栈

| 层级 | 技术 |
|------|------|
| 后端 | FastAPI + SQLModel + SQLite + LangChain |
| 前端 | Electron + Vue 3 + TypeScript + Pinia |
| 数据库 | SQLite (核心) / Neo4j (知识图谱，可选) |
| 工作流 | 代码式 DSL v2 |

## 核心概念

### 1. Schema-Driven 卡牌系统
Cards have types with JSON Schema definitions. AI generation validates against schema.

### 2. 工作流系统
Workflows defined as Python-like code, not visual DAG. DSL v2 stored in `Workflow.definition_code`.

### 3. 知识图谱
Entity relationship tracking with SQLModel (default) or Neo4j.

### 4. 上下文注入
@DSL syntax: `@card[id]`, `@type[name]`, `@relation[type]`

## 数据流

1. Frontend (Vue + Electron) → API (FastAPI)
2. API → Services (business logic)
3. Services → Database (SQLModel/SQLite)
4. Services → Events → Workflows (triggered by events)
5. Workflow Engine → Nodes (execute via AsyncIterator)
6. Workflow Run → SSE → Frontend status bar
