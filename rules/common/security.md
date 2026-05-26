# Security Guidelines

## Mandatory Security Checks

Before ANY commit:
- No hardcoded secrets (API keys, passwords, tokens)
- All user inputs validated
- SQL injection prevention
- XSS prevention
- Authentication/authorization verified

## Secret Management
- NEVER hardcode secrets in source code
- ALWAYS use environment variables
- Validate required secrets at startup
