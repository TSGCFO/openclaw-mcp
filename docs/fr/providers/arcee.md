---
source_url: https://docs.openclaw.ai/fr/providers/arcee
title: "Arcee AI - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/arcee#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Providers

Arcee AI

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Premiers pas](https://docs.openclaw.ai/fr/providers/arcee#premiers-pas)
- [Configuration non interactive](https://docs.openclaw.ai/fr/providers/arcee#configuration-non-interactive)
- [Catalogue intégré](https://docs.openclaw.ai/fr/providers/arcee#catalogue-int%C3%A9gr%C3%A9)
- [Fonctionnalités prises en charge](https://docs.openclaw.ai/fr/providers/arcee#fonctionnalit%C3%A9s-prises-en-charge)
- [Connexe](https://docs.openclaw.ai/fr/providers/arcee#connexe)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Arcee AI](https://arcee.ai/) donne accès à la famille Trinity de modèles à mélange d’experts via une API compatible OpenAI. Tous les modèles Trinity sont sous licence Apache 2.0.Les modèles Arcee AI sont accessibles directement via la plateforme Arcee ou via [OpenRouter](https://docs.openclaw.ai/fr/providers/openrouter).

| Propriété | Valeur |
| --- | --- |
| Fournisseur | `arcee` |
| Authentification | `ARCEEAI_API_KEY` (direct) ou `OPENROUTER_API_KEY` (via OpenRouter) |
| API | Compatible OpenAI |
| URL de base | `https://api.arcee.ai/api/v1` (direct) ou `https://openrouter.ai/api/v1` (OpenRouter) |

## [​](https://docs.openclaw.ai/fr/providers/arcee\#premiers-pas)  Premiers pas

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

## [​](https://docs.openclaw.ai/fr/providers/arcee\#configuration-non-interactive)  Configuration non interactive

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

## [​](https://docs.openclaw.ai/fr/providers/arcee\#catalogue-int%C3%A9gr%C3%A9)  Catalogue intégré

OpenClaw inclut actuellement ce catalogue Arcee groupé :

| Réf. du modèle | Nom | Entrée | Contexte | Coût (entrée/sortie par 1 M) | Notes |
| --- | --- | --- | --- | --- | --- |
| `arcee/trinity-large-thinking` | Trinity Large Thinking | texte | 256K | 0,25 /0,90/ 0,90/0,90 | Modèle par défaut ; raisonnement activé |
| `arcee/trinity-large-preview` | Trinity Large Preview | texte | 128K | 0,25 /1,00/ 1,00/1,00 | Usage général ; 400 Md de paramètres, 13 Md actifs |
| `arcee/trinity-mini` | Trinity Mini 26B | texte | 128K | 0,045 /0,15/ 0,15/0,15 | Rapide et économique ; appel de fonction |

Le préréglage d’onboarding définit `arcee/trinity-large-thinking` comme modèle par défaut.

## [​](https://docs.openclaw.ai/fr/providers/arcee\#fonctionnalit%C3%A9s-prises-en-charge)  Fonctionnalités prises en charge

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

## [​](https://docs.openclaw.ai/fr/providers/arcee\#connexe)  Connexe

[**OpenRouter** \\
\\
Accédez aux modèles Arcee et à de nombreux autres via une seule clé API.](https://docs.openclaw.ai/fr/providers/openrouter)

[**Model selection** \\
\\
Choisir les fournisseurs, les références de modèle et le comportement de basculement.](https://docs.openclaw.ai/fr/concepts/model-providers)

[Anthropic](https://docs.openclaw.ai/fr/providers/anthropic) [Azure Speech](https://docs.openclaw.ai/fr/providers/azure-speech)

Ctrl+I