# CLAUDE.md

This repo deploys AI agents to Tanzu Platform for Cloud Foundry using the
Agent Buildpack and MCP Gateway.

For deployment, model binding, or MCP Gateway work, use the
`tanzu-agent-deploy` skill (`.claude/skills/tanzu-agent-deploy/SKILL.md`) —
it covers:
- Agent Buildpack file layout and `cf push` deployment
- Writing/customizing AGENTS.md
- Binding chat models (Tanzu GenAI service, user-provided service, or
  OpenAI-compatible env vars) and the provider priority order
- Creating and configuring an MCP Gateway (on-platform and off-platform
  MCP servers)
- Connecting an agent to MCP servers via the gateway
- A troubleshooting checklist

[README.md](README.md) has the same material with extra prose/links if
more depth is needed.

For creating a *new* MCP server (Spring AI 2.0, Streamable-HTTP, `@McpTool`
tools in its own subdirectory), use the `spring-ai-mcp-server` skill
(`.claude/skills/spring-ai-mcp-server/SKILL.md`).
