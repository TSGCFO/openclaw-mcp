---
source_url: https://docs.openclaw.ai/gateway/openresponses-http-api
title: "OpenResponses API - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/openresponses-http-api#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Protocols and APIs

OpenResponses API

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Authentication, security, and routing](https://docs.openclaw.ai/gateway/openresponses-http-api#authentication-security-and-routing)
- [Session behavior](https://docs.openclaw.ai/gateway/openresponses-http-api#session-behavior)
- [Request shape (supported)](https://docs.openclaw.ai/gateway/openresponses-http-api#request-shape-supported)
- [Items (input)](https://docs.openclaw.ai/gateway/openresponses-http-api#items-input)
- [message](https://docs.openclaw.ai/gateway/openresponses-http-api#message)
- [function\_call\_output (turn-based tools)](https://docs.openclaw.ai/gateway/openresponses-http-api#function_call_output-turn-based-tools)
- [reasoning and item\_reference](https://docs.openclaw.ai/gateway/openresponses-http-api#reasoning-and-item_reference)
- [Tools (client-side function tools)](https://docs.openclaw.ai/gateway/openresponses-http-api#tools-client-side-function-tools)
- [Images (input\_image)](https://docs.openclaw.ai/gateway/openresponses-http-api#images-input_image)
- [Files (input\_file)](https://docs.openclaw.ai/gateway/openresponses-http-api#files-input_file)
- [File + image limits (config)](https://docs.openclaw.ai/gateway/openresponses-http-api#file-%2B-image-limits-config)
- [Streaming (SSE)](https://docs.openclaw.ai/gateway/openresponses-http-api#streaming-sse)
- [Usage](https://docs.openclaw.ai/gateway/openresponses-http-api#usage)
- [Errors](https://docs.openclaw.ai/gateway/openresponses-http-api#errors)
- [Examples](https://docs.openclaw.ai/gateway/openresponses-http-api#examples)
- [Related](https://docs.openclaw.ai/gateway/openresponses-http-api#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw’s Gateway can serve an OpenResponses-compatible `POST /v1/responses` endpoint.This endpoint is **disabled by default**. Enable it in config first.

- `POST /v1/responses`
- Same port as the Gateway (WS + HTTP multiplex): `http://<gateway-host>:<port>/v1/responses`

Under the hood, requests are executed as a normal Gateway agent run (same codepath as
`openclaw agent`), so routing/permissions/config match your Gateway.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#authentication-security-and-routing)  Authentication, security, and routing

Operational behavior matches [OpenAI Chat Completions](https://docs.openclaw.ai/gateway/openai-http-api):

- use the matching Gateway HTTP auth path:
  - shared-secret auth (`gateway.auth.mode="token"` or `"password"`): `Authorization: Bearer <token-or-password>`
  - trusted-proxy auth (`gateway.auth.mode="trusted-proxy"`): identity-aware proxy headers from a configured trusted proxy source; same-host loopback proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`
  - private-ingress open auth (`gateway.auth.mode="none"`): no auth header
- treat the endpoint as full operator access for the gateway instance
- for shared-secret auth modes (`token` and `password`), ignore narrower bearer-declared `x-openclaw-scopes` values and restore the normal full operator defaults
- for trusted identity-bearing HTTP modes (for example trusted proxy auth or `gateway.auth.mode="none"`), honor `x-openclaw-scopes` when present and otherwise fall back to the normal operator default scope set
- select agents with `model: "openclaw"`, `model: "openclaw/default"`, `model: "openclaw/<agentId>"`, or `x-openclaw-agent-id`
- use `x-openclaw-model` when you want to override the selected agent’s backend model
- use `x-openclaw-session-key` for explicit session routing
- use `x-openclaw-message-channel` when you want a non-default synthetic ingress channel context

Auth matrix:

- `gateway.auth.mode="token"` or `"password"` \+ `Authorization: Bearer ...`
  - proves possession of the shared gateway operator secret
  - ignores narrower `x-openclaw-scopes`
  - restores the full default operator scope set:
    `operator.admin`, `operator.approvals`, `operator.pairing`,
    `operator.read`, `operator.talk.secrets`, `operator.write`
  - treats chat turns on this endpoint as owner-sender turns
- trusted identity-bearing HTTP modes (for example trusted proxy auth, or `gateway.auth.mode="none"` on private ingress)

  - honor `x-openclaw-scopes` when the header is present
  - fall back to the normal operator default scope set when the header is absent
  - only lose owner semantics when the caller explicitly narrows scopes and omits `operator.admin`

Enable or disable this endpoint with `gateway.http.endpoints.responses.enabled`.The same compatibility surface also includes:

- `GET /v1/models`
- `GET /v1/models/{id}`
- `POST /v1/embeddings`
- `POST /v1/chat/completions`

For the canonical explanation of how agent-target models, `openclaw/default`, embeddings pass-through, and backend model overrides fit together, see [OpenAI Chat Completions](https://docs.openclaw.ai/gateway/openai-http-api#agent-first-model-contract) and [Model list and agent routing](https://docs.openclaw.ai/gateway/openai-http-api#model-list-and-agent-routing).

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#session-behavior)  Session behavior

By default the endpoint is **stateless per request** (a new session key is generated each call).If the request includes an OpenResponses `user` string, the Gateway derives a stable session key
from it, so repeated calls can share an agent session.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#request-shape-supported)  Request shape (supported)

The request follows the OpenResponses API with item-based input. Current support:

- `input`: string or array of item objects.
- `instructions`: merged into the system prompt.
- `tools`: client tool definitions (function tools).
- `tool_choice`: filter or require client tools.
- `stream`: enables SSE streaming.
- `max_output_tokens`: best-effort output limit (provider dependent).
- `user`: stable session routing.

Accepted but **currently ignored**:

- `max_tool_calls`
- `reasoning`
- `metadata`
- `store`
- `truncation`

Supported:

- `previous_response_id`: OpenClaw reuses the earlier response session when the request stays within the same agent/user/requested-session scope.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#items-input)  Items (input)

### [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#message)  `message`

Roles: `system`, `developer`, `user`, `assistant`.

- `system` and `developer` are appended to the system prompt.
- The most recent `user` or `function_call_output` item becomes the “current message.”
- Earlier user/assistant messages are included as history for context.

### [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#function_call_output-turn-based-tools)  `function_call_output` (turn-based tools)

Send tool results back to the model:

```
{
  "type": "function_call_output",
  "call_id": "call_123",
  "output": "{\"temperature\": \"72F\"}"
}
```

### [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#reasoning-and-item_reference)  `reasoning` and `item_reference`

Accepted for schema compatibility but ignored when building the prompt.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#tools-client-side-function-tools)  Tools (client-side function tools)

Provide tools with `tools: [{ type: "function", function: { name, description?, parameters? } }]`.If the agent decides to call a tool, the response returns a `function_call` output item.
You then send a follow-up request with `function_call_output` to continue the turn.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#images-input_image)  Images (`input_image`)

Supports base64 or URL sources:

```
{
  "type": "input_image",
  "source": { "type": "url", "url": "https://example.com/image.png" }
}
```

Allowed MIME types (current): `image/jpeg`, `image/png`, `image/gif`, `image/webp`, `image/heic`, `image/heif`.
Max size (current): 10MB.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#files-input_file)  Files (`input_file`)

Supports base64 or URL sources:

```
{
  "type": "input_file",
  "source": {
    "type": "base64",
    "media_type": "text/plain",
    "data": "SGVsbG8gV29ybGQh",
    "filename": "hello.txt"
  }
}
```

Allowed MIME types (current): `text/plain`, `text/markdown`, `text/html`, `text/csv`,
`application/json`, `application/pdf`.Max size (current): 5MB.Current behavior:

- File content is decoded and added to the **system prompt**, not the user message,
so it stays ephemeral (not persisted in session history).
- Decoded file text is wrapped as **untrusted external content** before it is added,
so file bytes are treated as data, not trusted instructions.
- The injected block uses explicit boundary markers like
`<<<EXTERNAL_UNTRUSTED_CONTENT id="...">>>` /
`<<<END_EXTERNAL_UNTRUSTED_CONTENT id="...">>>` and includes a
`Source: External` metadata line.
- This file-input path intentionally omits the long `SECURITY NOTICE:` banner to
preserve prompt budget; the boundary markers and metadata still stay in place.
- PDFs are parsed for text first. If little text is found, the first pages are
rasterized into images and passed to the model, and the injected file block uses
the placeholder `[PDF content rendered to images]`.

PDF parsing is provided by the bundled `document-extract` plugin, which uses the
Node-friendly `pdfjs-dist` legacy build (no worker). The modern PDF.js build
expects browser workers/DOM globals, so it is not used in the Gateway.URL fetch defaults:

- `files.allowUrl`: `true`
- `images.allowUrl`: `true`
- `maxUrlParts`: `8` (total URL-based `input_file` \+ `input_image` parts per request)
- Requests are guarded (DNS resolution, private IP blocking, redirect caps, timeouts).
- Optional hostname allowlists are supported per input type (`files.urlAllowlist`, `images.urlAllowlist`).

  - Exact host: `"cdn.example.com"`
  - Wildcard subdomains: `"*.assets.example.com"` (does not match apex)
  - Empty or omitted allowlists mean no hostname allowlist restriction.
- To disable URL-based fetches entirely, set `files.allowUrl: false` and/or `images.allowUrl: false`.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#file-+-image-limits-config)  File + image limits (config)

Defaults can be tuned under `gateway.http.endpoints.responses`:

```
{
  gateway: {
    http: {
      endpoints: {
        responses: {
          enabled: true,
          maxBodyBytes: 20000000,
          maxUrlParts: 8,
          files: {
            allowUrl: true,
            urlAllowlist: ["cdn.example.com", "*.assets.example.com"],
            allowedMimes: [\
              "text/plain",\
              "text/markdown",\
              "text/html",\
              "text/csv",\
              "application/json",\
              "application/pdf",\
            ],
            maxBytes: 5242880,
            maxChars: 200000,
            maxRedirects: 3,
            timeoutMs: 10000,
            pdf: {
              maxPages: 4,
              maxPixels: 4000000,
              minTextChars: 200,
            },
          },
          images: {
            allowUrl: true,
            urlAllowlist: ["images.example.com"],
            allowedMimes: [\
              "image/jpeg",\
              "image/png",\
              "image/gif",\
              "image/webp",\
              "image/heic",\
              "image/heif",\
            ],
            maxBytes: 10485760,
            maxRedirects: 3,
            timeoutMs: 10000,
          },
        },
      },
    },
  },
}
```

Defaults when omitted:

- `maxBodyBytes`: 20MB
- `maxUrlParts`: 8
- `files.maxBytes`: 5MB
- `files.maxChars`: 200k
- `files.maxRedirects`: 3
- `files.timeoutMs`: 10s
- `files.pdf.maxPages`: 4
- `files.pdf.maxPixels`: 4,000,000
- `files.pdf.minTextChars`: 200
- `images.maxBytes`: 10MB
- `images.maxRedirects`: 3
- `images.timeoutMs`: 10s
- HEIC/HEIF `input_image` sources are accepted and normalized to JPEG before provider delivery.

Security note:

- URL allowlists are enforced before fetch and on redirect hops.
- Allowlisting a hostname does not bypass private/internal IP blocking.
- For internet-exposed gateways, apply network egress controls in addition to app-level guards.
See [Security](https://docs.openclaw.ai/gateway/security).

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#streaming-sse)  Streaming (SSE)

Set `stream: true` to receive Server-Sent Events (SSE):

- `Content-Type: text/event-stream`
- Each event line is `event: <type>` and `data: <json>`
- Stream ends with `data: [DONE]`

Event types currently emitted:

- `response.created`
- `response.in_progress`
- `response.output_item.added`
- `response.content_part.added`
- `response.output_text.delta`
- `response.output_text.done`
- `response.content_part.done`
- `response.output_item.done`
- `response.completed`
- `response.failed` (on error)

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#usage)  Usage

`usage` is populated when the underlying provider reports token counts.
OpenClaw normalizes common OpenAI-style aliases before those counters reach
downstream status/session surfaces, including `input_tokens` / `output_tokens`
and `prompt_tokens` / `completion_tokens`.

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#errors)  Errors

Errors use a JSON object like:

```
{ "error": { "message": "...", "type": "invalid_request_error" } }
```

Common cases:

- `401` missing/invalid auth
- `400` invalid request body
- `405` wrong method

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#examples)  Examples

Non-streaming:

```
curl -sS http://127.0.0.1:18789/v1/responses \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-agent-id: main' \
  -d '{
    "model": "openclaw",
    "input": "hi"
  }'
```

Streaming:

```
curl -N http://127.0.0.1:18789/v1/responses \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-agent-id: main' \
  -d '{
    "model": "openclaw",
    "stream": true,
    "input": "hi"
  }'
```

## [​](https://docs.openclaw.ai/gateway/openresponses-http-api\#related)  Related

- [OpenAI chat completions](https://docs.openclaw.ai/gateway/openai-http-api)
- [OpenAI](https://docs.openclaw.ai/providers/openai)

[OpenAI chat completions](https://docs.openclaw.ai/gateway/openai-http-api) [Tools invoke API](https://docs.openclaw.ai/gateway/tools-invoke-http-api)

Ctrl+I