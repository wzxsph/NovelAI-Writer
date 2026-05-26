# 后端架构

## 技术栈
- FastAPI + Uvicorn
- SQLModel (Pydantic + SQLAlchemy)
- SQLite / Neo4j (可选)
- LangChain for AI integrations

## 目录结构

```
backend/app/
├── api/           # API endpoints
├── bootstrap/     # Built-in resources (prompts, knowledge, workflows)
├── core/          # Config, events, middleware
├── db/            # Database models
├── schemas/       # Pydantic models
├── services/      # Business logic
└── utils/         # Utilities
```

## 核心模块

| 目录 | 用途 |
|------|------|
| `app/api/endpoints/` | REST API 路由 |
| `app/services/` | 业务逻辑层 |
| `app/services/workflow/` | 工作流执行引擎 |
| `app/bootstrap/` | 初始化器 (prompts, card_types, knowledge) |

## 配置

使用 Pydantic Settings:
- `DatabaseSettings` - SQLite 路径
- `KnowledgeGraphSettings` - KG provider
- `Neo4jSettings` - Neo4j 连接
- `AISettings` - AI 提供商配置
