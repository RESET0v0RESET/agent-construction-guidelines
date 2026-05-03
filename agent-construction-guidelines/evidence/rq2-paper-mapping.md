# RQ2 Paper-to-Guideline Mapping

This document records how the paper's 14 agent-construction best practices are translated into developer-facing guidelines.

| BP | Paper Practice | Developer Guideline | Primary Concern | RQ3 Detectable |
|---|---|---|---|---|
| BP1 | Manage memory according to task requirements | Design memory for the task, not just chat history | Memory and information retention | Yes |
| BP2 | Provide clear and structured agent descriptions | Give every agent a clear operating description | Agent specification | Yes |
| BP3 | Equip agents with basic self-recovery capabilities | Bound execution and make recovery explicit | Recovery and control flow | Yes |
| BP4 | Keep tools atomic and limit the number of tools exposed to a single agent | Keep tools atomic and toolsets small | Tool design | Yes |
| BP5 | Use concise and explicit tool descriptions | Write tool descriptions as model-facing instructions | Tool interface quality | Yes |
| BP6 | Manage credentials through secure external configuration | Keep credentials out of source code | Secret management | Yes |
| BP7 | Incorporate explicit monitoring mechanisms into agent systems | Make agent execution observable | Monitoring and tracing | Yes |
| BP8 | Adopt consistent tool naming conventions | Name tools as stable agent-facing interfaces | Tool naming | No |
| BP9 | Design explicit and layered state management | Separate state from memory and manage it explicitly | State management | No |
| BP10 | Avoid over-engineering and keep agent systems as simple as task requirements allow | Keep the agent system as simple as the task allows | Complexity control | No |
| BP11 | Use LLMs only where model-based judgment is genuinely needed | Use LLMs only where judgment is needed | Determinism vs model use | No |
| BP12 | Decompose complex tasks into smaller, manageable, and atomic steps | Break complex tasks into small executable steps | Task decomposition | Not stable |
| BP13 | Select models according to task requirements | Select models according to task requirements | Model selection | No |
| BP14 | Start from planning before implementation | Plan before tool use, coding, or orchestration | Planning and workflow control | No |

## Inclusion Logic

Candidate practices are included when they:

1. relate to LLM-based agent construction or operation;
2. can be understood beyond a single vendor product, or can be mapped to reusable agent-construction mechanisms;
3. can be analyzed through framework documentation, APIs, examples, source-level evidence, or downstream implementation evidence.

The practices are recurring engineering concerns. They are not a strictly mutually exclusive taxonomy.

When practices overlap, distinguish them by primary engineering concern, unit of analysis, and observable implementation signal.
