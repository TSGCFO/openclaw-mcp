---
source_url: https://docs.openclaw.ai/ko/providers
title: "\ud504\ub85c\ubc14\uc774\ub354 \ub514\ub809\ud130\ub9ac - OpenClaw"
---

[메인 콘텐츠로 건너뛰기](https://docs.openclaw.ai/ko/providers#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ko)

![KR](https://d3gk2c5xim1je2.cloudfront.net/flags/KR.svg)

한국어

검색...

Ctrl K

검색...

Navigation

Overview

프로바이더 디렉터리

[Get started](https://docs.openclaw.ai/ko) [Install](https://docs.openclaw.ai/ko/install) [Channels](https://docs.openclaw.ai/ko/channels) [Agents](https://docs.openclaw.ai/ko/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ko/tools) [Models](https://docs.openclaw.ai/ko/providers) [Platforms](https://docs.openclaw.ai/ko/platforms) [Gateway & Ops](https://docs.openclaw.ai/ko/gateway) [Reference](https://docs.openclaw.ai/ko/cli) [Help](https://docs.openclaw.ai/ko/help)

이 페이지에서

- [모델 제공자](https://docs.openclaw.ai/ko/providers#%EB%AA%A8%EB%8D%B8-%EC%A0%9C%EA%B3%B5%EC%9E%90)
- [빠른 시작](https://docs.openclaw.ai/ko/providers#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)
- [제공자 문서](https://docs.openclaw.ai/ko/providers#%EC%A0%9C%EA%B3%B5%EC%9E%90-%EB%AC%B8%EC%84%9C)
- [공유 개요 페이지](https://docs.openclaw.ai/ko/providers#%EA%B3%B5%EC%9C%A0-%EA%B0%9C%EC%9A%94-%ED%8E%98%EC%9D%B4%EC%A7%80)
- [전사 제공자](https://docs.openclaw.ai/ko/providers#%EC%A0%84%EC%82%AC-%EC%A0%9C%EA%B3%B5%EC%9E%90)
- [커뮤니티 도구](https://docs.openclaw.ai/ko/providers#%EC%BB%A4%EB%AE%A4%EB%8B%88%ED%8B%B0-%EB%8F%84%EA%B5%AC)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ko/providers\#%EB%AA%A8%EB%8D%B8-%EC%A0%9C%EA%B3%B5%EC%9E%90)  모델 제공자

OpenClaw는 여러 LLM 제공자를 사용할 수 있습니다. 제공자를 선택하고 인증한 다음
기본 모델을 `provider/model`로 설정하세요.채팅 채널 문서(WhatsApp/Telegram/Discord/Slack/Mattermost (Plugin)/등)를 찾고 있나요? [채널](https://docs.openclaw.ai/ko/channels) 을 참조하세요.

## [​](https://docs.openclaw.ai/ko/providers\#%EB%B9%A0%EB%A5%B8-%EC%8B%9C%EC%9E%91)  빠른 시작

1. 제공자로 인증합니다(일반적으로 `openclaw onboard` 사용).
2. 기본 모델을 설정합니다.

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## [​](https://docs.openclaw.ai/ko/providers\#%EC%A0%9C%EA%B3%B5%EC%9E%90-%EB%AC%B8%EC%84%9C)  제공자 문서

- [Alibaba Model Studio](https://docs.openclaw.ai/ko/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/ko/providers/bedrock)
- [Amazon Bedrock Mantle](https://docs.openclaw.ai/ko/providers/bedrock-mantle)
- [Anthropic (API + Claude CLI)](https://docs.openclaw.ai/ko/providers/anthropic)
- [Arcee AI (Trinity 모델)](https://docs.openclaw.ai/ko/providers/arcee)
- [Azure Speech](https://docs.openclaw.ai/ko/providers/azure-speech)
- [BytePlus (International)](https://docs.openclaw.ai/ko/concepts/model-providers#byteplus-international)
- [Cerebras](https://docs.openclaw.ai/ko/providers/cerebras)
- [Chutes](https://docs.openclaw.ai/ko/providers/chutes)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/ko/providers/cloudflare-ai-gateway)
- [ComfyUI](https://docs.openclaw.ai/ko/providers/comfy)
- [DeepSeek](https://docs.openclaw.ai/ko/providers/deepseek)
- [ElevenLabs](https://docs.openclaw.ai/ko/providers/elevenlabs)
- [fal](https://docs.openclaw.ai/ko/providers/fal)
- [Fireworks](https://docs.openclaw.ai/ko/providers/fireworks)
- [GitHub Copilot](https://docs.openclaw.ai/ko/providers/github-copilot)
- [GLM 모델](https://docs.openclaw.ai/ko/providers/glm)
- [Google (Gemini)](https://docs.openclaw.ai/ko/providers/google)
- [Gradium](https://docs.openclaw.ai/ko/providers/gradium)
- [Groq (LPU 추론)](https://docs.openclaw.ai/ko/providers/groq)
- [Hugging Face (추론)](https://docs.openclaw.ai/ko/providers/huggingface)
- [inferrs (로컬 모델)](https://docs.openclaw.ai/ko/providers/inferrs)
- [Kilocode](https://docs.openclaw.ai/ko/providers/kilocode)
- [LiteLLM (통합 gateway)](https://docs.openclaw.ai/ko/providers/litellm)
- [LM Studio (로컬 모델)](https://docs.openclaw.ai/ko/providers/lmstudio)
- [MiniMax](https://docs.openclaw.ai/ko/providers/minimax)
- [Mistral](https://docs.openclaw.ai/ko/providers/mistral)
- [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/ko/providers/moonshot)
- [NVIDIA](https://docs.openclaw.ai/ko/providers/nvidia)
- [Ollama (클라우드 + 로컬 모델)](https://docs.openclaw.ai/ko/providers/ollama)
- [OpenAI (API + Codex)](https://docs.openclaw.ai/ko/providers/openai)
- [OpenCode](https://docs.openclaw.ai/ko/providers/opencode)
- [OpenCode Go](https://docs.openclaw.ai/ko/providers/opencode-go)
- [OpenRouter](https://docs.openclaw.ai/ko/providers/openrouter)
- [Perplexity (웹 검색)](https://docs.openclaw.ai/ko/providers/perplexity-provider)
- [Qianfan](https://docs.openclaw.ai/ko/providers/qianfan)
- [Qwen Cloud](https://docs.openclaw.ai/ko/providers/qwen)
- [Runway](https://docs.openclaw.ai/ko/providers/runway)
- [SenseAudio](https://docs.openclaw.ai/ko/providers/senseaudio)
- [SGLang (로컬 모델)](https://docs.openclaw.ai/ko/providers/sglang)
- [StepFun](https://docs.openclaw.ai/ko/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/ko/providers/synthetic)
- [Tencent Cloud (TokenHub)](https://docs.openclaw.ai/ko/providers/tencent)
- [Together AI](https://docs.openclaw.ai/ko/providers/together)
- [Venice (Venice AI, 개인정보 보호 중심)](https://docs.openclaw.ai/ko/providers/venice)
- [Vercel AI Gateway](https://docs.openclaw.ai/ko/providers/vercel-ai-gateway)
- [vLLM (로컬 모델)](https://docs.openclaw.ai/ko/providers/vllm)
- [Volcengine (Doubao)](https://docs.openclaw.ai/ko/providers/volcengine)
- [Vydra](https://docs.openclaw.ai/ko/providers/vydra)
- [xAI](https://docs.openclaw.ai/ko/providers/xai)
- [Xiaomi](https://docs.openclaw.ai/ko/providers/xiaomi)
- [Z.AI](https://docs.openclaw.ai/ko/providers/zai)

## [​](https://docs.openclaw.ai/ko/providers\#%EA%B3%B5%EC%9C%A0-%EA%B0%9C%EC%9A%94-%ED%8E%98%EC%9D%B4%EC%A7%80)  공유 개요 페이지

- [추가 번들 제공자 변형](https://docs.openclaw.ai/ko/providers/models#additional-bundled-provider-variants) \- Anthropic Vertex, Copilot Proxy 및 Gemini CLI OAuth
- [이미지 생성](https://docs.openclaw.ai/ko/tools/image-generation) \- 공유 `image_generate` 도구, 제공자 선택 및 장애 조치
- [음악 생성](https://docs.openclaw.ai/ko/tools/music-generation) \- 공유 `music_generate` 도구, 제공자 선택 및 장애 조치
- [동영상 생성](https://docs.openclaw.ai/ko/tools/video-generation) \- 공유 `video_generate` 도구, 제공자 선택 및 장애 조치

## [​](https://docs.openclaw.ai/ko/providers\#%EC%A0%84%EC%82%AC-%EC%A0%9C%EA%B3%B5%EC%9E%90)  전사 제공자

- [Deepgram (오디오 전사)](https://docs.openclaw.ai/ko/providers/deepgram)
- [ElevenLabs](https://docs.openclaw.ai/ko/providers/elevenlabs#speech-to-text)
- [Mistral](https://docs.openclaw.ai/ko/providers/mistral#audio-transcription-voxtral)
- [OpenAI](https://docs.openclaw.ai/ko/providers/openai#speech-to-text)
- [SenseAudio](https://docs.openclaw.ai/ko/providers/senseaudio)
- [xAI](https://docs.openclaw.ai/ko/providers/xai#speech-to-text)

## [​](https://docs.openclaw.ai/ko/providers\#%EC%BB%A4%EB%AE%A4%EB%8B%88%ED%8B%B0-%EB%8F%84%EA%B5%AC)  커뮤니티 도구

- [Claude Max API Proxy](https://docs.openclaw.ai/ko/providers/claude-max-api-proxy) \- Claude 구독 자격 증명을 위한 커뮤니티 프록시(사용 전 Anthropic 정책/약관 확인)

전체 제공자 카탈로그(xAI, Groq, Mistral 등)와 고급 구성은
[모델 제공자](https://docs.openclaw.ai/ko/concepts/model-providers) 를 참조하세요.

[모델 제공자 빠른 시작](https://docs.openclaw.ai/ko/providers/models)

Ctrl+I