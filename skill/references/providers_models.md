# Providers Models

_46 pages from docs.openclaw.ai — full content preserved._

## Contents

- [Arcee AI - OpenClaw](#arcee-ai---openclaw)
- [Inferrs - OpenClaw](#inferrs---openclaw)
- [Provider directory - OpenClaw](#provider-directory---openclaw)
- [Alibaba Model Studio - OpenClaw](#alibaba-model-studio---openclaw)
- [Anthropic - OpenClaw](#anthropic---openclaw)
- [Amazon Bedrock - OpenClaw](#amazon-bedrock---openclaw)
- [Cerebras - OpenClaw](#cerebras---openclaw)
- [Claude Max API proxy - OpenClaw](#claude-max-api-proxy---openclaw)
- [Cloudflare AI gateway - OpenClaw](#cloudflare-ai-gateway---openclaw)
- [Deepgram - OpenClaw](#deepgram---openclaw)
- [DeepSeek - OpenClaw](#deepseek---openclaw)
- [GitHub Copilot - OpenClaw](#github-copilot---openclaw)
- [GLM (Zhipu) - OpenClaw](#glm-zhipu---openclaw)
- [Google (Gemini) - OpenClaw](#google-gemini---openclaw)
- [Groq - OpenClaw](#groq---openclaw)
- [Hugging Face (inference) - OpenClaw](#hugging-face-inference---openclaw)
- [Kilocode - OpenClaw](#kilocode---openclaw)
- [LiteLLM - OpenClaw](#litellm---openclaw)
- [LM Studio - OpenClaw](#lm-studio---openclaw)
- [https://docs.openclaw.ai/providers/lmstudio.md](#httpsdocsopenclawaiproviderslmstudiomd)
- [MiniMax - OpenClaw](#minimax---openclaw)
- [Config example](#config-example)
- [Mistral - OpenClaw](#mistral---openclaw)
- [Model provider quickstart - OpenClaw](#model-provider-quickstart---openclaw)
- [Moonshot AI - OpenClaw](#moonshot-ai---openclaw)
- [NVIDIA - OpenClaw](#nvidia---openclaw)
- [Ollama - OpenClaw](#ollama---openclaw)
- [OpenAI - OpenClaw](#openai---openclaw)
- [OpenCode - OpenClaw](#opencode---openclaw)
- [OpenCode Go - OpenClaw](#opencode-go---openclaw)
- [OpenRouter - OpenClaw](#openrouter---openclaw)
- [Qianfan - OpenClaw](#qianfan---openclaw)
- [Qwen - OpenClaw](#qwen---openclaw)
- [SenseAudio - OpenClaw](#senseaudio---openclaw)
- [SGLang - OpenClaw](#sglang---openclaw)
- [Synthetic - OpenClaw](#synthetic---openclaw)
- [Together AI - OpenClaw](#together-ai---openclaw)
- [Venice AI - OpenClaw](#venice-ai---openclaw)
- [Vercel AI gateway - OpenClaw](#vercel-ai-gateway---openclaw)
- [vLLM - OpenClaw](#vllm---openclaw)
- [xAI - OpenClaw](#xai---openclaw)
- [Xiaomi MiMo - OpenClaw](#xiaomi-mimo---openclaw)
- [Z.AI - OpenClaw](#zai---openclaw)
- [ElevenLabs - OpenClaw](#elevenlabs---openclaw)
- [ElevenLabs - OpenClaw](#elevenlabs---openclaw)
- [Deepinfra - OpenClaw](#deepinfra---openclaw)

---

## Arcee AI - OpenClaw

_Source: <https://docs.openclaw.ai/fr/providers/arcee>_

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/arcee#content-area)

[OpenClaw home page](https://docs.openclaw.ai/fr)

Français

Rechercher...

Rechercher...

Providers

Arcee AI

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Premiers pas](https://docs.openclaw.ai/fr/providers/arcee#premiers-pas)
- [Configuration non interactive](https://docs.openclaw.ai/fr/providers/arcee#configuration-non-interactive)
- [Catalogue intégré](https://docs.openclaw.ai/fr/providers/arcee#catalogue-int%C3%A9gr%C3%A9)
- [Fonctionnalités prises en charge](https://docs.openclaw.ai/fr/providers/arcee#fonctionnalit%C3%A9s-prises-en-charge)
- [Connexe](https://docs.openclaw.ai/fr/providers/arcee#connexe)

[Arcee AI](https://arcee.ai/) donne accès à la famille Trinity de modèles à mélange d’experts via une API compatible OpenAI. Tous les modèles Trinity sont sous licence Apache 2.0.Les modèles Arcee AI sont accessibles directement via la plateforme Arcee ou via [OpenRouter](https://docs.openclaw.ai/fr/providers/openrouter).

| Propriété | Valeur |
| --- | --- |
| Fournisseur | `arcee` |
| Authentification | `ARCEEAI_API_KEY` (direct) ou `OPENROUTER_API_KEY` (via OpenRouter) |
| API | Compatible OpenAI |
| URL de base | `https://api.arcee.ai/api/v1` (direct) ou `https://openrouter.ai/api/v1` (OpenRouter) |

## Premiers pas

- Direct (Arcee platform)

- Via OpenRouter

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Get an API key

Créez une clé API sur [Arcee AI](https://chat.arcee.ai/).

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Run onboarding

```
openclaw onboard --auth-choice arceeai-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "arcee/trinity-large-thinking" },
    },
  },
}
```

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Get an API key

Créez une clé API sur [OpenRouter](https://openrouter.ai/keys).

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Run onboarding

```
openclaw onboard --auth-choice arceeai-openrouter
```

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/arcee#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "arcee/trinity-large-thinking" },
    },
  },
}
```

Les mêmes références de modèle fonctionnent pour les configurations directes et OpenRouter (par exemple `arcee/trinity-large-thinking`).

## Configuration non interactive

- Direct (Arcee platform)

- Via OpenRouter

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice arceeai-api-key \
  --arceeai-api-key "$ARCEEAI_API_KEY"
```

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice arceeai-openrouter \
  --openrouter-api-key "$OPENROUTER_API_KEY"
```

## Catalogue intégré

OpenClaw inclut actuellement ce catalogue Arcee groupé :

| Réf. du modèle | Nom | Entrée | Contexte | Coût (entrée/sortie par 1 M) | Notes |
| --- | --- | --- | --- | --- | --- |
| `arcee/trinity-large-thinking` | Trinity Large Thinking | texte | 256K | 0,25 /0,90/ 0,90/0,90 | Modèle par défaut ; raisonnement activé |
| `arcee/trinity-large-preview` | Trinity Large Preview | texte | 128K | 0,25 /1,00/ 1,00/1,00 | Usage général ; 400 Md de paramètres, 13 Md actifs |
| `arcee/trinity-mini` | Trinity Mini 26B | texte | 128K | 0,045 /0,15/ 0,15/0,15 | Rapide et économique ; appel de fonction |

Le préréglage d’onboarding définit `arcee/trinity-large-thinking` comme modèle par défaut.

## Fonctionnalités prises en charge

| Fonctionnalité | Pris en charge |
| --- | --- |
| Streaming | Oui |
| Utilisation d’outils / appel de fonction | Oui |
| Sortie structurée (mode JSON et schéma JSON) | Oui |
| Réflexion étendue | Oui (Trinity Large Thinking) |

Environment note

Si le Gateway s’exécute comme un démon (launchd/systemd), assurez-vous que `ARCEEAI_API_KEY`
(ou `OPENROUTER_API_KEY`) est disponible pour ce processus (par exemple, dans
`~/.openclaw/.env` ou via `env.shellEnv`).

OpenRouter routing

Lorsque vous utilisez des modèles Arcee via OpenRouter, les mêmes références de modèle `arcee/*` s’appliquent.
OpenClaw gère le routage de manière transparente selon votre choix d’authentification. Consultez la
[documentation du fournisseur OpenRouter](https://docs.openclaw.ai/fr/providers/openrouter) pour les détails de configuration
propres à OpenRouter.

## Connexe

[**OpenRouter** \\
\\
Accédez aux modèles Arcee et à de nombreux autres via une seule clé API.](https://docs.openclaw.ai/fr/providers/openrouter)

[**Model selection** \\
\\
Choisir les fournisseurs, les références de modèle et le comportement de basculement.](https://docs.openclaw.ai/fr/concepts/model-providers)

[Anthropic](https://docs.openclaw.ai/fr/providers/anthropic) [Azure Speech](https://docs.openclaw.ai/fr/providers/azure-speech)

Ctrl+I

---

## Inferrs - OpenClaw

_Source: <https://docs.openclaw.ai/fr/providers/inferrs>_

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/inferrs#content-area)

[OpenClaw home page](https://docs.openclaw.ai/fr)

Français

Rechercher...

Rechercher...

Providers

Inferrs

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Bien démarrer](https://docs.openclaw.ai/fr/providers/inferrs#bien-d%C3%A9marrer)
- [Exemple complet de configuration](https://docs.openclaw.ai/fr/providers/inferrs#exemple-complet-de-configuration)
- [Configuration avancée](https://docs.openclaw.ai/fr/providers/inferrs#configuration-avanc%C3%A9e)
- [Dépannage](https://docs.openclaw.ai/fr/providers/inferrs#d%C3%A9pannage)
- [Associé](https://docs.openclaw.ai/fr/providers/inferrs#associ%C3%A9)

[inferrs](https://github.com/ericcurtin/inferrs) peut servir des modèles locaux derrière une
API `/v1` compatible OpenAI. OpenClaw fonctionne avec `inferrs` via le chemin générique
`openai-completions`.`inferrs` est actuellement mieux traité comme un backend OpenAI-compatible auto-hébergé personnalisé,
et non comme un plugin fournisseur OpenClaw dédié.

## Bien démarrer

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/inferrs#)

Démarrer inferrs avec un modèle

```
inferrs serve google/gemma-4-E2B-it \
  --host 127.0.0.1 \
  --port 8080 \
  --device metal
```

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/inferrs#)

Vérifier que le serveur est joignable

```
curl http://127.0.0.1:8080/health
curl http://127.0.0.1:8080/v1/models
```

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/inferrs#)

Ajouter une entrée de fournisseur OpenClaw

Ajoutez une entrée explicite de fournisseur et pointez votre modèle par défaut vers elle. Voir l’exemple de configuration complet ci-dessous.

## Exemple complet de configuration

Cet exemple utilise Gemma 4 sur un serveur local `inferrs`.

```
{
  agents: {
    defaults: {
      model: { primary: "inferrs/google/gemma-4-E2B-it" },
      models: {
        "inferrs/google/gemma-4-E2B-it": {
          alias: "Gemma 4 (inferrs)",
        },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      inferrs: {
        baseUrl: "http://127.0.0.1:8080/v1",
        apiKey: "inferrs-local",
        api: "openai-completions",
        models: [\
          {\
            id: "google/gemma-4-E2B-it",\
            name: "Gemma 4 E2B (inferrs)",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 131072,\
            maxTokens: 4096,\
            compat: {\
              requiresStringContent: true,\
            },\
          },\
        ],
      },
    },
  },
}
```

## Configuration avancée

Pourquoi requiresStringContent est important

Certaines routes Chat Completions de `inferrs` n’acceptent que des
`messages[].content` de type chaîne, et non des tableaux structurés de content-part.

Si les exécutions OpenClaw échouent avec une erreur du type :

```
messages[1].content: invalid type: sequence, expected a string
```

définissez `compat.requiresStringContent: true` dans votre entrée de modèle.

```
compat: {
  requiresStringContent: true
}
```

OpenClaw aplatira les content parts purement textuels en chaînes simples avant d’envoyer
la requête.

Limite Gemma et schéma d’outil

Certaines combinaisons actuelles `inferrs` \+ Gemma acceptent de petites requêtes directes
`/v1/chat/completions` mais échouent encore sur des tours complets du runtime d’agent OpenClaw.Si cela arrive, essayez d’abord ceci :

```
compat: {
  requiresStringContent: true,
  supportsTools: false
}
```

Cela désactive la surface de schéma d’outil d’OpenClaw pour le modèle et peut réduire la pression de prompt
sur les backends locaux plus stricts.Si les petites requêtes directes fonctionnent toujours mais que les tours normaux d’agent OpenClaw continuent de
planter dans `inferrs`, le problème restant provient généralement du comportement amont du modèle/serveur
plutôt que de la couche de transport d’OpenClaw.

Test rapide manuel

Une fois configuré, testez les deux couches :

```
curl http://127.0.0.1:8080/v1/chat/completions \
  -H 'content-type: application/json' \
  -d '{"model":"google/gemma-4-E2B-it","messages":[{"role":"user","content":"What is 2 + 2?"}],"stream":false}'
```

```
openclaw infer model run \
  --model inferrs/google/gemma-4-E2B-it \
  --prompt "What is 2 + 2? Reply with one short sentence." \
  --json
```

Si la première commande fonctionne mais pas la seconde, consultez la section dépannage ci-dessous.

Comportement de type proxy

`inferrs` est traité comme un backend `/v1` compatible OpenAI de type proxy, pas comme un
endpoint OpenAI natif.

- La mise en forme de requête réservée à OpenAI natif ne s’applique pas ici
- Pas de `service_tier`, pas de `store` Responses, pas d’indices de prompt-cache, et pas de mise en forme de charge utile de compatibilité reasoning OpenAI
- Les en-têtes d’attribution OpenClaw cachés (`originator`, `version`, `User-Agent`) ne sont pas injectés sur des `baseUrl``inferrs` personnalisées

## Dépannage

curl /v1/models échoue

`inferrs` n’est pas en cours d’exécution, n’est pas joignable, ou n’est pas lié au
bon hôte/port. Assurez-vous que le serveur est démarré et écoute sur l’adresse que vous
avez configurée.

messages\[\].content expected a string

Définissez `compat.requiresStringContent: true` dans l’entrée de modèle. Voir la
section `requiresStringContent` ci-dessus pour les détails.

Les appels directs /v1/chat/completions passent, mais openclaw infer model run échoue

Essayez de définir `compat.supportsTools: false` pour désactiver la surface de schéma d’outil.
Voir la remarque ci-dessus sur la limite Gemma liée au schéma d’outil.

inferrs plante encore sur des tours d’agent plus gros

Si OpenClaw n’obtient plus d’erreurs de schéma mais que `inferrs` plante encore sur des tours
d’agent plus gros, traitez cela comme une limitation amont de `inferrs` ou du modèle. Réduisez
la pression de prompt ou passez à un autre backend ou modèle local.

Pour une aide générale, voir [Dépannage](https://docs.openclaw.ai/fr/help/troubleshooting) et [FAQ](https://docs.openclaw.ai/fr/help/faq).

## Associé

[**Modèles locaux** \\
\\
Exécuter OpenClaw sur des serveurs de modèles locaux.](https://docs.openclaw.ai/fr/gateway/local-models)

[**Dépannage du Gateway** \\
\\
Déboguer les backends locaux compatibles OpenAI qui passent les sondes mais échouent lors des exécutions d’agent.](https://docs.openclaw.ai/fr/gateway/troubleshooting#local-openai-compatible-backend-passes-direct-probes-but-agent-runs-fail)

[**Sélection de modèle** \\
\\
Vue d’ensemble de tous les fournisseurs, références de modèle et comportements de basculement.](https://docs.openclaw.ai/fr/concepts/model-providers)

[Hugging Face (inférence)](https://docs.openclaw.ai/fr/providers/huggingface) [Inworld](https://docs.openclaw.ai/fr/providers/inworld)

Ctrl+I

---

## Provider directory - OpenClaw

_Source: <https://docs.openclaw.ai/providers>_

# Model Providers

OpenClaw can use many LLM providers. Pick a provider, authenticate, then set the
default model as `provider/model`.Looking for chat channel docs (WhatsApp/Telegram/Discord/Slack/Mattermost (plugin)/etc.)? See [Channels](https://docs.openclaw.ai/channels).

## Quick start

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## Provider docs

- [Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)
- [Amazon Bedrock Mantle](https://docs.openclaw.ai/providers/bedrock-mantle)
- [Anthropic (API + Claude CLI)](https://docs.openclaw.ai/providers/anthropic)
- [Arcee AI (Trinity models)](https://docs.openclaw.ai/providers/arcee)
- [Azure Speech](https://docs.openclaw.ai/providers/azure-speech)
- [BytePlus (International)](https://docs.openclaw.ai/concepts/model-providers#byteplus-international)
- [Cerebras](https://docs.openclaw.ai/providers/cerebras)
- [Chutes](https://docs.openclaw.ai/providers/chutes)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)
- [ComfyUI](https://docs.openclaw.ai/providers/comfy)
- [DeepSeek](https://docs.openclaw.ai/providers/deepseek)
- [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs)
- [fal](https://docs.openclaw.ai/providers/fal)
- [Fireworks](https://docs.openclaw.ai/providers/fireworks)
- [GitHub Copilot](https://docs.openclaw.ai/providers/github-copilot)
- [GLM models](https://docs.openclaw.ai/providers/glm)
- [Google (Gemini)](https://docs.openclaw.ai/providers/google)
- [Gradium](https://docs.openclaw.ai/providers/gradium)
- [Groq (LPU inference)](https://docs.openclaw.ai/providers/groq)
- [Hugging Face (Inference)](https://docs.openclaw.ai/providers/huggingface)
- [inferrs (local models)](https://docs.openclaw.ai/providers/inferrs)
- [Kilocode](https://docs.openclaw.ai/providers/kilocode)
- [LiteLLM (unified gateway)](https://docs.openclaw.ai/providers/litellm)
- [LM Studio (local models)](https://docs.openclaw.ai/providers/lmstudio)
- [MiniMax](https://docs.openclaw.ai/providers/minimax)
- [Mistral](https://docs.openclaw.ai/providers/mistral)
- [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot)
- [NVIDIA](https://docs.openclaw.ai/providers/nvidia)
- [Ollama (cloud + local models)](https://docs.openclaw.ai/providers/ollama)
- [OpenAI (API + Codex)](https://docs.openclaw.ai/providers/openai)
- [OpenCode](https://docs.openclaw.ai/providers/opencode)
- [OpenCode Go](https://docs.openclaw.ai/providers/opencode-go)
- [OpenRouter](https://docs.openclaw.ai/providers/openrouter)
- [Perplexity (web search)](https://docs.openclaw.ai/providers/perplexity-provider)
- [Qianfan](https://docs.openclaw.ai/providers/qianfan)
- [Qwen Cloud](https://docs.openclaw.ai/providers/qwen)
- [Runway](https://docs.openclaw.ai/providers/runway)
- [SenseAudio](https://docs.openclaw.ai/providers/senseaudio)
- [SGLang (local models)](https://docs.openclaw.ai/providers/sglang)
- [StepFun](https://docs.openclaw.ai/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/providers/synthetic)
- [Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent)
- [Together AI](https://docs.openclaw.ai/providers/together)
- [Venice (Venice AI, privacy-focused)](https://docs.openclaw.ai/providers/venice)
- [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)
- [vLLM (local models)](https://docs.openclaw.ai/providers/vllm)
- [Volcengine (Doubao)](https://docs.openclaw.ai/providers/volcengine)
- [Vydra](https://docs.openclaw.ai/providers/vydra)
- [xAI](https://docs.openclaw.ai/providers/xai)
- [Xiaomi](https://docs.openclaw.ai/providers/xiaomi)
- [Z.AI](https://docs.openclaw.ai/providers/zai)

## Shared overview pages

- [Additional bundled variants](https://docs.openclaw.ai/providers/models#additional-bundled-provider-variants) \- Anthropic Vertex, Copilot Proxy, and Gemini CLI OAuth
- [Image Generation](https://docs.openclaw.ai/tools/image-generation) \- Shared `image_generate` tool, provider selection, and failover
- [Music Generation](https://docs.openclaw.ai/tools/music-generation) \- Shared `music_generate` tool, provider selection, and failover
- [Video Generation](https://docs.openclaw.ai/tools/video-generation) \- Shared `video_generate` tool, provider selection, and failover

## Transcription providers

- [Deepgram (audio transcription)](https://docs.openclaw.ai/providers/deepgram)
- [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs#speech-to-text)
- [Mistral](https://docs.openclaw.ai/providers/mistral#audio-transcription-voxtral)
- [OpenAI](https://docs.openclaw.ai/providers/openai#speech-to-text)
- [SenseAudio](https://docs.openclaw.ai/providers/senseaudio)
- [xAI](https://docs.openclaw.ai/providers/xai#speech-to-text)

## Community tools

- [Claude Max API Proxy](https://docs.openclaw.ai/providers/claude-max-api-proxy) \- Community proxy for Claude subscription credentials (verify Anthropic policy/terms before use)

For the full provider catalog (xAI, Groq, Mistral, etc.) and advanced configuration,
see [Model providers](https://docs.openclaw.ai/concepts/model-providers).

[Model provider quickstart](https://docs.openclaw.ai/providers/models)

Ctrl+I

---

## Alibaba Model Studio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/alibaba>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Alibaba Model Studio

OpenClaw ships a bundled `alibaba` video-generation provider for Wan models on
Alibaba Model Studio / DashScope.

- Provider: `alibaba`
- Preferred auth: `MODELSTUDIO_API_KEY`
- Also accepted: `DASHSCOPE_API_KEY`, `QWEN_API_KEY`
- API: DashScope / Model Studio async video generation

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set an API key

```
openclaw onboard --auth-choice qwen-standard-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set a default video model

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "alibaba/wan2.6-t2v",
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Verify the provider is available

```
openclaw models list --provider alibaba
```

Any of the accepted auth keys (`MODELSTUDIO_API_KEY`, `DASHSCOPE_API_KEY`, `QWEN_API_KEY`) will work. The `qwen-standard-api-key` onboarding choice configures the shared DashScope credential.

## Built-in Wan models

The bundled `alibaba` provider currently registers:

| Model ref | Mode |
| --- | --- |
| `alibaba/wan2.6-t2v` | Text-to-video |
| `alibaba/wan2.6-i2v` | Image-to-video |
| `alibaba/wan2.6-r2v` | Reference-to-video |
| `alibaba/wan2.6-r2v-flash` | Reference-to-video (fast) |
| `alibaba/wan2.7-r2v` | Reference-to-video |

## Current limits

| Parameter | Limit |
| --- | --- |
| Output videos | Up to **1** per request |
| Input images | Up to **1** |
| Input videos | Up to **4** |
| Duration | Up to **10 seconds** |
| Supported controls | `size`, `aspectRatio`, `resolution`, `audio`, `watermark` |
| Reference image/video | Remote `http(s)` URLs only |

Reference image/video mode currently requires **remote http(s) URLs**. Local file paths are not supported for reference inputs.

## Advanced configuration

Relationship to Qwen

The bundled `qwen` provider also uses Alibaba-hosted DashScope endpoints for
Wan video generation. Use:

- `qwen/...` when you want the canonical Qwen provider surface
- `alibaba/...` when you want the direct vendor-owned Wan video surface

See the [Qwen provider docs](https://docs.openclaw.ai/providers/qwen) for more detail.

Auth key priority

OpenClaw checks for auth keys in this order:

1. `MODELSTUDIO_API_KEY` (preferred)
2. `DASHSCOPE_API_KEY`
3. `QWEN_API_KEY`

Any of these will authenticate the `alibaba` provider.

## Related

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Qwen** \\
\\
Qwen provider setup and DashScope integration.](https://docs.openclaw.ai/providers/qwen)

[**Configuration reference** \\
\\
Agent defaults and model configuration.](https://docs.openclaw.ai/gateway/config-agents#agent-defaults)

[Model failover](https://docs.openclaw.ai/concepts/model-failover) [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)

Ctrl+I

---

## Anthropic - OpenClaw

_Source: <https://docs.openclaw.ai/providers/anthropic>_

# choose: Anthropic API key
```

Or pass the key directly:

```
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

### Config example

```
{
  env: { ANTHROPIC_API_KEY: "sk-ant-..." },
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

**Best for:** reusing an existing Claude CLI login without a separate API key.

1

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Ensure Claude CLI is installed and logged in

Verify with:

```
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Run onboarding

```
openclaw onboard
# choose: Claude CLI
```

OpenClaw detects and reuses the existing Claude CLI credentials.

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

Setup and runtime details for the Claude CLI backend are in [CLI Backends](https://docs.openclaw.ai/gateway/cli-backends).

### Config example

Prefer the canonical Anthropic model ref plus a CLI runtime override:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-7" },
      agentRuntime: { id: "claude-cli" },
    },
  },
}
```

Legacy `claude-cli/claude-opus-4-7` model refs still work for
compatibility, but new config should keep provider/model selection as
`anthropic/*` and put the execution backend in `agentRuntime.id`.

If you want the clearest billing path, use an Anthropic API key instead. OpenClaw also supports subscription-style options from [OpenAI Codex](https://docs.openclaw.ai/providers/openai), [Qwen Cloud](https://docs.openclaw.ai/providers/qwen), [MiniMax](https://docs.openclaw.ai/providers/minimax), and [Z.AI / GLM](https://docs.openclaw.ai/providers/glm).

## Thinking defaults (Claude 4.6)

Claude 4.6 models default to `adaptive` thinking in OpenClaw when no explicit thinking level is set.Override per-message with `/think:<level>` or in model params:

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { thinking: "adaptive" },
        },
      },
    },
  },
}
```

Related Anthropic docs:

- [Adaptive thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- [Extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)

## Prompt caching

OpenClaw supports Anthropic’s prompt caching feature for API-key auth.

| Value | Cache duration | Description |
| --- | --- | --- |
| `"short"` (default) | 5 minutes | Applied automatically for API-key auth |
| `"long"` | 1 hour | Extended cache |
| `"none"` | No caching | Disable prompt caching |

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
  },
}
```

Per-agent cache overrides

Use model-level params as your baseline, then override specific agents via `agents.list[].params`:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-6" },
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
    list: [\
      { id: "research", default: true },\
      { id: "alerts", params: { cacheRetention: "none" } },\
    ],
  },
}
```

Config merge order:

1. `agents.defaults.models["provider/model"].params`
2. `agents.list[].params` (matching `id`, overrides by key)

This lets one agent keep a long-lived cache while another agent on the same model disables caching for bursty/low-reuse traffic.

Bedrock Claude notes

- Anthropic Claude models on Bedrock (`amazon-bedrock/*anthropic.claude*`) accept `cacheRetention` pass-through when configured.
- Non-Anthropic Bedrock models are forced to `cacheRetention: "none"` at runtime.
- API-key smart defaults also seed `cacheRetention: "short"` for Claude-on-Bedrock refs when no explicit value is set.

## Advanced configuration

Fast mode

OpenClaw’s shared `/fast` toggle supports direct Anthropic traffic (API-key and OAuth to `api.anthropic.com`).

| Command | Maps to |
| --- | --- |
| `/fast on` | `service_tier: "auto"` |
| `/fast off` | `service_tier: "standard_only"` |

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-sonnet-4-6": {
          params: { fastMode: true },
        },
      },
    },
  },
}
```

- Only injected for direct `api.anthropic.com` requests. Proxy routes leave `service_tier` untouched.
- Explicit `serviceTier` or `service_tier` params override `/fast` when both are set.
- On accounts without Priority Tier capacity, `service_tier: "auto"` may resolve to `standard`.

Media understanding (image and PDF)

The bundled Anthropic plugin registers image and PDF understanding. OpenClaw
auto-resolves media capabilities from the configured Anthropic auth — no
additional config is needed.

| Property | Value |
| --- | --- |
| Default model | `claude-opus-4-6` |
| Supported input | Images, PDF documents |

When an image or PDF is attached to a conversation, OpenClaw automatically
routes it through the Anthropic media understanding provider.

1M context window (beta)

Anthropic’s 1M context window is beta-gated. Enable it per model:

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { context1m: true },
        },
      },
    },
  },
}
```

OpenClaw maps this to `anthropic-beta: context-1m-2025-08-07` on requests.`params.context1m: true` also applies to the Claude CLI backend
(`claude-cli/*`) for eligible Opus and Sonnet models, expanding the runtime
context window for those CLI sessions to match the direct-API behavior.

Requires long-context access on your Anthropic credential. Legacy token auth (`sk-ant-oat-*`) is rejected for 1M context requests — OpenClaw logs a warning and falls back to the standard context window.

Claude Opus 4.7 1M context

`anthropic/claude-opus-4.7` and its `claude-cli` variant have a 1M context
window by default — no `params.context1m: true` needed.

## Troubleshooting

401 errors / token suddenly invalid

Anthropic token auth expires and can be revoked. For new setups, use an Anthropic API key instead.

No API key found for provider "anthropic"

Anthropic auth is **per agent** — new agents do not inherit the main agent’s keys. Re-run onboarding for that agent (or configure an API key on the gateway host), then verify with `openclaw models status`.

No credentials found for profile "anthropic:default"

Run `openclaw models status` to see which auth profile is active. Re-run onboarding, or configure an API key for that profile path.

No available auth profile (all in cooldown)

Check `openclaw models status --json` for `auth.unusableProfiles`. Anthropic rate-limit cooldowns can be model-scoped, so a sibling Anthropic model may still be usable. Add another Anthropic profile or wait for cooldown.

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**CLI backends** \\
\\
Claude CLI backend setup and runtime details.](https://docs.openclaw.ai/gateway/cli-backends)

[**Prompt caching** \\
\\
How prompt caching works across providers.](https://docs.openclaw.ai/reference/prompt-caching)

[**OAuth and auth** \\
\\
Auth details and credential reuse rules.](https://docs.openclaw.ai/gateway/authentication)

[Amazon Bedrock Mantle](https://docs.openclaw.ai/providers/bedrock-mantle) [Arcee AI](https://docs.openclaw.ai/providers/arcee)

Ctrl+I

---

## Amazon Bedrock - OpenClaw

_Source: <https://docs.openclaw.ai/providers/bedrock>_

# Optional:
export AWS_SESSION_TOKEN="..."
export AWS_PROFILE="your-profile"
# Optional (Bedrock API key/bearer token):
export AWS_BEARER_TOKEN_BEDROCK="..."
```

2

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Add a Bedrock provider and model to your config

No `apiKey` is required. Configure the provider with `auth: "aws-sdk"`:

```
{
  models: {
    providers: {
      "amazon-bedrock": {
        baseUrl: "https://bedrock-runtime.us-east-1.amazonaws.com",
        api: "bedrock-converse-stream",
        auth: "aws-sdk",
        models: [\
          {\
            id: "us.anthropic.claude-opus-4-6-v1:0",\
            name: "Claude Opus 4.6 (Bedrock)",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 200000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "amazon-bedrock/us.anthropic.claude-opus-4-6-v1:0" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Verify models are available

```
openclaw models list
```

With env-marker auth (`AWS_ACCESS_KEY_ID`, `AWS_PROFILE`, or `AWS_BEARER_TOKEN_BEDROCK`), OpenClaw auto-enables the implicit Bedrock provider for model discovery without extra config.

**Best for:** EC2 instances with an IAM role attached, using the instance metadata service for authentication.

1

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Enable discovery explicitly

When using IMDS, OpenClaw cannot detect AWS auth from env markers alone, so you must opt in:

```
openclaw config set plugins.entries.amazon-bedrock.config.discovery.enabled true
openclaw config set plugins.entries.amazon-bedrock.config.discovery.region us-east-1
```

2

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Optionally add an env marker for auto mode

If you also want the env-marker auto-detection path to work (for example, for `openclaw status` surfaces):

```
export AWS_PROFILE=default
export AWS_REGION=us-east-1
```

You do **not** need a fake API key.

3

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Verify models are discovered

```
openclaw models list
```

The IAM role attached to your EC2 instance must have the following permissions:

- `bedrock:InvokeModel`
- `bedrock:InvokeModelWithResponseStream`
- `bedrock:ListFoundationModels` (for automatic discovery)
- `bedrock:ListInferenceProfiles` (for inference profile discovery)

Or attach the managed policy `AmazonBedrockFullAccess`.

You only need `AWS_PROFILE=default` if you specifically want an env marker for auto mode or status surfaces. The actual Bedrock runtime auth path uses the AWS SDK default chain, so IMDS instance-role auth works even without env markers.

## Automatic model discovery

OpenClaw can automatically discover Bedrock models that support **streaming**
and **text output**. Discovery uses `bedrock:ListFoundationModels` and
`bedrock:ListInferenceProfiles`, and results are cached (default: 1 hour).How the implicit provider is enabled:

- If `plugins.entries.amazon-bedrock.config.discovery.enabled` is `true`,
OpenClaw will try discovery even when no AWS env marker is present.
- If `plugins.entries.amazon-bedrock.config.discovery.enabled` is unset,
OpenClaw only auto-adds the
implicit Bedrock provider when it sees one of these AWS auth markers:
`AWS_BEARER_TOKEN_BEDROCK`, `AWS_ACCESS_KEY_ID` +
`AWS_SECRET_ACCESS_KEY`, or `AWS_PROFILE`.
- The actual Bedrock runtime auth path still uses the AWS SDK default chain, so
shared config, SSO, and IMDS instance-role auth can work even when discovery
needed `enabled: true` to opt in.

For explicit `models.providers["amazon-bedrock"]` entries, OpenClaw can still resolve Bedrock env-marker auth early from AWS env markers such as `AWS_BEARER_TOKEN_BEDROCK` without forcing full runtime auth loading. The actual model-call auth path still uses the AWS SDK default chain.

Discovery config options

Config options live under `plugins.entries.amazon-bedrock.config.discovery`:

```
{
  plugins: {
    entries: {
      "amazon-bedrock": {
        config: {
          discovery: {
            enabled: true,
            region: "us-east-1",
            providerFilter: ["anthropic", "amazon"],
            refreshInterval: 3600,
            defaultContextWindow: 32000,
            defaultMaxTokens: 4096,
          },
        },
      },
    },
  },
}
```

| Option | Default | Description |
| --- | --- | --- |
| `enabled` | auto | In auto mode, OpenClaw only enables the implicit Bedrock provider when it sees a supported AWS env marker. Set `true` to force discovery. |
| `region` | `AWS_REGION` / `AWS_DEFAULT_REGION` / `us-east-1` | AWS region used for discovery API calls. |
| `providerFilter` | (all) | Matches Bedrock provider names (for example `anthropic`, `amazon`). |
| `refreshInterval` | `3600` | Cache duration in seconds. Set to `0` to disable caching. |
| `defaultContextWindow` | `32000` | Context window used for discovered models (override if you know your model limits). |
| `defaultMaxTokens` | `4096` | Max output tokens used for discovered models (override if you know your model limits). |

## Quick setup (AWS path)

This walkthrough creates an IAM role, attaches Bedrock permissions, associates
the instance profile, and enables OpenClaw discovery on the EC2 host.

```
# 1. Create IAM role and instance profile
aws iam create-role --role-name EC2-Bedrock-Access \
  --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [{\
      "Effect": "Allow",\
      "Principal": {"Service": "ec2.amazonaws.com"},\
      "Action": "sts:AssumeRole"\
    }]
  }'

aws iam attach-role-policy --role-name EC2-Bedrock-Access \
  --policy-arn arn:aws:iam::aws:policy/AmazonBedrockFullAccess

aws iam create-instance-profile --instance-profile-name EC2-Bedrock-Access
aws iam add-role-to-instance-profile \
  --instance-profile-name EC2-Bedrock-Access \
  --role-name EC2-Bedrock-Access

# 2. Attach to your EC2 instance
aws ec2 associate-iam-instance-profile \
  --instance-id i-xxxxx \
  --iam-instance-profile Name=EC2-Bedrock-Access

# 3. On the EC2 instance, enable discovery explicitly
openclaw config set plugins.entries.amazon-bedrock.config.discovery.enabled true
openclaw config set plugins.entries.amazon-bedrock.config.discovery.region us-east-1

# 4. Optional: add an env marker if you want auto mode without explicit enable
echo 'export AWS_PROFILE=default' >> ~/.bashrc
echo 'export AWS_REGION=us-east-1' >> ~/.bashrc
source ~/.bashrc

# 5. Verify models are discovered
openclaw models list
```

## Advanced configuration

Inference profiles

OpenClaw discovers **regional and global inference profiles** alongside
foundation models. When a profile maps to a known foundation model, the
profile inherits that model’s capabilities (context window, max tokens,
reasoning, vision) and the correct Bedrock request region is injected
automatically. This means cross-region Claude profiles work without manual
provider overrides.Inference profile IDs look like `us.anthropic.claude-opus-4-6-v1:0` (regional)
or `anthropic.claude-opus-4-6-v1:0` (global). If the backing model is already
in the discovery results, the profile inherits its full capability set;
otherwise safe defaults apply.No extra configuration is needed. As long as discovery is enabled and the IAM
principal has `bedrock:ListInferenceProfiles`, profiles appear alongside
foundation models in `openclaw models list`.

Claude Opus 4.7 temperature

Bedrock rejects the `temperature` parameter for Claude Opus 4.7. OpenClaw
omits `temperature` automatically for any Opus 4.7 Bedrock ref, including
foundation model ids, named inference profiles, application inference
profiles whose underlying model resolves to Opus 4.7 via
`bedrock:GetInferenceProfile`, and dotted `opus-4.7` variants with
optional region prefixes (`us.`, `eu.`, `ap.`, `apac.`, `au.`, `jp.`,
`global.`). No config knob is required, and the omission applies to both
the request options object and the `inferenceConfig` payload field.

Guardrails

You can apply [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
to all Bedrock model invocations by adding a `guardrail` object to the
`amazon-bedrock` plugin config. Guardrails let you enforce content filtering,
topic denial, word filters, sensitive information filters, and contextual
grounding checks.

```
{
  plugins: {
    entries: {
      "amazon-bedrock": {
        config: {
          guardrail: {
            guardrailIdentifier: "abc123", // guardrail ID or full ARN
            guardrailVersion: "1", // version number or "DRAFT"
            streamProcessingMode: "sync", // optional: "sync" or "async"
            trace: "enabled", // optional: "enabled", "disabled", or "enabled_full"
          },
        },
      },
    },
  },
}
```

| Option | Required | Description |
| --- | --- | --- |
| `guardrailIdentifier` | Yes | Guardrail ID (e.g. `abc123`) or full ARN (e.g. `arn:aws:bedrock:us-east-1:123456789012:guardrail/abc123`). |
| `guardrailVersion` | Yes | Published version number, or `"DRAFT"` for the working draft. |
| `streamProcessingMode` | No | `"sync"` or `"async"` for guardrail evaluation during streaming. If omitted, Bedrock uses its default. |
| `trace` | No | `"enabled"` or `"enabled_full"` for debugging; omit or set `"disabled"` for production. |

The IAM principal used by the gateway must have the `bedrock:ApplyGuardrail` permission in addition to the standard invoke permissions.

Embeddings for memory search

Bedrock can also serve as the embedding provider for
[memory search](https://docs.openclaw.ai/concepts/memory-search). This is configured separately from the
inference provider — set `agents.defaults.memorySearch.provider` to `"bedrock"`:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "bedrock",
        model: "amazon.titan-embed-text-v2:0", // default
      },
    },
  },
}
```

Bedrock embeddings use the same AWS SDK credential chain as inference (instance
roles, SSO, access keys, shared config, and web identity). No API key is
needed. When `provider` is `"auto"`, Bedrock is auto-detected if that
credential chain resolves successfully.Supported embedding models include Amazon Titan Embed (v1, v2), Amazon Nova
Embed, Cohere Embed (v3, v4), and TwelveLabs Marengo. See
[Memory configuration reference — Bedrock](https://docs.openclaw.ai/reference/memory-config#bedrock-embedding-config)
for the full model list and dimension options.

Notes and caveats

- Bedrock requires **model access** enabled in your AWS account/region.
- Automatic discovery needs the `bedrock:ListFoundationModels` and
`bedrock:ListInferenceProfiles` permissions.
- If you rely on auto mode, set one of the supported AWS auth env markers on the
gateway host. If you prefer IMDS/shared-config auth without env markers, set
`plugins.entries.amazon-bedrock.config.discovery.enabled: true`.
- OpenClaw surfaces the credential source in this order: `AWS_BEARER_TOKEN_BEDROCK`,
then `AWS_ACCESS_KEY_ID` \+ `AWS_SECRET_ACCESS_KEY`, then `AWS_PROFILE`, then the
default AWS SDK chain.
- Reasoning support depends on the model; check the Bedrock model card for
current capabilities.
- If you prefer a managed key flow, you can also place an OpenAI-compatible
proxy in front of Bedrock and configure it as an OpenAI provider instead.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Memory search** \\
\\
Bedrock embeddings for memory search configuration.](https://docs.openclaw.ai/concepts/memory-search)

[**Memory config reference** \\
\\
Full Bedrock embedding model list and dimension options.](https://docs.openclaw.ai/reference/memory-config#bedrock-embedding-config)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba) [Amazon Bedrock Mantle](https://docs.openclaw.ai/providers/bedrock-mantle)

Ctrl+I

---

## Cerebras - OpenClaw

_Source: <https://docs.openclaw.ai/providers/cerebras>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Cerebras

[Cerebras](https://www.cerebras.ai/) provides high-speed OpenAI-compatible inference.

| Property | Value |
| --- | --- |
| Provider | `cerebras` |
| Auth | `CEREBRAS_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.cerebras.ai/v1` |

## Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Get an API key

Create an API key in the [Cerebras Cloud Console](https://cloud.cerebras.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Run onboarding

```
openclaw onboard --auth-choice cerebras-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Verify models are available

```
openclaw models list --provider cerebras
```

### Non-Interactive Setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cerebras-api-key \
  --cerebras-api-key "$CEREBRAS_API_KEY"
```

## Built-In Catalog

OpenClaw ships a static Cerebras catalog for the public OpenAI-compatible endpoint:

| Model ref | Name | Notes |
| --- | --- | --- |
| `cerebras/zai-glm-4.7` | Z.ai GLM 4.7 | Default model; preview reasoning model |
| `cerebras/gpt-oss-120b` | GPT OSS 120B | Production reasoning model |
| `cerebras/qwen-3-235b-a22b-instruct-2507` | Qwen 3 235B Instruct | Preview non-reasoning model |
| `cerebras/llama3.1-8b` | Llama 3.1 8B | Production speed-focused model |

Cerebras marks `zai-glm-4.7` and `qwen-3-235b-a22b-instruct-2507` as preview models, and `llama3.1-8b` / `qwen-3-235b-a22b-instruct-2507` are documented for deprecation on May 27, 2026. Check Cerebras’ supported-models page before relying on them for production.

## Manual Config

The bundled plugin usually means you only need the API key. Use explicit
`models.providers.cerebras` config when you want to override model metadata:

```
{
  env: { CEREBRAS_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "cerebras/zai-glm-4.7" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [\
          { id: "zai-glm-4.7", name: "Z.ai GLM 4.7" },\
          { id: "gpt-oss-120b", name: "GPT OSS 120B" },\
        ],
      },
    },
  },
}
```

If the Gateway runs as a daemon (launchd/systemd), make sure `CEREBRAS_API_KEY`
is available to that process, for example in `~/.openclaw/.env` or through
`env.shellEnv`.

[Azure Speech](https://docs.openclaw.ai/providers/azure-speech) [Chutes](https://docs.openclaw.ai/providers/chutes)

Ctrl+I

---

## Claude Max API proxy - OpenClaw

_Source: <https://docs.openclaw.ai/providers/claude-max-api-proxy>_

# Verify Claude CLI is authenticated
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Start the server

```
claude-max-api
# Server runs at http://localhost:3456
```

3

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Test the proxy

```
# Health check
curl http://localhost:3456/health

# List models
curl http://localhost:3456/v1/models

# Chat completion
curl http://localhost:3456/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-opus-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

4

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Configure OpenClaw

Point OpenClaw at the proxy as a custom OpenAI-compatible endpoint:

```
{
  env: {
    OPENAI_API_KEY: "not-needed",
    OPENAI_BASE_URL: "http://localhost:3456/v1",
  },
  agents: {
    defaults: {
      model: { primary: "openai/claude-opus-4" },
    },
  },
}
```

## Built-in catalog

| Model ID | Maps To |
| --- | --- |
| `claude-opus-4` | Claude Opus 4 |
| `claude-sonnet-4` | Claude Sonnet 4 |
| `claude-haiku-4` | Claude Haiku 4 |

## Advanced configuration

Proxy-style OpenAI-compatible notes

This path uses the same proxy-style OpenAI-compatible route as other custom
`/v1` backends:

- Native OpenAI-only request shaping does not apply
- No `service_tier`, no Responses `store`, no prompt-cache hints, and no
OpenAI reasoning-compat payload shaping
- Hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on the proxy URL

Auto-start on macOS with LaunchAgent

Create a LaunchAgent to run the proxy automatically:

```
cat > ~/Library/LaunchAgents/com.claude-max-api.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.claude-max-api</string>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <true/>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/bin/node</string>
    <string>/usr/local/lib/node_modules/claude-max-api-proxy/dist/server/standalone.js</string>
  </array>
  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/usr/local/bin:/opt/homebrew/bin:~/.local/bin:/usr/bin:/bin</string>
  </dict>
</dict>
</plist>
EOF

launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.claude-max-api.plist
```

## Links

- **npm:** [https://www.npmjs.com/package/claude-max-api-proxy](https://www.npmjs.com/package/claude-max-api-proxy)
- **GitHub:** [https://github.com/atalovesyou/claude-max-api-proxy](https://github.com/atalovesyou/claude-max-api-proxy)
- **Issues:** [https://github.com/atalovesyou/claude-max-api-proxy/issues](https://github.com/atalovesyou/claude-max-api-proxy/issues)

## Notes

- This is a **community tool**, not officially supported by Anthropic or OpenClaw
- Requires an active Claude Max/Pro subscription with Claude Code CLI authenticated
- The proxy runs locally and does not send data to any third-party servers
- Streaming responses are fully supported

For native Anthropic integration with Claude CLI or API keys, see [Anthropic provider](https://docs.openclaw.ai/providers/anthropic). For OpenAI/Codex subscriptions, see [OpenAI provider](https://docs.openclaw.ai/providers/openai).

## Related

[**Anthropic provider** \\
\\
Native OpenClaw integration with Claude CLI or API keys.](https://docs.openclaw.ai/providers/anthropic)

[**OpenAI provider** \\
\\
For OpenAI/Codex subscriptions.](https://docs.openclaw.ai/providers/openai)

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[Chutes](https://docs.openclaw.ai/providers/chutes) [Cloudflare AI gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)

Ctrl+I

---

## Cloudflare AI gateway - OpenClaw

_Source: <https://docs.openclaw.ai/providers/cloudflare-ai-gateway>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Cloudflare AI gateway

Cloudflare AI Gateway sits in front of provider APIs and lets you add analytics, caching, and controls. For Anthropic, OpenClaw uses the Anthropic Messages API through your Gateway endpoint.

| Property | Value |
| --- | --- |
| Provider | `cloudflare-ai-gateway` |
| Base URL | `https://gateway.ai.cloudflare.com/v1/<account_id>/<gateway_id>/anthropic` |
| Default model | `cloudflare-ai-gateway/claude-sonnet-4-6` |
| API key | `CLOUDFLARE_AI_GATEWAY_API_KEY` (your provider API key for requests through the Gateway) |

For Anthropic models routed through Cloudflare AI Gateway, use your **Anthropic API key** as the provider key.

When thinking is enabled for Anthropic Messages models, OpenClaw strips trailing
assistant prefill turns before sending the payload through Cloudflare AI Gateway.
Anthropic rejects response prefilling with extended thinking, while ordinary
non-thinking prefill remains available.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Set the provider API key and Gateway details

Run onboarding and choose the Cloudflare AI Gateway auth option:

```
openclaw onboard --auth-choice cloudflare-ai-gateway-api-key
```

This prompts for your account ID, gateway ID, and API key.

2

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Set a default model

Add the model to your OpenClaw config:

```
{
  agents: {
    defaults: {
      model: { primary: "cloudflare-ai-gateway/claude-sonnet-4-6" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Verify the model is available

```
openclaw models list --provider cloudflare-ai-gateway
```

## Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY"
```

## Advanced configuration

Authenticated gateways

If you enabled Gateway authentication in Cloudflare, add the `cf-aig-authorization` header. This is **in addition to** your provider API key.

```
{
  models: {
    providers: {
      "cloudflare-ai-gateway": {
        headers: {
          "cf-aig-authorization": "Bearer <cloudflare-ai-gateway-token>",
        },
      },
    },
  },
}
```

The `cf-aig-authorization` header authenticates with the Cloudflare Gateway itself, while the provider API key (for example, your Anthropic key) authenticates with the upstream provider.

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure `CLOUDFLARE_AI_GATEWAY_API_KEY` is available to that process.

A key sitting only in `~/.profile` will not help a launchd/systemd daemon unless that environment is imported there as well. Set the key in `~/.openclaw/.env` or via `env.shellEnv` to ensure the gateway process can read it.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Claude Max API proxy](https://docs.openclaw.ai/providers/claude-max-api-proxy) [ComfyUI](https://docs.openclaw.ai/providers/comfy)

Ctrl+I

---

## Deepgram - OpenClaw

_Source: <https://docs.openclaw.ai/providers/deepgram>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Deepgram

Deepgram is a speech-to-text API. In OpenClaw it is used for inbound
audio/voice-note transcription through `tools.media.audio` and for Voice Call
streaming STT through `plugins.entries.voice-call.config.streaming`.For batch transcription, OpenClaw uploads the complete audio file to Deepgram
and injects the transcript into the reply pipeline (`{{Transcript}}` +
`[Audio]` block). For Voice Call streaming, OpenClaw forwards live G.711
u-law frames over Deepgram’s WebSocket `listen` endpoint and emits partial or
final transcripts as Deepgram returns them.

| Detail | Value |
| --- | --- |
| Website | [deepgram.com](https://deepgram.com/) |
| Docs | [developers.deepgram.com](https://developers.deepgram.com/) |
| Auth | `DEEPGRAM_API_KEY` |
| Default model | `nova-3` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Set your API key

Add your Deepgram API key to the environment:

```
DEEPGRAM_API_KEY=dg_...
```

2

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Send a voice note

Send an audio message through any connected channel. OpenClaw transcribes it
via Deepgram and injects the transcript into the reply pipeline.

## Configuration options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | Deepgram model id (default: `nova-3`) |
| `language` | `tools.media.audio.models[].language` | Language hint (optional) |
| `detect_language` | `tools.media.audio.providerOptions.deepgram.detect_language` | Enable language detection (optional) |
| `punctuate` | `tools.media.audio.providerOptions.deepgram.punctuate` | Enable punctuation (optional) |
| `smart_format` | `tools.media.audio.providerOptions.deepgram.smart_format` | Enable smart formatting (optional) |

- With language hint

- With Deepgram options

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3", language: "en" }],
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        providerOptions: {
          deepgram: {
            detect_language: true,
            punctuate: true,
            smart_format: true,
          },
        },
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

## Voice Call streaming STT

The bundled `deepgram` plugin also registers a realtime transcription provider
for the Voice Call plugin.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.deepgram.apiKey` | Falls back to `DEEPGRAM_API_KEY` |
| Model | `...deepgram.model` | `nova-3` |
| Language | `...deepgram.language` | (unset) |
| Encoding | `...deepgram.encoding` | `mulaw` |
| Sample rate | `...deepgram.sampleRate` | `8000` |
| Endpointing | `...deepgram.endpointingMs` | `800` |
| Interim results | `...deepgram.interimResults` | `true` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "deepgram",
            providers: {
              deepgram: {
                apiKey: "${DEEPGRAM_API_KEY}",
                model: "nova-3",
                endpointingMs: 800,
                language: "en-US",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call receives telephony audio as 8 kHz G.711 u-law. The Deepgram
streaming provider defaults to `encoding: "mulaw"` and `sampleRate: 8000`, so
Twilio media frames can be forwarded directly.

## Notes

Authentication

Authentication follows the standard provider auth order. `DEEPGRAM_API_KEY` is
the simplest path.

Proxy and custom endpoints

Override endpoints or headers with `tools.media.audio.baseUrl` and
`tools.media.audio.headers` when using a proxy.

Output behavior

Output follows the same audio rules as other providers (size caps, timeouts,
transcript injection).

## Related

[**Media tools** \\
\\
Audio, image, and video processing pipeline overview.](https://docs.openclaw.ai/tools/media-overview)

[**Configuration** \\
\\
Full config reference including media tool settings.](https://docs.openclaw.ai/gateway/configuration)

[**Troubleshooting** \\
\\
Common issues and debugging steps.](https://docs.openclaw.ai/help/troubleshooting)

[**FAQ** \\
\\
Frequently asked questions about OpenClaw setup.](https://docs.openclaw.ai/help/faq)

[ComfyUI](https://docs.openclaw.ai/providers/comfy) [Deepinfra](https://docs.openclaw.ai/providers/deepinfra)

Ctrl+I

---

## DeepSeek - OpenClaw

_Source: <https://docs.openclaw.ai/providers/deepseek>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

DeepSeek

[DeepSeek](https://www.deepseek.com/) provides powerful AI models with an OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `deepseek` |
| Auth | `DEEPSEEK_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.deepseek.com` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Get your API key

Create an API key at [platform.deepseek.com](https://platform.deepseek.com/api_keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Run onboarding

```
openclaw onboard --auth-choice deepseek-api-key
```

This will prompt for your API key and set `deepseek/deepseek-v4-flash` as the default model.

3

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Verify models are available

```
openclaw models list --provider deepseek
```

To inspect the bundled static catalog without requiring a running Gateway,
use:

```
openclaw models list --all --provider deepseek
```

Non-interactive setup

For scripted or headless installations, pass all flags directly:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice deepseek-api-key \
  --deepseek-api-key "$DEEPSEEK_API_KEY" \
  --skip-health \
  --accept-risk
```

If the Gateway runs as a daemon (launchd/systemd), make sure `DEEPSEEK_API_KEY`
is available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

## Built-in catalog

| Model ref | Name | Input | Context | Max output | Notes |
| --- | --- | --- | --- | --- | --- |
| `deepseek/deepseek-v4-flash` | DeepSeek V4 Flash | text | 1,000,000 | 384,000 | Default model; V4 thinking-capable surface |
| `deepseek/deepseek-v4-pro` | DeepSeek V4 Pro | text | 1,000,000 | 384,000 | V4 thinking-capable surface |
| `deepseek/deepseek-chat` | DeepSeek Chat | text | 131,072 | 8,192 | DeepSeek V3.2 non-thinking surface |
| `deepseek/deepseek-reasoner` | DeepSeek Reasoner | text | 131,072 | 65,536 | Reasoning-enabled V3.2 surface |

V4 models support DeepSeek’s `thinking` control. OpenClaw also replays
DeepSeek `reasoning_content` on follow-up turns so thinking sessions with tool
calls can continue.
Use `/think xhigh` or `/think max` with DeepSeek V4 models to request DeepSeek’s
maximum `reasoning_effort`.

## Thinking and tools

DeepSeek V4 thinking sessions have a stricter replay contract than most
OpenAI-compatible providers: after a thinking-enabled turn uses tools, DeepSeek
expects replayed assistant messages from that turn to include
`reasoning_content` on follow-up requests. OpenClaw handles this inside the
DeepSeek plugin, so normal multi-turn tool use works with
`deepseek/deepseek-v4-flash` and `deepseek/deepseek-v4-pro`.If you switch an existing session from another OpenAI-compatible provider to a
DeepSeek V4 model, older assistant tool-call turns may not have native
DeepSeek `reasoning_content`. OpenClaw fills that missing field on replayed
assistant messages for DeepSeek V4 thinking requests so the provider can accept
the history without requiring `/new`.When thinking is disabled in OpenClaw (including the UI **None** selection),
OpenClaw sends DeepSeek `thinking: { type: "disabled" }` and strips replayed
`reasoning_content` from the outgoing history. This keeps disabled-thinking
sessions on the non-thinking DeepSeek path.Use `deepseek/deepseek-v4-flash` for the default fast path. Use
`deepseek/deepseek-v4-pro` when you want the stronger V4 model and can accept
higher cost or latency.

## Live testing

The direct live model suite includes DeepSeek V4 in the modern model set. To
run only the DeepSeek V4 direct-model checks:

```
OPENCLAW_LIVE_PROVIDERS=deepseek \
OPENCLAW_LIVE_MODELS="deepseek/deepseek-v4-flash,deepseek/deepseek-v4-pro" \
pnpm test:live src/agents/models.profiles.live.test.ts
```

That live check verifies both V4 models can complete and that thinking/tool
follow-up turns preserve the replay payload DeepSeek requires.

## Config example

```
{
  env: { DEEPSEEK_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "deepseek/deepseek-v4-flash" },
    },
  },
}
```

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[Deepinfra](https://docs.openclaw.ai/providers/deepinfra) [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs)

Ctrl+I

---

## GitHub Copilot - OpenClaw

_Source: <https://docs.openclaw.ai/providers/github-copilot>_

# Skip confirmation
openclaw models auth login-github-copilot --yes

# Login and set the default model in one step
openclaw models auth login --provider github-copilot --method device --set-default
```

## Non-interactive onboarding

If you already have a GitHub OAuth access token for Copilot, import it during
headless setup with `openclaw onboard --non-interactive`:

```
openclaw onboard --non-interactive --accept-risk \
  --auth-choice github-copilot \
  --github-copilot-token "$COPILOT_GITHUB_TOKEN" \
  --skip-channels --skip-health
```

You can also omit `--auth-choice`; passing `--github-copilot-token` infers the
GitHub Copilot provider auth choice. If the flag is omitted, onboarding falls
back to `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, then `GITHUB_TOKEN`. Use
`--secret-input-mode ref` with `COPILOT_GITHUB_TOKEN` set to store an env-backed
`tokenRef` instead of plaintext in `auth-profiles.json`.

Interactive TTY required

The device-login flow requires an interactive TTY. Run it directly in a
terminal, not in a non-interactive script or CI pipeline.

Model availability depends on your plan

Copilot model availability depends on your GitHub plan. If a model is
rejected, try another ID (for example `github-copilot/gpt-4.1`).

Transport selection

Claude model IDs use the Anthropic Messages transport automatically. GPT,
o-series, and Gemini models keep the OpenAI Responses transport. OpenClaw
selects the correct transport based on the model ref.

Request compatibility

OpenClaw sends Copilot IDE-style request headers on Copilot transports,
including built-in compaction, tool-result, and image follow-up turns. It
does not enable provider-level Responses continuation for Copilot unless
that behavior has been verified against Copilot’s API.

Environment variable resolution order

OpenClaw resolves Copilot auth from environment variables in the following
priority order:

| Priority | Variable | Notes |
| --- | --- | --- |
| 1 | `COPILOT_GITHUB_TOKEN` | Highest priority, Copilot-specific |
| 2 | `GH_TOKEN` | GitHub CLI token (fallback) |
| 3 | `GITHUB_TOKEN` | Standard GitHub token (lowest) |

When multiple variables are set, OpenClaw uses the highest-priority one.
The device-login flow (`openclaw models auth login-github-copilot`) stores
its token in the auth profile store and takes precedence over all environment
variables.

Token storage

The login stores a GitHub token in the auth profile store and exchanges it
for a Copilot API token when OpenClaw runs. You do not need to manage the
token manually.

The device-login command requires an interactive TTY. Use non-interactive
onboarding when you need headless setup.

## Memory search embeddings

GitHub Copilot can also serve as an embedding provider for
[memory search](https://docs.openclaw.ai/concepts/memory-search). If you have a Copilot subscription and
have logged in, OpenClaw can use it for embeddings without a separate API key.

### Auto-detection

When `memorySearch.provider` is `"auto"` (the default), GitHub Copilot is tried
at priority 15 — after local embeddings but before OpenAI and other paid
providers. If a GitHub token is available, OpenClaw discovers available
embedding models from the Copilot API and picks the best one automatically.

### Explicit config

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "github-copilot",
        // Optional: override the auto-discovered model
        model: "text-embedding-3-small",
      },
    },
  },
}
```

### How it works

1. OpenClaw resolves your GitHub token (from env vars or auth profile).
2. Exchanges it for a short-lived Copilot API token.
3. Queries the Copilot `/models` endpoint to discover available embedding models.
4. Picks the best model (prefers `text-embedding-3-small`).
5. Sends embedding requests to the Copilot `/embeddings` endpoint.

Model availability depends on your GitHub plan. If no embedding models are
available, OpenClaw skips Copilot and tries the next provider.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**OAuth and auth** \\
\\
Auth details and credential reuse rules.](https://docs.openclaw.ai/gateway/authentication)

[Fireworks](https://docs.openclaw.ai/providers/fireworks) [GLM (Zhipu)](https://docs.openclaw.ai/providers/glm)

Ctrl+I

---

## GLM (Zhipu) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/glm>_

# GLM models

GLM is a **model family** (not a company) available through the Z.AI platform. In OpenClaw, GLM
models are accessed via the `zai` provider and model IDs like `zai/glm-5`.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Choose an auth route and run onboarding

Pick the onboarding choice that matches your Z.AI plan and region:

| Auth choice | Best for |
| --- | --- |
| `zai-api-key` | Generic API-key setup with endpoint auto-detection |
| `zai-coding-global` | Coding Plan users (global) |
| `zai-coding-cn` | Coding Plan users (China region) |
| `zai-global` | General API (global) |
| `zai-cn` | General API (China region) |

```
# Example: generic auto-detect
openclaw onboard --auth-choice zai-api-key

# Example: Coding Plan global
openclaw onboard --auth-choice zai-coding-global
```

2

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Set GLM as the default model

```
openclaw config set agents.defaults.model.primary "zai/glm-5.1"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Verify models are available

```
openclaw models list --provider zai
```

## Config example

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

`zai-api-key` lets OpenClaw detect the matching Z.AI endpoint from the key and
apply the correct base URL automatically. Use the explicit regional choices when
you want to force a specific Coding Plan or general API surface.

## Built-in catalog

OpenClaw currently seeds the bundled `zai` provider with these GLM refs:

| Model | Model |
| --- | --- |
| `glm-5.1` | `glm-4.7` |
| `glm-5` | `glm-4.7-flash` |
| `glm-5-turbo` | `glm-4.7-flashx` |
| `glm-5v-turbo` | `glm-4.6` |
| `glm-4.5` | `glm-4.6v` |
| `glm-4.5-air` |  |
| `glm-4.5-flash` |  |
| `glm-4.5v` |  |

The default bundled model ref is `zai/glm-5.1`. GLM versions and availability
can change; check Z.AI’s docs for the latest.

## Advanced configuration

Endpoint auto-detection

When you use the `zai-api-key` auth choice, OpenClaw inspects the key format
to determine the correct Z.AI base URL. Explicit regional choices
(`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) override
auto-detection and pin the endpoint directly.

Provider details

GLM models are served by the `zai` runtime provider. For full provider
configuration, regional endpoints, and additional capabilities, see
[Z.AI provider docs](https://docs.openclaw.ai/providers/zai).

## Related

[**Z.AI provider** \\
\\
Full Z.AI provider configuration and regional endpoints.](https://docs.openclaw.ai/providers/zai)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[GitHub Copilot](https://docs.openclaw.ai/providers/github-copilot) [Google (Gemini)](https://docs.openclaw.ai/providers/google)

Ctrl+I

---

## Google (Gemini) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/google>_

# Homebrew
brew install gemini-cli

# or npm
npm install -g @google/gemini-cli
```

OpenClaw supports both Homebrew installs and global npm installs, including
common Windows/npm layouts.

2

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Log in via OAuth

```
openclaw models auth login --provider google-gemini-cli --set-default
```

3

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Verify the model is available

```
openclaw models list --provider google
```

- Default model: `google/gemini-3.1-pro-preview`
- Runtime: `google-gemini-cli`
- Alias: `gemini-cli`

Gemini 3.1 Pro’s Gemini API model id is `gemini-3.1-pro-preview`. OpenClaw accepts the shorter `google/gemini-3.1-pro` as a convenience alias and normalizes it before provider calls.**Environment variables:**

- `OPENCLAW_GEMINI_OAUTH_CLIENT_ID`
- `OPENCLAW_GEMINI_OAUTH_CLIENT_SECRET`

(Or the `GEMINI_CLI_*` variants.)

If Gemini CLI OAuth requests fail after login, set `GOOGLE_CLOUD_PROJECT` or
`GOOGLE_CLOUD_PROJECT_ID` on the gateway host and retry.

If login fails before the browser flow starts, make sure the local `gemini`
command is installed and on `PATH`.

`google-gemini-cli/*` model refs are legacy compatibility aliases. New
configs should use `google/*` model refs plus the `google-gemini-cli`
runtime when they want local Gemini CLI execution.

## Capabilities

| Capability | Supported |
| --- | --- |
| Chat completions | Yes |
| Image generation | Yes |
| Music generation | Yes |
| Text-to-speech | Yes |
| Realtime voice | Yes (Google Live API) |
| Image understanding | Yes |
| Audio transcription | Yes |
| Video understanding | Yes |
| Web search (Grounding) | Yes |
| Thinking/reasoning | Yes (Gemini 2.5+ / Gemini 3+) |
| Gemma 4 models | Yes |

## Web search

The bundled `gemini` web-search provider uses Gemini Google Search grounding.
Configure a dedicated search key under `plugins.entries.google.config.webSearch`,
or let it reuse `models.providers.google.apiKey` after `GEMINI_API_KEY`:

```
{
  plugins: {
    entries: {
      google: {
        config: {
          webSearch: {
            apiKey: "AIza...", // optional if GEMINI_API_KEY or models.providers.google.apiKey is set
            baseUrl: "https://generativelanguage.googleapis.com/v1beta", // falls back to models.providers.google.baseUrl
            model: "gemini-2.5-flash",
          },
        },
      },
    },
  },
}
```

Credential precedence is dedicated `webSearch.apiKey`, then `GEMINI_API_KEY`,
then `models.providers.google.apiKey`. `webSearch.baseUrl` is optional and
exists for operator proxies or compatible Gemini API endpoints; when omitted,
Gemini web search reuses `models.providers.google.baseUrl`. See
[Gemini search](https://docs.openclaw.ai/tools/gemini-search) for the provider-specific tool behavior.

Gemini 3 models use `thinkingLevel` rather than `thinkingBudget`. OpenClaw maps
Gemini 3, Gemini 3.1, and `gemini-*-latest` alias reasoning controls to
`thinkingLevel` so default/low-latency runs do not send disabled
`thinkingBudget` values.`/think adaptive` keeps Google’s dynamic thinking semantics instead of choosing
a fixed OpenClaw level. Gemini 3 and Gemini 3.1 omit a fixed `thinkingLevel` so
Google can choose the level; Gemini 2.5 sends Google’s dynamic sentinel
`thinkingBudget: -1`.Gemma 4 models (for example `gemma-4-26b-a4b-it`) support thinking mode. OpenClaw
rewrites `thinkingBudget` to a supported Google `thinkingLevel` for Gemma 4.
Setting thinking to `off` preserves thinking disabled instead of mapping to
`MINIMAL`.

## Image generation

The bundled `google` image-generation provider defaults to
`google/gemini-3.1-flash-image-preview`.

- Also supports `google/gemini-3-pro-image-preview`
- Generate: up to 4 images per request
- Edit mode: enabled, up to 5 input images
- Geometry controls: `size`, `aspectRatio`, and `resolution`

To use Google as the default image provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "google/gemini-3.1-flash-image-preview",
      },
    },
  },
}
```

See [Image Generation](https://docs.openclaw.ai/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

## Video generation

The bundled `google` plugin also registers video generation through the shared
`video_generate` tool.

- Default video model: `google/veo-3.1-fast-generate-preview`
- Modes: text-to-video, image-to-video, and single-video reference flows
- Supports `aspectRatio`, `resolution`, and `audio`
- Current duration clamp: **4 to 8 seconds**

To use Google as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "google/veo-3.1-fast-generate-preview",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

## Music generation

The bundled `google` plugin also registers music generation through the shared
`music_generate` tool.

- Default music model: `google/lyria-3-clip-preview`
- Also supports `google/lyria-3-pro-preview`
- Prompt controls: `lyrics` and `instrumental`
- Output format: `mp3` by default, plus `wav` on `google/lyria-3-pro-preview`
- Reference inputs: up to 10 images
- Session-backed runs detach through the shared task/status flow, including `action: "status"`

To use Google as the default music provider:

```
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "google/lyria-3-clip-preview",
      },
    },
  },
}
```

See [Music Generation](https://docs.openclaw.ai/tools/music-generation) for shared tool parameters, provider selection, and failover behavior.

## Text-to-speech

The bundled `google` speech provider uses the Gemini API TTS path with
`gemini-3.1-flash-tts-preview`.

- Default voice: `Kore`
- Auth: `messages.tts.providers.google.apiKey`, `models.providers.google.apiKey`, `GEMINI_API_KEY`, or `GOOGLE_API_KEY`
- Output: WAV for regular TTS attachments, Opus for voice-note targets, PCM for Talk/telephony
- Voice-note output: Google PCM is wrapped as WAV and transcoded to 48 kHz Opus with `ffmpeg`

To use Google as the default TTS provider:

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "google",
      providers: {
        google: {
          model: "gemini-3.1-flash-tts-preview",
          voiceName: "Kore",
          audioProfile: "Speak professionally with a calm tone.",
        },
      },
    },
  },
}
```

Gemini API TTS uses natural-language prompting for style control. Set
`audioProfile` to prepend a reusable style prompt before the spoken text. Set
`speakerName` when your prompt text refers to a named speaker.Gemini API TTS also accepts expressive square-bracket audio tags in the text,
such as `[whispers]` or `[laughs]`. To keep tags out of the visible chat reply
while sending them to TTS, put them inside a `[[tts:text]]...[[/tts:text]]`
block:

```
Here is the clean reply text.

[[tts:text]][whispers] Here is the spoken version.[[/tts:text]]
```

A Google Cloud Console API key restricted to the Gemini API is valid for this
provider. This is not the separate Cloud Text-to-Speech API path.

## Realtime voice

The bundled `google` plugin registers a realtime voice provider backed by the
Gemini Live API for backend audio bridges such as Voice Call and Google Meet.

| Setting | Config path | Default |
| --- | --- | --- |
| Model | `plugins.entries.voice-call.config.realtime.providers.google.model` | `gemini-2.5-flash-native-audio-preview-12-2025` |
| Voice | `...google.voice` | `Kore` |
| Temperature | `...google.temperature` | (unset) |
| VAD start sensitivity | `...google.startSensitivity` | (unset) |
| VAD end sensitivity | `...google.endSensitivity` | (unset) |
| Silence duration | `...google.silenceDurationMs` | (unset) |
| Activity handling | `...google.activityHandling` | Google default, `start-of-activity-interrupts` |
| Turn coverage | `...google.turnCoverage` | Google default, `only-activity` |
| Disable auto VAD | `...google.automaticActivityDetectionDisabled` | `false` |
| API key | `...google.apiKey` | Falls back to `models.providers.google.apiKey`, `GEMINI_API_KEY`, or `GOOGLE_API_KEY` |

Example Voice Call realtime config:

```
{
  plugins: {
    entries: {
      "voice-call": {
        enabled: true,
        config: {
          realtime: {
            enabled: true,
            provider: "google",
            providers: {
              google: {
                model: "gemini-2.5-flash-native-audio-preview-12-2025",
                voice: "Kore",
                activityHandling: "start-of-activity-interrupts",
                turnCoverage: "only-activity",
              },
            },
          },
        },
      },
    },
  },
}
```

Google Live API uses bidirectional audio and function calling over a WebSocket.
OpenClaw adapts telephony/Meet bridge audio to Gemini’s PCM Live API stream and
keeps tool calls on the shared realtime voice contract. Leave `temperature`
unset unless you need sampling changes; OpenClaw omits non-positive values
because Google Live can return transcripts without audio for `temperature: 0`.
Gemini API transcription is enabled without `languageCodes`; the current Google
SDK rejects language-code hints on this API path.

Control UI Talk supports Google Live browser sessions with constrained one-use
tokens. Backend-only realtime voice providers can also run through the generic
Gateway relay transport, which keeps provider credentials on the Gateway.

For maintainer live verification, run
`OPENAI_API_KEY=... GEMINI_API_KEY=... node --import tsx scripts/dev/realtime-talk-live-smoke.ts`.
The Google leg mints the same constrained Live API token shape used by Control
UI Talk, opens the browser WebSocket endpoint, sends the initial setup payload,
and waits for `setupComplete`.

## Advanced configuration

Direct Gemini cache reuse

For direct Gemini API runs (`api: "google-generative-ai"`), OpenClaw
passes a configured `cachedContent` handle through to Gemini requests.

- Configure per-model or global params with either
`cachedContent` or legacy `cached_content`
- If both are present, `cachedContent` wins
- Example value: `cachedContents/prebuilt-context`
- Gemini cache-hit usage is normalized into OpenClaw `cacheRead` from
upstream `cachedContentTokenCount`

```
{
  agents: {
    defaults: {
      models: {
        "google/gemini-2.5-pro": {
          params: {
            cachedContent: "cachedContents/prebuilt-context",
          },
        },
      },
    },
  },
}
```

Gemini CLI JSON usage notes

When using the `google-gemini-cli` OAuth provider, OpenClaw normalizes
the CLI JSON output as follows:

- Reply text comes from the CLI JSON `response` field.
- Usage falls back to `stats` when the CLI leaves `usage` empty.
- `stats.cached` is normalized into OpenClaw `cacheRead`.
- If `stats.input` is missing, OpenClaw derives input tokens from
`stats.input_tokens - stats.cached`.

Environment and daemon setup

If the Gateway runs as a daemon (launchd/systemd), make sure `GEMINI_API_KEY`
is available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Image generation** \\
\\
Shared image tool parameters and provider selection.](https://docs.openclaw.ai/tools/image-generation)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Music generation** \\
\\
Shared music tool parameters and provider selection.](https://docs.openclaw.ai/tools/music-generation)

[GLM (Zhipu)](https://docs.openclaw.ai/providers/glm) [Gradium](https://docs.openclaw.ai/providers/gradium)

Ctrl+I

---

## Groq - OpenClaw

_Source: <https://docs.openclaw.ai/providers/groq>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Groq

[Groq](https://groq.com/) provides ultra-fast inference on open-source models
(Llama, Gemma, Mistral, and more) using custom LPU hardware. OpenClaw connects
to Groq through its OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `groq` |
| Auth | `GROQ_API_KEY` |
| API | OpenAI-compatible |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Get an API key

Create an API key at [console.groq.com/keys](https://console.groq.com/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set the API key

```
export GROQ_API_KEY="gsk_..."
```

3

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

### Config file example

```
{
  env: { GROQ_API_KEY: "gsk_..." },
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

## Built-in catalog

OpenClaw ships a manifest-backed Groq catalog for fast provider-filtered model
listing. Run `openclaw models list --all --provider groq` to see the bundled
rows, or check
[console.groq.com/docs/models](https://console.groq.com/docs/models).

| Model | Notes |
| --- | --- |
| **Llama 3.3 70B Versatile** | General-purpose, large context |
| **Llama 3.1 8B Instant** | Fast, lightweight |
| **Gemma 2 9B** | Compact, efficient |
| **Mixtral 8x7B** | MoE architecture, strong reasoning |

Use `openclaw models list --all --provider groq` for the manifest-backed Groq
rows known to this OpenClaw version.

## Reasoning models

OpenClaw maps its shared `/think` levels to Groq’s model-specific
`reasoning_effort` values. For `qwen/qwen3-32b`, disabled thinking sends
`none` and enabled thinking sends `default`. For Groq GPT-OSS reasoning models,
OpenClaw sends `low`, `medium`, or `high`; disabled thinking omits
`reasoning_effort` because those models do not support a disabled value.

## Audio transcription

Groq also provides fast Whisper-based audio transcription. When configured as a
media-understanding provider, OpenClaw uses Groq’s `whisper-large-v3-turbo`
model to transcribe voice messages through the shared `tools.media.audio`
surface.

```
{
  tools: {
    media: {
      audio: {
        models: [{ provider: "groq" }],
      },
    },
  },
}
```

Audio transcription details

| Property | Value |
| --- | --- |
| Shared config path | `tools.media.audio` |
| Default base URL | `https://api.groq.com/openai/v1` |
| Default model | `whisper-large-v3-turbo` |
| API endpoint | OpenAI-compatible `/audio/transcriptions` |

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure `GROQ_API_KEY` is
available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider and audio settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Groq Console** \\
\\
Groq dashboard, API docs, and pricing.](https://console.groq.com/)

[**Groq model list** \\
\\
Official Groq model catalog.](https://console.groq.com/docs/models)

[Gradium](https://docs.openclaw.ai/providers/gradium) [Hugging Face (inference)](https://docs.openclaw.ai/providers/huggingface)

Ctrl+I

---

## Hugging Face (inference) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/huggingface>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Hugging Face (inference)

[Hugging Face Inference Providers](https://huggingface.co/docs/inference-providers) offer OpenAI-compatible chat completions through a single router API. You get access to many models (DeepSeek, Llama, and more) with one token. OpenClaw uses the **OpenAI-compatible endpoint** (chat completions only); for text-to-image, embeddings, or speech use the [HF inference clients](https://huggingface.co/docs/api-inference/quicktour) directly.

- Provider: `huggingface`
- Auth: `HUGGINGFACE_HUB_TOKEN` or `HF_TOKEN` (fine-grained token with **Make calls to Inference Providers**)
- API: OpenAI-compatible (`https://router.huggingface.co/v1`)
- Billing: Single HF token; [pricing](https://huggingface.co/docs/inference-providers/pricing) follows provider rates with a free tier.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Create a fine-grained token

Go to [Hugging Face Settings Tokens](https://huggingface.co/settings/tokens/new?ownUserPermissions=inference.serverless.write&tokenType=fineGrained) and create a new fine-grained token.

The token must have the **Make calls to Inference Providers** permission enabled or API requests will be rejected.

2

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Run onboarding

Choose **Hugging Face** in the provider dropdown, then enter your API key when prompted:

```
openclaw onboard --auth-choice huggingface-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Select a default model

In the **Default Hugging Face model** dropdown, pick the model you want. The list is loaded from the Inference API when you have a valid token; otherwise a built-in list is shown. Your choice is saved as the default model.You can also set or change the default model later in config:

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/deepseek-ai/DeepSeek-R1" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Verify the model is available

```
openclaw models list --provider huggingface
```

### Non-interactive setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice huggingface-api-key \
  --huggingface-api-key "$HF_TOKEN"
```

This will set `huggingface/deepseek-ai/DeepSeek-R1` as the default model.

## Model IDs

Model refs use the form `huggingface/<org>/<model>` (Hub-style IDs). The list below is from **GET**`https://router.huggingface.co/v1/models`; your catalog may include more.

| Model | Ref (prefix with `huggingface/`) |
| --- | --- |
| DeepSeek R1 | `deepseek-ai/DeepSeek-R1` |
| DeepSeek V3.2 | `deepseek-ai/DeepSeek-V3.2` |
| Qwen3 8B | `Qwen/Qwen3-8B` |
| Qwen2.5 7B Instruct | `Qwen/Qwen2.5-7B-Instruct` |
| Qwen3 32B | `Qwen/Qwen3-32B` |
| Llama 3.3 70B Instruct | `meta-llama/Llama-3.3-70B-Instruct` |
| Llama 3.1 8B Instruct | `meta-llama/Llama-3.1-8B-Instruct` |
| GPT-OSS 120B | `openai/gpt-oss-120b` |
| GLM 4.7 | `zai-org/GLM-4.7` |
| Kimi K2.5 | `moonshotai/Kimi-K2.5` |

You can append `:fastest` or `:cheapest` to any model id. Set your default order in [Inference Provider settings](https://hf.co/settings/inference-providers); see [Inference Providers](https://huggingface.co/docs/inference-providers) and **GET**`https://router.huggingface.co/v1/models` for the full list.

## Advanced configuration

Model discovery and onboarding dropdown

OpenClaw discovers models by calling the **Inference endpoint directly**:

```
GET https://router.huggingface.co/v1/models
```

(Optional: send `Authorization: Bearer $HUGGINGFACE_HUB_TOKEN` or `$HF_TOKEN` for the full list; some endpoints return a subset without auth.) The response is OpenAI-style `{ "object": "list", "data": [ { "id": "Qwen/Qwen3-8B", "owned_by": "Qwen", ... }, ... ] }`.When you configure a Hugging Face API key (via onboarding, `HUGGINGFACE_HUB_TOKEN`, or `HF_TOKEN`), OpenClaw uses this GET to discover available chat-completion models. During **interactive setup**, after you enter your token you see a **Default Hugging Face model** dropdown populated from that list (or the built-in catalog if the request fails). At runtime (e.g. Gateway startup), when a key is present, OpenClaw again calls **GET**`https://router.huggingface.co/v1/models` to refresh the catalog. The list is merged with a built-in catalog (for metadata like context window and cost). If the request fails or no key is set, only the built-in catalog is used.

Model names, aliases, and policy suffixes

- **Name from API:** The model display name is **hydrated from GET /v1/models** when the API returns `name`, `title`, or `display_name`; otherwise it is derived from the model id (e.g. `deepseek-ai/DeepSeek-R1` becomes “DeepSeek R1”).
- **Override display name:** You can set a custom label per model in config so it appears the way you want in the CLI and UI:

```
{
  agents: {
    defaults: {
      models: {
        "huggingface/deepseek-ai/DeepSeek-R1": { alias: "DeepSeek R1 (fast)" },
        "huggingface/deepseek-ai/DeepSeek-R1:cheapest": { alias: "DeepSeek R1 (cheap)" },
      },
    },
  },
}
```

- **Policy suffixes:** OpenClaw’s bundled Hugging Face docs and helpers currently treat these two suffixes as the built-in policy variants:

  - **`:fastest`** — highest throughput.
  - **`:cheapest`** — lowest cost per output token.

You can add these as separate entries in `models.providers.huggingface.models` or set `model.primary` with the suffix. You can also set your default provider order in [Inference Provider settings](https://hf.co/settings/inference-providers) (no suffix = use that order).
- **Config merge:** Existing entries in `models.providers.huggingface.models` (e.g. in `models.json`) are kept when config is merged. So any custom `name`, `alias`, or model options you set there are preserved.

Environment and daemon setup

If the Gateway runs as a daemon (launchd/systemd), make sure `HUGGINGFACE_HUB_TOKEN` or `HF_TOKEN` is available to that process (for example, in `~/.openclaw/.env` or via `env.shellEnv`).

OpenClaw accepts both `HUGGINGFACE_HUB_TOKEN` and `HF_TOKEN` as env var aliases. Either one works; if both are set, `HUGGINGFACE_HUB_TOKEN` takes precedence.

Config: DeepSeek R1 with Qwen fallback

```
{
  agents: {
    defaults: {
      model: {
        primary: "huggingface/deepseek-ai/DeepSeek-R1",
        fallbacks: ["huggingface/Qwen/Qwen3-8B"],
      },
      models: {
        "huggingface/deepseek-ai/DeepSeek-R1": { alias: "DeepSeek R1" },
        "huggingface/Qwen/Qwen3-8B": { alias: "Qwen3 8B" },
      },
    },
  },
}
```

Config: Qwen with cheapest and fastest variants

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/Qwen/Qwen3-8B" },
      models: {
        "huggingface/Qwen/Qwen3-8B": { alias: "Qwen3 8B" },
        "huggingface/Qwen/Qwen3-8B:cheapest": { alias: "Qwen3 8B (cheapest)" },
        "huggingface/Qwen/Qwen3-8B:fastest": { alias: "Qwen3 8B (fastest)" },
      },
    },
  },
}
```

Config: DeepSeek + Llama + GPT-OSS with aliases

```
{
  agents: {
    defaults: {
      model: {
        primary: "huggingface/deepseek-ai/DeepSeek-V3.2",
        fallbacks: [\
          "huggingface/meta-llama/Llama-3.3-70B-Instruct",\
          "huggingface/openai/gpt-oss-120b",\
        ],
      },
      models: {
        "huggingface/deepseek-ai/DeepSeek-V3.2": { alias: "DeepSeek V3.2" },
        "huggingface/meta-llama/Llama-3.3-70B-Instruct": { alias: "Llama 3.3 70B" },
        "huggingface/openai/gpt-oss-120b": { alias: "GPT-OSS 120B" },
      },
    },
  },
}
```

Config: Multiple Qwen and DeepSeek with policy suffixes

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/Qwen/Qwen2.5-7B-Instruct:cheapest" },
      models: {
        "huggingface/Qwen/Qwen2.5-7B-Instruct": { alias: "Qwen2.5 7B" },
        "huggingface/Qwen/Qwen2.5-7B-Instruct:cheapest": { alias: "Qwen2.5 7B (cheap)" },
        "huggingface/deepseek-ai/DeepSeek-R1:fastest": { alias: "DeepSeek R1 (fast)" },
        "huggingface/meta-llama/Llama-3.1-8B-Instruct": { alias: "Llama 3.1 8B" },
      },
    },
  },
}
```

## Related

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Model selection** \\
\\
How to choose and configure models.](https://docs.openclaw.ai/concepts/models)

[**Inference Providers docs** \\
\\
Official Hugging Face Inference Providers documentation.](https://huggingface.co/docs/inference-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[Groq](https://docs.openclaw.ai/providers/groq) [Inferrs](https://docs.openclaw.ai/providers/inferrs)

Ctrl+I

---

## Kilocode - OpenClaw

_Source: <https://docs.openclaw.ai/providers/kilocode>_

# Kilo Gateway

Kilo Gateway provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `kilocode` |
| Auth | `KILOCODE_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.kilo.ai/api/gateway/` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Create an account

Go to [app.kilo.ai](https://app.kilo.ai/), sign in or create an account, then navigate to API Keys and generate a new key.

2

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Run onboarding

```
openclaw onboard --auth-choice kilocode-api-key
```

Or set the environment variable directly:

```
export KILOCODE_API_KEY="<your-kilocode-api-key>" # pragma: allowlist secret
```

3

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Verify the model is available

```
openclaw models list --provider kilocode
```

## Default model

The default model is `kilocode/kilo/auto`, a provider-owned smart-routing
model managed by Kilo Gateway.

OpenClaw treats `kilocode/kilo/auto` as the stable default ref, but does not
publish a source-backed task-to-upstream-model mapping for that route. Exact
upstream routing behind `kilocode/kilo/auto` is owned by Kilo Gateway, not
hard-coded in OpenClaw.

## Built-in catalog

OpenClaw dynamically discovers available models from the Kilo Gateway at startup. Use
`/models kilocode` to see the full list of models available with your account.Any model available on the gateway can be used with the `kilocode/` prefix:

| Model ref | Notes |
| --- | --- |
| `kilocode/kilo/auto` | Default — smart routing |
| `kilocode/anthropic/claude-sonnet-4` | Anthropic via Kilo |
| `kilocode/openai/gpt-5.5` | OpenAI via Kilo |
| `kilocode/google/gemini-3-pro-preview` | Google via Kilo |
| …and many more | Use `/models kilocode` to list all |

At startup, OpenClaw queries `GET https://api.kilo.ai/api/gateway/models` and merges
discovered models ahead of the static fallback catalog. The bundled fallback always
includes `kilocode/kilo/auto` (`Kilo Auto`) with `input: ["text", "image"]`,
`reasoning: true`, `contextWindow: 1000000`, and `maxTokens: 128000`.

## Config example

```
{
  env: { KILOCODE_API_KEY: "<your-kilocode-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "kilocode/kilo/auto" },
    },
  },
}
```

Transport and compatibility

Kilo Gateway is documented in source as OpenRouter-compatible, so it stays on
the proxy-style OpenAI-compatible path rather than native OpenAI request shaping.

- Gemini-backed Kilo refs stay on the proxy-Gemini path, so OpenClaw keeps
Gemini thought-signature sanitation there without enabling native Gemini
replay validation or bootstrap rewrites.
- Kilo Gateway uses a Bearer token with your API key under the hood.

Stream wrapper and reasoning

Kilo’s shared stream wrapper adds the provider app header and normalizes
proxy reasoning payloads for supported concrete model refs.

`kilocode/kilo/auto` and other proxy-reasoning-unsupported hints skip reasoning
injection. If you need reasoning support, use a concrete model ref such as
`kilocode/anthropic/claude-sonnet-4`.

Troubleshooting

- If model discovery fails at startup, OpenClaw falls back to the bundled static catalog containing `kilocode/kilo/auto`.
- Confirm your API key is valid and that your Kilo account has the desired models enabled.
- When the Gateway runs as a daemon, ensure `KILOCODE_API_KEY` is available to that process (for example in `~/.openclaw/.env` or via `env.shellEnv`).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Kilo Gateway** \\
\\
Kilo Gateway dashboard, API keys, and account management.](https://app.kilo.ai/)

[Inworld](https://docs.openclaw.ai/providers/inworld) [LiteLLM](https://docs.openclaw.ai/providers/litellm)

Ctrl+I

---

## LiteLLM - OpenClaw

_Source: <https://docs.openclaw.ai/providers/litellm>_

# Key info
curl "http://localhost:4000/key/info" \
  -H "Authorization: Bearer sk-litellm-key"

# Spend logs
curl "http://localhost:4000/spend/logs" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY"
```

Proxy behavior notes

- LiteLLM runs on `http://localhost:4000` by default
- OpenClaw connects through LiteLLM’s proxy-style OpenAI-compatible `/v1`
endpoint
- Native OpenAI-only request shaping does not apply through LiteLLM:
no `service_tier`, no Responses `store`, no prompt-cache hints, and no
OpenAI reasoning-compat payload shaping
- Hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on custom LiteLLM base URLs

For general provider configuration and failover behavior, see [Model Providers](https://docs.openclaw.ai/concepts/model-providers).

## Related

[**LiteLLM Docs** \\
\\
Official LiteLLM documentation and API reference.](https://docs.litellm.ai/)

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[**Model selection** \\
\\
How to choose and configure models.](https://docs.openclaw.ai/concepts/models)

[Kilocode](https://docs.openclaw.ai/providers/kilocode) [LM Studio](https://docs.openclaw.ai/providers/lmstudio)

Ctrl+I

---

## LM Studio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/lmstudio>_

# Start via desktop app, or headless:
lms server start --port 1234
```

Verify the API is accessible:

```
curl http://localhost:1234/api/v1/models
```

### Authentication errors (HTTP 401)

If setup reports HTTP 401, verify your API key:

- Check that `LM_API_TOKEN` matches the key configured in LM Studio.
- For LM Studio auth setup details, see [LM Studio Authentication](https://lmstudio.ai/docs/developer/core/authentication).
- If your server does not require authentication, leave the key blank during setup.

### Just-in-time model loading

LM Studio supports just-in-time (JIT) model loading, where models are loaded on first request. OpenClaw preloads models through LM Studio’s native load endpoint by default, which helps when JIT is disabled. To let LM Studio’s JIT, idle TTL, and auto-evict behavior own model lifecycle, disable OpenClaw’s preload step:

```
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        api: "openai-completions",
        params: { preload: false },
        models: [{ id: "qwen/qwen3.5-9b" }],
      },
    },
  },
}
```

### LAN or tailnet LM Studio host

Use the LM Studio host’s reachable address, keep `/v1`, and make sure LM Studio is bound beyond loopback on that machine:

```
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://gpu-box.local:1234/v1",
        apiKey: "lmstudio",
        api: "openai-completions",
        models: [{ id: "qwen/qwen3.5-9b" }],
      },
    },
  },
}
```

Unlike generic OpenAI-compatible providers, `lmstudio` automatically trusts its configured local/private endpoint for guarded model requests. Custom loopback provider IDs such as `localhost` or `127.0.0.1` are also trusted automatically; for LAN, tailnet, or private DNS custom provider IDs, set `models.providers.<id>.request.allowPrivateNetwork: true` explicitly.

## Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Ollama](https://docs.openclaw.ai/providers/ollama)
- [Local models](https://docs.openclaw.ai/gateway/local-models)

[LiteLLM](https://docs.openclaw.ai/providers/litellm) [MiniMax](https://docs.openclaw.ai/providers/minimax)

Ctrl+I

---

## https://docs.openclaw.ai/providers/lmstudio.md

_Source: <https://docs.openclaw.ai/providers/lmstudio.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# LM Studio

LM Studio is a friendly yet powerful app for running open-weight models on your own hardware. It lets you run llama.cpp (GGUF) or MLX models (Apple Silicon). Comes in a GUI package or headless daemon (\`llmster\`). For product and setup docs, see \[lmstudio.ai\](https://lmstudio.ai/).

\## Quick start

1\. Install LM Studio (desktop) or \`llmster\` (headless), then start the local server:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
curl -fsSL https://lmstudio.ai/install.sh \| bash
\`\`\`

2\. Start the server

Make sure you either start the desktop app or run the daemon using the following command:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
lms daemon up
\`\`\`

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
lms server start --port 1234
\`\`\`

If you are using the app, make sure you have JIT enabled for a smooth experience. Learn more in the \[LM Studio JIT and TTL guide\](https://lmstudio.ai/docs/developer/core/ttl-and-auto-evict).

3\. If LM Studio authentication is enabled, set \`LM\_API\_TOKEN\`:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
export LM\_API\_TOKEN="your-lm-studio-api-token"
\`\`\`

If LM Studio authentication is disabled, you can leave the API key blank during interactive OpenClaw setup.

For LM Studio auth setup details, see \[LM Studio Authentication\](https://lmstudio.ai/docs/developer/core/authentication).

4\. Run onboarding and choose \`LM Studio\`:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard
\`\`\`

5\. In onboarding, use the \`Default model\` prompt to pick your LM Studio model.

You can also set or change it later:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw models set lmstudio/qwen/qwen3.5-9b
\`\`\`

LM Studio model keys follow a \`author/model-name\` format (e.g. \`qwen/qwen3.5-9b\`). OpenClaw
model refs prepend the provider name: \`lmstudio/qwen/qwen3.5-9b\`. You can find the exact key for
a model by running \`curl http://localhost:1234/api/v1/models\` and looking at the \`key\` field.

\## Non-interactive onboarding

Use non-interactive onboarding when you want to script setup (CI, provisioning, remote bootstrap):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard \
 --non-interactive \
 --accept-risk \
 --auth-choice lmstudio
\`\`\`

Or specify the base URL, model, and optional API key:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard \
 --non-interactive \
 --accept-risk \
 --auth-choice lmstudio \
 --custom-base-url http://localhost:1234/v1 \
 --lmstudio-api-key "$LM\_API\_TOKEN" \
 --custom-model-id qwen/qwen3.5-9b
\`\`\`

\`--custom-model-id\` takes the model key as returned by LM Studio (e.g. \`qwen/qwen3.5-9b\`), without
the \`lmstudio/\` provider prefix.

For authenticated LM Studio servers, pass \`--lmstudio-api-key\` or set \`LM\_API\_TOKEN\`.
For unauthenticated LM Studio servers, omit the key; OpenClaw stores a local non-secret marker.

\`--custom-api-key\` remains supported for compatibility, but \`--lmstudio-api-key\` is preferred for LM Studio.

This writes \`models.providers.lmstudio\` and sets the default model to
\`lmstudio/\`. When you provide an API key, setup also writes the
\`lmstudio:default\` auth profile.

Interactive setup can prompt for an optional preferred load context length and applies it across the discovered LM Studio models it saves into config.
LM Studio plugin config trusts the configured LM Studio endpoint for model requests, including loopback, LAN, and tailnet hosts. You can opt out by setting \`models.providers.lmstudio.request.allowPrivateNetwork: false\`.

\## Configuration

\### Streaming usage compatibility

LM Studio is streaming-usage compatible. When it does not emit an OpenAI-shaped
\`usage\` object, OpenClaw recovers token counts from llama.cpp-style
\`timings.prompt\_n\` / \`timings.predicted\_n\` metadata instead.

Same streaming usage behavior applies to these OpenAI-compatible local backends:

\\* vLLM
\\* SGLang
\\* llama.cpp
\\* LocalAI
\\* Jan
\\* TabbyAPI
\\* text-generation-webui

\### Thinking compatibility

When LM Studio's \`/api/v1/models\` discovery reports model-specific reasoning
options, OpenClaw preserves those native values in model compat metadata. For
binary thinking models that advertise \`allowed\_options: \["off", "on"\]\`,
OpenClaw maps disabled thinking to \`off\` and enabled \`/think\` levels to \`on\`
instead of sending OpenAI-only values such as \`low\` or \`medium\`.

\### Explicit configuration

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 models: {
 providers: {
 lmstudio: {
 baseUrl: "http://localhost:1234/v1",
 apiKey: "${LM\_API\_TOKEN}",
 api: "openai-completions",
 models: \[\
 {\
 id: "qwen/qwen3-coder-next",\
 name: "Qwen 3 Coder Next",\
 reasoning: false,\
 input: \["text"\],\
 cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
 contextWindow: 128000,\
 maxTokens: 8192,\
 },\
 \],
 },
 },
 },
}
\`\`\`

\## Troubleshooting

\### LM Studio not detected

Make sure LM Studio is running. If authentication is enabled, also set \`LM\_API\_TOKEN\`:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
\# Start via desktop app, or headless:
lms server start --port 1234
\`\`\`

Verify the API is accessible:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
curl http://localhost:1234/api/v1/models
\`\`\`

\### Authentication errors (HTTP 401)

If setup reports HTTP 401, verify your API key:

\\* Check that \`LM\_API\_TOKEN\` matches the key configured in LM Studio.
\\* For LM Studio auth setup details, see \[LM Studio Authentication\](https://lmstudio.ai/docs/developer/core/authentication).
\\* If your server does not require authentication, leave the key blank during setup.

\### Just-in-time model loading

LM Studio supports just-in-time (JIT) model loading, where models are loaded on first request. OpenClaw preloads models through LM Studio's native load endpoint by default, which helps when JIT is disabled. To let LM Studio's JIT, idle TTL, and auto-evict behavior own model lifecycle, disable OpenClaw's preload step:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 models: {
 providers: {
 lmstudio: {
 baseUrl: "http://localhost:1234/v1",
 api: "openai-completions",
 params: { preload: false },
 models: \[{ id: "qwen/qwen3.5-9b" }\],
 },
 },
 },
}
\`\`\`

\### LAN or tailnet LM Studio host

Use the LM Studio host's reachable address, keep \`/v1\`, and make sure LM Studio is bound beyond loopback on that machine:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 models: {
 providers: {
 lmstudio: {
 baseUrl: "http://gpu-box.local:1234/v1",
 apiKey: "lmstudio",
 api: "openai-completions",
 models: \[{ id: "qwen/qwen3.5-9b" }\],
 },
 },
 },
}
\`\`\`

Unlike generic OpenAI-compatible providers, \`lmstudio\` automatically trusts its configured local/private endpoint for guarded model requests. Custom loopback provider IDs such as \`localhost\` or \`127.0.0.1\` are also trusted automatically; for LAN, tailnet, or private DNS custom provider IDs, set \`models.providers..request.allowPrivateNetwork: true\` explicitly.

\## Related

\\* \[Model selection\](/concepts/model-providers)
\\* \[Ollama\](/providers/ollama)
\\* \[Local models\](/gateway/local-models)

---

## MiniMax - OpenClaw

_Source: <https://docs.openclaw.ai/providers/minimax>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

MiniMax

OpenClaw’s MiniMax provider defaults to **MiniMax M2.7**.MiniMax also provides:

- Bundled speech synthesis via T2A v2
- Bundled image understanding via `MiniMax-VL-01`
- Bundled music generation via `music-2.6`
- Bundled `web_search` through the MiniMax Token Plan search API

Provider split:

| Provider ID | Auth | Capabilities |
| --- | --- | --- |
| `minimax` | API key | Text, image generation, music generation, video generation, image understanding, speech, web search |
| `minimax-portal` | OAuth | Text, image generation, music generation, video generation, image understanding, speech |

## Built-in catalog

| Model | Type | Description |
| --- | --- | --- |
| `MiniMax-M2.7` | Chat (reasoning) | Default hosted reasoning model |
| `MiniMax-M2.7-highspeed` | Chat (reasoning) | Faster M2.7 reasoning tier |
| `MiniMax-VL-01` | Vision | Image understanding model |
| `image-01` | Image generation | Text-to-image and image-to-image editing |
| `music-2.6` | Music generation | Default music model |
| `music-2.5` | Music generation | Previous music generation tier |
| `music-2.0` | Music generation | Legacy music generation tier |
| `MiniMax-Hailuo-2.3` | Video generation | Text-to-video and image reference flows |

## Getting started

Choose your preferred auth method and follow the setup steps.

- OAuth (Coding Plan)

- API key

**Best for:** quick setup with MiniMax Coding Plan via OAuth, no API key required.

- International

- China

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-oauth
```

This authenticates against `api.minimax.io`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-oauth
```

This authenticates against `api.minimaxi.com`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

OAuth setups use the `minimax-portal` provider id. Model refs follow the form `minimax-portal/MiniMax-M2.7`.

Referral link for MiniMax Coding Plan (10% off): [MiniMax Coding Plan](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb&source=link)

**Best for:** hosted MiniMax with Anthropic-compatible API.

- International

- China

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-api
```

This configures `api.minimax.io` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-api
```

This configures `api.minimaxi.com` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

### Config example

```
{
  env: { MINIMAX_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "minimax/MiniMax-M2.7" } } },
  models: {
    mode: "merge",
    providers: {
      minimax: {
        baseUrl: "https://api.minimax.io/anthropic",
        apiKey: "${MINIMAX_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "MiniMax-M2.7",\
            name: "MiniMax M2.7",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.3, output: 1.2, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
          {\
            id: "MiniMax-M2.7-highspeed",\
            name: "MiniMax M2.7 Highspeed",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.6, output: 2.4, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
        ],
      },
    },
  },
}
```

On the Anthropic-compatible streaming path, OpenClaw disables MiniMax thinking by default unless you explicitly set `thinking` yourself. MiniMax’s streaming endpoint emits `reasoning_content` in OpenAI-style delta chunks instead of native Anthropic thinking blocks, which can leak internal reasoning into visible output if left enabled implicitly.

API-key setups use the `minimax` provider id. Model refs follow the form `minimax/MiniMax-M2.7`.

## Configure via `openclaw configure`

Use the interactive config wizard to set MiniMax without editing JSON:

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Launch the wizard

```
openclaw configure
```

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Select Model/auth

Choose **Model/auth** from the menu.

3

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Choose a MiniMax auth option

Pick one of the available MiniMax options:

| Auth choice | Description |
| --- | --- |
| `minimax-global-oauth` | International OAuth (Coding Plan) |
| `minimax-cn-oauth` | China OAuth (Coding Plan) |
| `minimax-global-api` | International API key |
| `minimax-cn-api` | China API key |

4

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Pick your default model

Select your default model when prompted.

## Capabilities

### Image generation

The MiniMax plugin registers the `image-01` model for the `image_generate` tool. It supports:

- **Text-to-image generation** with aspect ratio control
- **Image-to-image editing** (subject reference) with aspect ratio control
- Up to **9 output images** per request
- Up to **1 reference image** per edit request
- Supported aspect ratios: `1:1`, `16:9`, `4:3`, `3:2`, `2:3`, `3:4`, `9:16`, `21:9`

To use MiniMax for image generation, set it as the image generation provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: { primary: "minimax/image-01" },
    },
  },
}
```

The plugin uses the same `MINIMAX_API_KEY` or OAuth auth as the text models. No additional configuration is needed if MiniMax is already set up.Both `minimax` and `minimax-portal` register `image_generate` with the same
`image-01` model. API-key setups use `MINIMAX_API_KEY`; OAuth setups can use
the bundled `minimax-portal` auth path instead.Image generation always uses MiniMax’s dedicated image endpoint
(`/v1/image_generation`) and ignores `models.providers.minimax.baseUrl`,
since that field configures the chat/Anthropic-compatible base URL. Set
`MINIMAX_API_HOST=https://api.minimaxi.com` to route image generation
through the CN endpoint; the default global endpoint is
`https://api.minimax.io`.When onboarding or API-key setup writes explicit `models.providers.minimax`
entries, OpenClaw materializes `MiniMax-M2.7` and
`MiniMax-M2.7-highspeed` as text-only chat models. Image understanding is
exposed separately through the plugin-owned `MiniMax-VL-01` media provider.

See [Image Generation](https://docs.openclaw.ai/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

### Text-to-speech

The bundled `minimax` plugin registers MiniMax T2A v2 as a speech provider for
`messages.tts`.

- Default TTS model: `speech-2.8-hd`
- Default voice: `English_expressive_narrator`
- Supported bundled model ids include `speech-2.8-hd`, `speech-2.8-turbo`,
`speech-2.6-hd`, `speech-2.6-turbo`, `speech-02-hd`,
`speech-02-turbo`, `speech-01-hd`, and `speech-01-turbo`.
- Auth resolution is `messages.tts.providers.minimax.apiKey`, then
`minimax-portal` OAuth/token auth profiles, then Token Plan environment
keys (`MINIMAX_OAUTH_TOKEN`, `MINIMAX_CODE_PLAN_KEY`,
`MINIMAX_CODING_API_KEY`), then `MINIMAX_API_KEY`.
- If no TTS host is configured, OpenClaw reuses the configured
`minimax-portal` OAuth host and strips Anthropic-compatible path suffixes
such as `/anthropic`.
- Normal audio attachments stay MP3.
- Voice-note targets such as Feishu and Telegram are transcoded from MiniMax
MP3 to 48kHz Opus with `ffmpeg`, because the Feishu/Lark file API only
accepts `file_type: "opus"` for native audio messages.
- MiniMax T2A accepts fractional `speed` and `vol`, but `pitch` is sent as an
integer; OpenClaw truncates fractional `pitch` values before the API request.

| Setting | Env var | Default | Description |
| --- | --- | --- | --- |
| `messages.tts.providers.minimax.baseUrl` | `MINIMAX_API_HOST` | `https://api.minimax.io` | MiniMax T2A API host. |
| `messages.tts.providers.minimax.model` | `MINIMAX_TTS_MODEL` | `speech-2.8-hd` | TTS model id. |
| `messages.tts.providers.minimax.voiceId` | `MINIMAX_TTS_VOICE_ID` | `English_expressive_narrator` | Voice id used for speech output. |
| `messages.tts.providers.minimax.speed` |  | `1.0` | Playback speed, `0.5..2.0`. |
| `messages.tts.providers.minimax.vol` |  | `1.0` | Volume, `(0, 10]`. |
| `messages.tts.providers.minimax.pitch` |  | `0` | Integer pitch shift, `-12..12`. |

### Music generation

The bundled MiniMax plugin registers music generation through the shared
`music_generate` tool for both `minimax` and `minimax-portal`.

- Default music model: `minimax/music-2.6`
- OAuth music model: `minimax-portal/music-2.6`
- Also supports `minimax/music-2.5` and `minimax/music-2.0`
- Prompt controls: `lyrics`, `instrumental`, `durationSeconds`
- Output format: `mp3`
- Session-backed runs detach through the shared task/status flow, including `action: "status"`

To use MiniMax as the default music provider:

```
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "minimax/music-2.6",
      },
    },
  },
}
```

See [Music Generation](https://docs.openclaw.ai/tools/music-generation) for shared tool parameters, provider selection, and failover behavior.

### Video generation

The bundled MiniMax plugin registers video generation through the shared
`video_generate` tool for both `minimax` and `minimax-portal`.

- Default video model: `minimax/MiniMax-Hailuo-2.3`
- OAuth video model: `minimax-portal/MiniMax-Hailuo-2.3`
- Modes: text-to-video and single-image reference flows
- Supports `aspectRatio` and `resolution`

To use MiniMax as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "minimax/MiniMax-Hailuo-2.3",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

### Image understanding

The MiniMax plugin registers image understanding separately from the text
catalog:

| Provider ID | Default image model |
| --- | --- |
| `minimax` | `MiniMax-VL-01` |
| `minimax-portal` | `MiniMax-VL-01` |

That is why automatic media routing can use MiniMax image understanding even
when the bundled text-provider catalog still shows text-only M2.7 chat refs.

### Web search

The MiniMax plugin also registers `web_search` through the MiniMax Token Plan
search API.

- Provider id: `minimax`
- Structured results: titles, URLs, snippets, related queries
- Preferred env var: `MINIMAX_CODE_PLAN_KEY`
- Accepted env aliases: `MINIMAX_CODING_API_KEY`, `MINIMAX_OAUTH_TOKEN`
- Compatibility fallback: `MINIMAX_API_KEY` when it already points at a token-plan credential
- Region reuse: `plugins.entries.minimax.config.webSearch.region`, then `MINIMAX_API_HOST`, then MiniMax provider base URLs
- Search stays on provider id `minimax`; OAuth CN/global setup can steer region indirectly through `models.providers.minimax-portal.baseUrl` and can provide bearer auth through `MINIMAX_OAUTH_TOKEN`

Config lives under `plugins.entries.minimax.config.webSearch.*`.

See [MiniMax Search](https://docs.openclaw.ai/tools/minimax-search) for full web search configuration and usage.

## Advanced configuration

Configuration options

| Option | Description |
| --- | --- |
| `models.providers.minimax.baseUrl` | Prefer `https://api.minimax.io/anthropic` (Anthropic-compatible); `https://api.minimax.io/v1` is optional for OpenAI-compatible payloads |
| `models.providers.minimax.api` | Prefer `anthropic-messages`; `openai-completions` is optional for OpenAI-compatible payloads |
| `models.providers.minimax.apiKey` | MiniMax API key (`MINIMAX_API_KEY`) |
| `models.providers.minimax.models` | Define `id`, `name`, `reasoning`, `contextWindow`, `maxTokens`, `cost` |
| `agents.defaults.models` | Alias models you want in the allowlist |
| `models.mode` | Keep `merge` if you want to add MiniMax alongside built-ins |

Thinking defaults

On `api: "anthropic-messages"`, OpenClaw injects `thinking: { type: "disabled" }` unless thinking is already explicitly set in params/config.This prevents MiniMax’s streaming endpoint from emitting `reasoning_content` in OpenAI-style delta chunks, which would leak internal reasoning into visible output.

Fast mode

`/fast on` or `params.fastMode: true` rewrites `MiniMax-M2.7` to `MiniMax-M2.7-highspeed` on the Anthropic-compatible stream path.

Fallback example

**Best for:** keep your strongest latest-generation model as primary, fail over to MiniMax M2.7. Example below uses Opus as a concrete primary; swap to your preferred latest-gen primary model.

```
{
  env: { MINIMAX_API_KEY: "sk-..." },
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": { alias: "primary" },
        "minimax/MiniMax-M2.7": { alias: "minimax" },
      },
      model: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["minimax/MiniMax-M2.7"],
      },
    },
  },
}
```

Coding Plan usage details

- Coding Plan usage API: `https://api.minimaxi.com/v1/token_plan/remains` or `https://api.minimax.io/v1/token_plan/remains` (requires a coding plan key).
- Usage polling derives the host from `models.providers.minimax-portal.baseUrl` or `models.providers.minimax.baseUrl` when configured, so global setups using `https://api.minimax.io/anthropic` poll `api.minimax.io`. Missing or malformed base URLs keep the CN fallback for compatibility.
- OpenClaw normalizes MiniMax coding-plan usage to the same `% left` display used by other providers. MiniMax’s raw `usage_percent` / `usagePercent` fields are remaining quota, not consumed quota, so OpenClaw inverts them. Count-based fields win when present.
- When the API returns `model_remains`, OpenClaw prefers the chat-model entry, derives the window label from `start_time` / `end_time` when needed, and includes the selected model name in the plan label so coding-plan windows are easier to distinguish.
- Usage snapshots treat `minimax`, `minimax-cn`, and `minimax-portal` as the same MiniMax quota surface, and prefer stored MiniMax OAuth before falling back to Coding Plan key env vars.

## Notes

- Model refs follow the auth path:
  - API-key setup: `minimax/<model>`
  - OAuth setup: `minimax-portal/<model>`
- Default chat model: `MiniMax-M2.7`
- Alternate chat model: `MiniMax-M2.7-highspeed`
- Onboarding and direct API-key setup write text-only model definitions for both M2.7 variants
- Image understanding uses the plugin-owned `MiniMax-VL-01` media provider
- Update pricing values in `models.json` if you need exact cost tracking
- Use `openclaw models list` to confirm the current provider id, then switch with `openclaw models set minimax/MiniMax-M2.7` or `openclaw models set minimax-portal/MiniMax-M2.7`

Referral link for MiniMax Coding Plan (10% off): [MiniMax Coding Plan](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb&source=link)

See [Model providers](https://docs.openclaw.ai/concepts/model-providers) for provider rules.

## Troubleshooting

"Unknown model: minimax/MiniMax-M2.7"

This usually means the **MiniMax provider is not configured** (no matching provider entry and no MiniMax auth profile/env key found). A fix for this detection is in **2026.1.12**. Fix by:

- Upgrading to **2026.1.12** (or run from source `main`), then restarting the gateway.
- Running `openclaw configure` and selecting a **MiniMax** auth option, or
- Adding the matching `models.providers.minimax` or `models.providers.minimax-portal` block manually, or
- Setting `MINIMAX_API_KEY`, `MINIMAX_OAUTH_TOKEN`, or a MiniMax auth profile so the matching provider can be injected.

Make sure the model id is **case-sensitive**:

- API-key path: `minimax/MiniMax-M2.7` or `minimax/MiniMax-M2.7-highspeed`
- OAuth path: `minimax-portal/MiniMax-M2.7` or `minimax-portal/MiniMax-M2.7-highspeed`

Then recheck with:

```
openclaw models list
```

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Image generation** \\
\\
Shared image tool parameters and provider selection.](https://docs.openclaw.ai/tools/image-generation)

[**Music generation** \\
\\
Shared music tool parameters and provider selection.](https://docs.openclaw.ai/tools/music-generation)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**MiniMax Search** \\
\\
Web search configuration via MiniMax Token Plan.](https://docs.openclaw.ai/tools/minimax-search)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[LM Studio](https://docs.openclaw.ai/providers/lmstudio) [Mistral](https://docs.openclaw.ai/providers/mistral)

Ctrl+I

---

## Config example

_Source: <https://docs.openclaw.ai/providers/minimax.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# MiniMax

OpenClaw's MiniMax provider defaults to \*\*MiniMax M2.7\*\*.

MiniMax also provides:

\\* Bundled speech synthesis via T2A v2
\\* Bundled image understanding via \`MiniMax-VL-01\`
\\* Bundled music generation via \`music-2.6\`
\\* Bundled \`web\_search\` through the MiniMax Token Plan search API

Provider split:

\| Provider ID \| Auth \| Capabilities \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`minimax\` \| API key \| Text, image generation, music generation, video generation, image understanding, speech, web search \|
\| \`minimax-portal\` \| OAuth \| Text, image generation, music generation, video generation, image understanding, speech \|

\## Built-in catalog

\| Model \| Type \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`MiniMax-M2.7\` \| Chat (reasoning) \| Default hosted reasoning model \|
\| \`MiniMax-M2.7-highspeed\` \| Chat (reasoning) \| Faster M2.7 reasoning tier \|
\| \`MiniMax-VL-01\` \| Vision \| Image understanding model \|
\| \`image-01\` \| Image generation \| Text-to-image and image-to-image editing \|
\| \`music-2.6\` \| Music generation \| Default music model \|
\| \`music-2.5\` \| Music generation \| Previous music generation tier \|
\| \`music-2.0\` \| Music generation \| Legacy music generation tier \|
\| \`MiniMax-Hailuo-2.3\` \| Video generation \| Text-to-video and image reference flows \|

\## Getting started

Choose your preferred auth method and follow the setup steps.

 \*\*Best for:\*\* quick setup with MiniMax Coding Plan via OAuth, no API key required.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-global-oauth
 \`\`\`

 This authenticates against \`api.minimax.io\`.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax-portal
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-cn-oauth
 \`\`\`

 This authenticates against \`api.minimaxi.com\`.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax-portal
 \`\`\`

 OAuth setups use the \`minimax-portal\` provider id. Model refs follow the form \`minimax-portal/MiniMax-M2.7\`.

 Referral link for MiniMax Coding Plan (10% off): \[MiniMax Coding Plan\](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb\\&source=link)

 \*\*Best for:\*\* hosted MiniMax with Anthropic-compatible API.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-global-api
 \`\`\`

 This configures \`api.minimax.io\` as the base URL.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-cn-api
 \`\`\`

 This configures \`api.minimaxi.com\` as the base URL.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax
 \`\`\`

 ### Config example

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: { MINIMAX\_API\_KEY: "sk-..." },
 agents: { defaults: { model: { primary: "minimax/MiniMax-M2.7" } } },
 models: {
 mode: "merge",
 providers: {
 minimax: {
 baseUrl: "https://api.minimax.io/anthropic",
 apiKey: "${MINIMAX\_API\_KEY}",
 api: "anthropic-messages",
 models: \[\
 {\
 id: "MiniMax-M2.7",\
 name: "MiniMax M2.7",\
 reasoning: true,\
 input: \["text"\],\
 cost: { input: 0.3, output: 1.2, cacheRead: 0.06, cacheWrite: 0.375 },\
 contextWindow: 204800,\
 maxTokens: 131072,\
 },\
 {\
 id: "MiniMax-M2.7-highspeed",\
 name: "MiniMax M2.7 Highspeed",\
 reasoning: true,\
 input: \["text"\],\
 cost: { input: 0.6, output: 2.4, cacheRead: 0.06, cacheWrite: 0.375 },\
 contextWindow: 204800,\
 maxTokens: 131072,\
 },\
 \],
 },
 },
 },
 }
 \`\`\`

 On the Anthropic-compatible streaming path, OpenClaw disables MiniMax thinking by default unless you explicitly set \`thinking\` yourself. MiniMax's streaming endpoint emits \`reasoning\_content\` in OpenAI-style delta chunks instead of native Anthropic thinking blocks, which can leak internal reasoning into visible output if left enabled implicitly.

 API-key setups use the \`minimax\` provider id. Model refs follow the form \`minimax/MiniMax-M2.7\`.

\## Configure via \`openclaw configure\`

Use the interactive config wizard to set MiniMax without editing JSON:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw configure
 \`\`\`

 Choose \*\*Model/auth\*\* from the menu.

 Pick one of the available MiniMax options:

 \| Auth choice \| Description \|
 \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
 \| \`minimax-global-oauth\` \| International OAuth (Coding Plan) \|
 \| \`minimax-cn-oauth\` \| China OAuth (Coding Plan) \|
 \| \`minimax-global-api\` \| International API key \|
 \| \`minimax-cn-api\` \| China API key \|

 Select your default model when prompted.

\## Capabilities

\### Image generation

The MiniMax plugin registers the \`image-01\` model for the \`image\_generate\` tool. It supports:

\\* \*\*Text-to-image generation\*\* with aspect ratio control
\\* \*\*Image-to-image editing\*\* (subject reference) with aspect ratio control
\\* Up to \*\*9 output images\*\* per request
\\* Up to \*\*1 reference image\*\* per edit request
\\* Supported aspect ratios: \`1:1\`, \`16:9\`, \`4:3\`, \`3:2\`, \`2:3\`, \`3:4\`, \`9:16\`, \`21:9\`

To use MiniMax for image generation, set it as the image generation provider:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 imageGenerationModel: { primary: "minimax/image-01" },
 },
 },
}
\`\`\`

The plugin uses the same \`MINIMAX\_API\_KEY\` or OAuth auth as the text models. No additional configuration is needed if MiniMax is already set up.

Both \`minimax\` and \`minimax-portal\` register \`image\_generate\` with the same
\`image-01\` model. API-key setups use \`MINIMAX\_API\_KEY\`; OAuth setups can use
the bundled \`minimax-portal\` auth path instead.

Image generation always uses MiniMax's dedicated image endpoint
(\`/v1/image\_generation\`) and ignores \`models.providers.minimax.baseUrl\`,
since that field configures the chat/Anthropic-compatible base URL. Set
\`MINIMAX\_API\_HOST=https://api.minimaxi.com\` to route image generation
through the CN endpoint; the default global endpoint is
\`https://api.minimax.io\`.

When onboarding or API-key setup writes explicit \`models.providers.minimax\`
entries, OpenClaw materializes \`MiniMax-M2.7\` and
\`MiniMax-M2.7-highspeed\` as text-only chat models. Image understanding is
exposed separately through the plugin-owned \`MiniMax-VL-01\` media provider.

 See \[Image Generation\](/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

\### Text-to-speech

The bundled \`minimax\` plugin registers MiniMax T2A v2 as a speech provider for
\`messages.tts\`.

\\* Default TTS model: \`speech-2.8-hd\`
\\* Default voice: \`English\_expressive\_narrator\`
\\* Supported bundled model ids include \`speech-2.8-hd\`, \`speech-2.8-turbo\`,
 \`speech-2.6-hd\`, \`speech-2.6-turbo\`, \`speech-02-hd\`,
 \`speech-02-turbo\`, \`speech-01-hd\`, and \`speech-01-turbo\`.
\\* Auth resolution is \`messages.tts.providers.minimax.apiKey\`, then
 \`minimax-portal\` OAuth/token auth profiles, then Token Plan environment
 keys (\`MINIMAX\_OAUTH\_TOKEN\`, \`MINIMAX\_CODE\_PLAN\_KEY\`,
 \`MINIMAX\_CODING\_API\_KEY\`), then \`MINIMAX\_API\_KEY\`.
\\* If no TTS host is configured, OpenClaw reuses the configured
 \`minimax-portal\` OAuth host and strips Anthropic-compatible path suffixes
 such as \`/anthropic\`.
\\* Normal audio attachments stay MP3.
\\* Voice-note targets such as Feishu and Telegram are transcoded from MiniMax
 MP3 to 48kHz Opus with \`ffmpeg\`, because the Feishu/Lark file API only
 accepts \`file\_type: "opus"\` for native audio messages.
\\* MiniMax T2A accepts fractional \`speed\` and \`vol\`, but \`pitch\` is sent as an
 integer; OpenClaw truncates fractional \`pitch\` values before the API request.

\| Setting \| Env var \| Default \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`messages.tts.providers.minimax.baseUrl\` \| \`MINIMAX\_API\_HOST\` \| \`https://api.minimax.io\` \| MiniMax T2A API host. \|
\| \`messages.tts.providers.minimax.model\` \| \`MINIMAX\_TTS\_MODEL\` \| \`speech-2.8-hd\` \| TTS model id. \|
\| \`messages.tts.providers.minimax.voiceId\` \| \`MINIMAX\_TTS\_VOICE\_ID\` \| \`English\_expressive\_narrator\` \| Voice id used for speech output. \|
\| \`messages.tts.providers.minimax.speed\` \| \| \`1.0\` \| Playback speed, \`0.5..2.0\`. \|
\| \`messages.tts.providers.minimax.vol\` \| \| \`1.0\` \| Volume, \`(0, 10\]\`. \|
\| \`messages.tts.providers.minimax.pitch\` \| \| \`0\` \| Integer pitch shift, \`-12..12\`. \|

\### Music generation

The bundled MiniMax plugin registers music generation through the shared
\`music\_generate\` tool for both \`minimax\` and \`minimax-portal\`.

\\* Default music model: \`minimax/music-2.6\`
\\* OAuth music model: \`minimax-portal/music-2.6\`
\\* Also supports \`minimax/music-2.5\` and \`minimax/music-2.0\`
\\* Prompt controls: \`lyrics\`, \`instrumental\`, \`durationSeconds\`
\\* Output format: \`mp3\`
\\* Session-backed runs detach through the shared task/status flow, including \`action: "status"\`

To use MiniMax as the default music provider:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 musicGenerationModel: {
 primary: "minimax/music-2.6",
 },
 },
 },
}
\`\`\`

 See \[Music Generation\](/tools/music-generation) for shared tool parameters, provider selection, and failover behavior.

\### Video generation

The bundled MiniMax plugin registers video generation through the shared
\`video\_generate\` tool for both \`minimax\` and \`minimax-portal\`.

\\* Default video model: \`minimax/MiniMax-Hailuo-2.3\`
\\* OAuth video model: \`minimax-portal/MiniMax-Hailuo-2.3\`
\\* Modes: text-to-video and single-image reference flows
\\* Supports \`aspectRatio\` and \`resolution\`

To use MiniMax as the default video provider:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 videoGenerationModel: {
 primary: "minimax/MiniMax-Hailuo-2.3",
 },
 },
 },
}
\`\`\`

 See \[Video Generation\](/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

\### Image understanding

The MiniMax plugin registers image understanding separately from the text
catalog:

\| Provider ID \| Default image model \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`minimax\` \| \`MiniMax-VL-01\` \|
\| \`minimax-portal\` \| \`MiniMax-VL-01\` \|

That is why automatic media routing can use MiniMax image understanding even
when the bundled text-provider catalog still shows text-only M2.7 chat refs.

\### Web search

The MiniMax plugin also registers \`web\_search\` through the MiniMax Token Plan
search API.

\\* Provider id: \`minimax\`
\\* Structured results: titles, URLs, snippets, related queries
\\* Preferred env var: \`MINIMAX\_CODE\_PLAN\_KEY\`
\\* Accepted env aliases: \`MINIMAX\_CODING\_API\_KEY\`, \`MINIMAX\_OAUTH\_TOKEN\`
\\* Compatibility fallback: \`MINIMAX\_API\_KEY\` when it already points at a token-plan credential
\\* Region reuse: \`plugins.entries.minimax.config.webSearch.region\`, then \`MINIMAX\_API\_HOST\`, then MiniMax provider base URLs
\\* Search stays on provider id \`minimax\`; OAuth CN/global setup can steer region indirectly through \`models.providers.minimax-portal.baseUrl\` and can provide bearer auth through \`MINIMAX\_OAUTH\_TOKEN\`

Config lives under \`plugins.entries.minimax.config.webSearch.\*\`.

 See \[MiniMax Search\](/tools/minimax-search) for full web search configuration and usage.

\## Advanced configuration

 \| Option \| Description \|
 \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
 \| \`models.providers.minimax.baseUrl\` \| Prefer \`https://api.minimax.io/anthropic\` (Anthropic-compatible); \`https://api.minimax.io/v1\` is optional for OpenAI-compatible payloads \|
 \| \`models.providers.minimax.api\` \| Prefer \`anthropic-messages\`; \`openai-completions\` is optional for OpenAI-compatible payloads \|
 \| \`models.providers.minimax.apiKey\` \| MiniMax API key (\`MINIMAX\_API\_KEY\`) \|
 \| \`models.providers.minimax.models\` \| Define \`id\`, \`name\`, \`reasoning\`, \`contextWindow\`, \`maxTokens\`, \`cost\` \|
 \| \`agents.defaults.models\` \| Alias models you want in the allowlist \|
 \| \`models.mode\` \| Keep \`merge\` if you want to add MiniMax alongside built-ins \|

 On \`api: "anthropic-messages"\`, OpenClaw injects \`thinking: { type: "disabled" }\` unless thinking is already explicitly set in params/config.

 This prevents MiniMax's streaming endpoint from emitting \`reasoning\_content\` in OpenAI-style delta chunks, which would leak internal reasoning into visible output.

 \`/fast on\` or \`params.fastMode: true\` rewrites \`MiniMax-M2.7\` to \`MiniMax-M2.7-highspeed\` on the Anthropic-compatible stream path.

 \*\*Best for:\*\* keep your strongest latest-generation model as primary, fail over to MiniMax M2.7. Example below uses Opus as a concrete primary; swap to your preferred latest-gen primary model.

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: { MINIMAX\_API\_KEY: "sk-..." },
 agents: {
 defaults: {
 models: {
 "anthropic/claude-opus-4-6": { alias: "primary" },
 "minimax/MiniMax-M2.7": { alias: "minimax" },
 },
 model: {
 primary: "anthropic/claude-opus-4-6",
 fallbacks: \["minimax/MiniMax-M2.7"\],
 },
 },
 },
 }
 \`\`\`

 \\* Coding Plan usage API: \`https://api.minimaxi.com/v1/token\_plan/remains\` or \`https://api.minimax.io/v1/token\_plan/remains\` (requires a coding plan key).
 \\* Usage polling derives the host from \`models.providers.minimax-portal.baseUrl\` or \`models.providers.minimax.baseUrl\` when configured, so global setups using \`https://api.minimax.io/anthropic\` poll \`api.minimax.io\`. Missing or malformed base URLs keep the CN fallback for compatibility.
 \\* OpenClaw normalizes MiniMax coding-plan usage to the same \`% left\` display used by other providers. MiniMax's raw \`usage\_percent\` / \`usagePercent\` fields are remaining quota, not consumed quota, so OpenClaw inverts them. Count-based fields win when present.
 \\* When the API returns \`model\_remains\`, OpenClaw prefers the chat-model entry, derives the window label from \`start\_time\` / \`end\_time\` when needed, and includes the selected model name in the plan label so coding-plan windows are easier to distinguish.
 \\* Usage snapshots treat \`minimax\`, \`minimax-cn\`, and \`minimax-portal\` as the same MiniMax quota surface, and prefer stored MiniMax OAuth before falling back to Coding Plan key env vars.

\## Notes

\\* Model refs follow the auth path:
 \\* API-key setup: \`minimax/\`
 \\* OAuth setup: \`minimax-portal/\`
\\* Default chat model: \`MiniMax-M2.7\`
\\* Alternate chat model: \`MiniMax-M2.7-highspeed\`
\\* Onboarding and direct API-key setup write text-only model definitions for both M2.7 variants
\\* Image understanding uses the plugin-owned \`MiniMax-VL-01\` media provider
\\* Update pricing values in \`models.json\` if you need exact cost tracking
\\* Use \`openclaw models list\` to confirm the current provider id, then switch with \`openclaw models set minimax/MiniMax-M2.7\` or \`openclaw models set minimax-portal/MiniMax-M2.7\`

 Referral link for MiniMax Coding Plan (10% off): \[MiniMax Coding Plan\](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb\\&source=link)

 See \[Model providers\](/concepts/model-providers) for provider rules.

\## Troubleshooting

 This usually means the \*\*MiniMax provider is not configured\*\* (no matching provider entry and no MiniMax auth profile/env key found). A fix for this detection is in \*\*2026.1.12\*\*. Fix by:

 \\* Upgrading to \*\*2026.1.12\*\* (or run from source \`main\`), then restarting the gateway.
 \\* Running \`openclaw configure\` and selecting a \*\*MiniMax\*\* auth option, or
 \\* Adding the matching \`models.providers.minimax\` or \`models.providers.minimax-portal\` block manually, or
 \\* Setting \`MINIMAX\_API\_KEY\`, \`MINIMAX\_OAUTH\_TOKEN\`, or a MiniMax auth profile so the matching provider can be injected.

 Make sure the model id is \*\*case-sensitive\*\*:

 \\* API-key path: \`minimax/MiniMax-M2.7\` or \`minimax/MiniMax-M2.7-highspeed\`
 \\* OAuth path: \`minimax-portal/MiniMax-M2.7\` or \`minimax-portal/MiniMax-M2.7-highspeed\`

 Then recheck with:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list
 \`\`\`

 More help: \[Troubleshooting\](/help/troubleshooting) and \[FAQ\](/help/faq).

\## Related

 Choosing providers, model refs, and failover behavior.

 Shared image tool parameters and provider selection.

 Shared music tool parameters and provider selection.

 Shared video tool parameters and provider selection.

 Web search configuration via MiniMax Token Plan.

 General troubleshooting and FAQ.

---

## Mistral - OpenClaw

_Source: <https://docs.openclaw.ai/providers/mistral>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Mistral

OpenClaw supports Mistral for both text/image model routing (`mistral/...`) and
audio transcription via Voxtral in media understanding.
Mistral can also be used for memory embeddings (`memorySearch.provider = "mistral"`).

- Provider: `mistral`
- Auth: `MISTRAL_API_KEY`
- API: Mistral Chat Completions (`https://api.mistral.ai/v1`)

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Get your API key

Create an API key in the [Mistral Console](https://console.mistral.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Run onboarding

```
openclaw onboard --auth-choice mistral-api-key
```

Or pass the key directly:

```
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Set a default model

```
{
  env: { MISTRAL_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "mistral/mistral-large-latest" } } },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Verify the model is available

```
openclaw models list --provider mistral
```

## Built-in LLM catalog

OpenClaw currently ships this bundled Mistral catalog:

| Model ref | Input | Context | Max output | Notes |
| --- | --- | --- | --- | --- |
| `mistral/mistral-large-latest` | text, image | 262,144 | 16,384 | Default model |
| `mistral/mistral-medium-2508` | text, image | 262,144 | 8,192 | Mistral Medium 3.1 |
| `mistral/mistral-small-latest` | text, image | 128,000 | 16,384 | Mistral Small 4; adjustable reasoning via API `reasoning_effort` |
| `mistral/pixtral-large-latest` | text, image | 128,000 | 32,768 | Pixtral |
| `mistral/codestral-latest` | text | 256,000 | 4,096 | Coding |
| `mistral/devstral-medium-latest` | text | 262,144 | 32,768 | Devstral 2 |
| `mistral/magistral-small` | text | 128,000 | 40,000 | Reasoning-enabled |

## Audio transcription (Voxtral)

Use Voxtral for batch audio transcription through the media understanding
pipeline.

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

The media transcription path uses `/v1/audio/transcriptions`. The default audio model for Mistral is `voxtral-mini-latest`.

## Voice Call streaming STT

The bundled `mistral` plugin registers Voxtral Realtime as a Voice Call
streaming STT provider.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.mistral.apiKey` | Falls back to `MISTRAL_API_KEY` |
| Model | `...mistral.model` | `voxtral-mini-transcribe-realtime-2602` |
| Encoding | `...mistral.encoding` | `pcm_mulaw` |
| Sample rate | `...mistral.sampleRate` | `8000` |
| Target delay | `...mistral.targetStreamingDelayMs` | `800` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "mistral",
            providers: {
              mistral: {
                apiKey: "${MISTRAL_API_KEY}",
                targetStreamingDelayMs: 800,
              },
            },
          },
        },
      },
    },
  },
}
```

OpenClaw defaults Mistral realtime STT to `pcm_mulaw` at 8 kHz so Voice Call
can forward Twilio media frames directly. Use `encoding: "pcm_s16le"` and a
matching `sampleRate` only if your upstream stream is already raw PCM.

## Advanced configuration

Adjustable reasoning (mistral-small-latest)

`mistral/mistral-small-latest` maps to Mistral Small 4 and supports [adjustable reasoning](https://docs.mistral.ai/capabilities/reasoning/adjustable) on the Chat Completions API via `reasoning_effort` (`none` minimizes extra thinking in the output; `high` surfaces full thinking traces before the final answer).OpenClaw maps the session **thinking** level to Mistral’s API:

| OpenClaw thinking level | Mistral `reasoning_effort` |
| --- | --- |
| **off** / **minimal** | `none` |
| **low** / **medium** / **high** / **xhigh** / **adaptive** / **max** | `high` |

Other bundled Mistral catalog models do not use this parameter. Keep using `magistral-*` models when you want Mistral’s native reasoning-first behavior.

Memory embeddings

Mistral can serve memory embeddings via `/v1/embeddings` (default model: `mistral-embed`).

```
{
  memorySearch: { provider: "mistral" },
}
```

Auth and base URL

- Mistral auth uses `MISTRAL_API_KEY`.
- Provider base URL defaults to `https://api.mistral.ai/v1`.
- Onboarding default model is `mistral/mistral-large-latest`.
- Z.AI uses Bearer auth with your API key.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Media understanding** \\
\\
Audio transcription setup and provider selection.](https://docs.openclaw.ai/nodes/media-understanding)

[MiniMax](https://docs.openclaw.ai/providers/minimax) [Moonshot AI](https://docs.openclaw.ai/providers/moonshot)

Ctrl+I

---

## Model provider quickstart - OpenClaw

_Source: <https://docs.openclaw.ai/providers/models>_

# Model Providers

OpenClaw can use many LLM providers. Pick one, authenticate, then set the default
model as `provider/model`.

## Quick start (two steps)

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## Supported providers (starter set)

- [Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)
- [Anthropic (API + Claude CLI)](https://docs.openclaw.ai/providers/anthropic)
- [BytePlus (International)](https://docs.openclaw.ai/concepts/model-providers#byteplus-international)
- [Chutes](https://docs.openclaw.ai/providers/chutes)
- [ComfyUI](https://docs.openclaw.ai/providers/comfy)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)
- [DeepInfra](https://docs.openclaw.ai/providers/deepinfra)
- [fal](https://docs.openclaw.ai/providers/fal)
- [Fireworks](https://docs.openclaw.ai/providers/fireworks)
- [GLM models](https://docs.openclaw.ai/providers/glm)
- [MiniMax](https://docs.openclaw.ai/providers/minimax)
- [Mistral](https://docs.openclaw.ai/providers/mistral)
- [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot)
- [OpenAI (API + Codex)](https://docs.openclaw.ai/providers/openai)
- [OpenCode (Zen + Go)](https://docs.openclaw.ai/providers/opencode)
- [OpenRouter](https://docs.openclaw.ai/providers/openrouter)
- [Qianfan](https://docs.openclaw.ai/providers/qianfan)
- [Qwen](https://docs.openclaw.ai/providers/qwen)
- [Runway](https://docs.openclaw.ai/providers/runway)
- [StepFun](https://docs.openclaw.ai/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/providers/synthetic)
- [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)
- [Venice (Venice AI)](https://docs.openclaw.ai/providers/venice)
- [xAI](https://docs.openclaw.ai/providers/xai)
- [Z.AI](https://docs.openclaw.ai/providers/zai)

## Additional bundled provider variants

- `anthropic-vertex` \- implicit Anthropic on Google Vertex support when Vertex credentials are available; no separate onboarding auth choice
- `copilot-proxy` \- local VS Code Copilot Proxy bridge; use `openclaw onboard --auth-choice copilot-proxy`
- `google-gemini-cli` \- unofficial Gemini CLI OAuth flow; requires a local `gemini` install (`brew install gemini-cli` or `npm install -g @google/gemini-cli`); default model `google-gemini-cli/gemini-3-flash-preview`; use `openclaw onboard --auth-choice google-gemini-cli` or `openclaw models auth login --provider google-gemini-cli --set-default`

For the full provider catalog (xAI, Groq, Mistral, etc.) and advanced configuration,
see [Model providers](https://docs.openclaw.ai/concepts/model-providers).

## Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [Models CLI](https://docs.openclaw.ai/cli/models)

[Provider directory](https://docs.openclaw.ai/providers) [Models CLI](https://docs.openclaw.ai/concepts/models)

Ctrl+I

---

## Moonshot AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/moonshot>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Moonshot AI

Moonshot provides the Kimi API with OpenAI-compatible endpoints. Configure the
provider and set the default model to `moonshot/kimi-k2.6`, or use
Kimi Coding with `kimi/kimi-code`.

Moonshot and Kimi Coding are **separate providers**. Keys are not interchangeable, endpoints differ, and model refs differ (`moonshot/...` vs `kimi/...`).

## Built-in model catalog

| Model ref | Name | Reasoning | Input | Context | Max output |
| --- | --- | --- | --- | --- | --- |
| `moonshot/kimi-k2.6` | Kimi K2.6 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2.5` | Kimi K2.5 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking` | Kimi K2 Thinking | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking-turbo` | Kimi K2 Thinking Turbo | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-turbo` | Kimi K2 Turbo | No | text | 256,000 | 16,384 |

Bundled cost estimates for current Moonshot-hosted K2 models use Moonshot’s
published pay-as-you-go rates: Kimi K2.6 is 0.16/MTokcachehit,0.16/MTok cache hit,
0.16/MTokcachehit,0.95/MTok input, and 4.00/MTokoutput;KimiK2.5is4.00/MTok output; Kimi K2.5 is 4.00/MTokoutput;KimiK2.5is0.10/MTok cache hit,
0.60/MTokinput,and0.60/MTok input, and 0.60/MTokinput,and3.00/MTok output. Other legacy catalog entries keep
zero-cost placeholders unless you override them in config.

## Getting started

Choose your provider and follow the setup steps.

- Moonshot API

- Kimi Coding

**Best for:** Kimi K2 models via the Moonshot Open Platform.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Choose your endpoint region

| Auth choice | Endpoint | Region |
| --- | --- | --- |
| `moonshot-api-key` | `https://api.moonshot.ai/v1` | International |
| `moonshot-api-key-cn` | `https://api.moonshot.cn/v1` | China |

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run onboarding

```
openclaw onboard --auth-choice moonshot-api-key
```

Or for the China endpoint:

```
openclaw onboard --auth-choice moonshot-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Verify models are available

```
openclaw models list --provider moonshot
```

5

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run a live smoke test

Use an isolated state dir when you want to verify model access and cost
tracking without touching your normal sessions:

```
OPENCLAW_CONFIG_PATH=/tmp/openclaw-kimi/openclaw.json \
OPENCLAW_STATE_DIR=/tmp/openclaw-kimi \
openclaw agent --local \
  --session-id live-kimi-cost \
  --message 'Reply exactly: KIMI_LIVE_OK' \
  --thinking off \
  --json
```

The JSON response should report `provider: "moonshot"` and
`model: "kimi-k2.6"`. The assistant transcript entry stores normalized
token usage plus estimated cost under `usage.cost` when Moonshot returns
usage metadata.

### Config example

```
{
  env: { MOONSHOT_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
      models: {
        // moonshot-kimi-k2-aliases:start
        "moonshot/kimi-k2.6": { alias: "Kimi K2.6" },
        "moonshot/kimi-k2.5": { alias: "Kimi K2.5" },
        "moonshot/kimi-k2-thinking": { alias: "Kimi K2 Thinking" },
        "moonshot/kimi-k2-thinking-turbo": { alias: "Kimi K2 Thinking Turbo" },
        "moonshot/kimi-k2-turbo": { alias: "Kimi K2 Turbo" },
        // moonshot-kimi-k2-aliases:end
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      moonshot: {
        baseUrl: "https://api.moonshot.ai/v1",
        apiKey: "${MOONSHOT_API_KEY}",
        api: "openai-completions",
        models: [\
          // moonshot-kimi-k2-models:start\
          {\
            id: "kimi-k2.6",\
            name: "Kimi K2.6",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0.95, output: 4, cacheRead: 0.16, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2.5",\
            name: "Kimi K2.5",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0.6, output: 3, cacheRead: 0.1, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-thinking",\
            name: "Kimi K2 Thinking",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-thinking-turbo",\
            name: "Kimi K2 Thinking Turbo",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-turbo",\
            name: "Kimi K2 Turbo",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 256000,\
            maxTokens: 16384,\
          },\
          // moonshot-kimi-k2-models:end\
        ],
      },
    },
  },
}
```

**Best for:** code-focused tasks via the Kimi Coding endpoint.

Kimi Coding uses a different API key and provider prefix (`kimi/...`) than Moonshot (`moonshot/...`). Legacy model ref `kimi/k2p5` remains accepted as a compatibility id.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run onboarding

```
openclaw onboard --auth-choice kimi-code-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "kimi/kimi-code" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Verify the model is available

```
openclaw models list --provider kimi
```

### Config example

```
{
  env: { KIMI_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "kimi/kimi-code" },
      models: {
        "kimi/kimi-code": { alias: "Kimi" },
      },
    },
  },
}
```

## Kimi web search

OpenClaw also ships **Kimi** as a `web_search` provider, backed by Moonshot web
search.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run interactive web search setup

```
openclaw configure --section web
```

Choose **Kimi** in the web-search section to store
`plugins.entries.moonshot.config.webSearch.*`.

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Configure the web search region and model

Interactive setup prompts for:

| Setting | Options |
| --- | --- |
| API region | `https://api.moonshot.ai/v1` (international) or `https://api.moonshot.cn/v1` (China) |
| Web search model | Defaults to `kimi-k2.6` |

Config lives under `plugins.entries.moonshot.config.webSearch`:

```
{
  plugins: {
    entries: {
      moonshot: {
        config: {
          webSearch: {
            apiKey: "sk-...", // or use KIMI_API_KEY / MOONSHOT_API_KEY
            baseUrl: "https://api.moonshot.ai/v1",
            model: "kimi-k2.6",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "kimi",
      },
    },
  },
}
```

## Advanced configuration

Native thinking mode

Moonshot Kimi supports binary native thinking:

- `thinking: { type: "enabled" }`
- `thinking: { type: "disabled" }`

Configure it per model via `agents.defaults.models.<provider/model>.params`:

```
{
  agents: {
    defaults: {
      models: {
        "moonshot/kimi-k2.6": {
          params: {
            thinking: { type: "disabled" },
          },
        },
      },
    },
  },
}
```

OpenClaw also maps runtime `/think` levels for Moonshot:

| `/think` level | Moonshot behavior |
| --- | --- |
| `/think off` | `thinking.type=disabled` |
| Any non-off level | `thinking.type=enabled` |

When Moonshot thinking is enabled, `tool_choice` must be `auto` or `none`. OpenClaw normalizes incompatible `tool_choice` values to `auto` for compatibility.

Kimi K2.6 also accepts an optional `thinking.keep` field that controls
multi-turn retention of `reasoning_content`. Set it to `"all"` to keep full
reasoning across turns; omit it (or leave it `null`) to use the server
default strategy. OpenClaw only forwards `thinking.keep` for
`moonshot/kimi-k2.6` and strips it from other models.

```
{
  agents: {
    defaults: {
      models: {
        "moonshot/kimi-k2.6": {
          params: {
            thinking: { type: "enabled", keep: "all" },
          },
        },
      },
    },
  },
}
```

Tool call id sanitization

Moonshot Kimi serves tool\_call ids shaped like `functions.<name>:<index>`. OpenClaw preserves them unchanged so multi-turn tool calls keep working.To force strict sanitization on a custom OpenAI-compatible provider, set `sanitizeToolCallIds: true`:

```
{
  models: {
    providers: {
      "my-kimi-proxy": {
        api: "openai-completions",
        sanitizeToolCallIds: true,
      },
    },
  },
}
```

Streaming usage compatibility

Native Moonshot endpoints (`https://api.moonshot.ai/v1` and
`https://api.moonshot.cn/v1`) advertise streaming usage compatibility on the
shared `openai-completions` transport. OpenClaw keys that off endpoint
capabilities, so compatible custom provider ids targeting the same native
Moonshot hosts inherit the same streaming-usage behavior.With the bundled K2.6 pricing, streamed usage that includes input, output,
and cache-read tokens is also converted into local estimated USD cost for
`/status`, `/usage full`, `/usage cost`, and transcript-backed session
accounting.

Endpoint and model ref reference

| Provider | Model ref prefix | Endpoint | Auth env var |
| --- | --- | --- | --- |
| Moonshot | `moonshot/` | `https://api.moonshot.ai/v1` | `MOONSHOT_API_KEY` |
| Moonshot CN | `moonshot/` | `https://api.moonshot.cn/v1` | `MOONSHOT_API_KEY` |
| Kimi Coding | `kimi/` | Kimi Coding endpoint | `KIMI_API_KEY` |
| Web search | N/A | Same as Moonshot API region | `KIMI_API_KEY` or `MOONSHOT_API_KEY` |

- Kimi web search uses `KIMI_API_KEY` or `MOONSHOT_API_KEY`, and defaults to `https://api.moonshot.ai/v1` with model `kimi-k2.6`.
- Override pricing and context metadata in `models.providers` if needed.
- If Moonshot publishes different context limits for a model, adjust `contextWindow` accordingly.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Web search** \\
\\
Configuring web search providers including Kimi.](https://docs.openclaw.ai/tools/web)

[**Configuration reference** \\
\\
Full config schema for providers, models, and plugins.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Moonshot Open Platform** \\
\\
Moonshot API key management and documentation.](https://platform.moonshot.ai/)

[Mistral](https://docs.openclaw.ai/providers/mistral) [NVIDIA](https://docs.openclaw.ai/providers/nvidia)

Ctrl+I

---

## NVIDIA - OpenClaw

_Source: <https://docs.openclaw.ai/providers/nvidia>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

NVIDIA

NVIDIA provides an OpenAI-compatible API at `https://integrate.api.nvidia.com/v1` for
open models for free. Authenticate with an API key from
[build.nvidia.com](https://build.nvidia.com/settings/api-keys).

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Get your API key

Create an API key at [build.nvidia.com](https://build.nvidia.com/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Export the key and run onboarding

```
export NVIDIA_API_KEY="nvapi-..."
openclaw onboard --auth-choice nvidia-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Set an NVIDIA model

```
openclaw models set nvidia/nvidia/nemotron-3-super-120b-a12b
```

If you pass `--nvidia-api-key` instead of the env var, the value lands in shell
history and `ps` output. Prefer the `NVIDIA_API_KEY` environment variable when
possible.

For non-interactive setup, you can also pass the key directly:

```
openclaw onboard --auth-choice nvidia-api-key --nvidia-api-key "nvapi-..."
```

## Config example

```
{
  env: { NVIDIA_API_KEY: "nvapi-..." },
  models: {
    providers: {
      nvidia: {
        baseUrl: "https://integrate.api.nvidia.com/v1",
        api: "openai-completions",
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "nvidia/nvidia/nemotron-3-super-120b-a12b" },
    },
  },
}
```

## Built-in catalog

| Model ref | Name | Context | Max output |
| --- | --- | --- | --- |
| `nvidia/nvidia/nemotron-3-super-120b-a12b` | NVIDIA Nemotron 3 Super 120B | 262,144 | 8,192 |
| `nvidia/moonshotai/kimi-k2.5` | Kimi K2.5 | 262,144 | 8,192 |
| `nvidia/minimaxai/minimax-m2.5` | Minimax M2.5 | 196,608 | 8,192 |
| `nvidia/z-ai/glm5` | GLM 5 | 202,752 | 8,192 |

## Advanced configuration

Auto-enable behavior

The provider auto-enables when the `NVIDIA_API_KEY` environment variable is set.
No explicit provider config is required beyond the key.

Catalog and pricing

The bundled catalog is static. Costs default to `0` in source since NVIDIA
currently offers free API access for the listed models.

OpenAI-compatible endpoint

NVIDIA uses the standard `/v1` completions endpoint. Any OpenAI-compatible
tooling should work out of the box with the NVIDIA base URL.

NVIDIA models are currently free to use. Check
[build.nvidia.com](https://build.nvidia.com/) for the latest availability and
rate-limit details.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[Moonshot AI](https://docs.openclaw.ai/providers/moonshot) [Ollama](https://docs.openclaw.ai/providers/ollama)

Ctrl+I

---

## Ollama - OpenClaw

_Source: <https://docs.openclaw.ai/providers/ollama>_

# or
ollama pull gpt-oss:20b
# or
ollama pull llama3.3
```

3

[Navigate to header](https://docs.openclaw.ai/providers/ollama#)

Enable Ollama for OpenClaw

For `Cloud only`, use your real `OLLAMA_API_KEY`. For host-backed setups, any placeholder value works:

```
# Cloud
export OLLAMA_API_KEY="your-ollama-api-key"

# Local-only
export OLLAMA_API_KEY="ollama-local"

# Or configure in your config file
openclaw config set models.providers.ollama.apiKey "OLLAMA_API_KEY"
```

4

[Navigate to header](https://docs.openclaw.ai/providers/ollama#)

Inspect and set your model

```
openclaw models list
openclaw models set ollama/gemma4
```

Or set the default in config:

```
{
  agents: {
    defaults: {
      model: { primary: "ollama/gemma4" },
    },
  },
}
```

## Cloud models

- Cloud + Local

- Cloud only

- Local only

`Cloud + Local` uses a reachable Ollama host as the control point for both local and cloud models. This is Ollama’s preferred hybrid flow.Use **Cloud + Local** during setup. OpenClaw prompts for the Ollama base URL, discovers local models from that host, and checks whether the host is signed in for cloud access with `ollama signin`. When the host is signed in, OpenClaw also suggests hosted cloud defaults such as `kimi-k2.5:cloud`, `minimax-m2.7:cloud`, and `glm-5.1:cloud`.If the host is not signed in yet, OpenClaw keeps the setup local-only until you run `ollama signin`.

`Cloud only` runs against Ollama’s hosted API at `https://ollama.com`.Use **Cloud only** during setup. OpenClaw prompts for `OLLAMA_API_KEY`, sets `baseUrl: "https://ollama.com"`, and seeds the hosted cloud model list. This path does **not** require a local Ollama server or `ollama signin`.The cloud model list shown during `openclaw onboard` is populated live from `https://ollama.com/api/tags`, capped at 500 entries, so the picker reflects the current hosted catalog rather than a static seed. If `ollama.com` is unreachable or returns no models at setup time, OpenClaw falls back to the previous hardcoded suggestions so onboarding still completes.

In local-only mode, OpenClaw discovers models from the configured Ollama instance. This path is for local or self-hosted Ollama servers.OpenClaw currently suggests `gemma4` as the local default.

## Model discovery (implicit provider)

When you set `OLLAMA_API_KEY` (or an auth profile) and **do not** define `models.providers.ollama` or another custom remote provider with `api: "ollama"`, OpenClaw discovers models from the local Ollama instance at `http://127.0.0.1:11434`.

| Behavior | Detail |
| --- | --- |
| Catalog query | Queries `/api/tags` |
| Capability detection | Uses best-effort `/api/show` lookups to read `contextWindow`, expanded `num_ctx` Modelfile parameters, and capabilities including vision/tools |
| Vision models | Models with a `vision` capability reported by `/api/show` are marked as image-capable (`input: ["text", "image"]`), so OpenClaw auto-injects images into the prompt |
| Reasoning detection | Uses `/api/show` capabilities when available, including `thinking`; falls back to a model-name heuristic (`r1`, `reasoning`, `think`) when Ollama omits capabilities |
| Token limits | Sets `maxTokens` to the default Ollama max-token cap used by OpenClaw |
| Costs | Sets all costs to `0` |

This avoids manual model entries while keeping the catalog aligned with the local Ollama instance. You can use a full ref such as `ollama/<pulled-model>:latest` in local `infer model run`; OpenClaw resolves that installed model from Ollama’s live catalog without requiring a hand-written `models.json` entry.For signed-in Ollama hosts, some `:cloud` models may be usable through `/api/chat`
and `/api/show` before they appear in `/api/tags`. When you explicitly select a
full `ollama/<model>:cloud` ref, OpenClaw validates that exact missing model with
`/api/show` and adds it to the runtime catalog only if Ollama confirms model
metadata. Typos still fail as unknown models instead of being auto-created.

```
# See what models are available
ollama list
openclaw models list
```

For a narrow text-generation smoke test that avoids the full agent tool surface,
use local `infer model run` with a full Ollama model ref:

```
OLLAMA_API_KEY=ollama-local \
  openclaw infer model run \
    --local \
    --model ollama/llama3.2:latest \
    --prompt "Reply with exactly: pong" \
    --json
```

That path still uses OpenClaw’s configured provider, auth, and native Ollama
transport, but it does not start a chat-agent turn or load MCP/tool context. If
this succeeds while normal agent replies fail, troubleshoot the model’s agent
prompt/tool capacity next.For a narrow vision-model smoke test on the same lean path, add one or more
image files to `infer model run`. This sends the prompt and image directly to
the selected Ollama vision model without loading chat tools, memory, or prior
session context:

```
OLLAMA_API_KEY=ollama-local \
  openclaw infer model run \
    --local \
    --model ollama/qwen2.5vl:7b \
    --prompt "Describe this image in one sentence." \
    --file ./photo.jpg \
    --json
```

`model run --file` accepts files detected as `image/*`, including common PNG,
JPEG, and WebP inputs. Non-image files are rejected before Ollama is called.
For speech recognition, use `openclaw infer audio transcribe` instead.When you switch a conversation with `/model ollama/<model>`, OpenClaw treats
that as an exact user selection. If the configured Ollama `baseUrl` is
unreachable, the next reply fails with the provider error instead of silently
answering from another configured fallback model.Isolated cron jobs do one extra local safety check before they start the agent
turn. If the selected model resolves to a local, private-network, or `.local`
Ollama provider and `/api/tags` is unreachable, OpenClaw records that cron run
as `skipped` with the selected `ollama/<model>` in the error text. The endpoint
preflight is cached for 5 minutes, so multiple cron jobs pointed at the same
stopped Ollama daemon do not all launch failing model requests.Live-verify the local text path, native stream path, and embeddings against
local Ollama with:

```
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_OLLAMA=1 OPENCLAW_LIVE_OLLAMA_WEB_SEARCH=0 \
  pnpm test:live -- extensions/ollama/ollama.live.test.ts
```

To add a new model, simply pull it with Ollama:

```
ollama pull mistral
```

The new model will be automatically discovered and available to use.

If you set `models.providers.ollama` explicitly, or configure a custom remote provider such as `models.providers.ollama-cloud` with `api: "ollama"`, auto-discovery is skipped and you must define models manually. Loopback custom providers such as `http://127.0.0.2:11434` are still treated as local. See the explicit config section below.

## Vision and image description

The bundled Ollama plugin registers Ollama as an image-capable media-understanding provider. This lets OpenClaw route explicit image-description requests and configured image-model defaults through local or hosted Ollama vision models.For local vision, pull a model that supports images:

```
ollama pull qwen2.5vl:7b
export OLLAMA_API_KEY="ollama-local"
```

Then verify with the infer CLI:

```
openclaw infer image describe \
  --file ./photo.jpg \
  --model ollama/qwen2.5vl:7b \
  --json
```

`--model` must be a full `<provider/model>` ref. When it is set, `openclaw infer image describe` runs that model directly instead of skipping description because the model supports native vision.Use `infer image describe` when you want OpenClaw’s image-understanding provider flow, configured `agents.defaults.imageModel`, and image-description output shape. Use `infer model run --file` when you want a raw multimodal model probe with a custom prompt and one or more images.To make Ollama the default image-understanding model for inbound media, configure `agents.defaults.imageModel`:

```
{
  agents: {
    defaults: {
      imageModel: {
        primary: "ollama/qwen2.5vl:7b",
      },
    },
  },
}
```

Prefer the full `ollama/<model>` ref. If the same model is listed under `models.providers.ollama.models` with `input: ["text", "image"]` and no other configured image provider exposes that bare model ID, OpenClaw also normalizes a bare `imageModel` ref such as `qwen2.5vl:7b` to `ollama/qwen2.5vl:7b`. If more than one configured image provider has the same bare ID, use the provider prefix explicitly.Slow local vision models can need a longer image-understanding timeout than cloud models. They can also crash or stop when Ollama tries to allocate the full advertised vision context on constrained hardware. Set a capability timeout, and cap `num_ctx` on the model entry when you only need a normal image-description turn:

```
{
  models: {
    providers: {
      ollama: {
        models: [\
          {\
            id: "qwen2.5vl:7b",\
            name: "qwen2.5vl:7b",\
            input: ["text", "image"],\
            params: { num_ctx: 2048, keep_alive: "1m" },\
          },\
        ],
      },
    },
  },
  tools: {
    media: {
      image: {
        timeoutSeconds: 180,
        models: [{ provider: "ollama", model: "qwen2.5vl:7b", timeoutSeconds: 300 }],
      },
    },
  },
}
```

This timeout applies to inbound image understanding and to the explicit `image` tool the agent can call during a turn. Provider-level `models.providers.ollama.timeoutSeconds` still controls the underlying Ollama HTTP request guard for normal model calls.Live-verify the explicit image tool against local Ollama with:

```
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_OLLAMA_IMAGE=1 \
  pnpm test:live -- src/agents/tools/image-tool.ollama.live.test.ts
```

If you define `models.providers.ollama.models` manually, mark vision models with image input support:

```
{
  id: "qwen2.5vl:7b",
  name: "qwen2.5vl:7b",
  input: ["text", "image"],
  contextWindow: 128000,
  maxTokens: 8192,
}
```

OpenClaw rejects image-description requests for models that are not marked image-capable. With implicit discovery, OpenClaw reads this from Ollama when `/api/show` reports a vision capability.

## Configuration

- Basic (implicit discovery)

- Explicit (manual models)

- Custom base URL

The simplest local-only enablement path is via environment variable:

```
export OLLAMA_API_KEY="ollama-local"
```

If `OLLAMA_API_KEY` is set, you can omit `apiKey` in the provider entry and OpenClaw will fill it for availability checks.

Use explicit config when you want hosted cloud setup, Ollama runs on another host/port, you want to force specific context windows or model lists, or you want fully manual model definitions.

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [\
          {\
            id: "kimi-k2.5:cloud",\
            name: "kimi-k2.5:cloud",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192\
          }\
        ]
      }
    }
  }
}
```

If Ollama is running on a different host or port (explicit config disables auto-discovery, so define models manually):

```
{
  models: {
    providers: {
      ollama: {
        apiKey: "ollama-local",
        baseUrl: "http://ollama-host:11434", // No /v1 - use native Ollama API URL
        api: "ollama", // Set explicitly to guarantee native tool-calling behavior
        timeoutSeconds: 300, // Optional: give cold local models longer to connect and stream
        models: [\
          {\
            id: "qwen3:32b",\
            name: "qwen3:32b",\
            params: {\
              keep_alive: "15m", // Optional: keep the model loaded between turns\
            },\
          },\
        ],
      },
    },
  },
}
```

Do not add `/v1` to the URL. The `/v1` path uses OpenAI-compatible mode, where tool calling is not reliable. Use the base Ollama URL without a path suffix.

## Common recipes

Use these as starting points and replace model IDs with the exact names from `ollama list` or `openclaw models list --provider ollama`.

Local model with auto-discovery

Use this when Ollama runs on the same machine as the Gateway and you want OpenClaw to discover the installed models automatically.

```
ollama serve
ollama pull gemma4
export OLLAMA_API_KEY="ollama-local"
openclaw models list --provider ollama
openclaw models set ollama/gemma4
```

This path keeps config minimal. Do not add a `models.providers.ollama` block unless you want to define models manually.

LAN Ollama host with manual models

Use native Ollama URLs for LAN hosts. Do not add `/v1`.

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://gpu-box.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 300,
        contextWindow: 32768,
        maxTokens: 8192,
        models: [\
          {\
            id: "qwen3.5:9b",\
            name: "qwen3.5:9b",\
            reasoning: true,\
            input: ["text"],\
            params: {\
              num_ctx: 32768,\
              thinking: false,\
              keep_alive: "15m",\
            },\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "ollama/qwen3.5:9b" },
    },
  },
}
```

`contextWindow` is the OpenClaw-side context budget. `params.num_ctx` is sent to Ollama for the request. Keep them aligned when your hardware cannot run the model’s full advertised context.

Ollama Cloud only

Use this when you do not run a local daemon and want hosted Ollama models directly.

```
export OLLAMA_API_KEY="your-ollama-api-key"
```

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [\
          {\
            id: "kimi-k2.5:cloud",\
            name: "kimi-k2.5:cloud",\
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
      model: { primary: "ollama/kimi-k2.5:cloud" },
    },
  },
}
```

Cloud plus local through a signed-in daemon

Use this when a local or LAN Ollama daemon is signed in with `ollama signin` and should serve both local models and `:cloud` models.

```
ollama signin
ollama pull gemma4
```

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://127.0.0.1:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 300,
        models: [\
          { id: "gemma4", name: "gemma4", input: ["text"] },\
          { id: "kimi-k2.5:cloud", name: "kimi-k2.5:cloud", input: ["text", "image"] },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: {
        primary: "ollama/gemma4",
        fallbacks: ["ollama/kimi-k2.5:cloud"],
      },
    },
  },
}
```

Multiple Ollama hosts

Use custom provider IDs when you have more than one Ollama server. Each provider gets its own host, models, auth, timeout, and model refs.

```
{
  models: {
    providers: {
      "ollama-fast": {
        baseUrl: "http://mini.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        contextWindow: 32768,
        models: [{ id: "gemma4", name: "gemma4", input: ["text"] }],
      },
      "ollama-large": {
        baseUrl: "http://gpu-box.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 420,
        contextWindow: 131072,
        maxTokens: 16384,
        models: [{ id: "qwen3.5:27b", name: "qwen3.5:27b", input: ["text"] }],
      },
    },
  },
  agents: {
    defaults: {
      model: {
        primary: "ollama-fast/gemma4",
        fallbacks: ["ollama-large/qwen3.5:27b"],
      },
    },
  },
}
```

When OpenClaw sends the request, the active provider prefix is stripped so `ollama-large/qwen3.5:27b` reaches Ollama as `qwen3.5:27b`.

Lean local model profile

Some local models can answer simple prompts but struggle with the full agent tool surface. Start by limiting tools and context before changing global runtime settings.

```
{
  agents: {
    defaults: {
      experimental: {
        localModelLean: true,
      },
      model: { primary: "ollama/gemma4" },
    },
  },
  models: {
    providers: {
      ollama: {
        baseUrl: "http://127.0.0.1:11434",
        apiKey: "ollama-local",
        api: "ollama",
        contextWindow: 32768,
        models: [\
          {\
            id: "gemma4",\
            name: "gemma4",\
            input: ["text"],\
            params: { num_ctx: 32768 },\
            compat: { supportsTools: false },\
          },\
        ],
      },
    },
  },
}
```

Use `compat.supportsTools: false` only when the model or server reliably fails on tool schemas. It trades agent capability for stability.
`localModelLean` removes the browser, cron, and message tools from the agent surface, but it does not change Ollama’s runtime context or thinking mode. Pair it with explicit `params.num_ctx` and `params.thinking: false` for small Qwen-style thinking models that loop or spend their response budget on hidden reasoning.

### Model selection

Once configured, all your Ollama models are available:

```
{
  agents: {
    defaults: {
      model: {
        primary: "ollama/gpt-oss:20b",
        fallbacks: ["ollama/llama3.3", "ollama/qwen2.5-coder:32b"],
      },
    },
  },
}
```

Custom Ollama provider ids are also supported. When a model ref uses the active
provider prefix, such as `ollama-spark/qwen3:32b`, OpenClaw strips only that
prefix before calling Ollama so the server receives `qwen3:32b`.For slow local models, prefer provider-scoped request tuning before raising the
whole agent runtime timeout:

```
{
  models: {
    providers: {
      ollama: {
        timeoutSeconds: 300,
        models: [\
          {\
            id: "gemma4:26b",\
            name: "gemma4:26b",\
            params: { keep_alive: "15m" },\
          },\
        ],
      },
    },
  },
}
```

`timeoutSeconds` applies to the model HTTP request, including connection setup,
headers, body streaming, and the total guarded-fetch abort. `params.keep_alive`
is forwarded to Ollama as top-level `keep_alive` on native `/api/chat` requests;
set it per model when first-turn load time is the bottleneck.

### Quick verification

```
# Ollama daemon visible to this machine
curl http://127.0.0.1:11434/api/tags

# OpenClaw catalog and selected model
openclaw models list --provider ollama
openclaw models status

# Direct model smoke
openclaw infer model run \
  --model ollama/gemma4 \
  --prompt "Reply with exactly: ok"
```

For remote hosts, replace `127.0.0.1` with the host used in `baseUrl`. If `curl` works but OpenClaw does not, check whether the Gateway runs on a different machine, container, or service account.

## Ollama Web Search

OpenClaw supports **Ollama Web Search** as a bundled `web_search` provider.

| Property | Detail |
| --- | --- |
| Host | Uses your configured Ollama host (`models.providers.ollama.baseUrl` when set, otherwise `http://127.0.0.1:11434`); `https://ollama.com` uses the hosted API directly |
| Auth | Key-free for signed-in local Ollama hosts; `OLLAMA_API_KEY` or configured provider auth for direct `https://ollama.com` search or auth-protected hosts |
| Requirement | Local/self-hosted hosts must be running and signed in with `ollama signin`; direct hosted search requires `baseUrl: "https://ollama.com"` plus a real Ollama API key |

Choose **Ollama Web Search** during `openclaw onboard` or `openclaw configure --section web`, or set:

```
{
  tools: {
    web: {
      search: {
        provider: "ollama",
      },
    },
  },
}
```

For direct hosted search through Ollama Cloud:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [{ id: "kimi-k2.5:cloud", name: "kimi-k2.5:cloud", input: ["text"] }],
      },
    },
  },
  tools: {
    web: {
      search: { provider: "ollama" },
    },
  },
}
```

For a signed-in local daemon, OpenClaw uses the daemon’s `/api/experimental/web_search` proxy. For `https://ollama.com`, it calls the hosted `/api/web_search` endpoint directly.

For the full setup and behavior details, see [Ollama Web Search](https://docs.openclaw.ai/tools/ollama-search).

## Advanced configuration

Legacy OpenAI-compatible mode

**Tool calling is not reliable in OpenAI-compatible mode.** Use this mode only if you need OpenAI format for a proxy and do not depend on native tool calling behavior.

If you need to use the OpenAI-compatible endpoint instead (for example, behind a proxy that only supports OpenAI format), set `api: "openai-completions"` explicitly:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434/v1",
        api: "openai-completions",
        injectNumCtxForOpenAICompat: true, // default: true
        apiKey: "ollama-local",
        models: [...]
      }
    }
  }
}
```

This mode may not support streaming and tool calling simultaneously. You may need to disable streaming with `params: { streaming: false }` in model config.When `api: "openai-completions"` is used with Ollama, OpenClaw injects `options.num_ctx` by default so Ollama does not silently fall back to a 4096 context window. If your proxy/upstream rejects unknown `options` fields, disable this behavior:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434/v1",
        api: "openai-completions",
        injectNumCtxForOpenAICompat: false,
        apiKey: "ollama-local",
        models: [...]
      }
    }
  }
}
```

Context windows

For auto-discovered models, OpenClaw uses the context window reported by Ollama when available, including larger `PARAMETER num_ctx` values from custom Modelfiles. Otherwise it falls back to the default Ollama context window used by OpenClaw.You can set provider-level `contextWindow`, `contextTokens`, and `maxTokens` defaults for every model under that Ollama provider, then override them per model when needed. `contextWindow` is OpenClaw’s prompt and compaction budget. Native Ollama requests leave `options.num_ctx` unset unless you explicitly configure `params.num_ctx`, so Ollama can apply its own model, `OLLAMA_CONTEXT_LENGTH`, or VRAM-based default. To cap or force Ollama’s per-request runtime context without rebuilding a Modelfile, set `params.num_ctx`; invalid, zero, negative, and non-finite values are ignored. The OpenAI-compatible Ollama adapter still injects `options.num_ctx` by default from the configured `params.num_ctx` or `contextWindow`; disable that with `injectNumCtxForOpenAICompat: false` if your upstream rejects `options`.Native Ollama model entries also accept the common Ollama runtime options under `params`, including `temperature`, `top_p`, `top_k`, `min_p`, `num_predict`, `stop`, `repeat_penalty`, `num_batch`, `num_thread`, and `use_mmap`. OpenClaw forwards only Ollama request keys, so OpenClaw runtime params such as `streaming` are not leaked to Ollama. Use `params.think` or `params.thinking` to send top-level Ollama `think`; `false` disables API-level thinking for Qwen-style thinking models.

```
{
  models: {
    providers: {
      ollama: {
        contextWindow: 32768,
        models: [\
          {\
            id: "llama3.3",\
            contextWindow: 131072,\
            maxTokens: 65536,\
            params: {\
              num_ctx: 32768,\
              temperature: 0.7,\
              top_p: 0.9,\
              thinking: false,\
            },\
          }\
        ]
      }
    }
  }
}
```

Per-model `agents.defaults.models["ollama/<model>"].params.num_ctx` works too. If both are configured, the explicit provider model entry wins over the agent default.

Thinking control

For native Ollama models, OpenClaw forwards thinking control as Ollama expects it: top-level `think`, not `options.think`. Auto-discovered models whose `/api/show` response includes the `thinking` capability expose `/think low`, `/think medium`, `/think high`, and `/think max`; non-thinking models expose only `/think off`.

```
openclaw agent --model ollama/gemma4 --thinking off
openclaw agent --model ollama/gemma4 --thinking low
```

You can also set a model default:

```
{
  agents: {
    defaults: {
      models: {
        "ollama/gemma4": {
          thinking: "low",
        },
      },
    },
  },
}
```

Per-model `params.think` or `params.thinking` can disable or force Ollama API thinking for a specific configured model. OpenClaw preserves those explicit model params when the active run only has the implicit default `off`; non-off runtime commands such as `/think medium` still override the active run.

Reasoning models

OpenClaw treats models with names such as `deepseek-r1`, `reasoning`, or `think` as reasoning-capable by default.

```
ollama pull deepseek-r1:32b
```

No additional configuration is needed. OpenClaw marks them automatically.

Model costs

Ollama is free and runs locally, so all model costs are set to $0. This applies to both auto-discovered and manually defined models.

Memory embeddings

The bundled Ollama plugin registers a memory embedding provider for
[memory search](https://docs.openclaw.ai/concepts/memory). It uses the configured Ollama base URL
and API key, calls Ollama’s current `/api/embed` endpoint, and batches
multiple memory chunks into one `input` request when possible.

| Property | Value |
| --- | --- |
| Default model | `nomic-embed-text` |
| Auto-pull | Yes — the embedding model is pulled automatically if not present locally |

Query-time embeddings use retrieval prefixes for models that require or recommend them, including `nomic-embed-text`, `qwen3-embedding`, and `mxbai-embed-large`. Memory document batches stay raw so existing indexes do not need a format migration.To select Ollama as the memory search embedding provider:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama",
        remote: {
          // Default for Ollama. Raise on larger hosts if reindexing is too slow.
          nonBatchConcurrency: 1,
        },
      },
    },
  },
}
```

For a remote embedding host, keep auth scoped to that host:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama",
        model: "nomic-embed-text",
        remote: {
          baseUrl: "http://gpu-box.local:11434",
          apiKey: "ollama-local",
          nonBatchConcurrency: 2,
        },
      },
    },
  },
}
```

Streaming configuration

OpenClaw’s Ollama integration uses the **native Ollama API** (`/api/chat`) by default, which fully supports streaming and tool calling simultaneously. No special configuration is needed.For native `/api/chat` requests, OpenClaw also forwards thinking control directly to Ollama: `/think off` and `openclaw agent --thinking off` send top-level `think: false` unless an explicit model `params.think`/`params.thinking` value is configured, while `/think low|medium|high` send the matching top-level `think` effort string. `/think max` maps to Ollama’s highest native effort, `think: "high"`.

If you need to use the OpenAI-compatible endpoint, see the “Legacy OpenAI-compatible mode” section above. Streaming and tool calling may not work simultaneously in that mode.

## Troubleshooting

WSL2 crash loop (repeated reboots)

On WSL2 with NVIDIA/CUDA, the official Ollama Linux installer creates an `ollama.service` systemd unit with `Restart=always`. If that service autostarts and loads a GPU-backed model during WSL2 boot, Ollama can pin host memory while the model loads. Hyper-V memory reclaim cannot always reclaim those pinned pages, so Windows can terminate the WSL2 VM, systemd starts Ollama again, and the loop repeats.Common evidence:

- repeated WSL2 reboots or terminations from the Windows side
- high CPU in `app.slice` or `ollama.service` shortly after WSL2 startup
- SIGTERM from systemd rather than a Linux OOM-killer event

OpenClaw logs a startup warning when it detects WSL2, `ollama.service` enabled with `Restart=always`, and visible CUDA markers.Mitigation:

```
sudo systemctl disable ollama
```

Add this to `%USERPROFILE%\.wslconfig` on the Windows side, then run `wsl --shutdown`:

```
[experimental]
autoMemoryReclaim=disabled
```

Set a shorter keep-alive in the Ollama service environment, or start Ollama manually only when you need it:

```
export OLLAMA_KEEP_ALIVE=5m
ollama serve
```

See [ollama/ollama#11317](https://github.com/ollama/ollama/issues/11317).

Ollama not detected

Make sure Ollama is running and that you set `OLLAMA_API_KEY` (or an auth profile), and that you did **not** define an explicit `models.providers.ollama` entry:

```
ollama serve
```

Verify that the API is accessible:

```
curl http://localhost:11434/api/tags
```

No models available

If your model is not listed, either pull the model locally or define it explicitly in `models.providers.ollama`.

```
ollama list  # See what's installed
ollama pull gemma4
ollama pull gpt-oss:20b
ollama pull llama3.3     # Or another model
```

Connection refused

Check that Ollama is running on the correct port:

```
# Check if Ollama is running
ps aux | grep ollama

# Or restart Ollama
ollama serve
```

Remote host works with curl but not OpenClaw

Verify from the same machine and runtime that runs the Gateway:

```
openclaw gateway status --deep
curl http://ollama-host:11434/api/tags
```

Common causes:

- `baseUrl` points at `localhost`, but the Gateway runs in Docker or on another host.
- The URL uses `/v1`, which selects OpenAI-compatible behavior instead of native Ollama.
- The remote host needs firewall or LAN binding changes on the Ollama side.
- The model is present on your laptop’s daemon but not on the remote daemon.

Model outputs tool JSON as text

This usually means the provider is using OpenAI-compatible mode or the model cannot handle tool schemas.Prefer native Ollama mode:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434",
        api: "ollama",
      },
    },
  },
}
```

If a small local model still fails on tool schemas, set `compat.supportsTools: false` on that model entry and retest.

Kimi or GLM returns garbled symbols

Hosted Kimi/GLM responses that are long, non-linguistic symbol runs are treated as failed provider output instead of a successful assistant answer. That lets normal retry, fallback, or error handling take over without persisting the corrupted text into the session.If it happens repeatedly, capture the raw model name, the current session file, and whether the run used `Cloud + Local` or `Cloud only`, then try a fresh session and a fallback model:

```
openclaw infer model run --model ollama/kimi-k2.5:cloud --prompt "Reply with exactly: ok" --json
openclaw models set ollama/gemma4
```

Cold local model times out

Large local models can need a long first load before streaming begins. Keep the timeout scoped to the Ollama provider, and optionally ask Ollama to keep the model loaded between turns:

```
{
  models: {
    providers: {
      ollama: {
        timeoutSeconds: 300,
        models: [\
          {\
            id: "gemma4:26b",\
            name: "gemma4:26b",\
            params: { keep_alive: "15m" },\
          },\
        ],
      },
    },
  },
}
```

If the host itself is slow to accept connections, `timeoutSeconds` also extends the guarded Undici connect timeout for this provider.

Large-context model is too slow or runs out of memory

Many Ollama models advertise contexts that are larger than your hardware can run comfortably. Native Ollama uses Ollama’s own runtime context default unless you set `params.num_ctx`. Cap both OpenClaw’s budget and Ollama’s request context when you want predictable first-token latency:

```
{
  models: {
    providers: {
      ollama: {
        contextWindow: 32768,
        maxTokens: 8192,
        models: [\
          {\
            id: "qwen3.5:9b",\
            name: "qwen3.5:9b",\
            params: { num_ctx: 32768, thinking: false },\
          },\
        ],
      },
    },
  },
}
```

Lower `contextWindow` first if OpenClaw is sending too much prompt. Lower `params.num_ctx` if Ollama is loading a runtime context that is too large for the machine. Lower `maxTokens` if generation runs too long.

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Related

[**Model providers** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Model selection** \\
\\
How to choose and configure models.](https://docs.openclaw.ai/concepts/models)

[**Ollama Web Search** \\
\\
Full setup and behavior details for Ollama-powered web search.](https://docs.openclaw.ai/tools/ollama-search)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[NVIDIA](https://docs.openclaw.ai/providers/nvidia) [OpenAI](https://docs.openclaw.ai/providers/openai)

Ctrl+I

---

## OpenAI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/openai>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenAI

OpenAI provides developer APIs for GPT models, and Codex is also available as a
ChatGPT-plan coding agent through OpenAI’s Codex clients. OpenClaw keeps those
surfaces separate so config stays predictable.OpenClaw supports three OpenAI-family routes. Most ChatGPT/Codex subscribers
who want Codex behavior should use the native Codex app-server runtime. The
model prefix selects the provider/model name; a separate runtime setting selects
who executes the embedded agent loop:

- **API key** \- direct OpenAI Platform access with usage-based billing (`openai/*` models)
- **Codex subscription with native Codex runtime** \- ChatGPT/Codex sign-in plus Codex app-server execution (`openai/*` models plus `agents.defaults.agentRuntime.id: "codex"`)
- **Codex subscription through PI** \- ChatGPT/Codex sign-in with the normal OpenClaw PI runner (`openai-codex/*` models)

OpenAI explicitly supports subscription OAuth usage in external tools and workflows like OpenClaw.Provider, model, runtime, and channel are separate layers. If those labels are
getting mixed together, read [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes) before
changing config.

## Quick choice

| Goal | Use | Notes |
| --- | --- | --- |
| ChatGPT/Codex subscription with native Codex runtime | `openai/gpt-5.5` plus `agentRuntime.id: "codex"` | Recommended Codex setup for most users. Sign in with `openai-codex` auth. |
| Direct API-key billing | `openai/gpt-5.5` | Set `OPENAI_API_KEY` or run OpenAI API-key onboarding. |
| ChatGPT/Codex subscription auth through PI | `openai-codex/gpt-5.5` | Use only when you intentionally want the normal PI runner. |
| Image generation or editing | `openai/gpt-image-2` | Works with either `OPENAI_API_KEY` or OpenAI Codex OAuth. |
| Transparent-background images | `openai/gpt-image-1.5` | Use `outputFormat=png` or `webp` and `openai.background=transparent`. |

## Naming map

The names are similar but not interchangeable:

| Name you see | Layer | Meaning |
| --- | --- | --- |
| `openai` | Provider prefix | Direct OpenAI Platform API route. |
| `openai-codex` | Provider prefix | OpenAI Codex OAuth/subscription route through the normal OpenClaw PI runner. |
| `codex` plugin | Plugin | Bundled OpenClaw plugin that provides native Codex app-server runtime and `/codex` chat controls. |
| `agentRuntime.id: codex` | Agent runtime | Force the native Codex app-server harness for embedded turns. |
| `/codex ...` | Chat command set | Bind/control Codex app-server threads from a conversation. |
| `runtime: "acp", agentId: "codex"` | ACP session route | Explicit fallback path that runs Codex through ACP/acpx. |

This means a config can intentionally contain both `openai-codex/*` and the
`codex` plugin. That is valid when you want Codex OAuth through PI and also want
native `/codex` chat controls available. `openclaw doctor` warns about that
combination so you can confirm it is intentional; it does not rewrite it.

GPT-5.5 is available through both direct OpenAI Platform API-key access and
subscription/OAuth routes. For ChatGPT/Codex subscription plus native Codex
execution, use `openai/gpt-5.5` with `agentRuntime.id: "codex"`. Use
`openai-codex/gpt-5.5` only for Codex OAuth through PI, or `openai/gpt-5.5`
without a Codex runtime override for direct `OPENAI_API_KEY` traffic.

Enabling the OpenAI plugin, or selecting an `openai-codex/*` model, does not
enable the bundled Codex app-server plugin. OpenClaw enables that plugin only
when you explicitly select the native Codex harness with
`agentRuntime.id: "codex"` or use a legacy `codex/*` model ref.
If the bundled `codex` plugin is enabled but `openai-codex/*` still resolves
through PI, `openclaw doctor` warns and leaves the route unchanged.

## OpenClaw feature coverage

| OpenAI capability | OpenClaw surface | Status |
| --- | --- | --- |
| Chat / Responses | `openai/<model>` model provider | Yes |
| Codex subscription models | `openai-codex/<model>` with `openai-codex` OAuth | Yes |
| Codex app-server harness | `openai/<model>` with `agentRuntime.id: codex` | Yes |
| Server-side web search | Native OpenAI Responses tool | Yes, when web search is enabled and no provider pinned |
| Images | `image_generate` | Yes |
| Videos | `video_generate` | Yes |
| Text-to-speech | `messages.tts.provider: "openai"` / `tts` | Yes |
| Batch speech-to-text | `tools.media.audio` / media understanding | Yes |
| Streaming speech-to-text | Voice Call `streaming.provider: "openai"` | Yes |
| Realtime voice | Voice Call `realtime.provider: "openai"` / Control UI Talk | Yes |
| Embeddings | memory embedding provider | Yes |

## Memory embeddings

OpenClaw can use OpenAI, or an OpenAI-compatible embedding endpoint, for
`memory_search` indexing and query embeddings:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        model: "text-embedding-3-small",
      },
    },
  },
}
```

For OpenAI-compatible endpoints that require asymmetric embedding labels, set
`queryInputType` and `documentInputType` under `memorySearch`. OpenClaw forwards
those as provider-specific `input_type` request fields: query embeddings use
`queryInputType`; indexed memory chunks and batch indexing use
`documentInputType`. See the [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config#provider-specific-config) for the full example.

## Getting started

Choose your preferred auth method and follow the setup steps.

- API key (OpenAI Platform)

- Codex subscription

**Best for:** direct API access and usage-based billing.

1

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Get your API key

Create or copy an API key from the [OpenAI Platform dashboard](https://platform.openai.com/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Run onboarding

```
openclaw onboard --auth-choice openai-api-key
```

Or pass the key directly:

```
openclaw onboard --openai-api-key "$OPENAI_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Verify the model is available

```
openclaw models list --provider openai
```

### Route summary

| Model ref | Runtime config | Route | Auth |
| --- | --- | --- | --- |
| `openai/gpt-5.5` | omitted / `agentRuntime.id: "pi"` | Direct OpenAI Platform API | `OPENAI_API_KEY` |
| `openai/gpt-5.4-mini` | omitted / `agentRuntime.id: "pi"` | Direct OpenAI Platform API | `OPENAI_API_KEY` |
| `openai/gpt-5.5` | `agentRuntime.id: "codex"` | Codex app-server harness | Codex app-server |

`openai/*` is the direct OpenAI API-key route unless you explicitly force
the Codex app-server harness. Use `openai-codex/*` for Codex OAuth through
the default PI runner, or use `openai/gpt-5.5` with
`agentRuntime.id: "codex"` for native Codex app-server execution.

### Config example

```
{
  env: { OPENAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "openai/gpt-5.5" } } },
}
```

OpenClaw does **not** expose `openai/gpt-5.3-codex-spark`. Live OpenAI API requests reject that model, and the current Codex catalog does not expose it either.

**Best for:** using your ChatGPT/Codex subscription with native Codex app-server execution instead of a separate API key. Codex cloud requires ChatGPT sign-in.

1

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Run Codex OAuth

```
openclaw onboard --auth-choice openai-codex
```

Or run OAuth directly:

```
openclaw models auth login --provider openai-codex
```

For headless or callback-hostile setups, add `--device-code` to sign in with a ChatGPT device-code flow instead of the localhost browser callback:

```
openclaw models auth login --provider openai-codex --device-code
```

2

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Use the native Codex runtime

```
openclaw config set plugins.entries.codex '{"enabled":true}' --strict-json --merge
openclaw config set agents.defaults.model.primary openai/gpt-5.5
openclaw config set agents.defaults.agentRuntime '{"id":"codex"}' --strict-json
```

3

[Navigate to header](https://docs.openclaw.ai/providers/openai#)

Verify Codex auth is available

```
openclaw models list --provider openai-codex
```

After the gateway is running, send `/codex status` or `/codex models`
in chat to verify the native app-server runtime.

### Route summary

| Model ref | Runtime config | Route | Auth |
| --- | --- | --- | --- |
| `openai/gpt-5.5` | `agentRuntime.id: "codex"` | Native Codex app-server harness | Codex sign-in or selected `openai-codex` profile |
| `openai-codex/gpt-5.5` | omitted / `runtime: "pi"` | ChatGPT/Codex OAuth through PI | Codex sign-in |
| `openai-codex/gpt-5.4-mini` | omitted / `runtime: "pi"` | ChatGPT/Codex OAuth through PI | Codex sign-in |
| `openai-codex/gpt-5.5` | `runtime: "auto"` | Still PI unless a plugin explicitly claims `openai-codex` | Codex sign-in |

Keep using the `openai-codex` provider id for auth/profile commands. The
`openai-codex/*` model prefix is also the explicit PI route for Codex OAuth.
It does not select or auto-enable the bundled Codex app-server harness. For
the common subscription plus native runtime setup, sign in with
`openai-codex` but keep the model ref as `openai/gpt-5.5` and set
`agentRuntime.id: "codex"`.

### Config example

```
{
  plugins: { entries: { codex: { enabled: true } } },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      agentRuntime: { id: "codex" },
    },
  },
}
```

To keep Codex OAuth on the normal PI runner instead, use
`openai-codex/gpt-5.5` and omit the Codex runtime override.

Onboarding no longer imports OAuth material from `~/.codex`. Sign in with browser OAuth (default) or the device-code flow above — OpenClaw manages the resulting credentials in its own agent auth store.

### Status indicator

Chat `/status` shows which model runtime is active for the current session.
The default PI harness appears as `Runtime: OpenClaw Pi Default`. When the
bundled Codex app-server harness is selected, `/status` shows
`Runtime: OpenAI Codex`. Existing sessions keep their recorded harness id, so use
`/new` or `/reset` after changing `agentRuntime` if you want `/status` to
reflect a new PI/Codex choice.

### Doctor warning

If the bundled `codex` plugin is enabled while an `openai-codex/*` route is
selected, `openclaw doctor` warns that the model still resolves through PI.
Keep the config unchanged only when that PI subscription-auth route is
intentional. Switch to `openai/<model>` plus `agentRuntime.id: "codex"` when
you want native Codex app-server execution.

### Context window cap

OpenClaw treats model metadata and the runtime context cap as separate values.For `openai-codex/gpt-5.5` through Codex OAuth:

- Native `contextWindow`: `1000000`
- Default runtime `contextTokens` cap: `272000`

The smaller default cap has better latency and quality characteristics in practice. Override it with `contextTokens`:

```
{
  models: {
    providers: {
      "openai-codex": {
        models: [{ id: "gpt-5.5", contextTokens: 160000 }],
      },
    },
  },
}
```

Use `contextWindow` to declare native model metadata. Use `contextTokens` to limit the runtime context budget.

### Catalog recovery

OpenClaw uses upstream Codex catalog metadata for `gpt-5.5` when it is
present. If live Codex discovery omits the `openai-codex/gpt-5.5` row while
the account is authenticated, OpenClaw synthesizes that OAuth model row so
cron, sub-agent, and configured default-model runs do not fail with
`Unknown model`.

## Native Codex app-server auth

The native Codex app-server harness uses `openai/*` model refs plus
`agentRuntime.id: "codex"`, but its auth is still account-based. OpenClaw
selects auth in this order:

1. An explicit OpenClaw `openai-codex` auth profile bound to the agent.
2. The app-server’s existing account, such as a local Codex CLI ChatGPT sign-in.
3. For local stdio app-server launches only, `CODEX_API_KEY`, then
`OPENAI_API_KEY`, when the app-server reports no account and still requires
OpenAI auth.

That means a local ChatGPT/Codex subscription sign-in is not replaced just
because the gateway process also has `OPENAI_API_KEY` for direct OpenAI models
or embeddings. Env API-key fallback is only the local stdio no-account path; it
is not sent to WebSocket app-server connections. When a subscription-style Codex
profile is selected, OpenClaw also keeps `CODEX_API_KEY` and `OPENAI_API_KEY`
out of the spawned stdio app-server child and sends the selected credentials
through the app-server login RPC.

## Image generation

The bundled `openai` plugin registers image generation through the `image_generate` tool.
It supports both OpenAI API-key image generation and Codex OAuth image
generation through the same `openai/gpt-image-2` model ref.

| Capability | OpenAI API key | Codex OAuth |
| --- | --- | --- |
| Model ref | `openai/gpt-image-2` | `openai/gpt-image-2` |
| Auth | `OPENAI_API_KEY` | OpenAI Codex OAuth sign-in |
| Transport | OpenAI Images API | Codex Responses backend |
| Max images per request | 4 | 4 |
| Edit mode | Enabled (up to 5 reference images) | Enabled (up to 5 reference images) |
| Size overrides | Supported, including 2K/4K sizes | Supported, including 2K/4K sizes |
| Aspect ratio / resolution | Not forwarded to OpenAI Images API | Mapped to a supported size when safe |

```
{
  agents: {
    defaults: {
      imageGenerationModel: { primary: "openai/gpt-image-2" },
    },
  },
}
```

See [Image Generation](https://docs.openclaw.ai/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

`gpt-image-2` is the default for both OpenAI text-to-image generation and image
editing. `gpt-image-1.5`, `gpt-image-1`, and `gpt-image-1-mini` remain usable as
explicit model overrides. Use `openai/gpt-image-1.5` for transparent-background
PNG/WebP output; the current `gpt-image-2` API rejects
`background: "transparent"`.For a transparent-background request, agents should call `image_generate` with
`model: "openai/gpt-image-1.5"`, `outputFormat: "png"` or `"webp"`, and
`background: "transparent"`; the older `openai.background` provider option is
still accepted. OpenClaw also protects the public OpenAI and
OpenAI Codex OAuth routes by rewriting default `openai/gpt-image-2` transparent
requests to `gpt-image-1.5`; Azure and custom OpenAI-compatible endpoints keep
their configured deployment/model names.The same setting is exposed for headless CLI runs:

```
openclaw infer image generate \
  --model openai/gpt-image-1.5 \
  --output-format png \
  --background transparent \
  --prompt "A simple red circle sticker on a transparent background" \
  --json
```

Use the same `--output-format` and `--background` flags with
`openclaw infer image edit` when starting from an input file.
`--openai-background` remains available as an OpenAI-specific alias.For Codex OAuth installs, keep the same `openai/gpt-image-2` ref. When an
`openai-codex` OAuth profile is configured, OpenClaw resolves that stored OAuth
access token and sends image requests through the Codex Responses backend. It
does not first try `OPENAI_API_KEY` or silently fall back to an API key for that
request. Configure `models.providers.openai` explicitly with an API key,
custom base URL, or Azure endpoint when you want the direct OpenAI Images API
route instead.
If that custom image endpoint is on a trusted LAN/private address, also set
`browser.ssrfPolicy.dangerouslyAllowPrivateNetwork: true`; OpenClaw keeps
private/internal OpenAI-compatible image endpoints blocked unless this opt-in is
present.Generate:

```
/tool image_generate model=openai/gpt-image-2 prompt="A polished launch poster for OpenClaw on macOS" size=3840x2160 count=1
```

Generate a transparent PNG:

```
/tool image_generate model=openai/gpt-image-1.5 prompt="A simple red circle sticker on a transparent background" outputFormat=png background=transparent
```

Edit:

```
/tool image_generate model=openai/gpt-image-2 prompt="Preserve the object shape, change the material to translucent glass" image=/path/to/reference.png size=1024x1536
```

## Video generation

The bundled `openai` plugin registers video generation through the `video_generate` tool.

| Capability | Value |
| --- | --- |
| Default model | `openai/sora-2` |
| Modes | Text-to-video, image-to-video, single-video edit |
| Reference inputs | 1 image or 1 video |
| Size overrides | Supported |
| Other overrides | `aspectRatio`, `resolution`, `audio`, `watermark` are ignored with a tool warning |

```
{
  agents: {
    defaults: {
      videoGenerationModel: { primary: "openai/sora-2" },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

## GPT-5 prompt contribution

OpenClaw adds a shared GPT-5 prompt contribution for GPT-5-family runs across providers. It applies by model id, so `openai-codex/gpt-5.5`, `openai/gpt-5.5`, `openrouter/openai/gpt-5.5`, `opencode/gpt-5.5`, and other compatible GPT-5 refs receive the same overlay. Older GPT-4.x models do not.The bundled native Codex harness uses the same GPT-5 behavior and heartbeat overlay through Codex app-server developer instructions, so `openai/gpt-5.x` sessions forced through `agentRuntime.id: "codex"` keep the same follow-through and proactive heartbeat guidance even though Codex owns the rest of the harness prompt.The GPT-5 contribution adds a tagged behavior contract for persona persistence, execution safety, tool discipline, output shape, completion checks, and verification. Channel-specific reply and silent-message behavior stays in the shared OpenClaw system prompt and outbound delivery policy. The GPT-5 guidance is always enabled for matching models. The friendly interaction-style layer is separate and configurable.

| Value | Effect |
| --- | --- |
| `"friendly"` (default) | Enable the friendly interaction-style layer |
| `"on"` | Alias for `"friendly"` |
| `"off"` | Disable only the friendly style layer |

- Config

- CLI

```
{
  agents: {
    defaults: {
      promptOverlays: {
        gpt5: { personality: "friendly" },
      },
    },
  },
}
```

```
openclaw config set agents.defaults.promptOverlays.gpt5.personality off
```

Values are case-insensitive at runtime, so `"Off"` and `"off"` both disable the friendly style layer.

Legacy `plugins.entries.openai.config.personality` is still read as a compatibility fallback when the shared `agents.defaults.promptOverlays.gpt5.personality` setting is not set.

## Voice and speech

Speech synthesis (TTS)

The bundled `openai` plugin registers speech synthesis for the `messages.tts` surface.

| Setting | Config path | Default |
| --- | --- | --- |
| Model | `messages.tts.providers.openai.model` | `gpt-4o-mini-tts` |
| Voice | `messages.tts.providers.openai.voice` | `coral` |
| Speed | `messages.tts.providers.openai.speed` | (unset) |
| Instructions | `messages.tts.providers.openai.instructions` | (unset, `gpt-4o-mini-tts` only) |
| Format | `messages.tts.providers.openai.responseFormat` | `opus` for voice notes, `mp3` for files |
| API key | `messages.tts.providers.openai.apiKey` | Falls back to `OPENAI_API_KEY` |
| Base URL | `messages.tts.providers.openai.baseUrl` | `https://api.openai.com/v1` |
| Extra body | `messages.tts.providers.openai.extraBody` / `extra_body` | (unset) |

Available models: `gpt-4o-mini-tts`, `tts-1`, `tts-1-hd`. Available voices: `alloy`, `ash`, `ballad`, `cedar`, `coral`, `echo`, `fable`, `juniper`, `marin`, `onyx`, `nova`, `sage`, `shimmer`, `verse`.`extraBody` is merged into `/audio/speech` request JSON after OpenClaw’s generated fields, so use it for OpenAI-compatible endpoints that require additional keys such as `lang`. Prototype keys are ignored.

```
{
  messages: {
    tts: {
      providers: {
        openai: { model: "gpt-4o-mini-tts", voice: "coral" },
      },
    },
  },
}
```

Set `OPENAI_TTS_BASE_URL` to override the TTS base URL without affecting the chat API endpoint.

Speech-to-text

The bundled `openai` plugin registers batch speech-to-text through
OpenClaw’s media-understanding transcription surface.

- Default model: `gpt-4o-transcribe`
- Endpoint: OpenAI REST `/v1/audio/transcriptions`
- Input path: multipart audio file upload
- Supported by OpenClaw wherever inbound audio transcription uses
`tools.media.audio`, including Discord voice-channel segments and channel
audio attachments

To force OpenAI for inbound audio transcription:

```
{
  tools: {
    media: {
      audio: {
        models: [\
          {\
            type: "provider",\
            provider: "openai",\
            model: "gpt-4o-transcribe",\
          },\
        ],
      },
    },
  },
}
```

Language and prompt hints are forwarded to OpenAI when supplied by the
shared audio media config or per-call transcription request.

Realtime transcription

The bundled `openai` plugin registers realtime transcription for the Voice Call plugin.

| Setting | Config path | Default |
| --- | --- | --- |
| Model | `plugins.entries.voice-call.config.streaming.providers.openai.model` | `gpt-4o-transcribe` |
| Language | `...openai.language` | (unset) |
| Prompt | `...openai.prompt` | (unset) |
| Silence duration | `...openai.silenceDurationMs` | `800` |
| VAD threshold | `...openai.vadThreshold` | `0.5` |
| API key | `...openai.apiKey` | Falls back to `OPENAI_API_KEY` |

Uses a WebSocket connection to `wss://api.openai.com/v1/realtime` with G.711 u-law (`g711_ulaw` / `audio/pcmu`) audio. This streaming provider is for Voice Call’s realtime transcription path; Discord voice currently records short segments and uses the batch `tools.media.audio` transcription path instead.

Realtime voice

The bundled `openai` plugin registers realtime voice for the Voice Call plugin.

| Setting | Config path | Default |
| --- | --- | --- |
| Model | `plugins.entries.voice-call.config.realtime.providers.openai.model` | `gpt-realtime-1.5` |
| Voice | `...openai.voice` | `alloy` |
| Temperature | `...openai.temperature` | `0.8` |
| VAD threshold | `...openai.vadThreshold` | `0.5` |
| Silence duration | `...openai.silenceDurationMs` | `500` |
| API key | `...openai.apiKey` | Falls back to `OPENAI_API_KEY` |

Supports Azure OpenAI via `azureEndpoint` and `azureDeployment` config keys for backend realtime bridges. Supports bidirectional tool calling. Uses G.711 u-law audio format.

Control UI Talk uses OpenAI browser realtime sessions with a Gateway-minted
ephemeral client secret and a direct browser WebRTC SDP exchange against the
OpenAI Realtime API. Maintainer live verification is available with
`OPENAI_API_KEY=... GEMINI_API_KEY=... node --import tsx scripts/dev/realtime-talk-live-smoke.ts`;
the OpenAI leg mints a client secret in Node, generates a browser SDP offer
with fake microphone media, posts it to OpenAI, and applies the SDP answer
without logging secrets.

## Azure OpenAI endpoints

The bundled `openai` provider can target an Azure OpenAI resource for image
generation by overriding the base URL. On the image-generation path, OpenClaw
detects Azure hostnames on `models.providers.openai.baseUrl` and switches to
Azure’s request shape automatically.

Realtime voice uses a separate configuration path
(`plugins.entries.voice-call.config.realtime.providers.openai.azureEndpoint`)
and is not affected by `models.providers.openai.baseUrl`. See the **Realtime**
**voice** accordion under [Voice and speech](https://docs.openclaw.ai/providers/openai#voice-and-speech) for its Azure
settings.

Use Azure OpenAI when:

- You already have an Azure OpenAI subscription, quota, or enterprise agreement
- You need regional data residency or compliance controls Azure provides
- You want to keep traffic inside an existing Azure tenancy

### Configuration

For Azure image generation through the bundled `openai` provider, point
`models.providers.openai.baseUrl` at your Azure resource and set `apiKey` to
the Azure OpenAI key (not an OpenAI Platform key):

```
{
  models: {
    providers: {
      openai: {
        baseUrl: "https://<your-resource>.openai.azure.com",
        apiKey: "<azure-openai-api-key>",
      },
    },
  },
}
```

OpenClaw recognizes these Azure host suffixes for the Azure image-generation
route:

- `*.openai.azure.com`
- `*.services.ai.azure.com`
- `*.cognitiveservices.azure.com`

For image-generation requests on a recognized Azure host, OpenClaw:

- Sends the `api-key` header instead of `Authorization: Bearer`
- Uses deployment-scoped paths (`/openai/deployments/{deployment}/...`)
- Appends `?api-version=...` to each request
- Uses a 600s default request timeout for Azure image-generation calls.
Per-call `timeoutMs` values still override this default.

Other base URLs (public OpenAI, OpenAI-compatible proxies) keep the standard
OpenAI image request shape.

Azure routing for the `openai` provider’s image-generation path requires
OpenClaw 2026.4.22 or later. Earlier versions treat any custom
`openai.baseUrl` like the public OpenAI endpoint and will fail against Azure
image deployments.

### API version

Set `AZURE_OPENAI_API_VERSION` to pin a specific Azure preview or GA version
for the Azure image-generation path:

```
export AZURE_OPENAI_API_VERSION="2024-12-01-preview"
```

The default is `2024-12-01-preview` when the variable is unset.

### Model names are deployment names

Azure OpenAI binds models to deployments. For Azure image-generation requests
routed through the bundled `openai` provider, the `model` field in OpenClaw
must be the **Azure deployment name** you configured in the Azure portal, not
the public OpenAI model id.If you create a deployment called `gpt-image-2-prod` that serves `gpt-image-2`:

```
/tool image_generate model=openai/gpt-image-2-prod prompt="A clean poster" size=1024x1024 count=1
```

The same deployment-name rule applies to image-generation calls routed through
the bundled `openai` provider.

### Regional availability

Azure image generation is currently available only in a subset of regions
(for example `eastus2`, `swedencentral`, `polandcentral`, `westus3`,
`uaenorth`). Check Microsoft’s current region list before creating a
deployment, and confirm the specific model is offered in your region.

### Parameter differences

Azure OpenAI and public OpenAI do not always accept the same image parameters.
Azure may reject options that public OpenAI allows (for example certain
`background` values on `gpt-image-2`) or expose them only on specific model
versions. These differences come from Azure and the underlying model, not
OpenClaw. If an Azure request fails with a validation error, check the
parameter set supported by your specific deployment and API version in the
Azure portal.

Azure OpenAI uses native transport and compat behavior but does not receive
OpenClaw’s hidden attribution headers — see the **Native vs OpenAI-compatible**
**routes** accordion under [Advanced configuration](https://docs.openclaw.ai/providers/openai#advanced-configuration).For chat or Responses traffic on Azure (beyond image generation), use the
onboarding flow or a dedicated Azure provider config — `openai.baseUrl` alone
does not pick up the Azure API/auth shape. A separate
`azure-openai-responses/*` provider exists; see
the Server-side compaction accordion below.

## Advanced configuration

Transport (WebSocket vs SSE)

OpenClaw uses WebSocket-first with SSE fallback (`"auto"`) for both `openai/*` and `openai-codex/*`.In `"auto"` mode, OpenClaw:

- Retries one early WebSocket failure before falling back to SSE
- After a failure, marks WebSocket as degraded for ~60 seconds and uses SSE during cool-down
- Attaches stable session and turn identity headers for retries and reconnects
- Normalizes usage counters (`input_tokens` / `prompt_tokens`) across transport variants

| Value | Behavior |
| --- | --- |
| `"auto"` (default) | WebSocket first, SSE fallback |
| `"sse"` | Force SSE only |
| `"websocket"` | Force WebSocket only |

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": {
          params: { transport: "auto" },
        },
        "openai-codex/gpt-5.5": {
          params: { transport: "auto" },
        },
      },
    },
  },
}
```

Related OpenAI docs:

- [Realtime API with WebSocket](https://platform.openai.com/docs/guides/realtime-websocket)
- [Streaming API responses (SSE)](https://platform.openai.com/docs/guides/streaming-responses)

WebSocket warm-up

OpenClaw enables WebSocket warm-up by default for `openai/*` and `openai-codex/*` to reduce first-turn latency.

```
// Disable warm-up
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": {
          params: { openaiWsWarmup: false },
        },
      },
    },
  },
}
```

Fast mode

OpenClaw exposes a shared fast-mode toggle for `openai/*` and `openai-codex/*`:

- **Chat/UI:**`/fast status|on|off`
- **Config:**`agents.defaults.models["<provider>/<model>"].params.fastMode`

When enabled, OpenClaw maps fast mode to OpenAI priority processing (`service_tier = "priority"`). Existing `service_tier` values are preserved, and fast mode does not rewrite `reasoning` or `text.verbosity`.

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": { params: { fastMode: true } },
      },
    },
  },
}
```

Session overrides win over config. Clearing the session override in the Sessions UI returns the session to the configured default.

Priority processing (service\_tier)

OpenAI’s API exposes priority processing via `service_tier`. Set it per model in OpenClaw:

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": { params: { serviceTier: "priority" } },
      },
    },
  },
}
```

Supported values: `auto`, `default`, `flex`, `priority`.

`serviceTier` is only forwarded to native OpenAI endpoints (`api.openai.com`) and native Codex endpoints (`chatgpt.com/backend-api`). If you route either provider through a proxy, OpenClaw leaves `service_tier` untouched.

Server-side compaction (Responses API)

For direct OpenAI Responses models (`openai/*` on `api.openai.com`), the OpenAI plugin’s Pi-harness stream wrapper auto-enables server-side compaction:

- Forces `store: true` (unless model compat sets `supportsStore: false`)
- Injects `context_management: [{ type: "compaction", compact_threshold: ... }]`
- Default `compact_threshold`: 70% of `contextWindow` (or `80000` when unavailable)

This applies to the built-in Pi harness path and to OpenAI provider hooks used by embedded runs. The native Codex app-server harness manages its own context through Codex and is configured separately with `agents.defaults.agentRuntime.id`.

- Enable explicitly

- Custom threshold

- Disable

Useful for compatible endpoints like Azure OpenAI Responses:

```
{
  agents: {
    defaults: {
      models: {
        "azure-openai-responses/gpt-5.5": {
          params: { responsesServerCompaction: true },
        },
      },
    },
  },
}
```

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": {
          params: {
            responsesServerCompaction: true,
            responsesCompactThreshold: 120000,
          },
        },
      },
    },
  },
}
```

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": {
          params: { responsesServerCompaction: false },
        },
      },
    },
  },
}
```

`responsesServerCompaction` only controls `context_management` injection. Direct OpenAI Responses models still force `store: true` unless compat sets `supportsStore: false`.

Strict-agentic GPT mode

For GPT-5-family runs on `openai/*`, OpenClaw can use a stricter embedded execution contract:

```
{
  agents: {
    defaults: {
      embeddedPi: { executionContract: "strict-agentic" },
    },
  },
}
```

With `strict-agentic`, OpenClaw:

- No longer treats a plan-only turn as successful progress when a tool action is available
- Retries the turn with an act-now steer
- Auto-enables `update_plan` for substantial work
- Surfaces an explicit blocked state if the model keeps planning without acting

Scoped to OpenAI and Codex GPT-5-family runs only. Other providers and older model families keep default behavior.

Native vs OpenAI-compatible routes

OpenClaw treats direct OpenAI, Codex, and Azure OpenAI endpoints differently from generic OpenAI-compatible `/v1` proxies:**Native routes** (`openai/*`, Azure OpenAI):

- Keep `reasoning: { effort: "none" }` only for models that support the OpenAI `none` effort
- Omit disabled reasoning for models or proxies that reject `reasoning.effort: "none"`
- Default tool schemas to strict mode
- Attach hidden attribution headers on verified native hosts only
- Keep OpenAI-only request shaping (`service_tier`, `store`, reasoning-compat, prompt-cache hints)

**Proxy/compatible routes:**

- Use looser compat behavior
- Strip Completions `store` from non-native `openai-completions` payloads
- Accept advanced `params.extra_body`/`params.extraBody` pass-through JSON for OpenAI-compatible Completions proxies
- Accept `params.chat_template_kwargs` for OpenAI-compatible Completions proxies such as vLLM
- Do not force strict tool schemas or native-only headers

Azure OpenAI uses native transport and compat behavior but does not receive the hidden attribution headers.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Image generation** \\
\\
Shared image tool parameters and provider selection.](https://docs.openclaw.ai/tools/image-generation)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**OAuth and auth** \\
\\
Auth details and credential reuse rules.](https://docs.openclaw.ai/gateway/authentication)

[Ollama](https://docs.openclaw.ai/providers/ollama) [OpenCode](https://docs.openclaw.ai/providers/opencode)

Ctrl+I

---

## OpenCode - OpenClaw

_Source: <https://docs.openclaw.ai/providers/opencode>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenCode

OpenCode exposes two hosted catalogs in OpenClaw:

| Catalog | Prefix | Runtime provider |
| --- | --- | --- |
| **Zen** | `opencode/...` | `opencode` |
| **Go** | `opencode-go/...` | `opencode-go` |

Both catalogs use the same OpenCode API key. OpenClaw keeps the runtime provider ids
split so upstream per-model routing stays correct, but onboarding and docs treat them
as one OpenCode setup.

## Getting started

- Zen catalog

- Go catalog

**Best for:** the curated OpenCode multi-model proxy (Claude, GPT, Gemini).

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-zen
```

Or pass the key directly:

```
openclaw onboard --opencode-zen-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Zen model as the default

```
openclaw config set agents.defaults.model.primary "opencode/claude-opus-4-6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode
```

**Best for:** the OpenCode-hosted Kimi, GLM, and MiniMax lineup.

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

Or pass the key directly:

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Go model as the default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## Config example

```
{
  env: { OPENCODE_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "opencode/claude-opus-4-6" } } },
}
```

## Built-in catalogs

### Zen

| Property | Value |
| --- | --- |
| Runtime provider | `opencode` |
| Example models | `opencode/claude-opus-4-6`, `opencode/gpt-5.5`, `opencode/gemini-3-pro` |

### Go

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Example models | `opencode-go/kimi-k2.6`, `opencode-go/glm-5`, `opencode-go/minimax-m2.5` |

## Advanced configuration

API key aliases

`OPENCODE_ZEN_API_KEY` is also supported as an alias for `OPENCODE_API_KEY`.

Shared credentials

Entering one OpenCode key during setup stores credentials for both runtime
providers. You do not need to onboard each catalog separately.

Billing and dashboard

You sign in to OpenCode, add billing details, and copy your API key. Billing
and catalog availability are managed from the OpenCode dashboard.

Gemini replay behavior

Gemini-backed OpenCode refs stay on the proxy-Gemini path, so OpenClaw keeps
Gemini thought-signature sanitation there without enabling native Gemini
replay validation or bootstrap rewrites.

Non-Gemini replay behavior

Non-Gemini OpenCode refs keep the minimal OpenAI-compatible replay policy.

Entering one OpenCode key during setup stores credentials for both the Zen and
Go runtime providers, so you only need to onboard once.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[OpenAI](https://docs.openclaw.ai/providers/openai) [OpenCode Go](https://docs.openclaw.ai/providers/opencode-go)

Ctrl+I

---

## OpenCode Go - OpenClaw

_Source: <https://docs.openclaw.ai/providers/opencode-go>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenCode Go

OpenCode Go is the Go catalog within [OpenCode](https://docs.openclaw.ai/providers/opencode).
It uses the same `OPENCODE_API_KEY` as the Zen catalog, but keeps the runtime
provider id `opencode-go` so upstream per-model routing stays correct.

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Auth | `OPENCODE_API_KEY` |
| Parent setup | [OpenCode](https://docs.openclaw.ai/providers/opencode) |

## Built-in catalog

OpenClaw sources most Go catalog rows from the bundled pi model registry and
supplements current upstream rows while the registry catches up. Run
`openclaw models list --provider opencode-go` for the current model list.The provider includes:

| Model ref | Name |
| --- | --- |
| `opencode-go/glm-5` | GLM-5 |
| `opencode-go/glm-5.1` | GLM-5.1 |
| `opencode-go/kimi-k2.5` | Kimi K2.5 |
| `opencode-go/kimi-k2.6` | Kimi K2.6 (3x limits) |
| `opencode-go/deepseek-v4-pro` | DeepSeek V4 Pro |
| `opencode-go/deepseek-v4-flash` | DeepSeek V4 Flash |
| `opencode-go/mimo-v2-omni` | MiMo V2 Omni |
| `opencode-go/mimo-v2-pro` | MiMo V2 Pro |
| `opencode-go/minimax-m2.5` | MiniMax M2.5 |
| `opencode-go/minimax-m2.7` | MiniMax M2.7 |
| `opencode-go/qwen3.5-plus` | Qwen3.5 Plus |
| `opencode-go/qwen3.6-plus` | Qwen3.6 Plus |

## Getting started

- Interactive

- Non-interactive

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Set a Go model as default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Pass the key directly

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## Config example

```
{
  env: { OPENCODE_API_KEY: "YOUR_API_KEY_HERE" }, // pragma: allowlist secret
  agents: { defaults: { model: { primary: "opencode-go/kimi-k2.6" } } },
}
```

## Advanced configuration

Routing behavior

OpenClaw handles per-model routing automatically when the model ref uses
`opencode-go/...`. No additional provider config is required.

Runtime ref convention

Runtime refs stay explicit: `opencode/...` for Zen, `opencode-go/...` for Go.
This keeps upstream per-model routing correct across both catalogs.

Shared credentials

The same `OPENCODE_API_KEY` is used by both the Zen and Go catalogs. Entering
the key during setup stores credentials for both runtime providers.

See [OpenCode](https://docs.openclaw.ai/providers/opencode) for the shared onboarding overview and the full
Zen + Go catalog reference.

## Related

[**OpenCode (parent)** \\
\\
Shared onboarding, catalog overview, and advanced notes.](https://docs.openclaw.ai/providers/opencode)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[OpenCode](https://docs.openclaw.ai/providers/opencode) [OpenRouter](https://docs.openclaw.ai/providers/openrouter)

Ctrl+I

---

## OpenRouter - OpenClaw

_Source: <https://docs.openclaw.ai/providers/openrouter>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenRouter

OpenRouter provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Get your API key

Create an API key at [openrouter.ai/keys](https://openrouter.ai/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Run onboarding

```
openclaw onboard --auth-choice openrouter-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

(Optional) Switch to a specific model

Onboarding defaults to `openrouter/auto`. Pick a concrete model later:

```
openclaw models set openrouter/<provider>/<model>
```

## Config example

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      model: { primary: "openrouter/auto" },
    },
  },
}
```

## Model references

Model refs follow the pattern `openrouter/<provider>/<model>`. For the full list of
available providers and models, see [/concepts/model-providers](https://docs.openclaw.ai/concepts/model-providers).

Bundled fallback examples:

| Model ref | Notes |
| --- | --- |
| `openrouter/auto` | OpenRouter automatic routing |
| `openrouter/moonshotai/kimi-k2.6` | Kimi K2.6 via MoonshotAI |

## Image generation

OpenRouter can also back the `image_generate` tool. Use an OpenRouter image model under `agents.defaults.imageGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openrouter/google/gemini-3.1-flash-image-preview",
        timeoutMs: 180_000,
      },
    },
  },
}
```

OpenClaw sends image requests to OpenRouter’s chat completions image API with `modalities: ["image", "text"]`. Gemini image models receive supported `aspectRatio` and `resolution` hints through OpenRouter’s `image_config`. Use `agents.defaults.imageGenerationModel.timeoutMs` for slower OpenRouter image models; the `image_generate` tool’s per-call `timeoutMs` parameter still wins.

## Video generation

OpenRouter can also back the `video_generate` tool through its asynchronous `/videos` API. Use an OpenRouter video model under `agents.defaults.videoGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "openrouter/google/veo-3.1-fast",
      },
    },
  },
}
```

OpenClaw submits text-to-video and image-to-video jobs to OpenRouter, polls
the returned `polling_url`, and downloads the completed video from
OpenRouter’s `unsigned_urls` or the documented job content endpoint.
Reference images are sent as first/last frame images by default; images
tagged with `reference_image` are sent as OpenRouter input references. The
bundled `google/veo-3.1-fast` default advertises the currently supported 4/6/8
second durations, `720P`/`1080P` resolutions, and `16:9`/`9:16` aspect
ratios. Video-to-video is not registered for OpenRouter because the upstream
video generation API currently accepts text and image references.

## Text-to-speech

OpenRouter can also be used as a TTS provider through its OpenAI-compatible
`/audio/speech` endpoint.

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openrouter",
      providers: {
        openrouter: {
          model: "hexgrad/kokoro-82m",
          voice: "af_alloy",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

If `messages.tts.providers.openrouter.apiKey` is omitted, TTS reuses
`models.providers.openrouter.apiKey`, then `OPENROUTER_API_KEY`.

## Authentication and headers

OpenRouter uses a Bearer token with your API key under the hood.On real OpenRouter requests (`https://openrouter.ai/api/v1`), OpenClaw also adds
OpenRouter’s documented app-attribution headers:

| Header | Value |
| --- | --- |
| `HTTP-Referer` | `https://openclaw.ai` |
| `X-OpenRouter-Title` | `OpenClaw` |
| `X-OpenRouter-Categories` | `cli-agent` |

If you repoint the OpenRouter provider at some other proxy or base URL, OpenClaw
does **not** inject those OpenRouter-specific headers or Anthropic cache markers.

## Advanced configuration

Anthropic cache markers

On verified OpenRouter routes, Anthropic model refs keep the
OpenRouter-specific Anthropic `cache_control` markers that OpenClaw uses for
better prompt-cache reuse on system/developer prompt blocks.

Anthropic reasoning prefill

On verified OpenRouter routes, Anthropic model refs with reasoning enabled
drop trailing assistant prefill turns before the request reaches OpenRouter,
matching Anthropic’s requirement that reasoning conversations end with a user
turn.

Thinking / reasoning injection

On supported non-`auto` routes, OpenClaw maps the selected thinking level to
OpenRouter proxy reasoning payloads. Unsupported model hints and
`openrouter/auto` skip that reasoning injection. Hunter Alpha also skips
proxy reasoning for stale configured model refs because OpenRouter could
return final answer text in reasoning fields for that retired route.

DeepSeek V4 reasoning replay

On verified OpenRouter routes, `openrouter/deepseek/deepseek-v4-flash` and
`openrouter/deepseek/deepseek-v4-pro` fill missing `reasoning_content` on
replayed assistant turns so thinking/tool conversations keep DeepSeek V4’s
required follow-up shape.

OpenAI-only request shaping

OpenRouter still runs through the proxy-style OpenAI-compatible path, so
native OpenAI-only request shaping such as `serviceTier`, Responses `store`,
OpenAI reasoning-compat payloads, and prompt-cache hints is not forwarded.

Gemini-backed routes

Gemini-backed OpenRouter refs stay on the proxy-Gemini path: OpenClaw keeps
Gemini thought-signature sanitation there, but does not enable native Gemini
replay validation or bootstrap rewrites.

Provider routing metadata

If you pass OpenRouter provider routing under model params, OpenClaw forwards
it as OpenRouter routing metadata before the shared stream wrappers run.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[OpenCode Go](https://docs.openclaw.ai/providers/opencode-go) [Perplexity](https://docs.openclaw.ai/providers/perplexity-provider)

Ctrl+I

---

## Qianfan - OpenClaw

_Source: <https://docs.openclaw.ai/providers/qianfan>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Qianfan

Qianfan is Baidu’s MaaS platform, providing a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `qianfan` |
| Auth | `QIANFAN_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://qianfan.baidubce.com/v2` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Create a Baidu Cloud account

Sign up or log in at the [Qianfan Console](https://console.bce.baidu.com/qianfan/ais/console/apiKey) and ensure you have Qianfan API access enabled.

2

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Generate an API key

Create a new application or select an existing one, then generate an API key. The key format is `bce-v3/ALTAK-...`.

3

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Run onboarding

```
openclaw onboard --auth-choice qianfan-api-key
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Verify the model is available

```
openclaw models list --provider qianfan
```

## Built-in catalog

| Model ref | Input | Context | Max output | Reasoning | Notes |
| --- | --- | --- | --- | --- | --- |
| `qianfan/deepseek-v3.2` | text | 98,304 | 32,768 | Yes | Default model |
| `qianfan/ernie-5.0-thinking-preview` | text, image | 119,000 | 64,000 | Yes | Multimodal |

The default bundled model ref is `qianfan/deepseek-v3.2`. You only need to override `models.providers.qianfan` when you need a custom base URL or model metadata.

## Config example

```
{
  env: { QIANFAN_API_KEY: "bce-v3/ALTAK-..." },
  agents: {
    defaults: {
      model: { primary: "qianfan/deepseek-v3.2" },
      models: {
        "qianfan/deepseek-v3.2": { alias: "QIANFAN" },
      },
    },
  },
  models: {
    providers: {
      qianfan: {
        baseUrl: "https://qianfan.baidubce.com/v2",
        api: "openai-completions",
        models: [\
          {\
            id: "deepseek-v3.2",\
            name: "DEEPSEEK V3.2",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 98304,\
            maxTokens: 32768,\
          },\
          {\
            id: "ernie-5.0-thinking-preview",\
            name: "ERNIE-5.0-Thinking-Preview",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 119000,\
            maxTokens: 64000,\
          },\
        ],
      },
    },
  },
}
```

Transport and compatibility

Qianfan runs through the OpenAI-compatible transport path, not native OpenAI request shaping. This means standard OpenAI SDK features work, but provider-specific parameters may not be forwarded.

Catalog and overrides

The bundled catalog currently includes `deepseek-v3.2` and `ernie-5.0-thinking-preview`. Add or override `models.providers.qianfan` only when you need a custom base URL or model metadata.

Model refs use the `qianfan/` prefix (for example `qianfan/deepseek-v3.2`).

Troubleshooting

- Ensure your API key starts with `bce-v3/ALTAK-` and has Qianfan API access enabled in the Baidu Cloud console.
- If models are not listed, confirm your account has the Qianfan service activated.
- The default base URL is `https://qianfan.baidubce.com/v2`. Only change it if you use a custom endpoint or proxy.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Agent setup** \\
\\
Configuring agent defaults and model assignments.](https://docs.openclaw.ai/concepts/agent)

[**Qianfan API docs** \\
\\
Official Qianfan API documentation.](https://cloud.baidu.com/doc/qianfan-api/s/3m7of64lb)

[Perplexity](https://docs.openclaw.ai/providers/perplexity-provider) [Qwen](https://docs.openclaw.ai/providers/qwen)

Ctrl+I

---

## Qwen - OpenClaw

_Source: <https://docs.openclaw.ai/providers/qwen>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Qwen

**Qwen OAuth has been removed.** The free-tier OAuth integration
(`qwen-portal`) that used `portal.qwen.ai` endpoints is no longer available.
See [Issue #49557](https://github.com/openclaw/openclaw/issues/49557) for
background.

OpenClaw now treats Qwen as a first-class bundled provider with canonical id
`qwen`. The bundled provider targets the Qwen Cloud / Alibaba DashScope and
Coding Plan endpoints and keeps legacy `modelstudio` ids working as a
compatibility alias.

- Provider: `qwen`
- Preferred env var: `QWEN_API_KEY`
- Also accepted for compatibility: `MODELSTUDIO_API_KEY`, `DASHSCOPE_API_KEY`
- API style: OpenAI-compatible

If you want `qwen3.6-plus`, prefer the **Standard (pay-as-you-go)** endpoint.
Coding Plan support can lag behind the public catalog.

## Getting started

Choose your plan type and follow the setup steps.

- Coding Plan (subscription)

- Standard (pay-as-you-go)

**Best for:** subscription-based access through the Qwen Coding Plan.

1

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Get your API key

Create or copy an API key from [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Run onboarding

For the **Global** endpoint:

```
openclaw onboard --auth-choice qwen-api-key
```

For the **China** endpoint:

```
openclaw onboard --auth-choice qwen-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Verify the model is available

```
openclaw models list --provider qwen
```

Legacy `modelstudio-*` auth-choice ids and `modelstudio/...` model refs still
work as compatibility aliases, but new setup flows should prefer the canonical
`qwen-*` auth-choice ids and `qwen/...` model refs. If you define an exact
custom `models.providers.modelstudio` entry with another `api` value, that
custom provider owns `modelstudio/...` refs instead of the Qwen compatibility
alias.

**Best for:** pay-as-you-go access through the Standard Model Studio endpoint, including models like `qwen3.6-plus` that may not be available on the Coding Plan.

1

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Get your API key

Create or copy an API key from [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Run onboarding

For the **Global** endpoint:

```
openclaw onboard --auth-choice qwen-standard-api-key
```

For the **China** endpoint:

```
openclaw onboard --auth-choice qwen-standard-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Verify the model is available

```
openclaw models list --provider qwen
```

Legacy `modelstudio-*` auth-choice ids and `modelstudio/...` model refs still
work as compatibility aliases, but new setup flows should prefer the canonical
`qwen-*` auth-choice ids and `qwen/...` model refs. If you define an exact
custom `models.providers.modelstudio` entry with another `api` value, that
custom provider owns `modelstudio/...` refs instead of the Qwen compatibility
alias.

## Plan types and endpoints

| Plan | Region | Auth choice | Endpoint |
| --- | --- | --- | --- |
| Standard (pay-as-you-go) | China | `qwen-standard-api-key-cn` | `dashscope.aliyuncs.com/compatible-mode/v1` |
| Standard (pay-as-you-go) | Global | `qwen-standard-api-key` | `dashscope-intl.aliyuncs.com/compatible-mode/v1` |
| Coding Plan (subscription) | China | `qwen-api-key-cn` | `coding.dashscope.aliyuncs.com/v1` |
| Coding Plan (subscription) | Global | `qwen-api-key` | `coding-intl.dashscope.aliyuncs.com/v1` |

The provider auto-selects the endpoint based on your auth choice. Canonical
choices use the `qwen-*` family; `modelstudio-*` remains compatibility-only.
You can override with a custom `baseUrl` in config.

**Manage keys:** [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys) \|
**Docs:** [docs.qwencloud.com](https://docs.qwencloud.com/developer-guides/getting-started/introduction)

## Built-in catalog

OpenClaw currently ships this bundled Qwen catalog. The configured catalog is
endpoint-aware: Coding Plan configs omit models that are only known to work on
the Standard endpoint.

| Model ref | Input | Context | Notes |
| --- | --- | --- | --- |
| `qwen/qwen3.5-plus` | text, image | 1,000,000 | Default model |
| `qwen/qwen3.6-plus` | text, image | 1,000,000 | Prefer Standard endpoints when you need this model |
| `qwen/qwen3-max-2026-01-23` | text | 262,144 | Qwen Max line |
| `qwen/qwen3-coder-next` | text | 262,144 | Coding |
| `qwen/qwen3-coder-plus` | text | 1,000,000 | Coding |
| `qwen/MiniMax-M2.5` | text | 1,000,000 | Reasoning enabled |
| `qwen/glm-5` | text | 202,752 | GLM |
| `qwen/glm-4.7` | text | 202,752 | GLM |
| `qwen/kimi-k2.5` | text, image | 262,144 | Moonshot AI via Alibaba |

Availability can still vary by endpoint and billing plan even when a model is
present in the bundled catalog.

## Thinking Controls

For reasoning-enabled Qwen Cloud models, the bundled provider maps OpenClaw
thinking levels to DashScope’s top-level `enable_thinking` request flag. Disabled
thinking sends `enable_thinking: false`; other thinking levels send
`enable_thinking: true`.

## Multimodal add-ons

The `qwen` plugin also exposes multimodal capabilities on the **Standard**
DashScope endpoints (not the Coding Plan endpoints):

- **Video understanding** via `qwen-vl-max-latest`
- **Wan video generation** via `wan2.6-t2v` (default), `wan2.6-i2v`, `wan2.6-r2v`, `wan2.6-r2v-flash`, `wan2.7-r2v`

To use Qwen as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: { primary: "qwen/wan2.6-t2v" },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

## Advanced configuration

Image and video understanding

The bundled Qwen plugin registers media understanding for images and video
on the **Standard** DashScope endpoints (not the Coding Plan endpoints).

| Property | Value |
| --- | --- |
| Model | `qwen-vl-max-latest` |
| Supported input | Images, video |

Media understanding is auto-resolved from the configured Qwen auth — no
additional config is needed. Ensure you are using a Standard (pay-as-you-go)
endpoint for media understanding support.

Qwen 3.6 Plus availability

`qwen3.6-plus` is available on the Standard (pay-as-you-go) Model Studio
endpoints:

- China: `dashscope.aliyuncs.com/compatible-mode/v1`
- Global: `dashscope-intl.aliyuncs.com/compatible-mode/v1`

If the Coding Plan endpoints return an “unsupported model” error for
`qwen3.6-plus`, switch to Standard (pay-as-you-go) instead of the Coding Plan
endpoint/key pair.OpenClaw’s bundled Qwen catalog does not advertise `qwen3.6-plus` on Coding
Plan endpoints, but explicitly configured `qwen/qwen3.6-plus` entries under
`models.providers.qwen.models` are honored on Coding Plan baseUrls so you
can opt that model in if Aliyun enables it on your subscription. The
upstream API still decides whether the call succeeds.

Capability plan

The `qwen` plugin is being positioned as the vendor home for the full Qwen
Cloud surface, not just coding/text models.

- **Text/chat models:** bundled now
- **Tool calling, structured output, thinking:** inherited from the OpenAI-compatible transport
- **Image generation:** planned at the provider-plugin layer
- **Image/video understanding:** bundled now on the Standard endpoint
- **Speech/audio:** planned at the provider-plugin layer
- **Memory embeddings/reranking:** planned through the embedding adapter surface
- **Video generation:** bundled now through the shared video-generation capability

Video generation details

For video generation, OpenClaw maps the configured Qwen region to the matching
DashScope AIGC host before submitting the job:

- Global/Intl: `https://dashscope-intl.aliyuncs.com`
- China: `https://dashscope.aliyuncs.com`

That means a normal `models.providers.qwen.baseUrl` pointing at either the
Coding Plan or Standard Qwen hosts still keeps video generation on the correct
regional DashScope video endpoint.Current bundled Qwen video-generation limits:

- Up to **1** output video per request
- Up to **1** input image
- Up to **4** input videos
- Up to **10 seconds** duration
- Supports `size`, `aspectRatio`, `resolution`, `audio`, and `watermark`
- Reference image/video mode currently requires **remote http(s) URLs**. Local
file paths are rejected up front because the DashScope video endpoint does not
accept uploaded local buffers for those references.

Streaming usage compatibility

Native Model Studio endpoints advertise streaming usage compatibility on the
shared `openai-completions` transport. OpenClaw keys that off endpoint
capabilities now, so DashScope-compatible custom provider ids targeting the
same native hosts inherit the same streaming-usage behavior instead of
requiring the built-in `qwen` provider id specifically.Native-streaming usage compatibility applies to both the Coding Plan hosts and
the Standard DashScope-compatible hosts:

- `https://coding.dashscope.aliyuncs.com/v1`
- `https://coding-intl.dashscope.aliyuncs.com/v1`
- `https://dashscope.aliyuncs.com/compatible-mode/v1`
- `https://dashscope-intl.aliyuncs.com/compatible-mode/v1`

Multimodal endpoint regions

Multimodal surfaces (video understanding and Wan video generation) use the
**Standard** DashScope endpoints, not the Coding Plan endpoints:

- Global/Intl Standard base URL: `https://dashscope-intl.aliyuncs.com/compatible-mode/v1`
- China Standard base URL: `https://dashscope.aliyuncs.com/compatible-mode/v1`

Environment and daemon setup

If the Gateway runs as a daemon (launchd/systemd), make sure `QWEN_API_KEY` is
available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Alibaba (ModelStudio)** \\
\\
Legacy ModelStudio provider and migration notes.](https://docs.openclaw.ai/providers/alibaba)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Qianfan](https://docs.openclaw.ai/providers/qianfan) [Runway](https://docs.openclaw.ai/providers/runway)

Ctrl+I

---

## SenseAudio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/senseaudio>_

# SenseAudio

SenseAudio can transcribe inbound audio/voice-note attachments through
OpenClaw’s shared `tools.media.audio` pipeline. OpenClaw posts multipart audio
to the OpenAI-compatible transcription endpoint and injects the returned text
as `{{Transcript}}` plus an `[Audio]` block.

| Detail | Value |
| --- | --- |
| Website | [senseaudio.cn](https://senseaudio.cn/) |
| Docs | [senseaudio.cn/docs](https://senseaudio.cn/docs) |
| Auth | `SENSEAUDIO_API_KEY` |
| Default model | `senseaudio-asr-pro-1.5-260319` |
| Default URL | `https://api.senseaudio.cn/v1` |

## Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Set your API key

```
export SENSEAUDIO_API_KEY="..."
```

2

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "senseaudio", model: "senseaudio-asr-pro-1.5-260319" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Send a voice note

Send an audio message through any connected channel. OpenClaw uploads the
audio to SenseAudio and uses the transcript in the reply pipeline.

## Options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | SenseAudio ASR model id |
| `language` | `tools.media.audio.models[].language` | Optional language hint |
| `prompt` | `tools.media.audio.prompt` | Optional transcription prompt |
| `baseUrl` | `tools.media.audio.baseUrl` or model | Override the OpenAI-compatible base |
| `headers` | `tools.media.audio.request.headers` | Extra request headers |

SenseAudio is batch STT only in OpenClaw. Voice Call realtime transcription
continues to use providers with streaming STT support.

[Runway](https://docs.openclaw.ai/providers/runway) [SGLang](https://docs.openclaw.ai/providers/sglang)

Ctrl+I

---

## SGLang - OpenClaw

_Source: <https://docs.openclaw.ai/providers/sglang>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

SGLang

SGLang can serve open-source models via an **OpenAI-compatible** HTTP API.
OpenClaw can connect to SGLang using the `openai-completions` API.OpenClaw can also **auto-discover** available models from SGLang when you opt
in with `SGLANG_API_KEY` (any value works if your server does not enforce auth)
and you do not define an explicit `models.providers.sglang` entry.OpenClaw treats `sglang` as a local OpenAI-compatible provider that supports
streamed usage accounting, so status/context token counts can update from
`stream_options.include_usage` responses.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Start SGLang

Launch SGLang with an OpenAI-compatible server. Your base URL should expose
`/v1` endpoints (for example `/v1/models`, `/v1/chat/completions`). SGLang
commonly runs on:

- `http://127.0.0.1:30000/v1`

2

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Set an API key

Any value works if no auth is configured on your server:

```
export SGLANG_API_KEY="sglang-local"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Run onboarding or set a model directly

```
openclaw onboard
```

Or configure the model manually:

```
{
  agents: {
    defaults: {
      model: { primary: "sglang/your-model-id" },
    },
  },
}
```

## Model discovery (implicit provider)

When `SGLANG_API_KEY` is set (or an auth profile exists) and you **do not**
define `models.providers.sglang`, OpenClaw will query:

- `GET http://127.0.0.1:30000/v1/models`

and convert the returned IDs into model entries.

If you set `models.providers.sglang` explicitly, auto-discovery is skipped and
you must define models manually.

## Explicit configuration (manual models)

Use explicit config when:

- SGLang runs on a different host/port.
- You want to pin `contextWindow`/`maxTokens` values.
- Your server requires a real API key (or you want to control headers).

```
{
  models: {
    providers: {
      sglang: {
        baseUrl: "http://127.0.0.1:30000/v1",
        apiKey: "${SGLANG_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "your-model-id",\
            name: "Local SGLang Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

## Advanced configuration

Proxy-style behavior

SGLang is treated as a proxy-style OpenAI-compatible `/v1` backend, not a
native OpenAI endpoint.

| Behavior | SGLang |
| --- | --- |
| OpenAI-only request shaping | Not applied |
| `service_tier`, Responses `store`, prompt-cache hints | Not sent |
| Reasoning-compat payload shaping | Not applied |
| Hidden attribution headers (`originator`, `version`, `User-Agent`) | Not injected on custom SGLang base URLs |

Troubleshooting

**Server not reachable**Verify the server is running and responding:

```
curl http://127.0.0.1:30000/v1/models
```

**Auth errors**If requests fail with auth errors, set a real `SGLANG_API_KEY` that matches
your server configuration, or configure the provider explicitly under
`models.providers.sglang`.

If you run SGLang without authentication, any non-empty value for
`SGLANG_API_KEY` is sufficient to opt in to model discovery.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider entries.](https://docs.openclaw.ai/gateway/configuration-reference)

[SenseAudio](https://docs.openclaw.ai/providers/senseaudio) [StepFun](https://docs.openclaw.ai/providers/stepfun)

Ctrl+I

---

## Synthetic - OpenClaw

_Source: <https://docs.openclaw.ai/providers/synthetic>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Synthetic

[Synthetic](https://synthetic.new/) exposes Anthropic-compatible endpoints.
OpenClaw registers it as the `synthetic` provider and uses the Anthropic
Messages API.

| Property | Value |
| --- | --- |
| Provider | `synthetic` |
| Auth | `SYNTHETIC_API_KEY` |
| API | Anthropic Messages |
| Base URL | `https://api.synthetic.new/anthropic` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Get an API key

Obtain a `SYNTHETIC_API_KEY` from your Synthetic account, or let the
onboarding wizard prompt you for one.

2

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Run onboarding

```
openclaw onboard --auth-choice synthetic-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Verify the default model

After onboarding the default model is set to:

```
synthetic/hf:MiniMaxAI/MiniMax-M2.5
```

OpenClaw’s Anthropic client appends `/v1` to the base URL automatically, so use
`https://api.synthetic.new/anthropic` (not `/anthropic/v1`). If Synthetic
changes its base URL, override `models.providers.synthetic.baseUrl`.

## Config example

```
{
  env: { SYNTHETIC_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "synthetic/hf:MiniMaxAI/MiniMax-M2.5" },
      models: { "synthetic/hf:MiniMaxAI/MiniMax-M2.5": { alias: "MiniMax M2.5" } },
    },
  },
  models: {
    mode: "merge",
    providers: {
      synthetic: {
        baseUrl: "https://api.synthetic.new/anthropic",
        apiKey: "${SYNTHETIC_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "hf:MiniMaxAI/MiniMax-M2.5",\
            name: "MiniMax M2.5",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 192000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

## Built-in catalog

All Synthetic models use cost `0` (input/output/cache).

| Model ID | Context window | Max tokens | Reasoning | Input |
| --- | --- | --- | --- | --- |
| `hf:MiniMaxAI/MiniMax-M2.5` | 192,000 | 65,536 | no | text |
| `hf:moonshotai/Kimi-K2-Thinking` | 256,000 | 8,192 | yes | text |
| `hf:zai-org/GLM-4.7` | 198,000 | 128,000 | no | text |
| `hf:deepseek-ai/DeepSeek-R1-0528` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3-0324` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.1` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.1-Terminus` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.2` | 159,000 | 8,192 | no | text |
| `hf:meta-llama/Llama-3.3-70B-Instruct` | 128,000 | 8,192 | no | text |
| `hf:meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8` | 524,000 | 8,192 | no | text |
| `hf:moonshotai/Kimi-K2-Instruct-0905` | 256,000 | 8,192 | no | text |
| `hf:moonshotai/Kimi-K2.5` | 256,000 | 8,192 | yes | text + image |
| `hf:openai/gpt-oss-120b` | 128,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-235B-A22B-Instruct-2507` | 256,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-Coder-480B-A35B-Instruct` | 256,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-VL-235B-A22B-Instruct` | 250,000 | 8,192 | no | text + image |
| `hf:zai-org/GLM-4.5` | 128,000 | 128,000 | no | text |
| `hf:zai-org/GLM-4.6` | 198,000 | 128,000 | no | text |
| `hf:zai-org/GLM-5` | 256,000 | 128,000 | yes | text + image |
| `hf:deepseek-ai/DeepSeek-V3` | 128,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-235B-A22B-Thinking-2507` | 256,000 | 8,192 | yes | text |

Model refs use the form `synthetic/<modelId>`. Use
`openclaw models list --provider synthetic` to see all models available on your
account.

Model allowlist

If you enable a model allowlist (`agents.defaults.models`), add every
Synthetic model you plan to use. Models not in the allowlist will be hidden
from the agent.

Base URL override

If Synthetic changes its API endpoint, override the base URL in your config:

```
{
  models: {
    providers: {
      synthetic: {
        baseUrl: "https://new-api.synthetic.new/anthropic",
      },
    },
  },
}
```

Remember that OpenClaw appends `/v1` automatically.

## Related

[**Model selection** \\
\\
Provider rules, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Synthetic** \\
\\
Synthetic dashboard and API docs.](https://synthetic.new/)

[StepFun](https://docs.openclaw.ai/providers/stepfun) [Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent)

Ctrl+I

---

## Together AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/together>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Together AI

[Together AI](https://together.ai/) provides access to leading open-source
models including Llama, DeepSeek, Kimi, and more through a unified API.

| Property | Value |
| --- | --- |
| Provider | `together` |
| Auth | `TOGETHER_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.together.xyz/v1` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Get an API key

Create an API key at
[api.together.ai/settings/api-keys](https://api.together.ai/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Run onboarding

```
openclaw onboard --auth-choice together-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "together/moonshotai/Kimi-K2.5" },
    },
  },
}
```

### Non-interactive example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice together-api-key \
  --together-api-key "$TOGETHER_API_KEY"
```

The onboarding preset sets `together/moonshotai/Kimi-K2.5` as the default
model.

## Built-in catalog

OpenClaw ships this bundled Together catalog:

| Model ref | Name | Input | Context | Notes |
| --- | --- | --- | --- | --- |
| `together/moonshotai/Kimi-K2.5` | Kimi K2.5 | text, image | 262,144 | Default model; reasoning enabled |
| `together/zai-org/GLM-4.7` | GLM 4.7 Fp8 | text | 202,752 | General-purpose text model |
| `together/meta-llama/Llama-3.3-70B-Instruct-Turbo` | Llama 3.3 70B Instruct Turbo | text | 131,072 | Fast instruction model |
| `together/meta-llama/Llama-4-Scout-17B-16E-Instruct` | Llama 4 Scout 17B 16E Instruct | text, image | 10,000,000 | Multimodal |
| `together/meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8` | Llama 4 Maverick 17B 128E Instruct FP8 | text, image | 20,000,000 | Multimodal |
| `together/deepseek-ai/DeepSeek-V3.1` | DeepSeek V3.1 | text | 131,072 | General text model |
| `together/deepseek-ai/DeepSeek-R1` | DeepSeek R1 | text | 131,072 | Reasoning model |
| `together/moonshotai/Kimi-K2-Instruct-0905` | Kimi K2-Instruct 0905 | text | 262,144 | Secondary Kimi text model |

## Video generation

The bundled `together` plugin also registers video generation through the
shared `video_generate` tool.

| Property | Value |
| --- | --- |
| Default video model | `together/Wan-AI/Wan2.2-T2V-A14B` |
| Modes | text-to-video, single-image reference |
| Supported parameters | `aspectRatio`, `resolution` |

To use Together as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "together/Wan-AI/Wan2.2-T2V-A14B",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for the shared tool parameters,
provider selection, and failover behavior.

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure
`TOGETHER_API_KEY` is available to that process (for example, in
`~/.openclaw/.env` or via `env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

Troubleshooting

- Verify your key works: `openclaw models list --provider together`
- If models are not appearing, confirm the API key is set in the correct
environment for your Gateway process.
- Model refs use the form `together/<model-id>`.

## Related

[**Model selection** \\
\\
Provider rules, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video generation tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Configuration reference** \\
\\
Full config schema including provider settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Together AI** \\
\\
Together AI dashboard, API docs, and pricing.](https://together.ai/)

[Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent) [Venice AI](https://docs.openclaw.ai/providers/venice)

Ctrl+I

---

## Venice AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/venice>_

# Use the default private model
openclaw agent --model venice/kimi-k2-5 --message "Quick health check"

# Use Claude Opus via Venice (anonymized)
openclaw agent --model venice/claude-opus-4-6 --message "Summarize this task"

# Use uncensored model
openclaw agent --model venice/venice-uncensored --message "Draft options"

# Use vision model with image
openclaw agent --model venice/qwen3-vl-235b-a22b --message "Review attached image"

# Use coding model
openclaw agent --model venice/qwen3-coder-480b-a35b-instruct --message "Refactor this function"
```

## Troubleshooting

API key not recognized

```
echo $VENICE_API_KEY
openclaw models list | grep venice
```

Ensure the key starts with `vapi_`.

Model not available

The Venice model catalog updates dynamically. Run `openclaw models list` to see currently available models. Some models may be temporarily offline.

Connection issues

Venice API is at `https://api.venice.ai/api/v1`. Ensure your network allows HTTPS connections.

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Advanced configuration

Config file example

```
{
  env: { VENICE_API_KEY: "vapi_..." },
  agents: { defaults: { model: { primary: "venice/kimi-k2-5" } } },
  models: {
    mode: "merge",
    providers: {
      venice: {
        baseUrl: "https://api.venice.ai/api/v1",
        apiKey: "${VENICE_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "kimi-k2-5",\
            name: "Kimi K2.5",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 256000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Venice AI** \\
\\
Venice AI homepage and account signup.](https://venice.ai/)

[**API documentation** \\
\\
Venice API reference and developer docs.](https://docs.venice.ai/)

[**Pricing** \\
\\
Current Venice credit rates and plans.](https://venice.ai/pricing)

[Together AI](https://docs.openclaw.ai/providers/together) [Vercel AI gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)

Ctrl+I

---

## Vercel AI gateway - OpenClaw

_Source: <https://docs.openclaw.ai/providers/vercel-ai-gateway>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Vercel AI gateway

The [Vercel AI Gateway](https://vercel.com/ai-gateway) provides a unified API to
access hundreds of models through a single endpoint.

| Property | Value |
| --- | --- |
| Provider | `vercel-ai-gateway` |
| Auth | `AI_GATEWAY_API_KEY` |
| API | Anthropic Messages compatible |
| Model catalog | Auto-discovered via `/v1/models` |

OpenClaw auto-discovers the Gateway `/v1/models` catalog, so
`/models vercel-ai-gateway` includes current model refs such as
`vercel-ai-gateway/openai/gpt-5.5` and
`vercel-ai-gateway/moonshotai/kimi-k2.6`.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Set the API key

Run onboarding and choose the AI Gateway auth option:

```
openclaw onboard --auth-choice ai-gateway-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Set a default model

Add the model to your OpenClaw config:

```
{
  agents: {
    defaults: {
      model: { primary: "vercel-ai-gateway/anthropic/claude-opus-4.6" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Verify the model is available

```
openclaw models list --provider vercel-ai-gateway
```

## Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY"
```

## Model ID shorthand

OpenClaw accepts Vercel Claude shorthand model refs and normalizes them at
runtime:

| Shorthand input | Normalized model ref |
| --- | --- |
| `vercel-ai-gateway/claude-opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4.6` |
| `vercel-ai-gateway/opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4-6` |

You can use either the shorthand or the fully qualified model ref in your
configuration. OpenClaw resolves the canonical form automatically.

## Advanced configuration

Environment variable for daemon processes

If the OpenClaw Gateway runs as a daemon (launchd/systemd), make sure
`AI_GATEWAY_API_KEY` is available to that process.

A key set only in `~/.profile` will not be visible to a launchd/systemd
daemon unless that environment is explicitly imported. Set the key in
`~/.openclaw/.env` or via `env.shellEnv` to ensure the gateway process can
read it.

Provider routing

Vercel AI Gateway routes requests to the upstream provider based on the model
ref prefix. For example, `vercel-ai-gateway/anthropic/claude-opus-4.6` routes
through Anthropic, while `vercel-ai-gateway/openai/gpt-5.5` routes through
OpenAI and `vercel-ai-gateway/moonshotai/kimi-k2.6` routes through
MoonshotAI. Your single `AI_GATEWAY_API_KEY` handles authentication for all
upstream providers.

Thinking levels

`/think` options follow trusted upstream model prefixes when OpenClaw knows
the upstream provider contract. `vercel-ai-gateway/anthropic/...` uses the
Claude thinking profile, including adaptive defaults for Claude 4.6 models.
`vercel-ai-gateway/openai/gpt-5.4`, `gpt-5.5`, and Codex-style refs expose
`/think xhigh` just like the direct OpenAI/OpenAI Codex providers. Other
namespaced refs keep the normal reasoning levels unless their catalog
metadata declares more.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Venice AI](https://docs.openclaw.ai/providers/venice) [vLLM](https://docs.openclaw.ai/providers/vllm)

Ctrl+I

---

## vLLM - OpenClaw

_Source: <https://docs.openclaw.ai/providers/vllm>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

vLLM

vLLM can serve open-source (and some custom) models via an **OpenAI-compatible** HTTP API. OpenClaw connects to vLLM using the `openai-completions` API.OpenClaw can also **auto-discover** available models from vLLM when you opt in with `VLLM_API_KEY` (any value works if your server does not enforce auth) and you do not define an explicit `models.providers.vllm` entry.OpenClaw treats `vllm` as a local OpenAI-compatible provider that supports
streamed usage accounting, so status/context token counts can update from
`stream_options.include_usage` responses.

| Property | Value |
| --- | --- |
| Provider ID | `vllm` |
| API | `openai-completions` (OpenAI-compatible) |
| Auth | `VLLM_API_KEY` environment variable |
| Default base URL | `http://127.0.0.1:8000/v1` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/vllm#)

Start vLLM with an OpenAI-compatible server

Your base URL should expose `/v1` endpoints (e.g. `/v1/models`, `/v1/chat/completions`). vLLM commonly runs on:

```
http://127.0.0.1:8000/v1
```

2

[Navigate to header](https://docs.openclaw.ai/providers/vllm#)

Set the API key environment variable

Any value works if your server does not enforce auth:

```
export VLLM_API_KEY="vllm-local"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/vllm#)

Select a model

Replace with one of your vLLM model IDs:

```
{
  agents: {
    defaults: {
      model: { primary: "vllm/your-model-id" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/vllm#)

Verify the model is available

```
openclaw models list --provider vllm
```

## Model discovery (implicit provider)

When `VLLM_API_KEY` is set (or an auth profile exists) and you **do not** define `models.providers.vllm`, OpenClaw queries:

```
GET http://127.0.0.1:8000/v1/models
```

and converts the returned IDs into model entries.

If you set `models.providers.vllm` explicitly, auto-discovery is skipped and you must define models manually.

## Explicit configuration (manual models)

Use explicit config when:

- vLLM runs on a different host or port
- You want to pin `contextWindow` or `maxTokens` values
- Your server requires a real API key (or you want to control headers)
- You connect to a trusted loopback, LAN, or Tailscale vLLM endpoint

```
{
  models: {
    providers: {
      vllm: {
        baseUrl: "http://127.0.0.1:8000/v1",
        apiKey: "${VLLM_API_KEY}",
        api: "openai-completions",
        request: { allowPrivateNetwork: true },
        timeoutSeconds: 300, // Optional: extend connect/header/body/request timeout for slow local models
        models: [\
          {\
            id: "your-model-id",\
            name: "Local vLLM Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

## Advanced configuration

Proxy-style behavior

vLLM is treated as a proxy-style OpenAI-compatible `/v1` backend, not a native
OpenAI endpoint. This means:

| Behavior | Applied? |
| --- | --- |
| Native OpenAI request shaping | No |
| `service_tier` | Not sent |
| Responses `store` | Not sent |
| Prompt-cache hints | Not sent |
| OpenAI reasoning-compat payload shaping | Not applied |
| Hidden OpenClaw attribution headers | Not injected on custom base URLs |

Qwen thinking controls

For Qwen models served through vLLM, set
`params.qwenThinkingFormat: "chat-template"` on the model entry when the
server expects Qwen chat-template kwargs. OpenClaw maps `/think off` to:

```
{
  "chat_template_kwargs": {
    "enable_thinking": false,
    "preserve_thinking": true
  }
}
```

Non-`off` thinking levels send `enable_thinking: true`. If your endpoint
expects DashScope-style top-level flags instead, use
`params.qwenThinkingFormat: "top-level"` to send `enable_thinking` at the
request root. Snake-case `params.qwen_thinking_format` is also accepted.

Nemotron 3 thinking controls

vLLM/Nemotron 3 can use chat-template kwargs to control whether reasoning is
returned as hidden reasoning or visible answer text. When an OpenClaw session
uses `vllm/nemotron-3-*` with thinking off, the bundled vLLM plugin sends:

```
{
  "chat_template_kwargs": {
    "enable_thinking": false,
    "force_nonempty_content": true
  }
}
```

To customize these values, set `chat_template_kwargs` under the model params.
If you also set `params.extra_body.chat_template_kwargs`, that value has
final precedence because `extra_body` is the last request-body override.

```
{
  agents: {
    defaults: {
      models: {
        "vllm/nemotron-3-super": {
          params: {
            chat_template_kwargs: {
              enable_thinking: false,
              force_nonempty_content: true,
            },
          },
        },
      },
    },
  },
}
```

Qwen tool calls appear as text

First make sure vLLM was started with the right tool-call parser and chat
template for the model. For example, vLLM documents `hermes` for Qwen2.5
models and `qwen3_xml` for Qwen3-Coder models.Symptoms:

- skills or tools never run
- the assistant prints raw JSON/XML such as `{"name":"read","arguments":...}`
- vLLM returns an empty `tool_calls` array when OpenClaw sends
`tool_choice: "auto"`

Some Qwen/vLLM combinations return structured tool calls only when the
request uses `tool_choice: "required"`. For those model entries, force the
OpenAI-compatible request field with `params.extra_body`:

```
{
  agents: {
    defaults: {
      models: {
        "vllm/Qwen-Qwen2.5-Coder-32B-Instruct": {
          params: {
            extra_body: {
              tool_choice: "required",
            },
          },
        },
      },
    },
  },
}
```

Replace `Qwen-Qwen2.5-Coder-32B-Instruct` with the exact id returned by:

```
openclaw models list --provider vllm
```

You can apply the same override from the CLI:

```
openclaw config set agents.defaults.models '{"vllm/Qwen-Qwen2.5-Coder-32B-Instruct":{"params":{"extra_body":{"tool_choice":"required"}}}}' --strict-json --merge
```

This is an opt-in compatibility workaround. It makes every model turn with
tools require a tool call, so use it only for a dedicated local model entry
where that behavior is acceptable. Do not use it as a global default for all
vLLM models, and do not use a proxy that blindly converts arbitrary
assistant text into executable tool calls.

Custom base URL

If your vLLM server runs on a non-default host or port, set `baseUrl` in the explicit provider config:

```
{
  models: {
    providers: {
      vllm: {
        baseUrl: "http://192.168.1.50:9000/v1",
        apiKey: "${VLLM_API_KEY}",
        api: "openai-completions",
        request: { allowPrivateNetwork: true },
        timeoutSeconds: 300,
        models: [\
          {\
            id: "my-custom-model",\
            name: "Remote vLLM Model",\
            reasoning: false,\
            input: ["text"],\
            contextWindow: 64000,\
            maxTokens: 4096,\
          },\
        ],
      },
    },
  },
}
```

## Troubleshooting

Slow first response or remote server timeout

For large local models, remote LAN hosts, or tailnet links, set a
provider-scoped request timeout:

```
{
  models: {
    providers: {
      vllm: {
        baseUrl: "http://192.168.1.50:8000/v1",
        apiKey: "${VLLM_API_KEY}",
        api: "openai-completions",
        request: { allowPrivateNetwork: true },
        timeoutSeconds: 300,
        models: [{ id: "your-model-id", name: "Local vLLM Model" }],
      },
    },
  },
}
```

`timeoutSeconds` applies to vLLM model HTTP requests only, including
connection setup, response headers, body streaming, and the total
guarded-fetch abort. Prefer this before increasing
`agents.defaults.timeoutSeconds`, which controls the whole agent run.

Server not reachable

Check that the vLLM server is running and accessible:

```
curl http://127.0.0.1:8000/v1/models
```

If you see a connection error, verify the host, port, and that vLLM started with the OpenAI-compatible server mode.
For explicit loopback, LAN, or Tailscale endpoints, also set
`models.providers.vllm.request.allowPrivateNetwork: true`; provider
requests block private-network URLs by default unless the provider is
explicitly trusted.

Auth errors on requests

If requests fail with auth errors, set a real `VLLM_API_KEY` that matches your server configuration, or configure the provider explicitly under `models.providers.vllm`.

If your vLLM server does not enforce auth, any non-empty value for `VLLM_API_KEY` works as an opt-in signal for OpenClaw.

No models discovered

Auto-discovery requires `VLLM_API_KEY` to be set **and** no explicit `models.providers.vllm` config entry. If you have defined the provider manually, OpenClaw skips discovery and uses only your declared models.

Tools render as raw text

If a Qwen model prints JSON/XML tool syntax instead of executing a skill,
check the Qwen guidance in Advanced configuration above. The usual fix is:

- start vLLM with the correct parser/template for that model
- confirm the exact model id with `openclaw models list --provider vllm`
- add a dedicated per-model `params.extra_body.tool_choice: "required"`
override only if `tool_choice: "auto"` still returns empty or text-only
tool calls

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**OpenAI** \\
\\
Native OpenAI provider and OpenAI-compatible route behavior.](https://docs.openclaw.ai/providers/openai)

[**OAuth and auth** \\
\\
Auth details and credential reuse rules.](https://docs.openclaw.ai/gateway/authentication)

[**Troubleshooting** \\
\\
Common issues and how to resolve them.](https://docs.openclaw.ai/help/troubleshooting)

[Vercel AI gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway) [Volcengine (Doubao)](https://docs.openclaw.ai/providers/volcengine)

Ctrl+I

---

## xAI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/xai>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

xAI

OpenClaw ships a bundled `xai` provider plugin for Grok models.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Create an API key

Create an API key in the [xAI console](https://console.x.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Set your API key

Set `XAI_API_KEY`, or run:

```
openclaw onboard --auth-choice xai-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Pick a model

```
{
  agents: { defaults: { model: { primary: "xai/grok-4.3" } } },
}
```

OpenClaw uses the xAI Responses API as the bundled xAI transport. The same
`XAI_API_KEY` can also power Grok-backed `web_search`, first-class `x_search`,
and remote `code_execution`.
If you store an xAI key under `plugins.entries.xai.config.webSearch.apiKey`,
the bundled xAI model provider reuses that key as a fallback too.
Set `plugins.entries.xai.config.webSearch.baseUrl` to route Grok `web_search`
and, by default, `x_search` through an operator xAI Responses proxy.
`code_execution` tuning lives under `plugins.entries.xai.config.codeExecution`.

## Built-in catalog

OpenClaw includes these xAI model families out of the box:

| Family | Model ids |
| --- | --- |
| Grok 3 | `grok-3`, `grok-3-fast`, `grok-3-mini`, `grok-3-mini-fast` |
| Grok 4.3 | `grok-4.3` |
| Grok 4 | `grok-4`, `grok-4-0709` |
| Grok 4 Fast | `grok-4-fast`, `grok-4-fast-non-reasoning` |
| Grok 4.1 Fast | `grok-4-1-fast`, `grok-4-1-fast-non-reasoning` |
| Grok 4.20 Beta | `grok-4.20-beta-latest-reasoning`, `grok-4.20-beta-latest-non-reasoning` |
| Grok Code | `grok-code-fast-1` |

The plugin also forward-resolves newer `grok-4*` and `grok-code-fast*` ids when
they follow the same API shape.

`grok-4.3`, `grok-4-fast`, `grok-4-1-fast`, and the `grok-4.20-beta-*`
variants are the current image-capable Grok refs in the bundled catalog.

## OpenClaw feature coverage

The bundled plugin maps xAI’s current public API surface onto OpenClaw’s shared
provider and tool contracts. Capabilities that don’t fit the shared contract
(for example streaming TTS and realtime voice) are not exposed — see the table
below.

| xAI capability | OpenClaw surface | Status |
| --- | --- | --- |
| Chat / Responses | `xai/<model>` model provider | Yes |
| Server-side web search | `web_search` provider `grok` | Yes |
| Server-side X search | `x_search` tool | Yes |
| Server-side code execution | `code_execution` tool | Yes |
| Images | `image_generate` | Yes |
| Videos | `video_generate` | Yes |
| Batch text-to-speech | `messages.tts.provider: "xai"` / `tts` | Yes |
| Streaming TTS | — | Not exposed; OpenClaw’s TTS contract returns complete audio buffers |
| Batch speech-to-text | `tools.media.audio` / media understanding | Yes |
| Streaming speech-to-text | Voice Call `streaming.provider: "xai"` | Yes |
| Realtime voice | — | Not exposed yet; different session/WebSocket contract |
| Files / batches | Generic model API compatibility only | Not a first-class OpenClaw tool |

OpenClaw uses xAI’s REST image/video/TTS/STT APIs for media generation,
speech, and batch transcription, xAI’s streaming STT WebSocket for live
voice-call transcription, and the Responses API for model, search, and
code-execution tools. Features that need different OpenClaw contracts, such as
Realtime voice sessions, are documented here as upstream capabilities rather
than hidden plugin behavior.

### Fast-mode mappings

`/fast on` or `agents.defaults.models["xai/<model>"].params.fastMode: true`
rewrites native xAI requests as follows:

| Source model | Fast-mode target |
| --- | --- |
| `grok-3` | `grok-3-fast` |
| `grok-3-mini` | `grok-3-mini-fast` |
| `grok-4` | `grok-4-fast` |
| `grok-4-0709` | `grok-4-fast` |

### Legacy compatibility aliases

Legacy aliases still normalize to the canonical bundled ids:

| Legacy alias | Canonical id |
| --- | --- |
| `grok-4-fast-reasoning` | `grok-4-fast` |
| `grok-4-1-fast-reasoning` | `grok-4-1-fast` |
| `grok-4.20-reasoning` | `grok-4.20-beta-latest-reasoning` |
| `grok-4.20-non-reasoning` | `grok-4.20-beta-latest-non-reasoning` |

## Features

Web search

The bundled `grok` web-search provider uses `XAI_API_KEY` too:

```
openclaw config set tools.web.search.provider grok
```

Video generation

The bundled `xai` plugin registers video generation through the shared
`video_generate` tool.

- Default video model: `xai/grok-imagine-video`
- Modes: text-to-video, image-to-video, reference-image generation, remote
video edit, and remote video extension
- Aspect ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `3:2`, `2:3`
- Resolutions: `480P`, `720P`
- Duration: 1-15 seconds for generation/image-to-video, 1-10 seconds when
using `reference_image` roles, 2-10 seconds for extension
- Reference-image generation: set `imageRoles` to `reference_image` for
every supplied image; xAI accepts up to 7 such images

Local video buffers are not accepted. Use remote `http(s)` URLs for
video edit/extend inputs. Image-to-video accepts local image buffers because
OpenClaw can encode those as data URLs for xAI.

To use xAI as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "xai/grok-imagine-video",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters,
provider selection, and failover behavior.

Image generation

The bundled `xai` plugin registers image generation through the shared
`image_generate` tool.

- Default image model: `xai/grok-imagine-image`
- Additional model: `xai/grok-imagine-image-pro`
- Modes: text-to-image and reference-image edit
- Reference inputs: one `image` or up to five `images`
- Aspect ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `2:3`, `3:2`
- Resolutions: `1K`, `2K`
- Count: up to 4 images

OpenClaw asks xAI for `b64_json` image responses so generated media can be
stored and delivered through the normal channel attachment path. Local
reference images are converted to data URLs; remote `http(s)` references are
passed through.To use xAI as the default image provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "xai/grok-imagine-image",
      },
    },
  },
}
```

xAI also documents `quality`, `mask`, `user`, and additional native ratios
such as `1:2`, `2:1`, `9:20`, and `20:9`. OpenClaw forwards only the
shared cross-provider image controls today; unsupported native-only knobs
are intentionally not exposed through `image_generate`.

Text-to-speech

The bundled `xai` plugin registers text-to-speech through the shared `tts`
provider surface.

- Voices: `eve`, `ara`, `rex`, `sal`, `leo`, `una`
- Default voice: `eve`
- Formats: `mp3`, `wav`, `pcm`, `mulaw`, `alaw`
- Language: BCP-47 code or `auto`
- Speed: provider-native speed override
- Native Opus voice-note format is not supported

To use xAI as the default TTS provider:

```
{
  messages: {
    tts: {
      provider: "xai",
      providers: {
        xai: {
          voiceId: "eve",
        },
      },
    },
  },
}
```

OpenClaw uses xAI’s batch `/v1/tts` endpoint. xAI also offers streaming TTS
over WebSocket, but the OpenClaw speech provider contract currently expects
a complete audio buffer before reply delivery.

Speech-to-text

The bundled `xai` plugin registers batch speech-to-text through OpenClaw’s
media-understanding transcription surface.

- Default model: `grok-stt`
- Endpoint: xAI REST `/v1/stt`
- Input path: multipart audio file upload
- Supported by OpenClaw wherever inbound audio transcription uses
`tools.media.audio`, including Discord voice-channel segments and
channel audio attachments

To force xAI for inbound audio transcription:

```
{
  tools: {
    media: {
      audio: {
        models: [\
          {\
            type: "provider",\
            provider: "xai",\
            model: "grok-stt",\
          },\
        ],
      },
    },
  },
}
```

Language can be supplied through the shared audio media config or per-call
transcription request. Prompt hints are accepted by the shared OpenClaw
surface, but the xAI REST STT integration only forwards file, model, and
language because those map cleanly to the current public xAI endpoint.

Streaming speech-to-text

The bundled `xai` plugin also registers a realtime transcription provider
for live voice-call audio.

- Endpoint: xAI WebSocket `wss://api.x.ai/v1/stt`
- Default encoding: `mulaw`
- Default sample rate: `8000`
- Default endpointing: `800ms`
- Interim transcripts: enabled by default

Voice Call’s Twilio media stream sends G.711 µ-law audio frames, so the
xAI provider can forward those frames directly without transcoding:

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "xai",
            providers: {
              xai: {
                apiKey: "${XAI_API_KEY}",
                endpointingMs: 800,
                language: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

Provider-owned config lives under
`plugins.entries.voice-call.config.streaming.providers.xai`. Supported
keys are `apiKey`, `baseUrl`, `sampleRate`, `encoding` (`pcm`, `mulaw`, or
`alaw`), `interimResults`, `endpointingMs`, and `language`.

This streaming provider is for Voice Call’s realtime transcription path.
Discord voice currently records short segments and uses the batch
`tools.media.audio` transcription path instead.

x\_search configuration

The bundled xAI plugin exposes `x_search` as an OpenClaw tool for searching
X (formerly Twitter) content via Grok.Config path: `plugins.entries.xai.config.xSearch`

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | boolean | — | Enable or disable x\_search |
| `model` | string | `grok-4-1-fast` | Model used for x\_search requests |
| `baseUrl` | string | — | xAI Responses base URL override |
| `inlineCitations` | boolean | — | Include inline citations in results |
| `maxTurns` | number | — | Maximum conversation turns |
| `timeoutSeconds` | number | — | Request timeout in seconds |
| `cacheTtlMinutes` | number | — | Cache time-to-live in minutes |

```
{
  plugins: {
    entries: {
      xai: {
        config: {
          xSearch: {
            enabled: true,
            model: "grok-4-1-fast",
            baseUrl: "https://api.x.ai/v1",
            inlineCitations: true,
          },
        },
      },
    },
  },
}
```

Code execution configuration

The bundled xAI plugin exposes `code_execution` as an OpenClaw tool for
remote code execution in xAI’s sandbox environment.Config path: `plugins.entries.xai.config.codeExecution`

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | boolean | `true` (if key available) | Enable or disable code execution |
| `model` | string | `grok-4-1-fast` | Model used for code execution requests |
| `maxTurns` | number | — | Maximum conversation turns |
| `timeoutSeconds` | number | — | Request timeout in seconds |

This is remote xAI sandbox execution, not local [`exec`](https://docs.openclaw.ai/tools/exec).

```
{
  plugins: {
    entries: {
      xai: {
        config: {
          codeExecution: {
            enabled: true,
            model: "grok-4-1-fast",
          },
        },
      },
    },
  },
}
```

Known limits

- Auth is API-key only today. There is no xAI OAuth or device-code flow in
OpenClaw yet.
- `grok-4.20-multi-agent-experimental-beta-0304` is not supported on the
normal xAI provider path because it requires a different upstream API
surface than the standard OpenClaw xAI transport.
- xAI Realtime voice is not registered as an OpenClaw provider yet. It
needs a different bidirectional voice session contract than batch STT or
streaming transcription.
- xAI image `quality`, image `mask`, and extra native-only aspect ratios are
not exposed until the shared `image_generate` tool has corresponding
cross-provider controls.

Advanced notes

- OpenClaw applies xAI-specific tool-schema and tool-call compatibility fixes
automatically on the shared runner path.
- Native xAI requests default `tool_stream: true`. Set
`agents.defaults.models["xai/<model>"].params.tool_stream` to `false` to
disable it.
- The bundled xAI wrapper strips unsupported strict tool-schema flags and
reasoning payload keys before sending native xAI requests.
- `web_search`, `x_search`, and `code_execution` are exposed as OpenClaw
tools. OpenClaw enables the specific xAI built-in it needs inside each tool
request instead of attaching all native tools to every chat turn.
- Grok `web_search` reads `plugins.entries.xai.config.webSearch.baseUrl`.
`x_search` reads `plugins.entries.xai.config.xSearch.baseUrl`, then
falls back to the Grok web-search base URL.
- `x_search` and `code_execution` are owned by the bundled xAI plugin rather
than hardcoded into the core model runtime.
- `code_execution` is remote xAI sandbox execution, not local
[`exec`](https://docs.openclaw.ai/tools/exec).

## Live testing

The xAI media paths are covered by unit tests and opt-in live suites. The live
commands load secrets from your login shell, including `~/.profile`, before
probing `XAI_API_KEY`.

```
pnpm test extensions/xai
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_TEST_QUIET=1 pnpm test:live -- extensions/xai/xai.live.test.ts
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_TEST_QUIET=1 OPENCLAW_LIVE_IMAGE_GENERATION_PROVIDERS=xai pnpm test:live -- test/image-generation.runtime.live.test.ts
```

The provider-specific live file synthesizes normal TTS, telephony-friendly PCM
TTS, transcribes audio through xAI batch STT, streams the same PCM through xAI
realtime STT, generates text-to-image output, and edits a reference image. The
shared image live file verifies the same xAI provider through OpenClaw’s
runtime selection, fallback, normalization, and media attachment path.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**All providers** \\
\\
The broader provider overview.](https://docs.openclaw.ai/providers/index)

[**Troubleshooting** \\
\\
Common issues and fixes.](https://docs.openclaw.ai/help/troubleshooting)

[Vydra](https://docs.openclaw.ai/providers/vydra) [Xiaomi MiMo](https://docs.openclaw.ai/providers/xiaomi)

Ctrl+I

---

## Xiaomi MiMo - OpenClaw

_Source: <https://docs.openclaw.ai/providers/xiaomi>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Xiaomi MiMo

Xiaomi MiMo is the API platform for **MiMo** models. OpenClaw uses the Xiaomi
OpenAI-compatible endpoint with API-key authentication.

| Property | Value |
| --- | --- |
| Provider | `xiaomi` |
| Auth | `XIAOMI_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.xiaomimimo.com/v1` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Get an API key

Create an API key in the [Xiaomi MiMo console](https://platform.xiaomimimo.com/#/console/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Run onboarding

```
openclaw onboard --auth-choice xiaomi-api-key
```

Or pass the key directly:

```
openclaw onboard --auth-choice xiaomi-api-key --xiaomi-api-key "$XIAOMI_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Verify the model is available

```
openclaw models list --provider xiaomi
```

## Built-in catalog

| Model ref | Input | Context | Max output | Reasoning | Notes |
| --- | --- | --- | --- | --- | --- |
| `xiaomi/mimo-v2-flash` | text | 262,144 | 8,192 | No | Default model |
| `xiaomi/mimo-v2-pro` | text | 1,048,576 | 32,000 | Yes | Large context |
| `xiaomi/mimo-v2-omni` | text, image | 262,144 | 32,000 | Yes | Multimodal |

The default model ref is `xiaomi/mimo-v2-flash`. The provider is injected automatically when `XIAOMI_API_KEY` is set or an auth profile exists.

## Text-to-speech

The bundled `xiaomi` plugin also registers Xiaomi MiMo as a speech provider for
`messages.tts`. It calls Xiaomi’s chat-completions TTS contract with the text as
an `assistant` message and optional style guidance as a `user` message.

| Property | Value |
| --- | --- |
| TTS id | `xiaomi` (`mimo` alias) |
| Auth | `XIAOMI_API_KEY` |
| API | `POST /v1/chat/completions` with `audio` |
| Default | `mimo-v2.5-tts`, voice `mimo_default` |
| Output | MP3 by default; WAV when configured |

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "xiaomi",
      providers: {
        xiaomi: {
          apiKey: "xiaomi_api_key",
          model: "mimo-v2.5-tts",
          voice: "mimo_default",
          format: "mp3",
          style: "Bright, natural, conversational tone.",
        },
      },
    },
  },
}
```

Supported built-in voices include `mimo_default`, `default_zh`, `default_en`,
`Mia`, `Chloe`, `Milo`, and `Dean`. `mimo-v2-tts` is supported for older MiMo
TTS accounts; the default uses the current MiMo-V2.5 TTS model. For voice-note
targets such as Feishu and Telegram, OpenClaw transcodes Xiaomi output to 48kHz
Opus with `ffmpeg` before delivery.

## Config example

```
{
  env: { XIAOMI_API_KEY: "your-key" },
  agents: { defaults: { model: { primary: "xiaomi/mimo-v2-flash" } } },
  models: {
    mode: "merge",
    providers: {
      xiaomi: {
        baseUrl: "https://api.xiaomimimo.com/v1",
        api: "openai-completions",
        apiKey: "XIAOMI_API_KEY",
        models: [\
          {\
            id: "mimo-v2-flash",\
            name: "Xiaomi MiMo V2 Flash",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 8192,\
          },\
          {\
            id: "mimo-v2-pro",\
            name: "Xiaomi MiMo V2 Pro",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 1048576,\
            maxTokens: 32000,\
          },\
          {\
            id: "mimo-v2-omni",\
            name: "Xiaomi MiMo V2 Omni",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 32000,\
          },\
        ],
      },
    },
  },
}
```

Auto-injection behavior

The `xiaomi` provider is injected automatically when `XIAOMI_API_KEY` is set in your environment or an auth profile exists. You do not need to manually configure the provider unless you want to override model metadata or the base URL.

Model details

- **mimo-v2-flash** — lightweight and fast, ideal for general-purpose text tasks. No reasoning support.
- **mimo-v2-pro** — supports reasoning with a 1M token context window for long-document workloads.
- **mimo-v2-omni** — reasoning-enabled multimodal model that accepts both text and image inputs.

All models use the `xiaomi/` prefix (for example `xiaomi/mimo-v2-pro`).

Troubleshooting

- If models do not appear, confirm `XIAOMI_API_KEY` is set and valid.
- When the Gateway runs as a daemon, ensure the key is available to that process (for example in `~/.openclaw/.env` or via `env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for persistent availability.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Xiaomi MiMo console** \\
\\
Xiaomi MiMo dashboard and API key management.](https://platform.xiaomimimo.com/)

[xAI](https://docs.openclaw.ai/providers/xai) [Z.AI](https://docs.openclaw.ai/providers/zai)

Ctrl+I

---

## Z.AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/zai>_

# Coding Plan Global (recommended for Coding Plan users)
openclaw onboard --auth-choice zai-coding-global

# Coding Plan CN (China region)
openclaw onboard --auth-choice zai-coding-cn

# General API
openclaw onboard --auth-choice zai-global

# General API CN (China region)
openclaw onboard --auth-choice zai-cn
```

2

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Set a default model

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Verify the model is listed

```
openclaw models list --all --provider zai
```

## Built-in catalog

OpenClaw ships the bundled `zai` provider catalog in the plugin manifest, so read-only
listing can show known GLM rows without loading provider runtime:

```
openclaw models list --all --provider zai
```

The manifest-backed catalog currently includes:

| Model ref | Notes |
| --- | --- |
| `zai/glm-5.1` | Default model |
| `zai/glm-5` |  |
| `zai/glm-5-turbo` |  |
| `zai/glm-5v-turbo` |  |
| `zai/glm-4.7` |  |
| `zai/glm-4.7-flash` |  |
| `zai/glm-4.7-flashx` |  |
| `zai/glm-4.6` |  |
| `zai/glm-4.6v` |  |
| `zai/glm-4.5` |  |
| `zai/glm-4.5-air` |  |
| `zai/glm-4.5-flash` |  |
| `zai/glm-4.5v` |  |

GLM models are available as `zai/<model>` (example: `zai/glm-5`). The default bundled model ref is `zai/glm-5.1`.

## Advanced configuration

Forward-resolving unknown GLM-5 models

Unknown `glm-5*` ids still forward-resolve on the bundled provider path by
synthesizing provider-owned metadata from the `glm-4.7` template when the id
matches the current GLM-5 family shape.

Tool-call streaming

`tool_stream` is enabled by default for Z.AI tool-call streaming. To disable it:

```
{
  agents: {
    defaults: {
      models: {
        "zai/<model>": {
          params: { tool_stream: false },
        },
      },
    },
  },
}
```

Thinking and preserved thinking

Z.AI thinking follows OpenClaw’s `/think` controls. With thinking off,
OpenClaw sends `thinking: { type: "disabled" }` to avoid responses that
spend the output budget on `reasoning_content` before visible text.Preserved thinking is opt-in because Z.AI requires the full historical
`reasoning_content` to be replayed, which increases prompt tokens. Enable it
per model:

```
{
  agents: {
    defaults: {
      models: {
        "zai/glm-5.1": {
          params: { preserveThinking: true },
        },
      },
    },
  },
}
```

When enabled and thinking is on, OpenClaw sends
`thinking: { type: "enabled", clear_thinking: false }` and replays prior
`reasoning_content` for the same OpenAI-compatible transcript.Advanced users can still override the exact provider payload with
`params.extra_body.thinking`.

Image understanding

The bundled Z.AI plugin registers image understanding.

| Property | Value |
| --- | --- |
| Model | `glm-4.6v` |

Image understanding is auto-resolved from the configured Z.AI auth — no
additional config is needed.

Auth details

- Z.AI uses Bearer auth with your API key.
- The `zai-api-key` onboarding choice auto-detects the matching Z.AI endpoint from the key prefix.
- Use the explicit regional choices (`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) when you want to force a specific API surface.

## Related

[**GLM model family** \\
\\
Model family overview for GLM.](https://docs.openclaw.ai/providers/glm)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[Xiaomi MiMo](https://docs.openclaw.ai/providers/xiaomi)

Ctrl+I

---

## ElevenLabs - OpenClaw

_Source: <https://docs.openclaw.ai/tr/providers/elevenlabs>_

[Ana içeriğe atla](https://docs.openclaw.ai/tr/providers/elevenlabs#content-area)

[OpenClaw home page](https://docs.openclaw.ai/tr)

Türkçe

Ara...

Ara...

Providers

ElevenLabs

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Kimlik doğrulama](https://docs.openclaw.ai/tr/providers/elevenlabs#kimlik-do%C4%9Frulama)
- [Metinden konuşmaya](https://docs.openclaw.ai/tr/providers/elevenlabs#metinden-konu%C5%9Fmaya)
- [Speech-to-text](https://docs.openclaw.ai/tr/providers/elevenlabs#speech-to-text)
- [Voice Call akışlı STT](https://docs.openclaw.ai/tr/providers/elevenlabs#voice-call-ak%C4%B1%C5%9Fl%C4%B1-stt)
- [İlgili](https://docs.openclaw.ai/tr/providers/elevenlabs#i%CC%87lgili)

OpenClaw, metinden konuşmaya, Scribe
v2 ile toplu speech-to-text ve Voice Call akışlı STT için Scribe v2 Realtime amacıyla ElevenLabs kullanır.

| Yetenek | OpenClaw yüzeyi | Varsayılan |
| --- | --- | --- |
| Metinden konuşmaya | `messages.tts` / `talk` | `eleven_multilingual_v2` |
| Toplu speech-to-text | `tools.media.audio` | `scribe_v2` |
| Akışlı speech-to-text | Voice Call `streaming.provider: "elevenlabs"` | `scribe_v2_realtime` |

## Kimlik doğrulama

Ortamda `ELEVENLABS_API_KEY` ayarlayın. Mevcut
ElevenLabs araçlarıyla uyumluluk için `XI_API_KEY` de kabul edilir.

```
export ELEVENLABS_API_KEY="..."
```

## Metinden konuşmaya

```
{
  messages: {
    tts: {
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          voiceId: "pMsXgVXv3BLzUgSXRplE",
          modelId: "eleven_multilingual_v2",
        },
      },
    },
  },
}
```

ElevenLabs v3 TTS kullanmak için `modelId` değerini `eleven_v3` olarak ayarlayın. OpenClaw
mevcut kurulumlar için varsayılan olarak `eleven_multilingual_v2` değerini korur.

## Speech-to-text

Gelen ses ekleri ve kısa kaydedilmiş ses segmentleri için Scribe v2 kullanın:

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "elevenlabs", model: "scribe_v2" }],
      },
    },
  },
}
```

OpenClaw, çok parçalı sesi ElevenLabs `/v1/speech-to-text` uç noktasına
`model_id: "scribe_v2"` ile gönderir. Dil ipuçları mevcut olduğunda `language_code` alanına eşlenir.

## Voice Call akışlı STT

Paketlenmiş `elevenlabs` Plugin’i, Voice Call
akışlı transcription için Scribe v2 Realtime’ı kaydeder.

| Ayar | Yapılandırma yolu | Varsayılan |
| --- | --- | --- |
| API anahtarı | `plugins.entries.voice-call.config.streaming.providers.elevenlabs.apiKey` | `ELEVENLABS_API_KEY` / `XI_API_KEY` değerine geri düşer |
| Model | `...elevenlabs.modelId` | `scribe_v2_realtime` |
| Ses biçimi | `...elevenlabs.audioFormat` | `ulaw_8000` |
| Örnekleme oranı | `...elevenlabs.sampleRate` | `8000` |
| Commit stratejisi | `...elevenlabs.commitStrategy` | `vad` |
| Dil | `...elevenlabs.languageCode` | (ayarlanmamış) |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "elevenlabs",
            providers: {
              elevenlabs: {
                apiKey: "${ELEVENLABS_API_KEY}",
                audioFormat: "ulaw_8000",
                commitStrategy: "vad",
                languageCode: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call, Twilio medyasını 8 kHz G.711 u-law olarak alır. ElevenLabs realtime
sağlayıcısı varsayılan olarak `ulaw_8000` kullandığı için telefon çerçeveleri
yeniden kodlama olmadan iletilebilir.

## İlgili

- [Metinden konuşmaya](https://docs.openclaw.ai/tr/tools/tts)
- [Model seçimi](https://docs.openclaw.ai/tr/concepts/model-providers)

[DeepSeek](https://docs.openclaw.ai/tr/providers/deepseek) [Fal](https://docs.openclaw.ai/tr/providers/fal)

Ctrl+I

---

## ElevenLabs - OpenClaw

_Source: <https://docs.openclaw.ai/uk/providers/elevenlabs>_

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/providers/elevenlabs#content-area)

[OpenClaw home page](https://docs.openclaw.ai/uk)

Українська

Пошук...

Пошук...

Providers

ElevenLabs

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Автентифікація](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%B0%D0%B2%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D1%96%D0%BA%D0%B0%D1%86%D1%96%D1%8F)
- [Перетворення тексту на мовлення](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83-%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F)
- [Перетворення мовлення на текст](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BA%D1%81%D1%82)
- [Потокове STT Voice Call](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%B2%D0%B5-stt-voice-call)
- [Пов’язане](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

OpenClaw використовує ElevenLabs для перетворення тексту на мовлення, пакетного перетворення мовлення на текст із Scribe
v2 і потокового STT Voice Call із Scribe v2 Realtime.

| Можливість | Поверхня OpenClaw | За замовчуванням |
| --- | --- | --- |
| Перетворення тексту на мовлення | `messages.tts` / `talk` | `eleven_multilingual_v2` |
| Пакетне перетворення мовлення на текст | `tools.media.audio` | `scribe_v2` |
| Потокове перетворення мовлення на текст | Voice Call `streaming.provider: "elevenlabs"` | `scribe_v2_realtime` |

## Автентифікація

Установіть `ELEVENLABS_API_KEY` у середовищі. `XI_API_KEY` також підтримується для
сумісності з наявними інструментами ElevenLabs.

```
export ELEVENLABS_API_KEY="..."
```

## Перетворення тексту на мовлення

```
{
  messages: {
    tts: {
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          voiceId: "pMsXgVXv3BLzUgSXRplE",
          modelId: "eleven_multilingual_v2",
        },
      },
    },
  },
}
```

Установіть `modelId` на `eleven_v3`, щоб використовувати ElevenLabs v3 TTS. OpenClaw зберігає
`eleven_multilingual_v2` як значення за замовчуванням для наявних інсталяцій.

## Перетворення мовлення на текст

Використовуйте Scribe v2 для вхідних аудіовкладень і коротких записаних голосових сегментів:

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "elevenlabs", model: "scribe_v2" }],
      },
    },
  },
}
```

OpenClaw надсилає multipart-аудіо до ElevenLabs `/v1/speech-to-text` з
`model_id: "scribe_v2"`. Підказки мови зіставляються з `language_code`, якщо вони задані.

## Потокове STT Voice Call

Вбудований Plugin `elevenlabs` реєструє Scribe v2 Realtime для потокової
транскрипції Voice Call.

| Налаштування | Шлях конфігурації | За замовчуванням |
| --- | --- | --- |
| API-ключ | `plugins.entries.voice-call.config.streaming.providers.elevenlabs.apiKey` | Резервно використовує `ELEVENLABS_API_KEY` / `XI_API_KEY` |
| Модель | `...elevenlabs.modelId` | `scribe_v2_realtime` |
| Формат аудіо | `...elevenlabs.audioFormat` | `ulaw_8000` |
| Частота дискретизації | `...elevenlabs.sampleRate` | `8000` |
| Стратегія коміту | `...elevenlabs.commitStrategy` | `vad` |
| Мова | `...elevenlabs.languageCode` | (не задано) |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "elevenlabs",
            providers: {
              elevenlabs: {
                apiKey: "${ELEVENLABS_API_KEY}",
                audioFormat: "ulaw_8000",
                commitStrategy: "vad",
                languageCode: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call отримує медіапотік Twilio як 8 кГц G.711 u-law. Провайдер ElevenLabs realtime
за замовчуванням використовує `ulaw_8000`, тому телекомунікаційні фрейми можна пересилати без
транскодування.

## Пов’язане

- [Перетворення тексту на мовлення](https://docs.openclaw.ai/uk/tools/tts)
- [Вибір моделі](https://docs.openclaw.ai/uk/concepts/model-providers)

[DeepSeek](https://docs.openclaw.ai/uk/providers/deepseek) [Fal](https://docs.openclaw.ai/uk/providers/fal)

Ctrl+I

---

## Deepinfra - OpenClaw

_Source: <https://docs.openclaw.ai/zh-CN/providers/deepinfra>_

# DeepInfra

DeepInfra 提供一个 **统一 API**，可通过单一端点和 API 密钥，将请求路由到最受欢迎的开源模型和前沿模型。它兼容 OpenAI，因此大多数 OpenAI SDK 只需切换 base URL 即可使用。

## 获取 API 密钥

1. 前往 [https://deepinfra.com/](https://deepinfra.com/)
2. 登录或创建账户
3. 进入 Dashboard / Keys，生成新的 API 密钥，或使用自动创建的密钥

## CLI 设置

```
openclaw onboard --deepinfra-api-key <key>
```

或者设置环境变量：

```
export DEEPINFRA_API_KEY="<your-deepinfra-api-key>" # pragma: allowlist secret
```

## 配置片段

```
{
  env: { DEEPINFRA_API_KEY: "<your-deepinfra-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "deepinfra/deepseek-ai/DeepSeek-V3.2" },
    },
  },
}
```

## 支持的 OpenClaw 界面

内置插件会注册所有与当前 OpenClaw provider 契约匹配的 DeepInfra 界面：

| 界面 | 默认模型 | OpenClaw 配置/工具 |
| --- | --- | --- |
| 聊天 / 模型 provider | `deepseek-ai/DeepSeek-V3.2` | `agents.defaults.model` |
| 图像生成/编辑 | `black-forest-labs/FLUX-1-schnell` | `image_generate`, `agents.defaults.imageGenerationModel` |
| 媒体理解 | 图像使用 `moonshotai/Kimi-K2.5` | 入站图像理解 |
| 语音转文本 | `openai/whisper-large-v3-turbo` | 入站音频转录 |
| 文本转语音 | `hexgrad/Kokoro-82M` | `messages.tts.provider: "deepinfra"` |
| 视频生成 | `Pixverse/Pixverse-T2V` | `video_generate`, `agents.defaults.videoGenerationModel` |
| 记忆嵌入 | `BAAI/bge-m3` | `agents.defaults.memorySearch.provider: "deepinfra"` |

DeepInfra 还提供重排序、分类、目标检测及其他原生模型类型。OpenClaw 当前尚未为这些类别提供一流的 provider 契约，因此此插件暂时不会注册它们。

## 可用模型

OpenClaw 会在启动时动态发现可用的 DeepInfra 模型。使用 `/models deepinfra` 查看完整的可用模型列表。[DeepInfra.com](https://deepinfra.com/) 上可用的任何模型都可以通过 `deepinfra/` 前缀使用：

```
deepinfra/MiniMaxAI/MiniMax-M2.5
deepinfra/deepseek-ai/DeepSeek-V3.2
deepinfra/moonshotai/Kimi-K2.5
deepinfra/zai-org/GLM-5.1
...以及更多
```

## 说明

- 模型引用格式为 `deepinfra/<provider>/<model>`（例如 `deepinfra/Qwen/Qwen3-Max`）。
- 默认模型：`deepinfra/deepseek-ai/DeepSeek-V3.2`
- Base URL：`https://api.deepinfra.com/v1/openai`
- 原生视频生成使用 `https://api.deepinfra.com/v1/inference/<model>`。

Ctrl+I

---
