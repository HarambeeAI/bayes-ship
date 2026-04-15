---
description: "AI-specific regression testing for model output drift, prompt sensitivity, and embedding consistency. Use during QA phase for products with AI features. Source: everything-claude-code."
---

# AI Regression Testing

## Why standard tests aren't enough

Standard unit tests verify deterministic logic. AI features produce non-deterministic output. A model update, prompt change, or embedding version bump can silently degrade quality without any test failing.

## What to regression test

1. **Model output drift**: Same input produces substantially different output after model/prompt changes
   - Baseline: record outputs for a fixed set of inputs
   - On each change: re-run and compare (semantic similarity, not exact match)
   - Alert if similarity drops below threshold

2. **Prompt sensitivity**: Small prompt changes cause large output changes
   - Test multiple phrasings of the same question
   - Verify consistent answers across phrasings

3. **Embedding consistency**: Vector search results change after re-indexing
   - Baseline: record top-K results for test queries
   - After re-index: compare overlap (should be >80% for same data)

4. **Tool calling accuracy**: Agent calls the correct tool with correct arguments
   - Record expected tool call sequences for test scenarios
   - Verify the agent's actual tool calls match

## Implementation pattern

```python
def test_invoice_search_regression():
    baseline_results = load_baseline("invoice_search")
    current_results = agent.search("overdue invoices in March")
    overlap = len(set(baseline_results) & set(current_results)) / len(baseline_results)
    assert overlap >= 0.8, f"Search regression: only {overlap:.0%} overlap with baseline"
```

## When to run

- After any model version change
- After any prompt modification
- After re-indexing embeddings
- After changing chunking strategy or retrieval parameters
