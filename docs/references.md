# References

This guide uses references as engineering evidence, not as one-to-one prescriptions. A source may support a mechanism, a risk, or a review signal without defining the full practice by itself.

## Framework Documentation And APIs

- LangChain agents: https://docs.langchain.com/oss/python/langchain/agents
- LangChain tools: https://docs.langchain.com/oss/python/langchain/tools
- LangGraph memory: https://docs.langchain.com/oss/python/langgraph/add-memory
- LangGraph persistence and checkpointing: https://docs.langchain.com/oss/python/langgraph/persistence
- CrewAI agents: https://docs.crewai.com/en/concepts/agents
- CrewAI memory: https://docs.crewai.com/en/concepts/memory
- LlamaIndex agents: https://docs.llamaindex.ai/en/stable/module_guides/deploying/agents/
- LlamaIndex agent memory: https://docs.llamaindex.ai/en/stable/module_guides/deploying/agents/memory/
- AutoGen memory and RAG: https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/memory.html
- Haystack agent component: https://docs.haystack.deepset.ai/docs/agent
- Semantic Kernel planning: https://learn.microsoft.com/en-us/semantic-kernel/concepts/planning
- AutoGPT documentation: https://docs.agpt.co/

## Public Agent Engineering Guidance

- OpenAI, A Practical Guide to Building Agents: https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/
- AIHES, AI Agent Development Best Practices: https://aihes.github.io/ai-agent-best-practices/development.html
- AIHES, AI Agent Deployment and Maintenance Best Practices: https://aihes.github.io/ai-agent-best-practices/deployment.html
- CIGen, Best Practices for Production-Grade Azure AI Agents: https://www.cigen.io/insights/best-practices-for-developing-ai-agents-azure

## Coding-Agent Workflow Guidance

- GitHub, Best Practices for Using GitHub Copilot to Work on Tasks: https://docs.github.com/en/copilot/tutorials/cloud-agent/get-the-best-results
- Anthropic, Best Practices for Claude Code: https://code.claude.com/docs/en/best-practices
- Cursor, Best Practices for Coding with Agents: https://cursor.com/blog/agent-best-practices
- Cursor rules: https://docs.cursor.com/context/rules-for-ai

## Tool Design And Tool Use Guidance

- Composio, How to Build Great Tools for AI Agents: A Field Guide: https://composio.dev/blog/how-to-build-tools-for-ai-agents-a-field-guide
- Composio tool calling: https://docs.composio.dev/concepts/tool-calling
- Anthropic tool use: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/implement-tool-use
- OpenAI Agents SDK: https://platform.openai.com/docs/guides/agents-sdk/
- OpenAI Agents SDK tracing: https://openai.github.io/openai-agents-python/tracing/
- OpenAI prompt engineering guide: https://platform.openai.com/docs/guides/prompt-engineering/

## Security Guidance

- OWASP, AI Agent Security Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications

## How To Read These References

The references serve different purposes:

- framework docs show what mechanisms developers are expected to use;
- API docs and source-level mechanisms show what can be implemented or inspected;
- agent engineering guides provide reusable construction and operation practices;
- security guidance highlights risks that may not be consistently documented by agent frameworks;
- coding-agent workflow guidance is treated as contextual evidence when it maps to reusable agent-construction mechanisms;
- platform-specific deployment guidance is treated as contextual evidence unless the underlying practice generalizes beyond that platform.

Vendor-specific features are not automatically treated as general best practices. They are included only when the underlying engineering concern can be understood outside that product.
