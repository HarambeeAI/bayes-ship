---
description: "React and Next.js patterns for state management, component architecture, rendering, forms, and data fetching. Use when implementing frontend tasks. Source: everything-claude-code."
---

# Frontend Patterns

## Component architecture

- One component per file. File name matches component name.
- Props interface defined above the component.
- Prefer composition over prop drilling (use Context or Zustand for shared state).
- Server Components by default in Next.js App Router. Client Components only when needed (interactivity, hooks).

## State management

```typescript
// Local state for component-specific data
const [isOpen, setIsOpen] = useState(false)

// Server state for API data (use React Query / SWR)
const { data: invoices, isLoading } = useQuery({
  queryKey: ['invoices'],
  queryFn: fetchInvoices
})

// Global state for cross-component concerns (Zustand)
const useStore = create((set) => ({
  selectedInvoice: null,
  setSelected: (inv) => set({ selectedInvoice: inv })
}))
```

## Data fetching in Next.js

```typescript
// Server Component (preferred — no client JS, no loading state)
async function InvoicePage({ params }: { params: { id: string } }) {
  const invoice = await getInvoice(params.id)
  return <InvoiceDetail invoice={invoice} />
}

// Client Component (only when interactivity needed)
'use client'
function InvoiceForm() {
  const [formData, setFormData] = useState(initialState)
  // ...
}
```

## Forms

- Use Server Actions for form submissions in Next.js 15
- Validate on client (UX) AND server (security)
- Show inline errors next to the field, not in a toast

## Anti-patterns

- Do NOT use `useEffect` for data fetching in Next.js App Router (use Server Components)
- Do NOT put all state in a global store (most state is local)
- Do NOT use `any` type. Type everything.
- Do NOT skip loading and error states. Every async operation needs both.
