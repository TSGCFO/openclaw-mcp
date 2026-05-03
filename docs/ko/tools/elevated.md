---
source_url: https://docs.openclaw.ai/ko/tools/elevated
title: "Elevated \ubaa8\ub4dc - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko/tools/elevated#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Tools

Elevated 모드

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [지시문](https://docs.openclaw.ai/ko/tools/elevated#%EC%A7%80%EC%8B%9C%EB%AC%B8)
- [작동 방식](https://docs.openclaw.ai/ko/tools/elevated#%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D)
- [해석 순서](https://docs.openclaw.ai/ko/tools/elevated#%ED%95%B4%EC%84%9D-%EC%88%9C%EC%84%9C)
- [사용 가능 여부 및 허용 목록](https://docs.openclaw.ai/ko/tools/elevated#%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5-%EC%97%AC%EB%B6%80-%EB%B0%8F-%ED%97%88%EC%9A%A9-%EB%AA%A9%EB%A1%9D)
- [Elevated가 제어하지 않는 것](https://docs.openclaw.ai/ko/tools/elevated#elevated%EA%B0%80-%EC%A0%9C%EC%96%B4%ED%95%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EA%B2%83)
- [관련 문서](https://docs.openclaw.ai/ko/tools/elevated#%EA%B4%80%EB%A0%A8-%EB%AC%B8%EC%84%9C)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

에이전트가 샌드박스 안에서 실행될 때, 그 `exec` 명령은 샌드박스 환경 안에 제한됩니다. **Elevated 모드** 는 에이전트가 그 제한을 벗어나 샌드박스 밖에서 명령을 실행할 수 있게 해 주며, 승인 게이트는 구성 가능합니다.

Elevated 모드는 에이전트가 **샌드박스 상태일 때만** 동작을 바꿉니다. 샌드박스되지 않은 에이전트는 이미 호스트에서 exec를 실행합니다.

## [​](https://docs.openclaw.ai/ko/tools/elevated\#%EC%A7%80%EC%8B%9C%EB%AC%B8)  지시문

슬래시 명령으로 세션별 Elevated 모드를 제어합니다:

| 지시문 | 기능 |
| --- | --- |
| `/elevated on` | 구성된 호스트 경로에서 샌드박스 밖으로 실행, 승인 유지 |
| `/elevated ask` | `on`과 동일(별칭) |
| `/elevated full` | 구성된 호스트 경로에서 샌드박스 밖으로 실행하고 승인 건너뜀 |
| `/elevated off` | 다시 샌드박스 내부 실행으로 복귀 |

`/elev on|off|ask|full`로도 사용할 수 있습니다.인자 없이 `/elevated`를 보내면 현재 수준을 볼 수 있습니다.

## [​](https://docs.openclaw.ai/ko/tools/elevated\#%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D)  작동 방식

1

[Navigate to header](https://docs.openclaw.ai/ko/tools/elevated#)

사용 가능 여부 확인

Elevated는 구성에서 활성화되어 있어야 하고, 발신자가 허용 목록에 있어야 합니다:

```
{
  tools: {
    elevated: {
      enabled: true,
      allowFrom: {
        discord: ["user-id-123"],
        whatsapp: ["+15555550123"],
      },
    },
  },
}
```

2

[Navigate to header](https://docs.openclaw.ai/ko/tools/elevated#)

수준 설정

세션 기본값을 설정하려면 지시문만 있는 메시지를 보내세요:

```
/elevated full
```

또는 인라인으로 사용할 수 있습니다(해당 메시지에만 적용):

```
/elevated on run the deployment script
```

3

[Navigate to header](https://docs.openclaw.ai/ko/tools/elevated#)

명령이 샌드박스 밖에서 실행됨

Elevated가 활성화되면 `exec` 호출은 샌드박스를 벗어납니다. 유효 호스트는 기본적으로 `gateway`이며, 구성된/세션 exec 대상이 `node`일 때는 `node`입니다. `full` 모드에서는 exec 승인이 건너뛰어집니다. `on`/`ask` 모드에서는 구성된 승인 규칙이 계속 적용됩니다.

## [​](https://docs.openclaw.ai/ko/tools/elevated\#%ED%95%B4%EC%84%9D-%EC%88%9C%EC%84%9C)  해석 순서

1. 메시지의 **인라인 지시문** (해당 메시지에만 적용)
2. **세션 재정의** (지시문만 있는 메시지를 보내서 설정)
3. **전역 기본값** (`agents.defaults.elevatedDefault` in config)

## [​](https://docs.openclaw.ai/ko/tools/elevated\#%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5-%EC%97%AC%EB%B6%80-%EB%B0%8F-%ED%97%88%EC%9A%A9-%EB%AA%A9%EB%A1%9D)  사용 가능 여부 및 허용 목록

- **전역 게이트**: `tools.elevated.enabled` (`true`여야 함)
- **발신자 허용 목록**: 채널별 목록이 있는 `tools.elevated.allowFrom`
- **에이전트별 게이트**: `agents.list[].tools.elevated.enabled` (추가 제한만 가능)
- **에이전트별 허용 목록**: `agents.list[].tools.elevated.allowFrom` (발신자는 전역 \+ 에이전트별 둘 다 일치해야 함)
- **Discord 폴백**: `tools.elevated.allowFrom.discord`가 생략되면 `channels.discord.allowFrom`이 폴백으로 사용됨
- **모든 게이트를 통과해야 하며**, 그렇지 않으면 Elevated는 사용 불가로 처리됨

허용 목록 항목 형식:

| 접두사 | 일치 대상 |
| --- | --- |
| (없음) | 발신자 ID, E.164 또는 From 필드 |
| `name:` | 발신자 표시 이름 |
| `username:` | 발신자 사용자 이름 |
| `tag:` | 발신자 태그 |
| `id:`, `from:`, `e164:` | 명시적 신원 타기팅 |

## [​](https://docs.openclaw.ai/ko/tools/elevated\#elevated%EA%B0%80-%EC%A0%9C%EC%96%B4%ED%95%98%EC%A7%80-%EC%95%8A%EB%8A%94-%EA%B2%83)  Elevated가 제어하지 않는 것

- **도구 정책**: 도구 정책에서 `exec`가 거부되면 Elevated로 이를 재정의할 수 없습니다
- **호스트 선택 정책**: Elevated는 `auto`를 자유로운 크로스 호스트 재정의로 바꾸지 않습니다. 구성된/세션 exec 대상 규칙을 사용하며, 대상이 이미 `node`일 때만 `node`를 선택합니다.
- **`/exec`와는 별개**: `/exec` 지시문은 권한 있는 발신자를 위해 세션별 exec 기본값을 조정하며 Elevated 모드를 요구하지 않습니다

## [​](https://docs.openclaw.ai/ko/tools/elevated\#%EA%B4%80%EB%A0%A8-%EB%AC%B8%EC%84%9C)  관련 문서

- [Exec 도구](https://docs.openclaw.ai/ko/tools/exec) — 셸 명령 실행
- [Exec 승인](https://docs.openclaw.ai/ko/tools/exec-approvals) — 승인 및 허용 목록 시스템
- [샌드박싱](https://docs.openclaw.ai/ko/gateway/sandboxing) — 샌드박스 구성
- [샌드박스 vs 도구 정책 vs Elevated](https://docs.openclaw.ai/ko/gateway/sandbox-vs-tool-policy-vs-elevated)

[Diffs](https://docs.openclaw.ai/ko/tools/diffs) [Exec approvals](https://docs.openclaw.ai/ko/tools/exec-approvals)

Ctrl+I