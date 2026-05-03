---
source_url: https://docs.openclaw.ai/ko/providers/litellm
title: "LiteLLM - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko/providers/litellm#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Providers

LiteLLM

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [빠른 시작](https://docs.openclaw.ai/ko/providers/litellm#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)
- [설정](https://docs.openclaw.ai/ko/providers/litellm#%EC%84%A4%EC%A0%95)
- [환경 변수](https://docs.openclaw.ai/ko/providers/litellm#%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98)
- [설정 파일](https://docs.openclaw.ai/ko/providers/litellm#%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC)
- [고급 설정](https://docs.openclaw.ai/ko/providers/litellm#%EA%B3%A0%EA%B8%89-%EC%84%A4%EC%A0%95)
- [이미지 생성](https://docs.openclaw.ai/ko/providers/litellm#%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%83%9D%EC%84%B1)
- [관련 항목](https://docs.openclaw.ai/ko/providers/litellm#%EA%B4%80%EB%A0%A8-%ED%95%AD%EB%AA%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[LiteLLM](https://litellm.ai/) 은 100개 이상의 모델 제공업체에 통합 API를 제공하는 오픈 소스 LLM Gateway입니다. OpenClaw를 LiteLLM을 통해 라우팅하면 중앙 집중식 비용 추적, 로깅, 그리고 OpenClaw 설정을 변경하지 않고 백엔드를 전환할 수 있는 유연성을 얻을 수 있습니다.

**왜 OpenClaw와 함께 LiteLLM을 사용하나요?**

- **비용 추적** — OpenClaw가 모든 모델에서 지출하는 비용을 정확히 확인합니다
- **모델 라우팅** — 설정 변경 없이 Claude, GPT-4, Gemini, Bedrock 간 전환합니다
- **가상 키** — OpenClaw용 지출 한도가 있는 키를 생성합니다
- **로깅** — 디버깅을 위한 전체 요청/응답 로그
- **대체 처리** — 기본 제공업체가 중단된 경우 자동 장애 조치

## [​](https://docs.openclaw.ai/ko/providers/litellm\#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)  빠른 시작

- Onboarding(권장)

- 수동 설정


**적합한 경우:** 작동하는 LiteLLM 설정으로 가는 가장 빠른 경로.

1

[Navigate to header](https://docs.openclaw.ai/ko/providers/litellm#)

Onboarding 실행

```
openclaw onboard --auth-choice litellm-api-key
```

원격 프록시에 대해 비대화형 설정을 하려면 프록시 URL을 명시적으로 전달하세요.

```
openclaw onboard --non-interactive --auth-choice litellm-api-key --litellm-api-key "$LITELLM_API_KEY" --custom-base-url "https://litellm.example/v1"
```

**적합한 경우:** 설치와 설정을 완전히 제어해야 하는 경우.

1

[Navigate to header](https://docs.openclaw.ai/ko/providers/litellm#)

LiteLLM 프록시 시작

```
pip install 'litellm[proxy]'
litellm --model claude-opus-4-6
```

2

[Navigate to header](https://docs.openclaw.ai/ko/providers/litellm#)

OpenClaw가 LiteLLM을 가리키도록 설정

```
export LITELLM_API_KEY="your-litellm-key"

openclaw
```

이것으로 끝입니다. 이제 OpenClaw는 LiteLLM을 통해 라우팅됩니다.

## [​](https://docs.openclaw.ai/ko/providers/litellm\#%EC%84%A4%EC%A0%95)  설정

### [​](https://docs.openclaw.ai/ko/providers/litellm\#%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98)  환경 변수

```
export LITELLM_API_KEY="sk-litellm-key"
```

### [​](https://docs.openclaw.ai/ko/providers/litellm\#%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC)  설정 파일

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "claude-opus-4-6",\
            name: "Claude Opus 4.6",\
            reasoning: true,\
            input: ["text", "image"],\
            contextWindow: 200000,\
            maxTokens: 64000,\
          },\
          {\
            id: "gpt-4o",\
            name: "GPT-4o",\
            reasoning: false,\
            input: ["text", "image"],\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "litellm/claude-opus-4-6" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ko/providers/litellm\#%EA%B3%A0%EA%B8%89-%EC%84%A4%EC%A0%95)  고급 설정

### [​](https://docs.openclaw.ai/ko/providers/litellm\#%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%83%9D%EC%84%B1)  이미지 생성

LiteLLM은 OpenAI 호환
`/images/generations` 및 `/images/edits` 경로를 통해 OpenClaw의 `image_generate` 도구도 지원할 수 있습니다. `agents.defaults.imageGenerationModel` 아래에 LiteLLM 이미지
모델을 설정하세요.

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
      },
    },
  },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "litellm/gpt-image-2",
        timeoutMs: 180_000,
      },
    },
  },
}
```

`http://localhost:4000` 같은 loopback LiteLLM URL은 전역
사설 네트워크 재정의 없이 작동합니다. LAN에서 호스팅되는 프록시의 경우 API 키가
설정된 프록시 호스트로 전송되므로
`models.providers.litellm.request.allowPrivateNetwork: true`를 설정하세요.

가상 키

지출 한도가 있는 OpenClaw 전용 키를 생성하세요.

```
curl -X POST "http://localhost:4000/key/generate" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "key_alias": "openclaw",
    "max_budget": 50.00,
    "budget_duration": "monthly"
  }'
```

생성된 키를 `LITELLM_API_KEY`로 사용하세요.

모델 라우팅

LiteLLM은 모델 요청을 다른 백엔드로 라우팅할 수 있습니다. LiteLLM `config.yaml`에서 설정하세요.

```
model_list:
  - model_name: claude-opus-4-6
    litellm_params:
      model: claude-opus-4-6
      api_key: os.environ/ANTHROPIC_API_KEY

  - model_name: gpt-4o
    litellm_params:
      model: gpt-4o
      api_key: os.environ/OPENAI_API_KEY
```

OpenClaw는 계속 `claude-opus-4-6`을 요청하고, LiteLLM이 라우팅을 처리합니다.

사용량 보기

LiteLLM의 대시보드 또는 API를 확인하세요.

```
# Key info
curl "http://localhost:4000/key/info" \
  -H "Authorization: Bearer sk-litellm-key"

# Spend logs
curl "http://localhost:4000/spend/logs" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY"
```

프록시 동작 참고 사항

- LiteLLM은 기본적으로 `http://localhost:4000`에서 실행됩니다
- OpenClaw는 LiteLLM의 프록시 스타일 OpenAI 호환 `/v1`
엔드포인트를 통해 연결됩니다
- LiteLLM을 통해서는 네이티브 OpenAI 전용 요청 형태 조정이 적용되지 않습니다:
`service_tier` 없음, Responses `store` 없음, prompt-cache 힌트 없음, 그리고
OpenAI 추론 호환 페이로드 형태 조정 없음
- 사용자 지정 LiteLLM 기본 URL에는 숨겨진 OpenClaw 기여도 헤더(`originator`, `version`, `User-Agent`)가
삽입되지 않습니다

일반적인 제공업체 설정과 장애 조치 동작은 [모델 제공업체](https://docs.openclaw.ai/ko/concepts/model-providers) 를 참고하세요.

## [​](https://docs.openclaw.ai/ko/providers/litellm\#%EA%B4%80%EB%A0%A8-%ED%95%AD%EB%AA%A9)  관련 항목

[**LiteLLM 문서** \\
\\
공식 LiteLLM 문서와 API 참조입니다.](https://docs.litellm.ai/)

[**모델 선택** \\
\\
모든 제공업체, 모델 참조, 장애 조치 동작의 개요입니다.](https://docs.openclaw.ai/ko/concepts/model-providers)

[**설정** \\
\\
전체 설정 참조입니다.](https://docs.openclaw.ai/ko/gateway/configuration)

[**모델 선택** \\
\\
모델을 선택하고 설정하는 방법입니다.](https://docs.openclaw.ai/ko/concepts/models)

[Kilocode](https://docs.openclaw.ai/ko/providers/kilocode) [LM Studio](https://docs.openclaw.ai/ko/providers/lmstudio)

Ctrl+I