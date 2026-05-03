---
source_url: https://docs.openclaw.ai/fr/providers/zai
title: "Z.AI - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/zai#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Providers

Z.AI

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Bien démarrer](https://docs.openclaw.ai/fr/providers/zai#bien-d%C3%A9marrer)
- [Catalogue intégré](https://docs.openclaw.ai/fr/providers/zai#catalogue-int%C3%A9gr%C3%A9)
- [Configuration avancée](https://docs.openclaw.ai/fr/providers/zai#configuration-avanc%C3%A9e)
- [Connexe](https://docs.openclaw.ai/fr/providers/zai#connexe)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Z.AI est la plateforme d’API pour les modèles **GLM**. Elle fournit des API REST pour GLM et utilise des clés d’API
pour l’authentification. Créez votre clé d’API dans la console Z.AI. OpenClaw utilise le fournisseur `zai`
avec une clé d’API Z.AI.

- Fournisseur : `zai`
- Authentification : `ZAI_API_KEY`
- API : Chat Completions Z.AI (authentification Bearer)

## [​](https://docs.openclaw.ai/fr/providers/zai\#bien-d%C3%A9marrer)  Bien démarrer

- Détection automatique du point de terminaison

- Point de terminaison régional explicite


**Idéal pour :** la plupart des utilisateurs. OpenClaw détecte le point de terminaison Z.AI correspondant à partir de la clé et applique automatiquement la bonne URL de base.

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Exécuter l’onboarding

```
openclaw onboard --auth-choice zai-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Définir un modèle par défaut

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Vérifier que le modèle est listé

```
openclaw models list --all --provider zai
```

**Idéal pour :** les utilisateurs qui veulent forcer un Coding Plan spécifique ou une surface d’API générale.

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Choisir la bonne option d’onboarding

```
# Coding Plan Global (recommandé pour les utilisateurs du Coding Plan)
openclaw onboard --auth-choice zai-coding-global

# Coding Plan CN (région Chine)
openclaw onboard --auth-choice zai-coding-cn

# API générale
openclaw onboard --auth-choice zai-global

# API générale CN (région Chine)
openclaw onboard --auth-choice zai-cn
```

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Définir un modèle par défaut

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/zai#)

Vérifier que le modèle est listé

```
openclaw models list --all --provider zai
```

## [​](https://docs.openclaw.ai/fr/providers/zai\#catalogue-int%C3%A9gr%C3%A9)  Catalogue intégré

OpenClaw fournit le catalogue du fournisseur `zai` groupé dans le manifeste du plugin, afin que le
listing en lecture seule puisse afficher les lignes GLM connues sans charger le runtime du fournisseur :

```
openclaw models list --all --provider zai
```

Le catalogue basé sur le manifeste comprend actuellement :

| Réf. de modèle | Notes |
| --- | --- |
| `zai/glm-5.1` | Modèle par défaut |
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

Les modèles GLM sont disponibles sous la forme `zai/<model>` (exemple : `zai/glm-5`). La référence du modèle groupé par défaut est `zai/glm-5.1`.

## [​](https://docs.openclaw.ai/fr/providers/zai\#configuration-avanc%C3%A9e)  Configuration avancée

Résolution prospective des modèles GLM-5 inconnus

Les identifiants `glm-5*` inconnus sont encore résolus prospectivement sur le chemin du fournisseur groupé en
synthétisant les métadonnées appartenant au fournisseur à partir du modèle `glm-4.7` lorsque l’identifiant
correspond à la forme actuelle de la famille GLM-5.

Streaming des appels d’outils

`tool_stream` est activé par défaut pour le streaming des appels d’outils Z.AI. Pour le désactiver :

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

Thinking et thinking préservé

Le thinking Z.AI suit les contrôles `/think` d’OpenClaw. Lorsque le thinking est désactivé,
OpenClaw envoie `thinking: { type: "disabled" }` pour éviter les réponses qui
dépensent le budget de sortie en `reasoning_content` avant le texte visible.Le thinking préservé est opt-in, car Z.AI exige que l’intégralité de l’historique
`reasoning_content` soit rejouée, ce qui augmente le nombre de tokens du prompt. Activez-le
par modèle :

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

Lorsqu’il est activé et que le thinking est actif, OpenClaw envoie
`thinking: { type: "enabled", clear_thinking: false }` et rejoue le
`reasoning_content` précédent pour la même transcription compatible OpenAI.Les utilisateurs avancés peuvent toujours remplacer exactement la charge utile du fournisseur avec
`params.extra_body.thinking`.

Compréhension d’image

Le plugin Z.AI groupé enregistre la compréhension d’image.

| Propriété | Valeur |
| --- | --- |
| Modèle | `glm-4.6v` |

La compréhension d’image est automatiquement résolue à partir de l’authentification Z.AI configurée ; aucune
configuration supplémentaire n’est nécessaire.

Détails d’authentification

- Z.AI utilise l’authentification Bearer avec votre clé d’API.
- L’option d’onboarding `zai-api-key` détecte automatiquement le point de terminaison Z.AI correspondant à partir du préfixe de la clé.
- Utilisez les options régionales explicites (`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) lorsque vous voulez forcer une surface d’API spécifique.

## [​](https://docs.openclaw.ai/fr/providers/zai\#connexe)  Connexe

[**Famille de modèles GLM** \\
\\
Vue d’ensemble de la famille de modèles GLM.](https://docs.openclaw.ai/fr/providers/glm)

[**Sélection de modèle** \\
\\
Choisir les fournisseurs, les références de modèle et le comportement de basculement.](https://docs.openclaw.ai/fr/concepts/model-providers)

[Xiaomi MiMo](https://docs.openclaw.ai/fr/providers/xiaomi)

Ctrl+I