# Detectable Construction Defects

A construction defect is an observable violation of an agent-construction best practice that may weaken robustness, maintainability, security, observability, or controllability.

It is not the same as an observed runtime failure.

## Operationalized in RQ3

| BP | Defect Category | Detectable Clues |
|---|---|---|
| BP1 | Memory management | Over-reliance on trimming/deletion with limited information-retention strategies such as summarization |
| BP2 | Agent description | Missing goal, missing backstory/context, missing success criteria, missing example, semantic mismatch among description components |
| BP3 | Self-recovery | Missing iteration limits, missing retry/recovery, missing termination control, missing checkpointing |
| BP4 | Tool design | Excessive tool count, overlapping tool functions |
| BP5 | Tool descriptions | Missing explicit input/output specification, overly long descriptions |
| BP6 | Credential management | Hard-coded credentials, exposed API keys/tokens/secrets |
| BP7 | Monitoring | Absence of monitoring, tracing, or logging mechanisms |

## Not Currently Operationalized

| BP | Reason |
|---|---|
| BP8 | Tool naming quality is difficult to judge reliably without task semantics and naming context |
| BP9 | State design is framework- and application-specific |
| BP10 | Over-engineering depends on requirements and system context |
| BP11 | Whether an LLM call is necessary depends on task semantics and design intent |
| BP12 | Appropriate task granularity depends on task complexity |
| BP13 | Model fit requires workload, user, evaluation, cost, and deployment context |
| BP14 | Planning can be implicit, external, or task-dependent, making repository-level detection unstable |
