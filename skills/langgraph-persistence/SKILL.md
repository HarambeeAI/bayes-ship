---
description: "Implement checkpointers, thread_id-based memory, and cross-thread persistence for LangGraph agents. Use when building agents that remember conversation history or need durable execution across sessions."
---

# LangGraph Persistence

## When to use

When your agent needs to remember previous interactions (conversation memory), resume from interrupts, or share state across sessions.

## In-memory checkpointer (development)

```python
from langgraph.checkpoint.memory import InMemorySaver

checkpointer = InMemorySaver()
app = graph.compile(checkpointer=checkpointer)

# Thread-based memory
config = {"configurable": {"thread_id": "user-123"}}
result = app.invoke({"messages": [HumanMessage("Hello")]}, config)
# Next call with same thread_id continues the conversation
result = app.invoke({"messages": [HumanMessage("What did I say?")]}, config)
```

## PostgreSQL checkpointer (production)

```python
from langgraph.checkpoint.postgres import PostgresSaver

checkpointer = PostgresSaver.from_conn_string("postgresql://...")
app = graph.compile(checkpointer=checkpointer)
```

## Cross-thread memory (shared knowledge)

```python
from langgraph.store.memory import InMemoryStore

store = InMemoryStore()
# Store facts accessible across all threads
store.put(("user", "preferences"), "theme", {"value": "dark"})
```

## Rules

- Always use `thread_id` for per-user conversations
- Use PostgreSQL checkpointer in production (InMemorySaver loses data on restart)
- Store user preferences in the Store, not the checkpointer (checkpointer is for conversation state)
