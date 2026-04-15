---
description: "Package version and dependency management reference for LangChain ecosystem (Python + TypeScript). Use during planning and implementation to ensure correct versions and avoid conflicts."
---

# LangChain Dependencies

## Python packages (as of April 2026)

```
langchain>=0.3
langchain-core>=0.3
langchain-anthropic>=0.3
langchain-openai>=0.3
langchain-community>=0.3
langgraph>=0.3
langgraph-checkpoint-postgres>=2.0
langsmith>=0.2
```

## Common conflicts to avoid

- Do NOT mix langchain v0.2 with langgraph v0.3. They are incompatible.
- Do NOT install langchain-experimental unless specifically needed. It pulls heavy deps.
- Use `langchain-anthropic` (not `langchain-community` ChatAnthropic) for Anthropic models.
- Pin `pydantic>=2.0`. LangChain v0.3 requires Pydantic v2.

## TypeScript/JavaScript packages

```
@langchain/core
@langchain/anthropic
@langchain/langgraph
```

## Installation command

```bash
pip install langchain langchain-anthropic langgraph langsmith
```
