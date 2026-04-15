---
description: "Eval-driven development framework for AI features. Define pass/fail criteria, run evaluations, document results. Use during QA phase for AI-powered product features. Source: everything-claude-code."
---

# Agent Eval

## When to use

When the product has AI-powered features that produce non-deterministic output. Standard unit tests verify code logic; agent evals verify AI behavior.

## Eval workflow

1. **Define criteria**: What does "correct" look like? Be specific.
   - "Returns relevant documents" → "Top 3 results include at least one document from the correct legal category"
   - "Good summary" → "Summary contains the case name, ruling, and at least 2 key arguments"

2. **Create dataset**: Build a set of (input, expected_output) pairs.
   ```python
   eval_dataset = [
       {"input": "Find cases about land disputes in Nairobi", "expected": "must reference Land Act"},
       {"input": "What is the penalty for tax evasion?", "expected": "must cite Income Tax Act s.100"}
   ]
   ```

3. **Run eval**: Execute the agent on each input. Compare output to expected.

4. **Score**: Pass/fail per criterion. Overall pass rate.

5. **Iterate**: If pass rate < threshold, adjust prompts/tools/retrieval and re-eval.

## What to eval in a RAG system

- **Retrieval**: Are the right documents retrieved? (precision + recall)
- **Generation**: Does the answer use the retrieved documents? (faithfulness)
- **Relevance**: Does the answer address the question? (answer relevance)

## Anti-patterns

- "It seems to work" is NOT an eval
- Testing on 1-2 examples is NOT an eval (minimum 20 for meaningful signal)
- Evaluating only happy paths (must include edge cases, empty inputs, adversarial inputs)
