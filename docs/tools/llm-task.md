---
source_url: https://docs.openclaw.ai/tools/llm-task
title: "LLM task - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/llm-task#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

LLM task

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Enable the plugin](https://docs.openclaw.ai/tools/llm-task#enable-the-plugin)
- [Config (optional)](https://docs.openclaw.ai/tools/llm-task#config-optional)
- [Tool parameters](https://docs.openclaw.ai/tools/llm-task#tool-parameters)
- [Output](https://docs.openclaw.ai/tools/llm-task#output)
- [Example: Lobster workflow step](https://docs.openclaw.ai/tools/llm-task#example-lobster-workflow-step)
- [Safety notes](https://docs.openclaw.ai/tools/llm-task#safety-notes)
- [Related](https://docs.openclaw.ai/tools/llm-task#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`llm-task` is an **optional plugin tool** that runs a JSON-only LLM task and
returns structured output (optionally validated against JSON Schema).This is ideal for workflow engines like Lobster: you can add a single LLM step
without writing custom OpenClaw code for each workflow.

## [​](https://docs.openclaw.ai/tools/llm-task\#enable-the-plugin)  Enable the plugin

1. Enable the plugin:

```
{
  "plugins": {
    "entries": {
      "llm-task": { "enabled": true }
    }
  }
}
```

2. Allowlist the tool (it is registered with `optional: true`):

```
{
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "tools": { "allow": ["llm-task"] }\
      }\
    ]
  }
}
```

## [​](https://docs.openclaw.ai/tools/llm-task\#config-optional)  Config (optional)

```
{
  "plugins": {
    "entries": {
      "llm-task": {
        "enabled": true,
        "config": {
          "defaultProvider": "openai-codex",
          "defaultModel": "gpt-5.5",
          "defaultAuthProfileId": "main",
          "allowedModels": ["openai/gpt-5.4"],
          "maxTokens": 800,
          "timeoutMs": 30000
        }
      }
    }
  }
}
```

`allowedModels` is an allowlist of `provider/model` strings. If set, any request
outside the list is rejected.

## [​](https://docs.openclaw.ai/tools/llm-task\#tool-parameters)  Tool parameters

- `prompt` (string, required)
- `input` (any, optional)
- `schema` (object, optional JSON Schema)
- `provider` (string, optional)
- `model` (string, optional)
- `thinking` (string, optional)
- `authProfileId` (string, optional)
- `temperature` (number, optional)
- `maxTokens` (number, optional)
- `timeoutMs` (number, optional)

`thinking` accepts the standard OpenClaw reasoning presets, such as `low` or `medium`.

## [​](https://docs.openclaw.ai/tools/llm-task\#output)  Output

Returns `details.json` containing the parsed JSON (and validates against
`schema` when provided).

## [​](https://docs.openclaw.ai/tools/llm-task\#example-lobster-workflow-step)  Example: Lobster workflow step

```
openclaw.invoke --tool llm-task --action json --args-json '{
  "prompt": "Given the input email, return intent and draft.",
  "thinking": "low",
  "input": {
    "subject": "Hello",
    "body": "Can you help?"
  },
  "schema": {
    "type": "object",
    "properties": {
      "intent": { "type": "string" },
      "draft": { "type": "string" }
    },
    "required": ["intent", "draft"],
    "additionalProperties": false
  }
}'
```

## [​](https://docs.openclaw.ai/tools/llm-task\#safety-notes)  Safety notes

- The tool is **JSON-only** and instructs the model to output only JSON (no
code fences, no commentary).
- No tools are exposed to the model for this run.
- Treat output as untrusted unless you validate with `schema`.
- Put approvals before any side-effecting step (send, post, exec).

## [​](https://docs.openclaw.ai/tools/llm-task\#related)  Related

- [Thinking levels](https://docs.openclaw.ai/tools/thinking)
- [Sub-agents](https://docs.openclaw.ai/tools/subagents)
- [Slash commands](https://docs.openclaw.ai/tools/slash-commands)

[Image generation](https://docs.openclaw.ai/tools/image-generation) [Lobster](https://docs.openclaw.ai/tools/lobster)

Ctrl+I