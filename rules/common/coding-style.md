# Coding Style

## Immutability (CRITICAL)

ALWAYS create new objects, NEVER mutate existing ones:

```
WRONG:  modify(original, field, value) → changes original in-place
CORRECT: update(original, field, value) → returns new copy with change
```

## Core Principles

### KISS (Keep It Simple)
- Prefer the simplest solution that actually works
- Avoid premature optimization
- Optimize for clarity over cleverness

### DRY (Don't Repeat Yourself)
- Extract repeated logic into shared functions or utilities
- Avoid copy-paste implementation drift

### YAGNI (You Aren't Gonna Need It)
- Do not build features before they are needed

## File Organization
- Many small files > few large files
- 200-400 lines typical, 800 max
- Organize by feature/domain, not by type

## Error Handling
- Always handle errors comprehensively
- Provide user-friendly error messages in UI-facing code
- Log detailed error context on the server side

## Input Validation
- Always validate at system boundaries
- Fail fast with clear error messages
- Never trust external data

## Naming Conventions
- Variables and functions: camelCase
- Booleans: is, has, should, can prefixes
- Constants: UPPER_SNAKE_CASE
- Custom hooks: camelCase with use prefix
