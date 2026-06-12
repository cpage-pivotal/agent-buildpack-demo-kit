# CLAUDE.md

This repo is a kit for Tanzu Solution Architects to build and deploy AI agent
demos on Tanzu Platform for Cloud Foundry using the Agent Buildpack and MCP
Gateway. [README.md](README.md) describes the end-to-end demo workflow
(brainstorm → build MCP servers + AGENTS.md → deploy MCP servers behind a
gateway → deploy the agent).

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

For creating a *new* MCP server (Spring AI 2.0, Streamable-HTTP, `@McpTool`
tools in its own subdirectory), use the `spring-ai-mcp-server` skill
(`.claude/skills/spring-ai-mcp-server/SKILL.md`).
