---
description: "Patterns for building agentic systems: tool calling, context management, eval-driven development, multi-agent orchestration. Use when designing or implementing AI-powered product features. Source: everything-claude-code."
---

# Agentic Engineering

## Core principles

1. **Tools over prompts**: Give agents tools to ACT, not just instructions to THINK. A web search tool is more reliable than asking the LLM to recall facts.
2. **Context is everything**: Agent quality = f(context quality). Give precise, relevant context. Remove noise.
3. **Eval-driven**: Define what "correct" means BEFORE building. If you can't eval it, you can't improve it.
4. **Fail gracefully**: Agents will fail. Design for recovery (retries, fallbacks, human escalation).

## Tool design

- Tools should have clear, specific descriptions (the LLM reads them to decide when to use each)
- One tool per action (don't combine "search AND create" into one tool)
- Return structured data, not prose
- Include error information in tool responses (don't silently fail)

## Context management

- Limit context to what's relevant for the current step
- Use progressive disclosure: metadata first, full content on demand
- For long documents: chunk and retrieve, don't dump the whole thing in context
- Track token usage. Context windows are finite.

## Multi-agent patterns

- **Router**: One agent classifies the task, routes to specialist agents
- **Supervisor**: One agent delegates tasks and reviews results from worker agents
- **Pipeline**: Sequential handoff between specialized agents (research → write → review)
- **Swarm**: Multiple agents with shared state, each working independently

## Eval patterns

- Define pass/fail criteria BEFORE implementation
- Use LangSmith datasets for regression testing
- Test tool calling accuracy (does it call the right tool with the right args?)
- Test output quality (does the response answer the question?)
- Test edge cases (what happens with empty input? malformed data?)
