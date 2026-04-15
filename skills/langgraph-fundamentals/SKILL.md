---
description: "Build stateful agent workflows with LangGraph's StateGraph, nodes, edges, and state reducers. Use when implementing multi-step AI workflows, cyclic graphs, or complex agent orchestration."
---

# LangGraph Fundamentals

## When to use

When building multi-step AI workflows with branching logic, cycles, or state that persists across steps. Use LangGraph (not plain LangChain) when you need: stateful workflows, cyclic graphs, multi-agent systems, or human-in-the-loop.

## Core pattern: StateGraph

```python
from langgraph.graph import StateGraph, START, END
from typing_extensions import TypedDict, Annotated
import operator

# 1. Define state (what data flows through the graph)
class State(TypedDict):
    messages: Annotated[list, operator.add]  # append mode
    step_count: int

# 2. Define nodes (each is a function that takes state, returns partial state)
def process(state: State) -> dict:
    return {"step_count": state["step_count"] + 1}

def decide(state: State) -> dict:
    return {"messages": ["Decision made"]}

# 3. Build graph
graph = StateGraph(State)
graph.add_node("process", process)
graph.add_node("decide", decide)
graph.add_edge(START, "process")
graph.add_edge("process", "decide")
graph.add_edge("decide", END)

# 4. Compile and invoke
app = graph.compile()
result = app.invoke({"messages": [], "step_count": 0})
```

## State reducers

- `Annotated[list, operator.add]` — append items to a list (most common for messages)
- `Annotated[int, operator.add]` — sum integers
- Plain types — overwrite (last write wins)

## Conditional edges

```python
def route(state: State) -> str:
    if state["step_count"] > 3:
        return "end"
    return "continue"

graph.add_conditional_edges("process", route, {"continue": "process", "end": END})
```

## Anti-patterns

- Do NOT put business logic in edge conditions. Edges route; nodes compute.
- Do NOT use global state. All state flows through the State TypedDict.
- Do NOT skip type annotations. LangGraph validates state types.
