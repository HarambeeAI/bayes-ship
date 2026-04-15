---
description: "REST API endpoint design, request/response patterns, versioning, error handling, and authentication patterns. Use during architecture and implementation phases. Source: everything-claude-code."
---

# API Design

## URL conventions

- Nouns for resources: `/api/invoices`, `/api/clients`
- Plural: `/invoices` not `/invoice`
- Nested for relationships: `/invoices/:id/line-items`
- No verbs in URLs: `/invoices` not `/getInvoices`

## HTTP methods

| Method | Purpose | Response code |
|--------|---------|---------------|
| GET | Read | 200 |
| POST | Create | 201 |
| PATCH | Partial update | 200 |
| PUT | Full replace | 200 |
| DELETE | Remove | 204 |

## Request/response patterns

- Always return the created/updated resource on POST/PATCH/PUT
- Use `?page=1&limit=20` for pagination
- Use `?status=paid&sort=-created_at` for filtering and sorting
- Return `{ data: [...], meta: { total, page, limit } }` for lists

## Authentication

- Use Bearer tokens in Authorization header
- Public endpoints explicitly marked (no auth middleware)
- Webhook endpoints validate source (IP whitelist or signature verification)

## Error handling

- 400: Client error (validation, bad input)
- 401: Not authenticated
- 403: Not authorized (authenticated but insufficient permissions)
- 404: Resource not found
- 409: Conflict (duplicate, state conflict)
- 500: Server error (never expose internals)

## Versioning

- URL prefix: `/api/v1/invoices`
- Only version when breaking changes are needed
- New fields on existing endpoints are NOT breaking changes
