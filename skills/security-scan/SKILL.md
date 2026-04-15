---
description: "Security scanning against OWASP Top 10, secret detection, input validation, auth hardening. Used by the security reviewer during QA phase. Source: everything-claude-code."
---

# Security Scan

## OWASP Top 10 checklist

### 1. Injection (SQL, NoSQL, command)
- All user input parameterized (never string-concatenated into queries)
- ORM used for database queries (Prisma, Drizzle, TypeORM)
- No `eval()`, `exec()`, or dynamic code execution with user input

### 2. Broken authentication
- Passwords hashed with bcrypt/argon2 (never stored in plain text)
- Session tokens are cryptographically random, HTTPOnly, Secure, SameSite
- Rate limiting on login endpoints (prevent brute force)
- Account lockout after N failed attempts

### 3. Sensitive data exposure
- No secrets in code (API keys, passwords, tokens)
- All secrets in environment variables
- HTTPS enforced (no HTTP)
- Sensitive data not logged

### 4. XSS (Cross-Site Scripting)
- All user-generated content escaped before rendering
- React's JSX auto-escapes by default (don't use dangerouslySetInnerHTML)
- Content-Security-Policy headers set

### 5. CSRF
- CSRF tokens on all state-changing forms
- SameSite cookie attribute set to "Strict" or "Lax"

### 6. Security misconfiguration
- Remove default credentials and example pages
- Disable verbose error messages in production
- Keep dependencies updated (run `npm audit`)

### M-Pesa specific security
- Validate callback source IP against Safaricom's published ranges
- Verify transaction amounts match expected values
- Implement idempotent callback handling (Daraja can send duplicates)
- Never log full M-Pesa credentials

## Automated checks

```bash
# Dependency vulnerabilities
npm audit

# Secret detection
grep -r "password\|secret\|api_key\|token" --include="*.ts" --include="*.js" src/

# Check for console.log with sensitive data
grep -rn "console.log.*password\|console.log.*token\|console.log.*secret" src/
```
