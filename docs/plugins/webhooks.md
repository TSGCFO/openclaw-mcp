---
source_url: https://docs.openclaw.ai/plugins/webhooks
title: "Webhooks plugin - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/webhooks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Webhooks plugin

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Webhooks (plugin)](https://docs.openclaw.ai/plugins/webhooks#webhooks-plugin)
- [Where it runs](https://docs.openclaw.ai/plugins/webhooks#where-it-runs)
- [Configure routes](https://docs.openclaw.ai/plugins/webhooks#configure-routes)
- [Security model](https://docs.openclaw.ai/plugins/webhooks#security-model)
- [Request format](https://docs.openclaw.ai/plugins/webhooks#request-format)
- [Supported actions](https://docs.openclaw.ai/plugins/webhooks#supported-actions)
- [create\_flow](https://docs.openclaw.ai/plugins/webhooks#create_flow)
- [run\_task](https://docs.openclaw.ai/plugins/webhooks#run_task)
- [Response shape](https://docs.openclaw.ai/plugins/webhooks#response-shape)
- [Related docs](https://docs.openclaw.ai/plugins/webhooks#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/plugins/webhooks\#webhooks-plugin)  Webhooks (plugin)

The Webhooks plugin adds authenticated HTTP routes that bind external
automation to OpenClaw TaskFlows.Use it when you want a trusted system such as Zapier, n8n, a CI job, or an
internal service to create and drive managed TaskFlows without writing a custom
plugin first.

## [​](https://docs.openclaw.ai/plugins/webhooks\#where-it-runs)  Where it runs

The Webhooks plugin runs inside the Gateway process.If your Gateway runs on another machine, install and configure the plugin on
that Gateway host, then restart the Gateway.

## [​](https://docs.openclaw.ai/plugins/webhooks\#configure-routes)  Configure routes

Set config under `plugins.entries.webhooks.config`:

```
{
  plugins: {
    entries: {
      webhooks: {
        enabled: true,
        config: {
          routes: {
            zapier: {
              path: "/plugins/webhooks/zapier",
              sessionKey: "agent:main:main",
              secret: {
                source: "env",
                provider: "default",
                id: "OPENCLAW_WEBHOOK_SECRET",
              },
              controllerId: "webhooks/zapier",
              description: "Zapier TaskFlow bridge",
            },
          },
        },
      },
    },
  },
}
```

Route fields:

- `enabled`: optional, defaults to `true`
- `path`: optional, defaults to `/plugins/webhooks/<routeId>`
- `sessionKey`: required session that owns the bound TaskFlows
- `secret`: required shared secret or SecretRef
- `controllerId`: optional controller id for created managed flows
- `description`: optional operator note

Supported `secret` inputs:

- Plain string
- SecretRef with `source: "env" | "file" | "exec"`

If a secret-backed route cannot resolve its secret at startup, the plugin skips
that route and logs a warning instead of exposing a broken endpoint.

## [​](https://docs.openclaw.ai/plugins/webhooks\#security-model)  Security model

Each route is trusted to act with the TaskFlow authority of its configured
`sessionKey`.This means the route can inspect and mutate TaskFlows owned by that session, so
you should:

- Use a strong unique secret per route
- Prefer secret references over inline plaintext secrets
- Bind routes to the narrowest session that fits the workflow
- Expose only the specific webhook path you need

The plugin applies:

- Shared-secret authentication
- Request body size and timeout guards
- Fixed-window rate limiting
- In-flight request limiting
- Owner-bound TaskFlow access through `api.runtime.tasks.managedFlows.bindSession(...)`

## [​](https://docs.openclaw.ai/plugins/webhooks\#request-format)  Request format

Send `POST` requests with:

- `Content-Type: application/json`
- `Authorization: Bearer <secret>` or `x-openclaw-webhook-secret: <secret>`

Example:

```
curl -X POST https://gateway.example.com/plugins/webhooks/zapier \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_SHARED_SECRET' \
  -d '{"action":"create_flow","goal":"Review inbound queue"}'
```

## [​](https://docs.openclaw.ai/plugins/webhooks\#supported-actions)  Supported actions

The plugin currently accepts these JSON `action` values:

- `create_flow`
- `get_flow`
- `list_flows`
- `find_latest_flow`
- `resolve_flow`
- `get_task_summary`
- `set_waiting`
- `resume_flow`
- `finish_flow`
- `fail_flow`
- `request_cancel`
- `cancel_flow`
- `run_task`

### [​](https://docs.openclaw.ai/plugins/webhooks\#create_flow)  `create_flow`

Creates a managed TaskFlow for the route’s bound session.Example:

```
{
  "action": "create_flow",
  "goal": "Review inbound queue",
  "status": "queued",
  "notifyPolicy": "done_only"
}
```

### [​](https://docs.openclaw.ai/plugins/webhooks\#run_task)  `run_task`

Creates a managed child task inside an existing managed TaskFlow.Allowed runtimes are:

- `subagent`
- `acp`

Example:

```
{
  "action": "run_task",
  "flowId": "flow_123",
  "runtime": "acp",
  "childSessionKey": "agent:main:acp:worker",
  "task": "Inspect the next message batch"
}
```

## [​](https://docs.openclaw.ai/plugins/webhooks\#response-shape)  Response shape

Successful responses return:

```
{
  "ok": true,
  "routeId": "zapier",
  "result": {}
}
```

Rejected requests return:

```
{
  "ok": false,
  "routeId": "zapier",
  "code": "not_found",
  "error": "TaskFlow not found.",
  "result": {}
}
```

The plugin intentionally scrubs owner/session metadata from webhook responses.

## [​](https://docs.openclaw.ai/plugins/webhooks\#related-docs)  Related docs

- [Plugin runtime SDK](https://docs.openclaw.ai/plugins/sdk-runtime)
- [Hooks and webhooks overview](https://docs.openclaw.ai/automation/hooks)
- [CLI webhooks](https://docs.openclaw.ai/cli/webhooks)

[Google Meet plugin](https://docs.openclaw.ai/plugins/google-meet) [Voice call](https://docs.openclaw.ai/plugins/voice-call)

Ctrl+I