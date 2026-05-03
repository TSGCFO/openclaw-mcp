---
source_url: https://docs.openclaw.ai/fr/providers/bedrock
title: "Amazon Bedrock - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/providers/bedrock#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Providers

Amazon Bedrock

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Bien démarrer](https://docs.openclaw.ai/fr/providers/bedrock#bien-d%C3%A9marrer)
- [Découverte automatique des modèles](https://docs.openclaw.ai/fr/providers/bedrock#d%C3%A9couverte-automatique-des-mod%C3%A8les)
- [Configuration rapide (chemin AWS)](https://docs.openclaw.ai/fr/providers/bedrock#configuration-rapide-chemin-aws)
- [Configuration avancée](https://docs.openclaw.ai/fr/providers/bedrock#configuration-avanc%C3%A9e)
- [Connexe](https://docs.openclaw.ai/fr/providers/bedrock#connexe)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw peut utiliser les modèles **Amazon Bedrock** via le fournisseur de streaming **Bedrock Converse** de pi-ai. L’authentification Bedrock utilise la **chaîne d’identifiants par défaut du SDK AWS**, et non une clé API.

| Propriété | Valeur |
| --- | --- |
| Fournisseur | `amazon-bedrock` |
| API | `bedrock-converse-stream` |
| Authentification | Identifiants AWS (variables d’environnement, configuration partagée ou rôle d’instance) |
| Région | `AWS_REGION` ou `AWS_DEFAULT_REGION` (par défaut : `us-east-1`) |

## [​](https://docs.openclaw.ai/fr/providers/bedrock\#bien-d%C3%A9marrer)  Bien démarrer

Choisissez votre méthode d’authentification préférée et suivez les étapes de configuration.

- Access keys / env vars

- EC2 instance roles (IMDS)


**Idéal pour :** les machines de développement, la CI ou les hôtes où vous gérez directement les identifiants AWS.

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Set AWS credentials on the gateway host

```
export AWS_ACCESS_KEY_ID="AKIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_REGION="us-east-1"
# Optional:
export AWS_SESSION_TOKEN="..."
export AWS_PROFILE="your-profile"
# Optional (Bedrock API key/bearer token):
export AWS_BEARER_TOKEN_BEDROCK="..."
```

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Add a Bedrock provider and model to your config

Aucun `apiKey` n’est requis. Configurez le fournisseur avec `auth: "aws-sdk"` :

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

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Verify models are available

```
openclaw models list
```

Avec l’authentification par marqueur d’environnement (`AWS_ACCESS_KEY_ID`, `AWS_PROFILE` ou `AWS_BEARER_TOKEN_BEDROCK`), OpenClaw active automatiquement le fournisseur Bedrock implicite pour la découverte de modèles sans configuration supplémentaire.

**Idéal pour :** les instances EC2 auxquelles un rôle IAM est attaché, utilisant le service de métadonnées d’instance pour l’authentification.

1

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Enable discovery explicitly

Avec IMDS, OpenClaw ne peut pas détecter l’authentification AWS à partir des seuls marqueurs d’environnement ; vous devez donc l’activer explicitement :

```
openclaw config set plugins.entries.amazon-bedrock.config.discovery.enabled true
openclaw config set plugins.entries.amazon-bedrock.config.discovery.region us-east-1
```

2

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Optionally add an env marker for auto mode

Si vous voulez aussi que le chemin d’auto-détection par marqueur d’environnement fonctionne (par exemple, pour les surfaces `openclaw status`) :

```
export AWS_PROFILE=default
export AWS_REGION=us-east-1
```

Vous n’avez **pas** besoin d’une fausse clé API.

3

[Navigate to header](https://docs.openclaw.ai/fr/providers/bedrock#)

Verify models are discovered

```
openclaw models list
```

Le rôle IAM attaché à votre instance EC2 doit disposer des autorisations suivantes :

- `bedrock:InvokeModel`
- `bedrock:InvokeModelWithResponseStream`
- `bedrock:ListFoundationModels` (pour la découverte automatique)
- `bedrock:ListInferenceProfiles` (pour la découverte des profils d’inférence)

Ou attachez la politique gérée `AmazonBedrockFullAccess`.

Vous n’avez besoin de `AWS_PROFILE=default` que si vous voulez spécifiquement un marqueur d’environnement pour le mode automatique ou les surfaces de statut. Le chemin réel d’authentification d’exécution Bedrock utilise la chaîne par défaut du SDK AWS ; l’authentification par rôle d’instance IMDS fonctionne donc même sans marqueurs d’environnement.

## [​](https://docs.openclaw.ai/fr/providers/bedrock\#d%C3%A9couverte-automatique-des-mod%C3%A8les)  Découverte automatique des modèles

OpenClaw peut découvrir automatiquement les modèles Bedrock qui prennent en charge le **streaming**
et la **sortie texte**. La découverte utilise `bedrock:ListFoundationModels` et
`bedrock:ListInferenceProfiles`, et les résultats sont mis en cache (par défaut : 1 heure).Comment le fournisseur implicite est activé :

- Si `plugins.entries.amazon-bedrock.config.discovery.enabled` vaut `true`,
OpenClaw tentera la découverte même lorsqu’aucun marqueur d’environnement AWS n’est présent.
- Si `plugins.entries.amazon-bedrock.config.discovery.enabled` n’est pas défini,
OpenClaw n’ajoute automatiquement le
fournisseur Bedrock implicite que lorsqu’il voit l’un de ces marqueurs d’authentification AWS :
`AWS_BEARER_TOKEN_BEDROCK`, `AWS_ACCESS_KEY_ID` +
`AWS_SECRET_ACCESS_KEY` ou `AWS_PROFILE`.
- Le chemin réel d’authentification d’exécution Bedrock utilise toujours la chaîne par défaut du SDK AWS, donc
la configuration partagée, SSO et l’authentification par rôle d’instance IMDS peuvent fonctionner même lorsque la découverte
nécessitait `enabled: true` pour être activée explicitement.

Pour les entrées explicites `models.providers["amazon-bedrock"]`, OpenClaw peut toujours résoudre tôt l’authentification Bedrock par marqueur d’environnement à partir de marqueurs d’environnement AWS tels que `AWS_BEARER_TOKEN_BEDROCK`, sans forcer le chargement complet de l’authentification d’exécution. Le chemin réel d’authentification des appels de modèle utilise toujours la chaîne par défaut du SDK AWS.

Discovery config options

Les options de configuration se trouvent sous `plugins.entries.amazon-bedrock.config.discovery` :

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

| Option | Par défaut | Description |
| --- | --- | --- |
| `enabled` | auto | En mode automatique, OpenClaw n’active le fournisseur Bedrock implicite que lorsqu’il voit un marqueur d’environnement AWS pris en charge. Définissez `true` pour forcer la découverte. |
| `region` | `AWS_REGION` / `AWS_DEFAULT_REGION` / `us-east-1` | Région AWS utilisée pour les appels d’API de découverte. |
| `providerFilter` | (tous) | Correspond aux noms de fournisseurs Bedrock (par exemple `anthropic`, `amazon`). |
| `refreshInterval` | `3600` | Durée du cache en secondes. Définissez `0` pour désactiver la mise en cache. |
| `defaultContextWindow` | `32000` | Fenêtre de contexte utilisée pour les modèles découverts (remplacez si vous connaissez les limites de votre modèle). |
| `defaultMaxTokens` | `4096` | Nombre maximal de jetons de sortie utilisé pour les modèles découverts (remplacez si vous connaissez les limites de votre modèle). |

## [​](https://docs.openclaw.ai/fr/providers/bedrock\#configuration-rapide-chemin-aws)  Configuration rapide (chemin AWS)

Ce guide crée un rôle IAM, attache les autorisations Bedrock, associe
le profil d’instance et active la découverte OpenClaw sur l’hôte EC2.

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

## [​](https://docs.openclaw.ai/fr/providers/bedrock\#configuration-avanc%C3%A9e)  Configuration avancée

Inference profiles

OpenClaw découvre les **profils d’inférence régionaux et globaux** en même temps que
les modèles de fondation. Lorsqu’un profil correspond à un modèle de fondation connu, le
profil hérite des capacités de ce modèle (fenêtre de contexte, jetons maximum,
raisonnement, vision) et la région correcte de requête Bedrock est injectée
automatiquement. Cela signifie que les profils Claude interrégionaux fonctionnent sans
remplacements manuels du fournisseur.Les ID de profils d’inférence ressemblent à `us.anthropic.claude-opus-4-6-v1:0` (régional)
ou `anthropic.claude-opus-4-6-v1:0` (global). Si le modèle sous-jacent est déjà
dans les résultats de découverte, le profil hérite de l’ensemble complet de ses capacités ;
sinon, des valeurs par défaut sûres s’appliquent.Aucune configuration supplémentaire n’est nécessaire. Tant que la découverte est activée et que le principal IAM
dispose de `bedrock:ListInferenceProfiles`, les profils apparaissent avec
les modèles de fondation dans `openclaw models list`.

Claude Opus 4.7 temperature

Bedrock rejette le paramètre `temperature` pour Claude Opus 4.7. OpenClaw
omet automatiquement `temperature` pour toute référence Bedrock Opus 4.7, y compris
les ID de modèles de fondation, les profils d’inférence nommés, les profils d’inférence
d’application dont le modèle sous-jacent se résout en Opus 4.7 via
`bedrock:GetInferenceProfile`, et les variantes pointées `opus-4.7` avec
préfixes de région facultatifs (`us.`, `eu.`, `ap.`, `apac.`, `au.`, `jp.`,
`global.`). Aucun réglage de configuration n’est requis, et l’omission s’applique à la fois à
l’objet d’options de requête et au champ de charge utile `inferenceConfig`.

Guardrails

Vous pouvez appliquer [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
à tous les appels de modèles Bedrock en ajoutant un objet `guardrail` à la
configuration du Plugin `amazon-bedrock`. Les garde-fous vous permettent d’imposer le filtrage de contenu,
le refus de sujets, des filtres de mots, des filtres d’informations sensibles et des contrôles
d’ancrage contextuel.

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

| Option | Obligatoire | Description |
| --- | --- | --- |
| `guardrailIdentifier` | Oui | ID du garde-fou (p. ex. `abc123`) ou ARN complet (p. ex. `arn:aws:bedrock:us-east-1:123456789012:guardrail/abc123`). |
| `guardrailVersion` | Oui | Numéro de version publiée, ou `"DRAFT"` pour le brouillon de travail. |
| `streamProcessingMode` | Non | `"sync"` ou `"async"` pour l’évaluation des garde-fous pendant le streaming. Si omis, Bedrock utilise sa valeur par défaut. |
| `trace` | Non | `"enabled"` ou `"enabled_full"` pour le débogage ; omettez ou définissez `"disabled"` pour la production. |

Le principal IAM utilisé par le Gateway doit disposer de l’autorisation `bedrock:ApplyGuardrail` en plus des autorisations d’appel standard.

Intégrations pour la recherche en mémoire

Bedrock peut également servir de fournisseur d’intégrations pour la
[recherche en mémoire](https://docs.openclaw.ai/fr/concepts/memory-search). Cette option est configurée séparément du
fournisseur d’inférence — définissez `agents.defaults.memorySearch.provider` sur `"bedrock"` :

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

Les intégrations Bedrock utilisent la même chaîne d’identifiants du SDK AWS que l’inférence (rôles
d’instance, SSO, clés d’accès, configuration partagée et identité web). Aucune clé d’API n’est
nécessaire. Lorsque `provider` vaut `"auto"`, Bedrock est détecté automatiquement si cette
chaîne d’identifiants se résout correctement.Les modèles d’intégration pris en charge incluent Amazon Titan Embed (v1, v2), Amazon Nova
Embed, Cohere Embed (v3, v4) et TwelveLabs Marengo. Consultez la
[référence de configuration de la mémoire — Bedrock](https://docs.openclaw.ai/fr/reference/memory-config#bedrock-embedding-config)
pour la liste complète des modèles et les options de dimensions.

Notes et mises en garde

- Bedrock nécessite que **l’accès au modèle** soit activé dans votre compte/région AWS.
- La découverte automatique nécessite les autorisations `bedrock:ListFoundationModels` et
`bedrock:ListInferenceProfiles`.
- Si vous utilisez le mode automatique, définissez l’un des marqueurs d’environnement d’authentification AWS pris en charge sur
l’hôte Gateway. Si vous préférez l’authentification IMDS/configuration partagée sans marqueurs d’environnement, définissez
`plugins.entries.amazon-bedrock.config.discovery.enabled: true`.
- OpenClaw expose la source des identifiants dans cet ordre : `AWS_BEARER_TOKEN_BEDROCK`,
puis `AWS_ACCESS_KEY_ID` \+ `AWS_SECRET_ACCESS_KEY`, puis `AWS_PROFILE`, puis la
chaîne par défaut du SDK AWS.
- La prise en charge du raisonnement dépend du modèle ; consultez la fiche du modèle Bedrock pour
connaître les capacités actuelles.
- Si vous préférez un flux de clés géré, vous pouvez également placer un proxy compatible OpenAI
devant Bedrock et le configurer comme fournisseur OpenAI à la place.

## [​](https://docs.openclaw.ai/fr/providers/bedrock\#connexe)  Connexe

[**Sélection de modèle** \\
\\
Choix des fournisseurs, des références de modèles et du comportement de basculement.](https://docs.openclaw.ai/fr/concepts/model-providers)

[**Recherche en mémoire** \\
\\
Intégrations Bedrock pour la configuration de la recherche en mémoire.](https://docs.openclaw.ai/fr/concepts/memory-search)

[**Référence de configuration de la mémoire** \\
\\
Liste complète des modèles d’intégration Bedrock et options de dimensions.](https://docs.openclaw.ai/fr/reference/memory-config#bedrock-embedding-config)

[**Dépannage** \\
\\
Dépannage général et FAQ.](https://docs.openclaw.ai/fr/help/troubleshooting)

[Alibaba Model Studio](https://docs.openclaw.ai/fr/providers/alibaba) [Amazon Bedrock Mantle](https://docs.openclaw.ai/fr/providers/bedrock-mantle)

Ctrl+I