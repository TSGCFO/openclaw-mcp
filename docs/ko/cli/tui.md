---
source_url: https://docs.openclaw.ai/ko/cli/tui
title: "TUI - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko/cli/tui#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Interfaces

TUI

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [openclaw tui](https://docs.openclaw.ai/ko/cli/tui#openclaw-tui)
- [예시](https://docs.openclaw.ai/ko/cli/tui#%EC%98%88%EC%8B%9C)
- [구성 복구 루프](https://docs.openclaw.ai/ko/cli/tui#%EA%B5%AC%EC%84%B1-%EB%B3%B5%EA%B5%AC-%EB%A3%A8%ED%94%84)
- [관련](https://docs.openclaw.ai/ko/cli/tui#%EA%B4%80%EB%A0%A8)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ko/cli/tui\#openclaw-tui)  `openclaw tui`

Gateway에 연결된 터미널 UI를 열거나 로컬 임베디드
모드에서 실행합니다.관련:

- TUI 가이드: [TUI](https://docs.openclaw.ai/ko/web/tui)

참고:

- `chat`과 `terminal`은 `openclaw tui --local`의 별칭입니다.
- `--local`은 `--url`, `--token`, 또는 `--password`와 함께 사용할 수 없습니다.
- `tui`는 가능하면 토큰/비밀번호 인증에 대해 구성된 Gateway 인증 SecretRef를 확인합니다(`env`/`file`/`exec` provider).
- 구성된 에이전트 워크스페이스 디렉터리 내부에서 실행하면 TUI는 세션 키 기본값으로 해당 에이전트를 자동 선택합니다(`--session`이 명시적으로 `agent:<id>:...`인 경우 제외).
- 로컬 모드는 임베디드 에이전트 런타임을 직접 사용합니다. 대부분의 로컬 도구는 작동하지만 Gateway 전용 기능은 사용할 수 없습니다.
- 로컬 모드는 TUI 명령 표면에 `/auth [provider]`를 추가합니다.
- 로컬 모드에서도 Plugin 승인 게이트가 계속 적용됩니다. 승인이 필요한 도구는 터미널에서 결정 프롬프트를 표시하며, Gateway가 관여하지 않기 때문에 아무것도 조용히 자동 승인되지 않습니다.

## [​](https://docs.openclaw.ai/ko/cli/tui\#%EC%98%88%EC%8B%9C)  예시

```
openclaw chat
openclaw tui --local
openclaw tui
openclaw tui --url ws://127.0.0.1:18789 --token <token>
openclaw tui --session main --deliver
openclaw chat --message "내 구성을 문서와 비교하고 무엇을 수정해야 하는지 알려줘"
# 에이전트 워크스페이스 안에서 실행하면 해당 에이전트를 자동으로 추론
openclaw tui --session bugfix
```

## [​](https://docs.openclaw.ai/ko/cli/tui\#%EA%B5%AC%EC%84%B1-%EB%B3%B5%EA%B5%AC-%EB%A3%A8%ED%94%84)  구성 복구 루프

현재 구성이 이미 유효하고, 임베디드 에이전트가 이를 검사하고 문서와 비교한 뒤 같은 터미널에서 복구를 도와주길 원할 때 로컬 모드를 사용하세요.`openclaw config validate`가 이미 실패하는 경우 먼저 `openclaw configure` 또는
`openclaw doctor --fix`를 사용하세요. `openclaw chat`은 잘못된 구성 가드를 우회하지 않습니다.

```
openclaw chat
```

그런 다음 TUI 내부에서:

```
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

`openclaw config set` 또는 `openclaw configure`로 대상 수정 사항을 적용한 다음
`openclaw config validate`를 다시 실행하세요. [TUI](https://docs.openclaw.ai/ko/web/tui) 및 [구성](https://docs.openclaw.ai/ko/cli/config) 을 참조하세요.

## [​](https://docs.openclaw.ai/ko/cli/tui\#%EA%B4%80%EB%A0%A8)  관련

- [CLI 참조](https://docs.openclaw.ai/ko/cli)
- [TUI](https://docs.openclaw.ai/ko/web/tui)

[대시보드](https://docs.openclaw.ai/ko/cli/dashboard) [ACP](https://docs.openclaw.ai/ko/cli/acp)

Ctrl+I