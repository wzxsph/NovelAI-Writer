# 前端架构

## 技术栈
- Electron + Vue 3 + TypeScript
- Pinia (状态管理)
- Element Plus (UI 组件)
- @vue-flow/core (工作流可视化)
- CodeMirror (代码编辑)

## 目录结构

```
frontend/src/
├── main/          # Electron main process
├── preload/       # Preload scripts
└── renderer/src/
    ├── api/       # API clients
    ├── components/# Vue components
    ├── composables/# Vue composables
    ├── stores/    # Pinia stores
    ├── types/     # TypeScript types
    └── views/     # Page views
```

## Pinia Stores

| Store | 用途 |
|-------|------|
| `useAIStore` | AI 生成状态 |
| `useAppStore` | 应用状态 (主题) |
| `useCardStore` | 卡牌管理 |
| `useWorkflowStore` | 工作流状态 |
| `useProjectStore` | 项目管理 |
