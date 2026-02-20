---
title: "I made Postman work with AI agents"
date: 2026-02-20
categories: ai tools
layout: post
---

I built [pmctl][pmctl], a CLI for browsing Postman collections from the terminal. This week I turned it into something more interesting: a skill that AI coding agents can use to discover and call your APIs.

## The problem

Every team I've worked with has hundreds of Postman requests sitting in shared workspaces. When you're pair-programming with an AI agent -- Claude Code, Cursor, Codex -- and you need to hit an API, you end up copy-pasting URLs, headers, and auth tokens from Postman into your conversation. It's tedious.

What if the agent could just browse your Postman workspace itself?

## What pmctl does

It's a Python CLI (`pip install pmctl`) that wraps the Postman API:

```bash
# Browse collections
pmctl collections list

# Fuzzy search for requests
pmctl requests list -c "My API" --search "getUser"

# Get full request details (headers, body, params)
pmctl requests show "create User" -c "My API"

# Resolve environment variables to real URLs
pmctl environments show "Production" --json
```

The `--json` flag makes everything machine-readable, which is exactly what AI agents need.

## Making it agent-native

The CLI alone isn't enough. AI agents need instructions -- they need to know *when* to use a tool and *how* to use it effectively. That's what the [Agent Skills][agentskills] spec is for: a `SKILL.md` file that agents load when relevant.

I wrote one for pmctl and published it everywhere I could find:

- **[ClawHub][clawhub]** -- OpenClaw's skill registry
- **[Context7][context7]** -- skills registry with trust scoring (pmctl got a 9.4)
- **[Cursor Marketplace][cursor]** -- submitted, pending review
- **Claude Code** -- installable as a plugin marketplace
- **OpenAI Codex** -- `.agents/skills/` directory convention

The skill teaches agents workflows like "resolve a Postman `\{\{base-url\}\}` variable for a specific environment" or "construct a curl command from a Postman request." These are the patterns developers do manually dozens of times a day.

## The agent skills ecosystem is exploding

While publishing pmctl, I mapped out the current landscape. It's early 2026 and there are already:

- 96K+ skills on [SkillsMP][skillsmp]
- 5,700+ on ClawHub
- 7,000+ on [SkillHub][skillhub]

Most of these registries auto-index from GitHub. If your repo has a `skills/*/SKILL.md` file following the [Agent Skills spec][agentskills], crawlers will find it.

The interesting part: the same `SKILL.md` works across Claude Code, Codex, Cursor, Gemini CLI, and OpenCode. Write once, run everywhere. The tooling differences are just about *where* you put the file:

| Tool | Skill location |
|------|---------------|
| Claude Code | `skills/*/SKILL.md` + `.claude-plugin/` |
| Cursor | `.cursor/rules/*.mdc` or `skills/*/SKILL.md` + `.cursor-plugin/` |
| Codex | `.agents/skills/*/SKILL.md` |
| OpenClaw | `skill/SKILL.md` |

## Try it

```bash
pip install pmctl
pmctl profile add mywork --api-key "PMAK-..." --default
```

Or install the agent skill directly:

```bash
# Context7
npx ctx7 skills install /wbingli/pmctl pmctl

# Claude Code
claude plugin marketplace add wbingli/pmctl
claude plugin install pmctl@pmctl

# ClawHub
clawhub install pmctl
```

The repo is at [github.com/wbingli/pmctl][pmctl]. PRs welcome.

[pmctl]: https://github.com/wbingli/pmctl
[agentskills]: https://agentskills.io
[clawhub]: https://clawhub.ai/skills/pmctl
[context7]: https://context7.com/wbingli/pmctl
[cursor]: https://cursor.com/marketplace
[skillsmp]: https://skillsmp.com
[skillhub]: https://www.skillhub.club
