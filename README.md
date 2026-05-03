# Agent Construction Guidelines

Practical guidelines for building robust LLM agent systems.

This project translates 14 agent-construction best practices from a TOSEM-oriented empirical study into a developer-facing skill/guideline document. The practices are derived from multi-source qualitative synthesis across open-source LLM agent frameworks, framework APIs and source-level mechanisms, and public engineering/security guidance.

## Contents

- `skills/agent-construction-guidelines/SKILL.md`: the main guideline document
- `evidence/rq2-paper-mapping.md`: mapping from paper BP definitions to developer-facing rules
- `evidence/detectable-defects.md`: which BPs are operationalized as downstream construction defects
- `evidence/framework-support-matrix.md`: framework-support clues used by the paper

## Research Scope

The study covers ten popular open-source LLM agent frameworks:

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

## Use

Use the skill when designing, implementing, reviewing, or debugging LLM-based agents, tools, memory, recovery, monitoring, model selection, and multi-agent workflows.

The guidelines are engineering concerns rather than a mutually exclusive taxonomy. Some practices can be detected as downstream construction defects; others remain useful design guidance but are not stable enough for repository-level defect detection.
