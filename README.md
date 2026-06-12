# Tanzu Agent Demo Kit

This repo is a starting point for Tanzu Solution Architects to rapidly build
and deploy **agentic application demos** on Tanzu Platform for Cloud Foundry.
The goal of every demo built from this repo is the same: show a customer how
the **Agent Buildpack** and **MCP Gateway** let them stand up a custom agent —
with its own tools and behavior — in a handful of `cf` commands.

It's designed to be used with [Claude Code](https://claude.com/claude-code)
running in this directory. The `.claude/skills/` folder contains two project
skills that Claude Code uses automatically:

- **`spring-ai-mcp-server`** — scaffolds a Java/Spring AI MCP tool server
  (Streamable-HTTP, `@McpTool`-annotated tools, sample data) in its own
  subdirectory.
- **`tanzu-agent-deploy`** — deploys agents and MCP servers to Cloud Foundry,
  writes/edits `AGENTS.md` and `manifest.yaml`, binds a chat model, and wires
  everything together through an MCP Gateway.

You don't need to read the skills yourself — just describe what you want and
Claude Code will use them. They're referenced below if you want the details.

## Prerequisites

- Claude Code CLI, launched from the root of this repo (so the skills load)
- `cf` CLI, logged into a Tanzu Platform for Cloud Foundry foundation with:
  - the Agent Buildpack available
  - the `mcp-gateway` service available in the target marketplace
  - a GenAI service instance (or credentials for an OpenAI-compatible model)
- Java 21 + Maven, to build and smoke-test MCP servers locally before deploying

## The demo workflow

Each step below is just a prompt to Claude Code — it does the work using the
skills in this repo.

### 1. Brainstorm a demo

Describe the customer's application domain and ask for ideas. A good demo
proposal includes: **two or more MCP servers** (each exposing a handful of
tools backed by realistic sample data) and an **`AGENTS.md`** that gives the
agent a persona and behavioral guidelines that lead to interesting output.

> *"Our customer is a regional airline focused on crew scheduling and
> on-time performance. Brainstorm 2-3 demo agent ideas built on sample data —
> each should use at least two MCP servers and an AGENTS.md that makes the
> agent's responses compelling in a live demo."*

### 2. Build the chosen demo

Pick one of the proposals and ask Claude Code to implement it. It will create
one Maven subdirectory per MCP server (via the `spring-ai-mcp-server` skill)
plus `AGENTS.md` and `manifest.yaml` for the agent (via the
`tanzu-agent-deploy` skill).

> *"Let's build option 2. Implement the MCP servers with sample data and
> write the AGENTS.md."*

Ask Claude Code to build and run each MCP server locally first, and to
exercise its tools via the Streamable-HTTP endpoint, before moving on to
deployment.

### 3. Deploy the MCP servers and wire up the gateway

Ask Claude Code to push the MCP server(s) to Cloud Foundry, create (or reuse)
an MCP Gateway service instance, and register each server with it.

> *"Deploy the MCP servers to Cloud Foundry and wire them up to an MCP
> Gateway."*

### 4. Deploy the agent and connect everything

Ask Claude Code to push the agent app with the Agent Buildpack, bind a chat
model, and connect the agent to the MCP servers through the gateway.

> *"Deploy the agent with the agent buildpack, bind it to our GenAI service,
> and connect it to the MCP servers through the gateway."*

After this step the agent should be out of degraded mode and able to call
tools on every MCP server — that's the demo.

## Repo layout (after a demo is built)

```
.
├── AGENTS.md                  # agent system prompt
├── manifest.yaml              # CF manifest for the agent (agent_buildpack)
├── <feature>-mcp-server/      # one subdirectory per MCP server
│   ├── pom.xml
│   ├── manifest.yaml
│   └── src/main/java/...
└── <feature2>-mcp-server/
```

## Skills reference

- [`.claude/skills/spring-ai-mcp-server/SKILL.md`](.claude/skills/spring-ai-mcp-server/SKILL.md) —
  MCP server project layout, pom.xml, tool annotations, local testing.
- [`.claude/skills/tanzu-agent-deploy/SKILL.md`](.claude/skills/tanzu-agent-deploy/SKILL.md) —
  Agent Buildpack basics, AGENTS.md, model binding (3 options + priority
  order), MCP Gateway setup (on/off-platform servers), connecting an agent to
  the gateway, and a troubleshooting checklist.

## Tips for good demos

- Bake realistic sample data directly into each MCP server so the demo runs
  without external dependencies or network access.
- Use `AGENTS.md` to steer the agent toward demo-friendly behavior: combining
  data from multiple tools, formatting results clearly, and proactively
  surfacing the kind of insight the customer cares about.
- Keep each MCP server focused on a handful of well-named tools — it keeps
  the tool-call trace easy to narrate during a live demo.
- After any new MCP Gateway binding or model binding change, `cf restage` the
  agent — bindings are only discovered at startup.
