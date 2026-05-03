---
source_url: https://docs.openclaw.ai/ko
title: "OpenClaw - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Overview

OpenClaw

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [OpenClaw 🦞](https://docs.openclaw.ai/ko#openclaw-)
- [OpenClaw이란?](https://docs.openclaw.ai/ko#openclaw%EC%9D%B4%EB%9E%80)
- [작동 방식](https://docs.openclaw.ai/ko#%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D)
- [주요 기능](https://docs.openclaw.ai/ko#%EC%A3%BC%EC%9A%94-%EA%B8%B0%EB%8A%A5)
- [빠른 시작](https://docs.openclaw.ai/ko#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)
- [대시보드](https://docs.openclaw.ai/ko#%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C)
- [구성(선택 사항)](https://docs.openclaw.ai/ko#%EA%B5%AC%EC%84%B1-%EC%84%A0%ED%83%9D-%EC%82%AC%ED%95%AD)
- [여기서 시작하세요](https://docs.openclaw.ai/ko#%EC%97%AC%EA%B8%B0%EC%84%9C-%EC%8B%9C%EC%9E%91%ED%95%98%EC%84%B8%EC%9A%94)
- [더 알아보기](https://docs.openclaw.ai/ko#%EB%8D%94-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ko\#openclaw-)  OpenClaw 🦞

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=61797dcb0c37d6e9279b8c5ad2e850e4)![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=d799bea41acb92d4c9fd1075c575879f)

> _“EXFOLIATE! EXFOLIATE!”_ — 아마도 우주 바닷가재

**Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo 등 전반에서 AI 에이전트를 위한 모든 OS용 Gateway.**

메시지를 보내면 주머니 속에서 에이전트 응답을 받을 수 있습니다. 내장 채널, 번들 채널 Plugin, WebChat, 모바일 Node 전반에 걸쳐 하나의 Gateway를 실행하세요.

[**시작하기** \\
\\
OpenClaw을 설치하고 몇 분 안에 Gateway를 실행하세요.](https://docs.openclaw.ai/ko/start/getting-started)

[**온보딩 실행** \\
\\
`openclaw onboard`와 pairing 흐름을 사용한 안내형 설정입니다.](https://docs.openclaw.ai/ko/start/wizard)

[**Control UI 열기** \\
\\
채팅, config, 세션을 위한 브라우저 대시보드를 실행하세요.](https://docs.openclaw.ai/web/control-ui)

## [​](https://docs.openclaw.ai/ko\#openclaw%EC%9D%B4%EB%9E%80)  OpenClaw이란?

OpenClaw은 **셀프 호스팅 Gateway** 로, Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo 등의 번들 또는 외부 채널 Plugin과 내장 채널을 Pi 같은 AI 코딩 에이전트에 연결합니다. 사용자는 자신의 머신(또는 서버)에서 단일 Gateway 프로세스를 실행하며, 이 프로세스가 메시징 앱과 항상 사용 가능한 AI 도우미 사이의 브리지 역할을 합니다.**누구를 위한 제품인가요?** 데이터를 계속 제어하고 호스팅 서비스에 의존하지 않으면서, 어디서나 메시지를 보낼 수 있는 개인 AI 도우미를 원하는 개발자와 고급 사용자입니다.**무엇이 다른가요?**

- **셀프 호스팅**: 내 하드웨어, 내 규칙으로 실행
- **멀티채널**: 하나의 Gateway가 내장 채널과 번들 또는 외부 채널 Plugin을 동시에 제공
- **에이전트 네이티브**: 도구 사용, 세션, 메모리, 다중 에이전트 라우팅을 갖춘 코딩 에이전트용으로 설계
- **오픈 소스**: MIT 라이선스, 커뮤니티 중심

**무엇이 필요한가요?** Node 24(권장) 또는 호환성을 위한 Node 22 LTS(`22.14+`), 선택한 provider의 API 키, 그리고 5분이면 됩니다. 최상의 품질과 보안을 위해 사용 가능한 최신 세대의 가장 강력한 모델을 사용하세요.

## [​](https://docs.openclaw.ai/ko\#%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D)  작동 방식

Gateway는 세션, 라우팅, 채널 연결을 위한 단일 진실 공급원입니다.

## [​](https://docs.openclaw.ai/ko\#%EC%A3%BC%EC%9A%94-%EA%B8%B0%EB%8A%A5)  주요 기능

[**멀티채널 Gateway** \\
\\
단일 Gateway 프로세스로 Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat 등을 지원합니다.](https://docs.openclaw.ai/ko/channels)

[**Plugin 채널** \\
\\
번들 Plugin이 현재 일반 릴리스에서 Matrix, Nostr, Twitch, Zalo 등을 추가합니다.](https://docs.openclaw.ai/ko/tools/plugin)

[**다중 에이전트 라우팅** \\
\\
에이전트, 워크스페이스 또는 발신자별로 세션을 격리합니다.](https://docs.openclaw.ai/ko/concepts/multi-agent)

[**미디어 지원** \\
\\
이미지, 오디오, 문서를 보내고 받을 수 있습니다.](https://docs.openclaw.ai/ko/nodes/images)

[**Web Control UI** \\
\\
채팅, config, 세션, Node를 위한 브라우저 대시보드입니다.](https://docs.openclaw.ai/web/control-ui)

[**모바일 Node** \\
\\
Canvas, 카메라, 음성 지원 워크플로를 위한 iOS 및 Android Node를 pairing합니다.](https://docs.openclaw.ai/ko/nodes)

## [​](https://docs.openclaw.ai/ko\#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)  빠른 시작

1

[Navigate to header](https://docs.openclaw.ai/ko#)

OpenClaw 설치

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/ko#)

온보딩 및 서비스 설치

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/ko#)

채팅

브라우저에서 Control UI를 열고 메시지를 보내세요:

```
openclaw dashboard
```

또는 채널을 연결하고( [Telegram](https://docs.openclaw.ai/ko/channels/telegram) 이 가장 빠름) 휴대폰에서 채팅하세요.

전체 설치 및 개발 설정이 필요하신가요? [시작하기](https://docs.openclaw.ai/ko/start/getting-started) 를 참고하세요.

## [​](https://docs.openclaw.ai/ko\#%EB%8C%80%EC%8B%9C%EB%B3%B4%EB%93%9C)  대시보드

Gateway가 시작되면 브라우저 Control UI를 여세요.

- 로컬 기본값: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- 원격 액세스: [Web 표면](https://docs.openclaw.ai/web) 및 [Tailscale](https://docs.openclaw.ai/ko/gateway/tailscale)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=b74a3630b0e971f466eff15fbdc642cb)

## [​](https://docs.openclaw.ai/ko\#%EA%B5%AC%EC%84%B1-%EC%84%A0%ED%83%9D-%EC%82%AC%ED%95%AD)  구성(선택 사항)

config는 `~/.openclaw/openclaw.json`에 있습니다.

- **아무것도 하지 않으면**, OpenClaw은 번들 Pi 바이너리를 RPC 모드와 발신자별 세션으로 사용합니다.
- 잠그고 싶다면 `channels.whatsapp.allowFrom`과 (그룹의 경우) 멘션 규칙부터 시작하세요.

예시:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## [​](https://docs.openclaw.ai/ko\#%EC%97%AC%EA%B8%B0%EC%84%9C-%EC%8B%9C%EC%9E%91%ED%95%98%EC%84%B8%EC%9A%94)  여기서 시작하세요

[**문서 허브** \\
\\
사용 사례별로 정리된 모든 문서와 가이드입니다.](https://docs.openclaw.ai/ko/start/hubs)

[**구성** \\
\\
핵심 Gateway 설정, 토큰, provider config입니다.](https://docs.openclaw.ai/ko/gateway/configuration)

[**원격 액세스** \\
\\
SSH 및 tailnet 액세스 패턴입니다.](https://docs.openclaw.ai/ko/gateway/remote)

[**채널** \\
\\
Feishu, Microsoft Teams, WhatsApp, Telegram, Discord 등의 채널별 설정입니다.](https://docs.openclaw.ai/ko/channels/telegram)

[**Node** \\
\\
pairing, Canvas, 카메라, 디바이스 작업을 지원하는 iOS 및 Android Node입니다.](https://docs.openclaw.ai/ko/nodes)

[**도움말** \\
\\
일반적인 수정 사항과 문제 해결 시작점입니다.](https://docs.openclaw.ai/ko/help)

## [​](https://docs.openclaw.ai/ko\#%EB%8D%94-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)  더 알아보기

[**전체 기능 목록** \\
\\
완전한 채널, 라우팅, 미디어 기능입니다.](https://docs.openclaw.ai/ko/concepts/features)

[**다중 에이전트 라우팅** \\
\\
워크스페이스 격리 및 에이전트별 세션입니다.](https://docs.openclaw.ai/ko/concepts/multi-agent)

[**보안** \\
\\
토큰, 허용 목록, 안전 제어입니다.](https://docs.openclaw.ai/ko/gateway/security)

[**문제 해결** \\
\\
Gateway 진단 및 일반적인 오류입니다.](https://docs.openclaw.ai/ko/gateway/troubleshooting)

[**정보 및 크레딧** \\
\\
프로젝트 기원, 기여자, 라이선스입니다.](https://docs.openclaw.ai/ko/reference/credits)

[쇼케이스](https://docs.openclaw.ai/ko/start/showcase)

Ctrl+I

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?w=1100&fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=ed926636a9752c9ce39acccf51c3b271)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=88255cdd2554a6b341c89ae709743441)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=72f5064ba581433011975bde37c74964)