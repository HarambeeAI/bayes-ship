---
description: "Build agents with LangChain's create_agent, tools, structured output, and middleware. Use when implementing AI agent features with the LangChain framework. Covers tool calling, agent loops, and integration patterns."
---

# LangChain Fundamentals

## When to use

When building AI-powered features that require tool-calling agents, structured LLM output, or LangChain middleware patterns. Applicable during execution phase tasks tagged with AI/agent functionality.

## Core patterns

### Agent creation (Python)

```python
from langchain_anthropic import ChatAnthropic
from langchain.agents import create_agent

llm = ChatAnthropic(model="claude-sonnet-4-20250514")
agent = create_agent(
    model=llm,
    tools=[tool_a, tool_b],
    system_prompt="You are a helpful assistant."
)
result = agent.invoke({"messages": [{"role": "user", "content": "..."}]})
```

### Tool definition

```python
from langchain_core.tools import tool

@tool
def search_database(query: str) -> str:
    """Search the product database. Use when user asks about products."""
    # Implementation
    return results
```

### Structured output

```python
from pydantic import BaseModel

class Analysis(BaseModel):
    summary: str
    key_points: list[str]
    confidence: float

structured_llm = llm.with_structured_output(Analysis)
result = structured_llm.invoke("Analyze this document...")
```

### LCEL (LangChain Expression Language)

Always prefer LCEL for chains:

```python
from langchain_core.runnables import RunnablePassthrough

chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | output_parser
)
```

## Dependencies

```
langchain>=0.3
langchain-anthropic>=0.3
langchain-core>=0.3
```

## Reference

- Docs: https://docs.langchain.com
- API: https://reference.langchain.com/python
