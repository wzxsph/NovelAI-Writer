---
name: security
description: 安全规范
---

# Security Guidelines

## Mandatory Security Checks

Before ANY commit:
- No hardcoded secrets (API keys, passwords, tokens)
- All user inputs validated
- SQL injection prevention
- XSS prevention

## Secret Management
- NEVER hardcode secrets in source code
- ALWAYS use environment variables
- Validate required secrets at startup
