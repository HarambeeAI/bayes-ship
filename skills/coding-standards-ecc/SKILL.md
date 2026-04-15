---
description: "Universal coding conventions for naming, readability, immutability, and code quality. Applies to TypeScript, JavaScript, React, and Node.js. Auto-invoked by the code-quality-reviewer during Stage 2 review. Source: everything-claude-code."
---

# Coding Standards

Baseline cross-project coding conventions. This is the shared quality floor for all code produced by Bayes Ship implementers.

## Naming

- Variables and functions: camelCase, descriptive. `marketSearchQuery` not `q`.
- Functions: verb-noun pattern. `fetchMarketData()` not `market()`.
- Booleans: `is/has/should` prefix. `isAuthenticated` not `flag`.
- Constants: UPPER_SNAKE_CASE. `MAX_RETRY_COUNT`.
- Types/interfaces: PascalCase. `InvoiceLineItem`.

## Immutability

Always use spread operator for updates. Never mutate directly.

```typescript
// GOOD
const updated = { ...user, name: 'New Name' }
const newItems = [...items, newItem]

// BAD — never do this
user.name = 'New Name'
items.push(newItem)
```

## Functions

- Single responsibility. One function does one thing.
- Maximum 20 lines. If longer, extract helpers.
- Pure functions preferred (no side effects).
- Always type parameters and return values in TypeScript.

## Error handling

- Use typed errors, not string messages.
- Catch at the boundary (controller/route), not deep in business logic.
- Always log the original error before wrapping.

## Imports

- Group: external libs → internal modules → types → styles.
- No circular imports. If two modules import each other, extract shared code.
- Prefer named exports over default exports (better refactoring).

## Tests

- Test behavior, not implementation.
- Arrange-Act-Assert pattern.
- Descriptive test names: `"returns empty array when no invoices match filter"` not `"test filter"`.

## Code review checklist (for Stage 2 reviewer)

1. Naming follows conventions above?
2. No direct mutation of objects or arrays?
3. Functions under 20 lines?
4. Error handling at boundaries?
5. TypeScript types on all parameters and returns?
6. Tests test behavior, not implementation details?
