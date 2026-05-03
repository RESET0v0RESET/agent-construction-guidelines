# Agent Review Checklist

Use this checklist when reviewing an LLM agent system. It is intentionally practical: look for concrete code, configuration, traces, tests, or framework APIs.

## Agent Specification

- Is each agent's role clear?
- Is the task goal explicit?
- Is the operating context stated?
- Are constraints and success criteria visible?
- Do examples match the intended behavior?

## Memory And State

- Does memory match the task's information needs?
- Is important information summarized, retrieved, or otherwise preserved instead of only trimmed away?
- Is execution state represented separately from long-term memory?
- Can progress, pending actions, tool results, and errors be recovered after interruption?

## Tools

- Does each tool perform one clear action?
- Is the toolset scoped to the agent's task?
- Are overlapping tools removed or made clearly distinguishable?
- Are tool names short, stable, and action-oriented?
- Do tool descriptions explain what the tool does, when to use it, required inputs, constraints, and return values?

## Runtime Control

- Are iteration, step, or round limits configured?
- Are retry policies explicit and bounded?
- Are termination conditions clear?
- Are long-running workflows checkpointed or resumable?

## Security And Configuration

- Are credentials loaded from environment variables, secret managers, or controlled configuration?
- Are `.env` files ignored?
- Are example secrets fake?
- Are secrets excluded from logs, traces, and error messages?

## Observability

- Are tool calls, tool inputs, tool outputs, errors, retries, and state transitions visible?
- Are logs structured enough for debugging?
- Is tracing or monitoring enabled for non-trivial workflows?
- Can failures be reproduced from available records?

## Model And Workflow Design

- Is the chosen model appropriate for the task's reasoning, tool use, latency, cost, modality, and deployment constraints?
- Are deterministic checks handled with code instead of model calls?
- Are complex tasks decomposed into smaller steps?
- Is there an explicit plan before tool use, code generation, or multi-agent orchestration?
- Is the system as simple as the task allows?
