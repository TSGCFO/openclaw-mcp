---
source_url: https://docs.openclaw.ai/fr/providers/inferrs
title: "Inferrs - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/inferrs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Providers

Inferrs

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Bien démarrer](https://docs.openclaw.ai/fr/providers/inferrs#bien-d%C3%A9marrer)
- [Exemple complet de configuration](https://docs.openclaw.ai/fr/providers/inferrs#exemple-complet-de-configuration)
- [Configuration avancée](https://docs.openclaw.ai/fr/providers/inferrs#configuration-avanc%C3%A9e)
- [Dépannage](https://docs.openclaw.ai/fr/providers/inferrs#d%C3%A9pannage)
- [Associé](https://docs.openclaw.ai/fr/providers/inferrs#associ%C3%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[inferrs](https://github.com/ericcurtin/inferrs) peut servir des modèles locaux derrière une
API `/v1` compatible OpenAI. OpenClaw fonctionne avec `inferrs` via le chemin générique
`openai-completions`.`inferrs` est actuellement mieux traité comme un backend OpenAI-compatible auto-hébergé personnalisé,
et non comme un plugin fournisseur OpenClaw dédié.

## [​](https://docs.openclaw.ai/fr/providers/inferrs\#bien-d%C3%A9marrer)  Bien démarrer

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

## [​](https://docs.openclaw.ai/fr/providers/inferrs\#exemple-complet-de-configuration)  Exemple complet de configuration

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

## [​](https://docs.openclaw.ai/fr/providers/inferrs\#configuration-avanc%C3%A9e)  Configuration avancée

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

## [​](https://docs.openclaw.ai/fr/providers/inferrs\#d%C3%A9pannage)  Dépannage

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

## [​](https://docs.openclaw.ai/fr/providers/inferrs\#associ%C3%A9)  Associé

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