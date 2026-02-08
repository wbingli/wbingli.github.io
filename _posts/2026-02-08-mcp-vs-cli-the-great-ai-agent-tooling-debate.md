---
title: "MCP vs CLI: the great AI agent tooling debate"
date: 2026-02-08
categories: ai tools architecture
layout: post
---

How should AI agents interact with external services? This question has split the developer community into two camps, and the answer has real implications for how we build and use AI coding assistants.

## The two approaches

When you want an AI agent like Claude Code to interact with Jira, Confluence, Jenkins, or any external service, there are two fundamentally different architectures:

**MCP (Model Context Protocol):** A structured protocol where tool definitions, input schemas, and output formats are loaded into the agent's context. The agent calls typed functions with validated parameters.

**CLI + Skills:** The agent runs command-line tools via bash, guided by skill prompts that teach it the right commands. Just like a human developer would.

I've been running both approaches side by side and the tradeoffs are more nuanced than I expected.

## What OpenClaw taught us

[OpenClaw][openclaw] (formerly Clawdbot), the open-source AI agent built by Peter Steinberger, made a bold design choice: **no MCP support at all**. Its underlying agent [Pi][pi-blog], built by Armin Ronacher and Mario Zechner, ships with just four tools: `read`, `write`, `edit`, and `bash`.

Their reasoning is compelling:

- **Context bloat is real.** MCP front-loads all tool schemas into the context window every session. Connect six MCP servers with 25 tools each and you have 150 tool definitions loaded before the agent does anything useful. [Benchmarks][benchmark] show MCP consuming 43x more tokens than equivalent CLI approaches.
- **LLMs are good at writing code, so let them.** Instead of pre-built integrations, Pi encourages the agent to write its own scripts. Need to query Confluence? The agent writes a curl command, runs it, and parses the output.
- **CLI is self-documenting.** A CLI tool has `--help`. The agent discovers capabilities on demand, paying context cost only when it actually needs the tool.

The outcome? OpenClaw feels fast and token-efficient in long-running workflows. Power users love it. But it sacrifices predictability -- the agent might write a slightly different script each time, making behavior harder to audit.

## The tradeoffs, honestly

| Dimension | MCP | CLI + Skills |
|---|---|---|
| Context cost | High upfront (all schemas loaded) | Low (on-demand via `--help` or skill) |
| Predictability | High (structured I/O schemas) | Lower (agent improvises) |
| Discoverability | Automatic (tools in tool list) | Manual (agent must be told) |
| Error handling | Structured error responses | Raw stderr parsing |
| Auth/permissions | Built into protocol | You manage it yourself |
| Enterprise governance | Auditable, permissioned | Harder to constrain |
| Flexibility | Limited to what MCP exposes | Full CLI power, any flag, any pipe |
| Maintenance | Server must be running | CLI just needs to be installed |

## The case for CLI + Skills

Having used [jira-cli][jira-cli] alongside Jira MCP, the CLI approach has a surprising advantage: it's how developers already work. The mental model is simple -- teach the AI what you already know.

A skill is essentially a prompt that says: "Here's the CLI tool, here are the common patterns, here's how to interpret the output." The agent reads it, runs the commands, and gets results. No server process, no schema overhead, no version compatibility issues.

For tools with mature CLIs -- Jira, Jenkins, AWS, Git, Docker -- this works remarkably well. The agent already knows bash. It can pipe, grep, and jq its way through any output format.

## The case for keeping MCP

But MCP earns its place for certain categories:

- **Stateful interactions** like browser automation, where you need persistent sessions and complex state management
- **Complex APIs** like Notion's nested block structure, where structured schemas prevent expensive mistakes
- **Enterprise compliance** where every tool invocation needs to be auditable and permissioned

## The hybrid approach

After living with both, I think the answer isn't either/or. It's about matching the approach to the tool:

- **Use CLI + Skills** for services with good CLIs (Jira, Jenkins, AWS, Confluence, kubectl). The agent already knows bash. A guiding skill is all it needs.
- **Keep MCP** for stateful, complex, or security-sensitive integrations where structured schemas genuinely prevent errors.
- **Use deferred tool loading** where available, so MCP tools only load into context when actually needed, not upfront.

The [trend in the ecosystem][cli-new-mcp] supports this convergence. Even Anthropic published on [code execution with MCP][anthropic-mcp] as a way to reduce context overhead by letting agents run code instead of loading all tools upfront.

## The deeper lesson

What OpenClaw got right is a philosophical point: AI agents are fundamentally code-writing machines. When we wrap everything in protocols and schemas, we're sometimes adding complexity that the agent could handle with a simple bash command.

The best tool integration is often the simplest one. A well-written skill that says "use `jira issue list --status 'In Progress'` to see active tickets" costs almost nothing in context and works just as well as a typed MCP function call.

The question isn't "MCP or CLI?" -- it's "does this integration benefit from structure, or does structure just add overhead?" Answer that honestly for each tool, and the right architecture follows.

[openclaw]: https://openclaw.ai/
[pi-blog]: https://lucumr.pocoo.org/2026/1/31/pi/
[benchmark]: https://gist.github.com/szymdzum/c3acad9ea58f2982548ef3a9b2cdccce
[jira-cli]: https://github.com/ankitpokhrel/jira-cli
[cli-new-mcp]: https://oneuptime.com/blog/post/2026-02-03-cli-is-the-new-mcp/view
[anthropic-mcp]: https://www.anthropic.com/engineering/code-execution-with-mcp
