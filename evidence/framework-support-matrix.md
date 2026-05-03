# Framework Support Clues

This file summarizes the framework-support dimensions used by the guideline document. These are evidence clues, not complete taxonomies.

## BP1 Memory Support Dimensions

- sliding-window buffering
- context truncation
- working memory
- episodic memory
- semantic memory
- procedural memory

## BP2 Agent Description Dimensions

- system prompt
- structured fields
- exposed description fields

## BP3 Self-Recovery Dimensions

- iteration/step limits
- retry/recovery handling
- termination control
- checkpoint/resume support

## BP4 Tool Ecosystem Categories

- Knowledge
- Computation
- Action
- AI Service

## BP5 Tool Description Dimensions

- custom tool support
- JSON schema support
- annotated parameter descriptions

## BP7 Monitoring Dimensions

- built-in monitoring parameters
- external monitoring APIs
- native monitoring platforms

## BP14 Planning Support Matrix

Binary support should count only when official documentation or APIs explicitly provide a module, configuration option, or end-to-end example. Custom composition alone does not count.

| Framework | Multi-Agent | Plan-and-Execute | ReAct | Reflection | Human-in-the-Loop |
|---|---|---|---|---|---|
| LangChain | Yes | No | Yes | No | Yes |
| LangGraph | Yes | Yes | Yes | Yes | Yes |
| CrewAI | Yes | Yes | No | Yes | Yes |
| LlamaIndex | No | Yes | Yes | Yes | Yes |
| AutoGen | Yes | No | Yes | Yes | Yes |
| Haystack | No | No | No | No | Yes |
| Semantic Kernel | Yes | Yes | No | No | Yes |
| MetaGPT | Yes | No | Yes | No | Yes |
| CAMEL | Yes | Yes | No | No | Yes |
| AutoGPT | No | No | No | No | No |
