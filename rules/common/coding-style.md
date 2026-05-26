---
name: coding-style
description: 通用编码风格规范
---

# Coding Style

## Immutability (CRITICAL)

ALWAYS create new objects, NEVER mutate existing ones.

## Core Principles

### KISS (Keep It Simple)
- Prefer the simplest solution that actually works
- Avoid premature optimization

### DRY (Don't Repeat Yourself)
- Extract repeated logic into shared functions or utilities

### YAGNI (You Aren't Gonna Need It)
- Do not build features before they are needed

## File Organization
- Many small files > few large files
- 200-400 lines typical, 800 max
- Organize by feature/domain, not by type

## Error Handling
- Always handle errors comprehensively
- Provide user-friendly error messages
- Log detailed error context

## Input Validation
- Always validate at system boundaries
- Fail fast with clear error messages
