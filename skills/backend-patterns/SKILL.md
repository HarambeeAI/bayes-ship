---
description: "API design, query optimization, repository/service layer patterns for Node.js, Express, and Next.js backends. Use when implementing backend tasks. Source: everything-claude-code."
---

# Backend Patterns

## Architecture layers

```
Controller (routes) → Service (business logic) → Repository (data access) → Database
```

- **Controller**: HTTP concerns only. Parse request, validate input, call service, format response.
- **Service**: Business logic. No HTTP concepts (no req/res). Testable in isolation.
- **Repository**: Data access. Raw queries or ORM calls. No business logic.

## API endpoint pattern

```typescript
// Controller
async function createInvoice(req: Request, res: Response) {
  const dto = CreateInvoiceDto.parse(req.body)  // validate
  const invoice = await invoiceService.create(dto)  // delegate
  res.status(201).json(invoice)  // respond
}
```

## Query optimization

- Select only needed columns: `.select('id, name, status')` not `.select('*')`
- Always add `.limit()` to list queries
- Use database indexes on columns used in WHERE and ORDER BY
- Use pagination (cursor-based preferred over offset for large datasets)

## Input validation

- Validate at the boundary (controller) with Zod or class-validator
- Never trust client input inside service or repository layers
- Validate types AND business rules (e.g., amount > 0, date in future)

## Error responses

```typescript
// Consistent error shape
{
  "error": {
    "code": "INVOICE_NOT_FOUND",
    "message": "Invoice with ID abc123 not found",
    "status": 404
  }
}
```

## Nest.js specifics

- One module per domain (InvoicesModule, PaymentsModule, AuthModule)
- Use guards for auth, interceptors for logging/transformation, pipes for validation
- DTOs with class-validator decorators for all input
