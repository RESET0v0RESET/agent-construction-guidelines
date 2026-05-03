---
name: agent-construction-guidelines
description: Practical guidelines for building robust LLM agent systems. Use when designing, implementing, reviewing, or debugging LLM-based agents, tools, memory, recovery, monitoring, model selection, and multi-agent workflows.
license: MIT
---

# Agent Construction Guidelines

Practical engineering guidelines for building LLM agent systems.

These guidelines are derived from a multi-source synthesis of popular open-source LLM agent frameworks, framework APIs and source-level mechanisms, and public engineering/security guidance. They are not copied from a single framework, vendor, or product manual.

Use them when:

- designing a new LLM agent system
- reviewing an existing agent implementation
- choosing framework features
- defining review checklists
- translating broad agent guidance into implementation checks

These guidelines bias toward robustness, controllability, maintainability, security, and observability. For small prototypes, apply judgment.

## Source Basis

The practices come from evidence across:

1. official documentation and tutorials of LangChain, LangGraph, CrewAI, LlamaIndex, AutoGen, Haystack, Semantic Kernel, MetaGPT, CAMEL, and AutoGPT;
2. framework API references and source-level mechanisms, including class definitions, configuration parameters, tool-registration interfaces, memory modules, recovery mechanisms, and monitoring hooks;
3. public engineering and security guidance from sources such as OWASP, OpenAI, Anthropic/Claude, GitHub, Cursor, and Composio.

A practice is included only when it relates to LLM-agent construction or operation, can be understood beyond a single vendor product, and can be mapped to reusable implementation mechanisms or observable evidence.

The initial practice set was shaped during a research study on open-source LLM agent frameworks, then rewritten here as standalone engineering guidance. The practices are recurring engineering concerns, not a mutually exclusive taxonomy.

## How To Apply

For each agent system, inspect the agent description, memory design, tool interface, execution control, configuration, observability, state handling, task decomposition, model choice, and planning workflow.

Prefer concrete implementation evidence over intent. Look for code, configuration, examples, tests, traces, and framework-supported mechanisms.

## Guideline Index

| BP | Guideline | Reviewability |
|---|---|---|
| BP1 | Design memory for the task, not just chat history | Often reviewable |
| BP2 | Give every agent a clear operating description | Often reviewable |
| BP3 | Bound execution and make recovery explicit | Often reviewable |
| BP4 | Keep tools atomic and toolsets small | Often reviewable |
| BP5 | Write tool descriptions as model-facing instructions | Often reviewable |
| BP6 | Keep credentials out of source code | Often reviewable |
| BP7 | Make agent execution observable | Often reviewable |
| BP8 | Name tools as stable agent-facing interfaces | Context-dependent |
| BP9 | Separate state from memory and manage it explicitly | Context-dependent |
| BP10 | Keep the agent system as simple as the task allows | Context-dependent |
| BP11 | Use LLMs only where judgment is needed | Context-dependent |
| BP12 | Break complex tasks into small executable steps | Context-dependent |
| BP13 | Select models according to task requirements | Context-dependent |
| BP14 | Plan before tool use, coding, or orchestration | Context-dependent |

## BP1. Design Memory for the Task, Not Just Chat History

Agent memory should match the information the task needs to retain, reuse, summarize, retrieve, or update.

### What It Means

Treat memory as a task-specific information design problem. Conversation history is only one possible memory source. Some agents need short-term buffering, some need summarized working memory, and some need episodic, semantic, or procedural memory.

### Why It Matters

Poor memory design causes agents to lose important facts, carry irrelevant context, exceed context limits, repeat work, or make decisions from stale information.

### Do

- Identify what information must survive across turns, steps, sessions, or tasks.
- Use summarization when information should be retained but compressed.
- Use retrieval when the agent needs selective access to larger stores.
- Separate temporary context from durable memory.
- Make truncation and deletion explicit design choices.

### Avoid

- Treating all memory as raw chat history.
- Relying only on trimming or deletion when key information must be preserved.
- Mixing task state, tool results, user preferences, and long-term knowledge into one unstructured message list.

### Check Signals

- Memory buffer configuration.
- Context trimming or truncation logic.
- Summarization chains or reducers.
- Checkpointers, stores, retrievers, or vector indexes.
- Clear boundaries between temporary and persistent information.

### Framework Support Clues

Look for support for sliding-window buffering, context truncation, working memory, episodic memory, semantic memory, and procedural memory. These are support indicators, not an exhaustive memory taxonomy.

### Implementation Smells

The implementation over-relies on trimming or deletion while rarely using summarization, retrieval, or other information-retention strategies.

## BP2. Give Every Agent a Clear Operating Description

Every agent should have a clear description of what it does, where it operates, and what boundaries it must respect.

### What It Means

An agent description is the model-facing operating contract. It should specify role, goal, context, constraints, success criteria, and examples where useful.

### Why It Matters

Vague descriptions lead to unstable behavior, poor tool choice, ambiguous delegation, and outputs that are hard to evaluate.

### Do

- State the agent's role and goal.
- Provide relevant backstory or operating context.
- Define constraints and behavior boundaries.
- Describe success criteria.
- Include examples for ambiguous tasks.

### Avoid

- Using only a generic role label.
- Splitting role, goal, context, and constraints into fields that contradict each other.
- Giving examples that do not match the intended task.

### Check Signals

- System prompts.
- Structured agent fields.
- Exposed description fields.
- Role, goal, backstory, constraints, success criteria, and examples.

### Framework Support Clues

Look for system prompt configuration, structured fields such as role/goal/backstory, and explicit description fields exposed by the framework.

### Implementation Smells

Common smells include missing goal, missing backstory or context, missing success criteria, missing examples for ambiguous behavior, and semantic mismatch among description components.

## BP3. Bound Execution and Make Recovery Explicit

Agent execution should be bounded, recoverable, and able to stop cleanly.

### What It Means

Agents that reason iteratively, call tools, or interact with environments need limits and recovery paths. Failures should be expected, not treated as surprises.

### Why It Matters

Without execution bounds and recovery, agents can loop, retry blindly, lose progress, hide failures, or require manual reconstruction after interruption.

### Do

- Set iteration, step, or round limits.
- Add retry handling for transient failures.
- Define termination conditions.
- Use checkpointing or resume support for long-running workflows.
- Record enough context to recover after partial failure.

### Avoid

- Infinite or unbounded loops.
- Retrying without limits or error classification.
- Treating termination as only a model decision.
- Losing all progress when a step fails.

### Check Signals

- `max_iterations`, `max_steps`, `max_rounds`, or equivalent limits.
- Retry policies.
- Error handlers.
- Termination predicates.
- Checkpointers or resume APIs.

### Framework Support Clues

Look for iteration/step limits, retry/recovery handling, termination control, and checkpoint/resume support.

### Implementation Smells

Common smells include missing iteration limits, missing retry/recovery, missing termination control, and missing checkpointing for long-running workflows.

## BP4. Keep Tools Atomic and Toolsets Small

Expose small, well-scoped tools, and avoid giving one agent more tools than it can reliably choose among.

### What It Means

A tool should represent one well-defined action. The agent should see only the tools relevant to its task and operating context.

### Why It Matters

Large or overlapping toolsets increase selection errors, ambiguous calls, harder debugging, and unnecessary attack surface.

### Do

- Design one tool around one action.
- Keep names and descriptions distinguishable.
- Split broad tools when their modes require different inputs or decisions.
- Route agents to task-specific toolsets.
- Review tool count per agent.

### Avoid

- "Do everything" tools.
- Many tools with overlapping functions.
- Exposing all available tools to every agent.
- Hiding multiple unrelated actions behind one tool name.

### Check Signals

- Number of tools exposed to one agent.
- Tool function names and descriptions.
- Tool input schemas.
- Overlapping capabilities.
- Router or role-specific tool assignment.

### Framework Support Clues

Map tools into broad ecosystem categories such as Knowledge, Computation, Action, and AI Service. Use this mapping to inspect breadth and overlap, not as a strict taxonomy.

### Implementation Smells

Common smells include excessive tool count and overlapping tool functions. As a conservative review heuristic, 20 or more tools exposed to one agent deserves closer inspection.

## BP5. Write Tool Descriptions as Model-Facing Instructions

Tool descriptions should tell the model exactly what the tool does, when to use it, and how to call it.

### What It Means

A tool description is not a normal API comment. It is part of the agent's operating interface and should guide tool selection and parameter construction.

### Why It Matters

Weak tool descriptions cause wrong tool selection, malformed arguments, incorrect assumptions about return values, and fragile tool-use behavior.

### Do

- Say what the tool does.
- Say when to use it.
- Specify required inputs and important constraints.
- Describe the return value.
- Use JSON schema or annotated parameters when available.

### Avoid

- Long narrative descriptions.
- Generic comments such as "useful helper function."
- Missing input or output expectations.
- Descriptions that duplicate the tool name without adding operational guidance.

### Check Signals

- Tool docstrings or description fields.
- JSON schema definitions.
- Annotated parameter descriptions.
- Input/output examples.
- Description length and specificity.

### Framework Support Clues

Look for custom tool support, JSON schema support, and annotated parameter descriptions.

### Implementation Smells

Common smells include missing explicit input/output specification and overly long descriptions. As a practical heuristic, descriptions over 600 characters deserve closer review.

## BP6. Keep Credentials Out of Source Code

Credentials should be provided through secure external configuration, not hard-coded in the implementation.

### What It Means

API keys, tokens, secret identifiers, and private endpoints should come from environment variables, `.env` files, secret managers, or controlled configuration channels.

### Why It Matters

Hard-coded secrets leak easily through commits, logs, examples, screenshots, package artifacts, or downstream forks.

### Do

- Load secrets from environment variables or secret managers.
- Keep `.env` files out of Git.
- Separate configuration names from secret values.
- Fail clearly when required credentials are missing.

### Avoid

- Hard-coded API keys, tokens, passwords, or private URLs.
- Example code with real credentials.
- Logging secret values.
- Treating framework compatibility with environment variables as a complete security policy.

### Check Signals

- Environment variable reads.
- Secret manager integration.
- `.gitignore` entries for local secret files.
- Credential-looking string literals.
- Logs and config files.

### Framework Support Clues

Most frameworks are compatible with externalized configuration, but only some frame it explicitly as a security concern. Check both docs and examples.

### Implementation Smells

Common smells include hard-coded credentials and exposed API keys, tokens, or secrets.

## BP7. Make Agent Execution Observable

Agent systems should expose execution status, tool interactions, and failure conditions.

### What It Means

Monitoring is part of agent construction. Developers need visibility into prompts, tool calls, parameters, errors, retries, state transitions, and outputs.

### Why It Matters

Without observability, agent failures are hard to reproduce. Prompt design, tool selection, parameter construction, external dependencies, and control flow all become guesswork.

### Do

- Enable tracing, logging, or callbacks.
- Capture tool calls and tool results.
- Record failure conditions and retry behavior.
- Expose status for long-running workflows.
- Use native or external monitoring platforms when appropriate.

### Avoid

- Silent agent execution.
- Logging only final answers.
- Swallowing tool errors.
- Relying on ad hoc print statements for production workflows.

### Check Signals

- Tracing configuration.
- Callback handlers.
- Structured logs.
- Monitoring hooks.
- External monitoring API calls.
- Native monitoring platform integration.

### Framework Support Clues

Look for built-in monitoring parameters, external monitoring APIs, and native monitoring platforms.

### Implementation Smells

Common smell: absence of monitoring, tracing, or logging mechanisms for a non-trivial agent workflow.

## BP8. Name Tools as Stable Agent-Facing Interfaces

Tool names should be short, stable, descriptive, predictable, and distinguishable.

### What It Means

Tool names are part of the agent-facing interface. A model uses names as semantic handles when deciding which action to call.

### Why It Matters

Unclear or unstable names increase tool-selection errors and make traces harder to read.

### Do

- Use verb-object names when possible.
- Keep names concise.
- Make similar tools easy to distinguish.
- Treat renaming as an interface change.

### Avoid

- Cryptic abbreviations.
- Duplicate or near-duplicate names.
- Names that describe implementation rather than action.
- Renaming tools without updating prompts, examples, and tests.

### Check Signals

- Tool name fields.
- Naming consistency across tools.
- Semantic overlap among tool names.
- Trace readability.

### Framework Support Clues

Framework-level support is usually weak. Most frameworks expose a `name` field but provide limited naming enforcement.

### Implementation Smells

This usually requires human judgment. Watch for names that are cryptic, unstable, duplicated, or hard to distinguish in traces.

## BP9. Separate State from Memory and Manage It Explicitly

State should be structured execution context, not unbounded message accumulation.

### What It Means

State tracks execution progress. Memory retains and reuses information for reasoning. Keep these responsibilities separate.

State can include task goal, current plan, progress, tool results, intermediate artifacts, pending actions, errors, and approval signals.

### Why It Matters

When state is implicit, agents lose progress, repeat steps, recover poorly, and mix workflow control with long-term memory.

### Do

- Define current execution state.
- Define session-level state.
- Define cross-session long-term memory separately.
- Store tool results and pending actions in structured fields.
- Make state transitions inspectable.

### Avoid

- Treating messages as the only state store.
- Mixing durable user knowledge with temporary execution progress.
- Hiding critical workflow status inside free-form text.

### Check Signals

- Typed state objects.
- Checkpointed workflow state.
- Structured tool-result storage.
- Pending-action or approval fields.
- Explicit state transition logic.

### Framework Support Clues

Look for graph state, workflow state, checkpointers, stores, session objects, and explicit execution context APIs.

### Implementation Smells

This usually requires human judgment. Watch for systems where execution progress, durable memory, approvals, errors, and tool results are all hidden in free-form message history.

## BP10. Keep the Agent System as Simple as the Task Allows

Use the minimum agent-system complexity needed to solve the task well.

### What It Means

Avoid speculative abstractions, premature extensibility, unused framework features, and unjustified configurability.

### Why It Matters

Over-engineered agents are harder to test, debug, monitor, secure, and maintain. Extra moving parts also increase latency and cost.

### Do

- Start with the simplest workflow that meets the task requirements.
- Add agents, tools, memory, or orchestration only when they solve a concrete problem.
- Remove unused capabilities.
- Prefer deterministic code for simple control flow.

### Avoid

- Multi-agent orchestration for tasks that need one call or one tool.
- Abstract factories and plugins before there are real variants.
- Unused memory, reflection, planning, or routing modules.
- Configuration options nobody can justify.

### Check Signals

- Unused agents, tools, memory modules, or routers.
- Deep orchestration for simple tasks.
- Configuration fields with no active use.
- Test difficulty caused by unnecessary indirection.

### Framework Support Clues

This is mainly a developer guideline. It is not usually represented as explicit framework support.

### Implementation Smells

This requires task context. Watch for unused agents, unused tools, speculative plugins, unnecessary multi-agent orchestration, and configuration that no workflow reads.

## BP11. Use LLMs Only Where Judgment Is Needed

Use model calls for semantic interpretation, open-ended reasoning, and language generation. Use deterministic code for deterministic work.

### What It Means

Routing, parameter validation, state updates, result formatting, retry triggering, and termination checks should usually be coded directly when rules are clear.

### Why It Matters

Unnecessary model calls add uncertainty, latency, cost, and failure modes.

### Do

- Use code for validation and control flow.
- Use LLMs where rules are incomplete, ambiguous, or language-heavy.
- Keep model outputs constrained when possible.
- Test deterministic components separately.

### Avoid

- Asking an LLM to do simple boolean checks.
- Letting the model decide termination when a clear predicate exists.
- Using free-form model output where a schema is enough.
- Replacing straightforward code with prompts.

### Check Signals

- LLM calls inside routing, validation, retry, or formatting paths.
- Missing schema validation.
- Deterministic conditions expressed in prompts.
- Repeated model calls for simple transformations.

### Framework Support Clues

Look for structured output support, routers, validators, middleware, and explicit control-flow APIs.

### Implementation Smells

This requires task context. Watch for model calls used for simple validation, routing, formatting, retry triggering, or termination checks that could be deterministic code.

## BP12. Break Complex Tasks Into Small Executable Steps

Complex tasks should be decomposed into smaller, manageable, and atomic steps.

### What It Means

Do not hand a complex objective to one monolithic agent call. Break it into subgoals, actions, validations, and recovery points.

### Why It Matters

Decomposition reduces cognitive load, makes progress monitorable, enables intermediate validation, and limits the blast radius of local failures.

### Do

- Split complex tasks into subgoals.
- Define expected intermediate outputs.
- Validate after important steps.
- Keep recovery local where possible.
- Use workflow or graph abstractions when decomposition becomes explicit.

### Avoid

- One giant prompt for multi-stage work.
- Hiding all progress inside a final response.
- Combining planning, tool use, validation, and reporting into one opaque step.

### Check Signals

- Subtask definitions.
- Workflow nodes or graph steps.
- Intermediate artifacts.
- Validation checkpoints.
- Local retry/recovery boundaries.

### Framework Support Clues

Framework support is partial. Look for workflow, pipeline, graph, crew/task, or plan-and-execute abstractions.

### Implementation Smells

This requires task context. Watch for one giant prompt or one opaque agent call doing planning, action, validation, and reporting together.

## BP13. Select Models According to Task Requirements

Choose models based on task fit, not only availability or headline capability.

### What It Means

Model selection should consider reasoning capability, tool-calling reliability, multimodal support, latency, cost, deployment flexibility, hallucination risk, bias, undesirable behavior, target users, and target scenarios.

### Why It Matters

A model that is too weak fails silently. A model that is too expensive or slow can make the system impractical. A model that is mismatched to users or domain risks poor behavior.

### Do

- Match reasoning difficulty to model capability.
- Check tool-calling reliability for tool-heavy agents.
- Consider latency and cost budgets.
- Consider deployment and provider constraints.
- Revisit model choice when the task or user population changes.

### Avoid

- Hard-coding one model for all tasks.
- Choosing only by benchmark reputation.
- Ignoring latency, cost, privacy, or deployment constraints.
- Using a model without validating behavior in the target workflow.

### Check Signals

- Model configuration.
- Provider and gateway integrations.
- Task-specific model routing.
- Evaluation results.
- Cost and latency constraints.

### Framework Support Clues

Look for provider integrations and gateway integrations.

### Implementation Smells

This requires workload context. Watch for one hard-coded model used for every task, no evaluation notes, no latency/cost consideration, and no tool-calling validation.

## BP14. Plan Before Tool Use, Coding, or Orchestration

Agent construction should start from explicit planning before implementation or execution.

### What It Means

Before tool use, code generation, or multi-agent orchestration, clarify the task objective, decompose subgoals, identify constraints and dependencies, and define the expected action path.

### Why It Matters

Planning reduces aimless tool use, premature implementation, missing constraints, and uncontrolled orchestration.

### Do

- Clarify the objective before execution.
- Decompose subgoals.
- Identify dependencies, constraints, and approval points.
- Define the expected action path.
- Use human-in-the-loop checkpoints for high-impact actions.

### Avoid

- Jumping directly into tool calls or code changes.
- Treating planning as optional for multi-step tasks.
- Using reflection or multi-agent coordination without a clear objective.
- Equating planning with verbose chain-of-thought exposure.

### Check Signals

- Plan-and-execute modules.
- ReAct-style action loops.
- Reflection mechanisms.
- Multi-agent coordination.
- Human-in-the-loop checkpoints.
- Explicit task plans or workflow definitions.

### Framework Support Clues

Use five documentation-derived indicators: Multi-Agent Coordination, Plan-and-Execute, ReAct, Reflection, and Human-in-the-Loop.

Count support only when official documentation or APIs explicitly provide a module, configuration option, or end-to-end example. Custom composition alone does not count.

### Implementation Smells

This requires workflow context. Watch for immediate tool use, code generation, or multi-agent orchestration without a clear objective, constraints, subgoals, or expected action path.

## Reviewability And Automation

Some practices leave strong implementation signals and can be checked during code review or partially automated scanning. Others require task context and developer judgment.

Often reviewable from code, configuration, or traces:

- BP1 memory management
- BP2 agent description
- BP3 self-recovery
- BP4 tool design
- BP5 tool descriptions
- BP6 credential management
- BP7 monitoring

Context-dependent and best reviewed with task knowledge:

- BP8 tool naming
- BP9 state management
- BP10 simplicity
- BP11 deterministic vs model-based logic
- BP12 task decomposition
- BP13 model selection
- BP14 planning
