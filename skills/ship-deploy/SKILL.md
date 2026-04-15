---
description: Deploy the product to production. Runs deployment checklist, executes deploy commands, verifies health post-deployment. Supports Railway and Vercel.
disable-model-invocation: true
---

# Deploy — Production deployment

## Process

1. Read ARCHITECTURE.md for deployment targets.
2. Run deployment checklist:
   - On main branch (or approved branch)?
   - Environment variables configured?
   - Database migrations ready?
   - Build passing?
   - Full test suite passing?
   - Security scan clean?
3. Check for missing environment variables. If any are missing, prompt the user for each one. Offer to generate secrets (like auth keys) where appropriate.
4. Execute deployment:
   - Backend → Railway (or configured provider)
   - Frontend → Vercel (or configured provider)
   - Database → Run migrations
5. **Post-deployment verification**:
   - Hit health check endpoints
   - Verify key user flows work in production
   - Report deployment URLs
6. Update `progress.json` with deployment status and URLs.

## Rules

- Never deploy without passing tests.
- Never deploy with critical security findings unresolved.
- Always verify post-deployment. A deployment without health check is incomplete.
