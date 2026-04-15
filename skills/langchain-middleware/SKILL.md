---
description: "Implement human-in-the-loop approval flows, custom middleware, and Command resume patterns with LangChain. Use when building agent workflows that require user confirmation, interrupts, or approval gates."
---

# LangChain Middleware

## When to use

When building agent workflows that pause for human approval, implement custom processing steps, or need to resume from interruption points. Relevant for any Bayes venture product with approval flows (e.g., payment confirmations, document review gates).

## Human-in-the-loop pattern

```python
from langgraph.graph import StateGraph
from langgraph.types import interrupt, Command

def approval_node(state):
    """Pause execution and ask the human for approval."""
    decision = interrupt(
        {"question": "Approve this action?", "context": state["pending_action"]}
    )
    if decision == "approved":
        return {"status": "approved"}
    return {"status": "rejected"}
```

## Custom middleware

```python
from langchain_core.runnables import RunnableConfig

def logging_middleware(func):
    async def wrapper(state, config: RunnableConfig):
        print(f"Entering {func.__name__} with state keys: {list(state.keys())}")
        result = await func(state, config)
        print(f"Exiting {func.__name__}")
        return result
    return wrapper
```

## Command resume

```python
from langgraph.types import Command

def handle_resume(state):
    """Resume from where the agent was interrupted."""
    return Command(goto="next_step", update={"resumed": True})
```

## When NOT to use

- Simple sequential LLM calls without approval gates
- Stateless API endpoints (use standard request/response instead)
