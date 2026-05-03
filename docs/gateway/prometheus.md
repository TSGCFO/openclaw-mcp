---
source_url: https://docs.openclaw.ai/gateway/prometheus
title: "Prometheus metrics - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/prometheus#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Health and diagnostics

Prometheus metrics

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/gateway/prometheus#quick-start)
- [Metrics exported](https://docs.openclaw.ai/gateway/prometheus#metrics-exported)
- [Label policy](https://docs.openclaw.ai/gateway/prometheus#label-policy)
- [PromQL recipes](https://docs.openclaw.ai/gateway/prometheus#promql-recipes)
- [Choosing between Prometheus and OpenTelemetry export](https://docs.openclaw.ai/gateway/prometheus#choosing-between-prometheus-and-opentelemetry-export)
- [Troubleshooting](https://docs.openclaw.ai/gateway/prometheus#troubleshooting)
- [Related](https://docs.openclaw.ai/gateway/prometheus#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can expose diagnostics metrics through the official `diagnostics-prometheus` plugin. It listens to trusted internal diagnostics and renders a Prometheus text endpoint at:

```
GET /api/diagnostics/prometheus
```

Content type is `text/plain; version=0.0.4; charset=utf-8`, the standard Prometheus exposition format.

The route uses Gateway authentication (operator scope). Do not expose it as a public unauthenticated `/metrics` endpoint. Scrape it through the same auth path you use for other operator APIs.

For traces, logs, OTLP push, and OpenTelemetry GenAI semantic attributes, see [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).

## [​](https://docs.openclaw.ai/gateway/prometheus\#quick-start)  Quick start

1

[Navigate to header](https://docs.openclaw.ai/gateway/prometheus#)

Install the plugin

```
openclaw plugins install clawhub:@openclaw/diagnostics-prometheus
```

2

[Navigate to header](https://docs.openclaw.ai/gateway/prometheus#)

Enable the plugin

- Config

- CLI


```
{
  plugins: {
    allow: ["diagnostics-prometheus"],
    entries: {
      "diagnostics-prometheus": { enabled: true },
    },
  },
  diagnostics: {
    enabled: true,
  },
}
```

```
openclaw plugins enable diagnostics-prometheus
```

3

[Navigate to header](https://docs.openclaw.ai/gateway/prometheus#)

Restart the Gateway

The HTTP route is registered at plugin startup, so reload after enabling.

4

[Navigate to header](https://docs.openclaw.ai/gateway/prometheus#)

Scrape the protected route

Send the same gateway auth your operator clients use:

```
curl -H "Authorization: Bearer $OPENCLAW_GATEWAY_TOKEN" \
  http://127.0.0.1:18789/api/diagnostics/prometheus
```

5

[Navigate to header](https://docs.openclaw.ai/gateway/prometheus#)

Wire Prometheus

```
# prometheus.yml
scrape_configs:
  - job_name: openclaw
    scrape_interval: 30s
    metrics_path: /api/diagnostics/prometheus
    authorization:
      credentials_file: /etc/prometheus/openclaw-gateway-token
    static_configs:
      - targets: ["openclaw-gateway:18789"]
```

`diagnostics.enabled: true` is required. Without it, the plugin still registers the HTTP route but no diagnostic events flow into the exporter, so the response is empty.

## [​](https://docs.openclaw.ai/gateway/prometheus\#metrics-exported)  Metrics exported

| Metric | Type | Labels |
| --- | --- | --- |
| `openclaw_run_completed_total` | counter | `channel`, `model`, `outcome`, `provider`, `trigger` |
| `openclaw_run_duration_seconds` | histogram | `channel`, `model`, `outcome`, `provider`, `trigger` |
| `openclaw_model_call_total` | counter | `api`, `error_category`, `model`, `outcome`, `provider`, `transport` |
| `openclaw_model_call_duration_seconds` | histogram | `api`, `error_category`, `model`, `outcome`, `provider`, `transport` |
| `openclaw_model_tokens_total` | counter | `agent`, `channel`, `model`, `provider`, `token_type` |
| `openclaw_gen_ai_client_token_usage` | histogram | `model`, `provider`, `token_type` |
| `openclaw_model_cost_usd_total` | counter | `agent`, `channel`, `model`, `provider` |
| `openclaw_tool_execution_total` | counter | `error_category`, `outcome`, `params_kind`, `tool` |
| `openclaw_tool_execution_duration_seconds` | histogram | `error_category`, `outcome`, `params_kind`, `tool` |
| `openclaw_harness_run_total` | counter | `channel`, `error_category`, `harness`, `model`, `outcome`, `phase`, `plugin`, `provider` |
| `openclaw_harness_run_duration_seconds` | histogram | `channel`, `error_category`, `harness`, `model`, `outcome`, `phase`, `plugin`, `provider` |
| `openclaw_message_processed_total` | counter | `channel`, `outcome`, `reason` |
| `openclaw_message_processed_duration_seconds` | histogram | `channel`, `outcome`, `reason` |
| `openclaw_message_delivery_total` | counter | `channel`, `delivery_kind`, `error_category`, `outcome` |
| `openclaw_message_delivery_duration_seconds` | histogram | `channel`, `delivery_kind`, `error_category`, `outcome` |
| `openclaw_queue_lane_size` | gauge | `lane` |
| `openclaw_queue_lane_wait_seconds` | histogram | `lane` |
| `openclaw_session_state_total` | counter | `reason`, `state` |
| `openclaw_session_queue_depth` | gauge | `state` |
| `openclaw_memory_bytes` | gauge | `kind` |
| `openclaw_memory_rss_bytes` | histogram | none |
| `openclaw_memory_pressure_total` | counter | `level`, `reason` |
| `openclaw_telemetry_exporter_total` | counter | `exporter`, `reason`, `signal`, `status` |
| `openclaw_prometheus_series_dropped_total` | counter | none |

## [​](https://docs.openclaw.ai/gateway/prometheus\#label-policy)  Label policy

Bounded, low-cardinality labels

Prometheus labels stay bounded and low-cardinality. The exporter does not emit raw diagnostic identifiers such as `runId`, `sessionKey`, `sessionId`, `callId`, `toolCallId`, message IDs, chat IDs, or provider request IDs.Label values are redacted and must match OpenClaw’s low-cardinality character policy. Values that fail the policy are replaced with `unknown`, `other`, or `none`, depending on the metric.

Series cap and overflow accounting

The exporter caps retained time series in memory at **2048** series across counters, gauges, and histograms combined. New series beyond that cap are dropped, and `openclaw_prometheus_series_dropped_total` increments by one each time.Watch this counter as a hard signal that an attribute upstream is leaking high-cardinality values. The exporter never lifts the cap automatically; if it climbs, fix the source rather than disabling the cap.

What never appears in Prometheus output

- prompt text, response text, tool inputs, tool outputs, system prompts
- raw provider request IDs (only bounded hashes, where applicable, on spans — never on metrics)
- session keys and session IDs
- hostnames, file paths, secret values

## [​](https://docs.openclaw.ai/gateway/prometheus\#promql-recipes)  PromQL recipes

```
# Tokens per minute, split by provider
sum by (provider) (rate(openclaw_model_tokens_total[1m]))

# Spend (USD) over the last hour, by model
sum by (model) (increase(openclaw_model_cost_usd_total[1h]))

# 95th percentile model run duration
histogram_quantile(
  0.95,
  sum by (le, provider, model)
    (rate(openclaw_run_duration_seconds_bucket[5m]))
)

# Queue wait time SLO (95p under 2s)
histogram_quantile(
  0.95,
  sum by (le, lane) (rate(openclaw_queue_lane_wait_seconds_bucket[5m]))
) < 2

# Dropped Prometheus series (cardinality alarm)
increase(openclaw_prometheus_series_dropped_total[15m]) > 0
```

Prefer `gen_ai_client_token_usage` for cross-provider dashboards: it follows the OpenTelemetry GenAI semantic conventions and is consistent with metrics from non-OpenClaw GenAI services.

## [​](https://docs.openclaw.ai/gateway/prometheus\#choosing-between-prometheus-and-opentelemetry-export)  Choosing between Prometheus and OpenTelemetry export

OpenClaw supports both surfaces independently. You can run either, both, or neither.

- diagnostics-prometheus

- diagnostics-otel


- **Pull** model: Prometheus scrapes `/api/diagnostics/prometheus`.
- No external collector required.
- Authenticated through normal Gateway auth.
- Surface is metrics only (no traces or logs).
- Best for stacks already standardized on Prometheus + Grafana.

- **Push** model: OpenClaw sends OTLP/HTTP to a collector or OTLP-compatible backend.
- Surface includes metrics, traces, and logs.
- Bridges to Prometheus through an OpenTelemetry Collector (`prometheus` or `prometheusremotewrite` exporter) when you need both.
- See [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) for the full catalog.

## [​](https://docs.openclaw.ai/gateway/prometheus\#troubleshooting)  Troubleshooting

Empty response body

- Check `diagnostics.enabled: true` in config.
- Confirm the plugin is enabled and loaded with `openclaw plugins list --enabled`.
- Generate some traffic; counters and histograms only emit lines after at least one event.

401 / unauthorized

The endpoint requires the Gateway operator scope (`auth: "gateway"` with `gatewayRuntimeScopeSurface: "trusted-operator"`). Use the same token or password Prometheus uses for any other Gateway operator route. There is no public unauthenticated mode.

\`openclaw\_prometheus\_series\_dropped\_total\` is climbing

A new attribute is exceeding the **2048**-series cap. Inspect recent metrics for an unexpectedly high-cardinality label and fix it at the source. The exporter intentionally drops new series instead of silently rewriting labels.

Prometheus shows stale series after a restart

The plugin keeps state in memory only. After a Gateway restart, counters reset to zero and gauges restart at their next reported value. Use PromQL `rate()` and `increase()` to handle resets cleanly.

## [​](https://docs.openclaw.ai/gateway/prometheus\#related)  Related

- [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics) — local diagnostics zip for support bundles
- [Health and readiness](https://docs.openclaw.ai/gateway/health) — `/healthz` and `/readyz` probes
- [Logging](https://docs.openclaw.ai/logging) — file-based logging
- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — OTLP push for traces, metrics, and logs

[OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) [Gateway logging](https://docs.openclaw.ai/gateway/logging)

Ctrl+I