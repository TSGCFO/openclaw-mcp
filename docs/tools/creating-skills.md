---
source_url: https://docs.openclaw.ai/tools/creating-skills#creating-skills
title: "Creating skills - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/creating-skills#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Skills

Creating skills

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Create your first skill](https://docs.openclaw.ai/tools/creating-skills#create-your-first-skill)
- [Skill metadata reference](https://docs.openclaw.ai/tools/creating-skills#skill-metadata-reference)
- [Best practices](https://docs.openclaw.ai/tools/creating-skills#best-practices)
- [Where skills live](https://docs.openclaw.ai/tools/creating-skills#where-skills-live)
- [Related](https://docs.openclaw.ai/tools/creating-skills#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Skills teach the agent how and when to use tools. Each skill is a directory
containing a `SKILL.md` file with YAML frontmatter and markdown instructions.For how skills are loaded and prioritized, see [Skills](https://docs.openclaw.ai/tools/skills).

## [​](https://docs.openclaw.ai/tools/creating-skills\#create-your-first-skill)  Create your first skill

1

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Create the skill directory

Skills live in your workspace. Create a new folder:

```
mkdir -p ~/.openclaw/workspace/skills/hello-world
```

2

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Write SKILL.md

Create `SKILL.md` inside that directory. The frontmatter defines metadata,
and the markdown body contains instructions for the agent.

```
---
name: hello-world
description: A simple skill that says hello.
---

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
```

Use hyphen-case with lowercase letters, digits, and hyphens for the skill
`name`. Keep the folder name and frontmatter `name` aligned.

3

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Add tools (optional)

You can define custom tool schemas in the frontmatter or instruct the agent
to use existing system tools (like `exec` or `browser`). Skills can also
ship inside plugins alongside the tools they document.

4

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Load the skill

Start a new session so OpenClaw picks up the skill:

```
# From chat
/new

# Or restart the gateway
openclaw gateway restart
```

Verify the skill loaded:

```
openclaw skills list
```

5

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Test it

Send a message that should trigger the skill:

```
openclaw agent --message "give me a greeting"
```

Or just chat with the agent and ask for a greeting.

## [​](https://docs.openclaw.ai/tools/creating-skills\#skill-metadata-reference)  Skill metadata reference

The YAML frontmatter supports these fields:

| Field | Required | Description |
| --- | --- | --- |
| `name` | Yes | Unique identifier using lowercase letters, digits, and hyphens |
| `description` | Yes | One-line description shown to the agent |
| `metadata.openclaw.os` | No | OS filter (`["darwin"]`, `["linux"]`, etc.) |
| `metadata.openclaw.requires.bins` | No | Required binaries on PATH |
| `metadata.openclaw.requires.config` | No | Required config keys |

## [​](https://docs.openclaw.ai/tools/creating-skills\#best-practices)  Best practices

- **Be concise** — instruct the model on _what_ to do, not how to be an AI
- **Safety first** — if your skill uses `exec`, ensure prompts don’t allow arbitrary command injection from untrusted input
- **Test locally** — use `openclaw agent --message "..."` to test before sharing
- **Use ClawHub** — browse and contribute skills at [ClawHub](https://clawhub.ai/)

## [​](https://docs.openclaw.ai/tools/creating-skills\#where-skills-live)  Where skills live

| Location | Precedence | Scope |
| --- | --- | --- |
| `\<workspace\>/skills/` | Highest | Per-agent |
| `\<workspace\>/.agents/skills/` | High | Per-workspace agent |
| `~/.agents/skills/` | Medium | Shared agent profile |
| `~/.openclaw/skills/` | Medium | Shared (all agents) |
| Bundled (shipped with OpenClaw) | Low | Global |
| `skills.load.extraDirs` | Lowest | Custom shared folders |

## [​](https://docs.openclaw.ai/tools/creating-skills\#related)  Related

- [Skills reference](https://docs.openclaw.ai/tools/skills) — loading, precedence, and gating rules
- [Skills config](https://docs.openclaw.ai/tools/skills-config) — `skills.*` config schema
- [ClawHub](https://docs.openclaw.ai/tools/clawhub) — public skill registry
- [Building Plugins](https://docs.openclaw.ai/plugins/building-plugins) — plugins can ship skills

[Skills](https://docs.openclaw.ai/tools/skills) [Skills config](https://docs.openclaw.ai/tools/skills-config)

Ctrl+I