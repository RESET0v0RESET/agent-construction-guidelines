# Agent Construction Guidelines

Practical engineering guidelines for building robust LLM agent systems.

This repository packages recurring agent-construction practices into a GitHub-skill-style guide that developers can use when designing, implementing, or reviewing LLM agents. It is meant to read like an engineering playbook, not a research appendix.

## What This Is

LLM agents combine model calls, tools, memory, state, runtime control, credentials, and monitoring. Small design choices in those areas can change whether an agent is controllable, debuggable, secure, and maintainable.

This project turns 14 recurring best practices into practical guidance:

- what each practice means
- why it matters
- what to do
- what to avoid
- what signals to check in code, configuration, traces, or framework APIs
- what implementation smells may be detectable during review

## Where The Practices Come From

The practice set is based on a multi-source synthesis of agent engineering materials, including:

- official documentation and tutorials from popular open-source LLM agent frameworks
- framework API references and source-level mechanisms such as memory modules, tool registration, recovery controls, state abstractions, and monitoring hooks
- public agent engineering, coding-agent workflow, tool-design, deployment, and security guidance from sources such as OpenAI, OWASP, Anthropic/Claude, GitHub, Cursor, Composio, and AIHES

The initial synthesis was developed in the context of a research draft on open-source LLM agent frameworks, but this repository is written as a standalone developer guide. The practices are not copied from one framework or vendor guide.

## Covered Frameworks

The framework-facing clues were informed by materials from:

- LangChain
- LangGraph
- CrewAI
- LlamaIndex
- AutoGen
- Haystack
- Semantic Kernel
- MetaGPT
- CAMEL
- AutoGPT

## Repository Layout

- `skills/agent-construction-guidelines/SKILL.md`: main skill/guideline document
- `docs/source-method.md`: how the practices were derived and scoped
- `docs/references.md`: official framework, engineering, and security references
- `docs/practice-map.md`: concise map of the 14 practices
- `docs/review-checklist.md`: practical checklist for reviewing an agent system

## Use

Use the skill when:

- designing a new agent workflow
- reviewing an agent repository
- deciding which framework abstractions to use
- writing internal agent-development standards
- translating broad safety guidance into implementation checks

The practices are recurring engineering concerns, not a strict taxonomy. They overlap in real systems. For example, memory and state often interact, but memory focuses on retained information for reasoning while state focuses on execution progress and recoverable workflow context.
