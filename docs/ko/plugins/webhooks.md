---
source_url: https://docs.openclaw.ai/ko/plugins/webhooks
title: "Webhook Plugin - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko/plugins/webhooks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Plugins

Webhook Plugin

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [Webhooks (Plugin)](https://docs.openclaw.ai/ko/plugins/webhooks#webhooks-plugin)
- [실행 위치](https://docs.openclaw.ai/ko/plugins/webhooks#%EC%8B%A4%ED%96%89-%EC%9C%84%EC%B9%98)
- [라우트 구성](https://docs.openclaw.ai/ko/plugins/webhooks#%EB%9D%BC%EC%9A%B0%ED%8A%B8-%EA%B5%AC%EC%84%B1)
- [보안 모델](https://docs.openclaw.ai/ko/plugins/webhooks#%EB%B3%B4%EC%95%88-%EB%AA%A8%EB%8D%B8)
- [요청 형식](https://docs.openclaw.ai/ko/plugins/webhooks#%EC%9A%94%EC%B2%AD-%ED%98%95%EC%8B%9D)
- [지원되는 작업](https://docs.openclaw.ai/ko/plugins/webhooks#%EC%A7%80%EC%9B%90%EB%90%98%EB%8A%94-%EC%9E%91%EC%97%85)
- [create\_flow](https://docs.openclaw.ai/ko/plugins/webhooks#create_flow)
- [run\_task](https://docs.openclaw.ai/ko/plugins/webhooks#run_task)
- [응답 형태](https://docs.openclaw.ai/ko/plugins/webhooks#%EC%9D%91%EB%8B%B5-%ED%98%95%ED%83%9C)
- [관련 문서](https://docs.openclaw.ai/ko/plugins/webhooks#%EA%B4%80%EB%A0%A8-%EB%AC%B8%EC%84%9C)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ko/plugins/webhooks\#webhooks-plugin)  Webhooks (Plugin)

Webhooks Plugin은 외부 자동화를 OpenClaw TaskFlows에 바인딩하는 인증된 HTTP 라우트를 추가합니다.Zapier, n8n, CI 작업 또는 내부 서비스 같은 신뢰할 수 있는 시스템이 먼저 사용자 지정 Plugin을 작성하지 않고도 관리형 TaskFlows를 생성하고 구동하게 하려는 경우 사용하세요.

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EC%8B%A4%ED%96%89-%EC%9C%84%EC%B9%98)  실행 위치

Webhooks Plugin은 Gateway 프로세스 내부에서 실행됩니다.Gateway가 다른 머신에서 실행 중이면 해당 Gateway 호스트에 Plugin을 설치하고 구성한 다음 Gateway를 다시 시작하세요.

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EB%9D%BC%EC%9A%B0%ED%8A%B8-%EA%B5%AC%EC%84%B1)  라우트 구성

`plugins.entries.webhooks.config` 아래에 구성을 설정합니다.

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

라우트 필드:

- `enabled`: 선택 사항, 기본값은 `true`
- `path`: 선택 사항, 기본값은 `/plugins/webhooks/<routeId>`
- `sessionKey`: 바인딩된 TaskFlows를 소유하는 필수 세션
- `secret`: 필수 공유 비밀 또는 SecretRef
- `controllerId`: 생성된 관리형 플로우의 선택적 컨트롤러 ID
- `description`: 선택적 운영자 메모

지원되는 `secret` 입력:

- 일반 문자열
- `source: "env" | "file" | "exec"`가 있는 SecretRef

비밀 기반 라우트가 시작 시 비밀을 확인할 수 없으면, Plugin은 손상된 엔드포인트를 노출하는 대신 해당 라우트를 건너뛰고 경고를 기록합니다.

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EB%B3%B4%EC%95%88-%EB%AA%A8%EB%8D%B8)  보안 모델

각 라우트는 구성된 `sessionKey`의 TaskFlow 권한으로 동작하도록 신뢰됩니다.즉, 라우트는 해당 세션이 소유한 TaskFlows를 검사하고 변경할 수 있으므로 다음을 권장합니다.

- 라우트마다 강력하고 고유한 비밀 사용
- 인라인 일반 텍스트 비밀보다 비밀 참조 선호
- 워크플로에 맞는 가장 좁은 세션에 라우트 바인딩
- 필요한 특정 Webhook 경로만 노출

Plugin은 다음을 적용합니다.

- 공유 비밀 인증
- 요청 본문 크기 및 시간 제한 보호
- 고정 창 속도 제한
- 진행 중 요청 제한
- `api.runtime.tasks.managedFlows.bindSession(...)`를 통한 소유자 바인딩 TaskFlow 접근

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EC%9A%94%EC%B2%AD-%ED%98%95%EC%8B%9D)  요청 형식

다음과 함께 `POST` 요청을 보냅니다.

- `Content-Type: application/json`
- `Authorization: Bearer <secret>` 또는 `x-openclaw-webhook-secret: <secret>`

예시:

```
curl -X POST https://gateway.example.com/plugins/webhooks/zapier \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_SHARED_SECRET' \
  -d '{"action":"create_flow","goal":"Review inbound queue"}'
```

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EC%A7%80%EC%9B%90%EB%90%98%EB%8A%94-%EC%9E%91%EC%97%85)  지원되는 작업

Plugin은 현재 다음 JSON `action` 값을 허용합니다.

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

### [​](https://docs.openclaw.ai/ko/plugins/webhooks\#create_flow)  `create_flow`

라우트의 바인딩된 세션에 대한 관리형 TaskFlow를 생성합니다.예시:

```
{
  "action": "create_flow",
  "goal": "Review inbound queue",
  "status": "queued",
  "notifyPolicy": "done_only"
}
```

### [​](https://docs.openclaw.ai/ko/plugins/webhooks\#run_task)  `run_task`

기존 관리형 TaskFlow 내부에 관리형 하위 작업을 생성합니다.허용되는 런타임은 다음과 같습니다.

- `subagent`
- `acp`

예시:

```
{
  "action": "run_task",
  "flowId": "flow_123",
  "runtime": "acp",
  "childSessionKey": "agent:main:acp:worker",
  "task": "Inspect the next message batch"
}
```

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EC%9D%91%EB%8B%B5-%ED%98%95%ED%83%9C)  응답 형태

성공한 응답은 다음을 반환합니다.

```
{
  "ok": true,
  "routeId": "zapier",
  "result": {}
}
```

거부된 요청은 다음을 반환합니다.

```
{
  "ok": false,
  "routeId": "zapier",
  "code": "not_found",
  "error": "TaskFlow not found.",
  "result": {}
}
```

Plugin은 의도적으로 Webhook 응답에서 소유자/세션 메타데이터를 제거합니다.

## [​](https://docs.openclaw.ai/ko/plugins/webhooks\#%EA%B4%80%EB%A0%A8-%EB%AC%B8%EC%84%9C)  관련 문서

- [Plugin 런타임 SDK](https://docs.openclaw.ai/ko/plugins/sdk-runtime)
- [Hook 및 Webhook 개요](https://docs.openclaw.ai/ko/automation/hooks)
- [CLI Webhook](https://docs.openclaw.ai/ko/cli/webhooks)

[Google Meet Plugin](https://docs.openclaw.ai/ko/plugins/google-meet) [Voice call](https://docs.openclaw.ai/ko/plugins/voice-call)

Ctrl+I