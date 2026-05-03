---
source_url: https://docs.openclaw.ai/fr/gateway/logging
title: "Journalisation du Gateway - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/gateway/logging#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Health and diagnostics

Journalisation du Gateway

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Journalisation](https://docs.openclaw.ai/fr/gateway/logging#journalisation)
- [Journaliseur basé sur des fichiers](https://docs.openclaw.ai/fr/gateway/logging#journaliseur-bas%C3%A9-sur-des-fichiers)
- [Capture de la console](https://docs.openclaw.ai/fr/gateway/logging#capture-de-la-console)
- [Biffage](https://docs.openclaw.ai/fr/gateway/logging#biffage)
- [Journaux WebSocket du Gateway](https://docs.openclaw.ai/fr/gateway/logging#journaux-websocket-du-gateway)
- [Style de journal WS](https://docs.openclaw.ai/fr/gateway/logging#style-de-journal-ws)
- [Formatage de la console (journalisation par sous-système)](https://docs.openclaw.ai/fr/gateway/logging#formatage-de-la-console-journalisation-par-sous-syst%C3%A8me)
- [Liens connexes](https://docs.openclaw.ai/fr/gateway/logging#liens-connexes)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/fr/gateway/logging\#journalisation)  Journalisation

Pour une vue d’ensemble destinée aux utilisateurs (CLI + interface de contrôle + configuration), consultez [/logging](https://docs.openclaw.ai/fr/logging).OpenClaw possède deux « surfaces » de journalisation :

- **Sortie console** (ce que vous voyez dans le terminal / l’interface de débogage).
- **Journaux de fichiers** (lignes JSON) écrits par le journaliseur du Gateway.

## [​](https://docs.openclaw.ai/fr/gateway/logging\#journaliseur-bas%C3%A9-sur-des-fichiers)  Journaliseur basé sur des fichiers

- Le fichier journal rotatif par défaut se trouve sous `/tmp/openclaw/` (un fichier par jour) : `openclaw-YYYY-MM-DD.log`
  - La date utilise le fuseau horaire local de l’hôte du Gateway.
- Les fichiers journaux actifs sont remplacés à `logging.maxFileBytes` (par défaut : 100 Mo), en conservant
jusqu’à cinq archives numérotées et en continuant à écrire dans un nouveau fichier actif.
- Le chemin et le niveau du fichier journal peuvent être configurés via `~/.openclaw/openclaw.json` :

  - `logging.file`
  - `logging.level`

Le format de fichier est d’un objet JSON par ligne.L’onglet Journaux de l’interface de contrôle suit ce fichier via le Gateway (`logs.tail`).
La CLI peut faire la même chose :

```
openclaw logs --follow
```

**Verbosité vs niveaux de journalisation**

- Les **journaux de fichiers** sont contrôlés exclusivement par `logging.level`.
- `--verbose` affecte uniquement la **verbosité de la console** (et le style des journaux WS) ; il ne
relève **pas** le niveau de journalisation des fichiers.
- Pour capturer dans les journaux de fichiers les détails visibles uniquement en mode verbeux, définissez `logging.level` sur `debug` ou
`trace`.
- La journalisation de trace inclut également des résumés de minutage de diagnostic pour certains chemins critiques,
comme la préparation de la fabrique d’outils de Plugin. Voir
[/tools/plugin#slow-plugin-tool-setup](https://docs.openclaw.ai/fr/tools/plugin#slow-plugin-tool-setup).

## [​](https://docs.openclaw.ai/fr/gateway/logging\#capture-de-la-console)  Capture de la console

La CLI capture `console.log/info/warn/error/debug/trace` et les écrit dans les journaux de fichiers,
tout en continuant à les imprimer vers stdout/stderr.Vous pouvez régler indépendamment la verbosité de la console via :

- `logging.consoleLevel` (par défaut `info`)
- `logging.consoleStyle` (`pretty` \| `compact` \| `json`)

## [​](https://docs.openclaw.ai/fr/gateway/logging\#biffage)  Biffage

OpenClaw peut masquer les jetons sensibles avant que la sortie de journal ou de transcription ne quitte le
processus. Cette politique de biffage de journalisation est appliquée aux sorties console, journaux de fichiers, enregistrements de journaux OTLP
et texte de transcription de session, afin que les valeurs secrètes correspondantes soient
masquées avant l’écriture des lignes JSONL ou des messages sur disque.

- `logging.redactSensitive` : `off` \| `tools` (par défaut : `tools`)
- `logging.redactPatterns`: tableau de chaînes regex (remplace les valeurs par défaut)

  - Utilisez des chaînes regex brutes (`gi` automatique), ou `/pattern/flags` si vous avez besoin d’indicateurs personnalisés.
  - Les correspondances sont masquées en conservant les 6 premiers + 4 derniers caractères (longueur >= 18), sinon `***`.
  - Les valeurs par défaut couvrent les affectations de clés courantes, les options CLI, les champs JSON, les en-têtes bearer, les blocs PEM, les préfixes de jetons populaires et les noms de champs d’identifiants de paiement tels que numéro de carte, CVC/CVV, jeton de paiement partagé et identifiant de paiement.

Certaines limites de sécurité biffent toujours, quelle que soit la valeur de `logging.redactSensitive`.
Cela inclut les événements d’appels d’outils de l’interface de contrôle, la sortie de l’outil `sessions_history`,
les exports de support de diagnostics, les observations d’erreurs de fournisseurs, l’affichage des commandes
d’approbation exec et les journaux du protocole WebSocket du Gateway. Ces surfaces peuvent toujours utiliser
`logging.redactPatterns` comme motifs supplémentaires, mais `redactSensitive: "off"`
ne leur fait pas émettre de secrets bruts.

## [​](https://docs.openclaw.ai/fr/gateway/logging\#journaux-websocket-du-gateway)  Journaux WebSocket du Gateway

Le Gateway imprime les journaux du protocole WebSocket dans deux modes :

- **Mode normal (sans `--verbose`)**: seuls les résultats RPC « intéressants » sont imprimés :

  - erreurs (`ok=false`)
  - appels lents (seuil par défaut : `>= 50ms`)
  - erreurs d’analyse
- **Mode verbeux (`--verbose`)** : imprime tout le trafic de requête/réponse WS.

### [​](https://docs.openclaw.ai/fr/gateway/logging\#style-de-journal-ws)  Style de journal WS

`openclaw gateway` prend en charge un sélecteur de style par Gateway :

- `--ws-log auto` (par défaut) : le mode normal est optimisé ; le mode verbeux utilise une sortie compacte
- `--ws-log compact` : sortie compacte (requête/réponse associées) en mode verbeux
- `--ws-log full` : sortie complète par trame en mode verbeux
- `--compact` : alias de `--ws-log compact`

Exemples :

```
# optimized (only errors/slow)
openclaw gateway

# show all WS traffic (paired)
openclaw gateway --verbose --ws-log compact

# show all WS traffic (full meta)
openclaw gateway --verbose --ws-log full
```

## [​](https://docs.openclaw.ai/fr/gateway/logging\#formatage-de-la-console-journalisation-par-sous-syst%C3%A8me)  Formatage de la console (journalisation par sous-système)

Le formateur de console est **sensible au TTY** et imprime des lignes cohérentes et préfixées.
Les journaliseurs de sous-systèmes gardent la sortie groupée et facile à parcourir.Comportement :

- **Préfixes de sous-système** sur chaque ligne (p. ex. `[gateway]`, `[canvas]`, `[tailscale]`)
- **Couleurs de sous-système** (stables par sous-système) plus coloration par niveau
- **Couleur lorsque la sortie est un TTY ou que l’environnement ressemble à un terminal riche** (`TERM`/`COLORTERM`/`TERM_PROGRAM`), respecte `NO_COLOR`
- **Préfixes de sous-système raccourcis** : supprime les préfixes `gateway/` \+ `channels/`, conserve les 2 derniers segments (p. ex. `whatsapp/outbound`)
- **Sous-journaliseurs par sous-système** (préfixe automatique + champ structuré `{ subsystem }`)
- **`logRaw()`** pour les sorties QR/UX (pas de préfixe, pas de formatage)
- **Styles de console** (p. ex. `pretty | compact | json`)
- **Niveau de journalisation console** distinct du niveau de journalisation des fichiers (le fichier conserve tous les détails lorsque `logging.level` est défini sur `debug`/`trace`)
- **Les corps de messages WhatsApp** sont journalisés à `debug` (utilisez `--verbose` pour les voir)

Cela garde les journaux de fichiers existants stables tout en rendant la sortie interactive facile à parcourir.

## [​](https://docs.openclaw.ai/fr/gateway/logging\#liens-connexes)  Liens connexes

- [Journalisation](https://docs.openclaw.ai/fr/logging)
- [Export OpenTelemetry](https://docs.openclaw.ai/fr/gateway/opentelemetry)
- [Export des diagnostics](https://docs.openclaw.ai/fr/gateway/diagnostics)

[Prometheus](https://docs.openclaw.ai/fr/gateway/prometheus) [Exportation des diagnostics](https://docs.openclaw.ai/fr/gateway/diagnostics)

Ctrl+I