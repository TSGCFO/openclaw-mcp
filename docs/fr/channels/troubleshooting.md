---
source_url: https://docs.openclaw.ai/fr/channels/troubleshooting
title: "D\u00e9pannage des canaux - OpenClaw"
---

[Passer au contenu principal](https://docs.openclaw.ai/fr/channels/troubleshooting#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/fr)

![FR](https://d3gk2c5xim1je2.cloudfront.net/flags/FR.svg)

Français

Rechercher...

Ctrl K

Rechercher...

Navigation

Configuration

Dépannage des canaux

[Get started](https://docs.openclaw.ai/fr) [Install](https://docs.openclaw.ai/fr/install) [Channels](https://docs.openclaw.ai/fr/channels) [Agents](https://docs.openclaw.ai/fr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/fr/tools) [Models](https://docs.openclaw.ai/fr/providers) [Platforms](https://docs.openclaw.ai/fr/platforms) [Gateway & Ops](https://docs.openclaw.ai/fr/gateway) [Reference](https://docs.openclaw.ai/fr/cli) [Help](https://docs.openclaw.ai/fr/help)

Sur cette page

- [Échelle de commandes](https://docs.openclaw.ai/fr/channels/troubleshooting#%C3%A9chelle-de-commandes)
- [WhatsApp](https://docs.openclaw.ai/fr/channels/troubleshooting#whatsapp)
- [Signatures d’échec WhatsApp](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-whatsapp)
- [Telegram](https://docs.openclaw.ai/fr/channels/troubleshooting#telegram)
- [Signatures d’échec Telegram](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-telegram)
- [Discord](https://docs.openclaw.ai/fr/channels/troubleshooting#discord)
- [Signatures d’échec Discord](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-discord)
- [Slack](https://docs.openclaw.ai/fr/channels/troubleshooting#slack)
- [Signatures d’échec Slack](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-slack)
- [iMessage et BlueBubbles](https://docs.openclaw.ai/fr/channels/troubleshooting#imessage-et-bluebubbles)
- [Signatures d’échec iMessage et BlueBubbles](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-imessage-et-bluebubbles)
- [Signal](https://docs.openclaw.ai/fr/channels/troubleshooting#signal)
- [Signatures d’échec Signal](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-signal)
- [QQ Bot](https://docs.openclaw.ai/fr/channels/troubleshooting#qq-bot)
- [Signatures d’échec QQ Bot](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-qq-bot)
- [Matrix](https://docs.openclaw.ai/fr/channels/troubleshooting#matrix)
- [Signatures d’échec Matrix](https://docs.openclaw.ai/fr/channels/troubleshooting#signatures-d%E2%80%99%C3%A9chec-matrix)
- [Connexe](https://docs.openclaw.ai/fr/channels/troubleshooting#connexe)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Utilisez cette page lorsqu’un canal se connecte, mais que le comportement est incorrect.

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#%C3%A9chelle-de-commandes)  Échelle de commandes

Exécutez d’abord ces commandes dans l’ordre :

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

État de référence sain :

- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, ou `admin-capable`
- La sonde du canal indique que le transport est connecté et, lorsque c’est pris en charge, `works` ou `audit ok`

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#whatsapp)  WhatsApp

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-whatsapp)  Signatures d’échec WhatsApp

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Connecté, mais aucune réponse DM | `openclaw pairing list whatsapp` | Approuvez l’expéditeur ou modifiez la politique DM/liste d’autorisation. |
| Messages de groupe ignorés | Vérifiez `requireMention` \+ les modèles de mention dans la configuration | Mentionnez le bot ou assouplissez la politique de mention pour ce groupe. |
| La connexion QR expire avec 408 | Vérifiez les variables d’environnement `HTTPS_PROXY` / `HTTP_PROXY` du Gateway | Définissez un proxy joignable ; utilisez `NO_PROXY` uniquement pour les contournements. |
| Boucles aléatoires de déconnexion/reconnexion | `openclaw channels status --probe` \+ journaux | Les reconnexions récentes sont signalées même lorsque le canal est actuellement connecté ; surveillez les journaux, redémarrez le Gateway, puis réassociez si l’instabilité continue. |

Dépannage complet : [Dépannage WhatsApp](https://docs.openclaw.ai/fr/channels/whatsapp#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#telegram)  Telegram

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-telegram)  Signatures d’échec Telegram

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| `/start`, mais aucun flux de réponse utilisable | `openclaw pairing list telegram` | Approuvez l’association ou modifiez la politique DM. |
| Bot en ligne, mais le groupe reste silencieux | Vérifiez l’exigence de mention et le mode confidentialité du bot | Désactivez le mode confidentialité pour la visibilité du groupe ou mentionnez le bot. |
| Échecs d’envoi avec erreurs réseau | Inspectez les journaux pour les échecs d’appels à l’API Telegram | Corrigez le routage DNS/IPv6/proxy vers `api.telegram.org`. |
| Le démarrage signale `getMe returned 401` | Vérifiez la source du jeton configurée | Recopiez ou régénérez le jeton BotFather et mettez à jour `botToken`, `tokenFile` ou le compte par défaut `TELEGRAM_BOT_TOKEN`. |
| Le polling se bloque ou se reconnecte lentement | `openclaw logs --follow` pour les diagnostics de polling | Mettez à niveau ; si les redémarrages sont des faux positifs, ajustez `pollingStallThresholdMs`. Les blocages persistants indiquent toujours un problème de proxy/DNS/IPv6. |
| `setMyCommands` rejeté au démarrage | Inspectez les journaux pour `BOT_COMMANDS_TOO_MUCH` | Réduisez les commandes Telegram de Plugin/Skills/personnalisées ou désactivez les menus natifs. |
| Après mise à niveau, la liste d’autorisation vous bloque | `openclaw security audit` et listes d’autorisation de la configuration | Exécutez `openclaw doctor --fix` ou remplacez `@username` par des ID d’expéditeur numériques. |

Dépannage complet : [Dépannage Telegram](https://docs.openclaw.ai/fr/channels/telegram#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#discord)  Discord

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-discord)  Signatures d’échec Discord

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Bot en ligne, mais aucune réponse de serveur | `openclaw channels status --probe` | Autorisez le serveur/canal et vérifiez l’intention de contenu des messages. |
| Messages de groupe ignorés | Vérifiez les journaux pour les rejets dus au filtrage par mention | Mentionnez le bot ou définissez `requireMention: false` pour le serveur/canal. |
| Réponses DM manquantes | `openclaw pairing list discord` | Approuvez l’association DM ou ajustez la politique DM. |

Dépannage complet : [Dépannage Discord](https://docs.openclaw.ai/fr/channels/discord#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#slack)  Slack

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-slack)  Signatures d’échec Slack

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Socket mode connecté, mais aucune réponse | `openclaw channels status --probe` | Vérifiez le jeton d’application + le jeton de bot et les portées requises ; surveillez `botTokenStatus` / `appTokenStatus = configured_unavailable` sur les configurations adossées à SecretRef. |
| DM bloqués | `openclaw pairing list slack` | Approuvez l’association ou assouplissez la politique DM. |
| Message de canal ignoré | Vérifiez `groupPolicy` et la liste d’autorisation du canal | Autorisez le canal ou passez la politique à `open`. |

Dépannage complet : [Dépannage Slack](https://docs.openclaw.ai/fr/channels/slack#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#imessage-et-bluebubbles)  iMessage et BlueBubbles

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-imessage-et-bluebubbles)  Signatures d’échec iMessage et BlueBubbles

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Aucun événement entrant | Vérifiez la joignabilité du webhook/serveur et les autorisations de l’application | Corrigez l’URL du webhook ou l’état du serveur BlueBubbles. |
| Envoi possible, mais aucune réception sur macOS | Vérifiez les autorisations de confidentialité macOS pour l’automatisation de Messages | Réaccordez les autorisations TCC et redémarrez le processus du canal. |
| Expéditeur DM bloqué | `openclaw pairing list imessage` ou `openclaw pairing list bluebubbles` | Approuvez l’association ou mettez à jour la liste d’autorisation. |

Dépannage complet :

- [Dépannage iMessage](https://docs.openclaw.ai/fr/channels/imessage#troubleshooting)
- [Dépannage BlueBubbles](https://docs.openclaw.ai/fr/channels/bluebubbles#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signal)  Signal

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-signal)  Signatures d’échec Signal

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Démon joignable, mais bot silencieux | `openclaw channels status --probe` | Vérifiez l’URL/le compte du démon `signal-cli` et le mode de réception. |
| DM bloqué | `openclaw pairing list signal` | Approuvez l’expéditeur ou ajustez la politique DM. |
| Les réponses de groupe ne se déclenchent pas | Vérifiez la liste d’autorisation du groupe et les modèles de mention | Ajoutez l’expéditeur/le groupe ou assouplissez le filtrage. |

Dépannage complet : [Dépannage Signal](https://docs.openclaw.ai/fr/channels/signal#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#qq-bot)  QQ Bot

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-qq-bot)  Signatures d’échec QQ Bot

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Le bot répond « gone to Mars » | Vérifiez `appId` et `clientSecret` dans la configuration | Définissez les identifiants ou redémarrez le Gateway. |
| Aucun message entrant | `openclaw channels status --probe` | Vérifiez les identifiants sur la plateforme QQ Open Platform. |
| Voix non transcrite | Vérifiez la configuration du fournisseur STT | Configurez `channels.qqbot.stt` ou `tools.media.audio`. |
| Les messages proactifs n’arrivent pas | Vérifiez les exigences d’interaction de la plateforme QQ | QQ peut bloquer les messages initiés par le bot sans interaction récente. |

Dépannage complet : [Dépannage QQ Bot](https://docs.openclaw.ai/fr/channels/qqbot#troubleshooting)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#matrix)  Matrix

### [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#signatures-d%E2%80%99%C3%A9chec-matrix)  Signatures d’échec Matrix

| Symptôme | Vérification la plus rapide | Correction |
| --- | --- | --- |
| Connecté, mais ignore les messages de salon | `openclaw channels status --probe` | Vérifiez `groupPolicy`, la liste d’autorisation du salon et le filtrage par mention. |
| Les DM ne sont pas traités | `openclaw pairing list matrix` | Approuvez l’expéditeur ou ajustez la politique DM. |
| Les salons chiffrés échouent | `openclaw matrix verify status` | Revérifiez l’appareil, puis vérifiez `openclaw matrix verify backup status`. |
| La restauration de sauvegarde est en attente/en échec | `openclaw matrix verify backup status` | Exécutez `openclaw matrix verify backup restore` ou relancez avec une clé de récupération. |
| La signature croisée/l’amorçage semble incorrect | `openclaw matrix verify bootstrap` | Réparez le stockage secret, la signature croisée et l’état de sauvegarde en une seule passe. |

Configuration complète : [Matrix](https://docs.openclaw.ai/fr/channels/matrix)

## [​](https://docs.openclaw.ai/fr/channels/troubleshooting\#connexe)  Connexe

- [Association](https://docs.openclaw.ai/fr/channels/pairing)
- [Routage des canaux](https://docs.openclaw.ai/fr/channels/channel-routing)
- [Dépannage Gateway](https://docs.openclaw.ai/fr/gateway/troubleshooting)

[Analyse de l’emplacement des canaux](https://docs.openclaw.ai/fr/channels/location) [Canal AQ](https://docs.openclaw.ai/fr/channels/qa-channel)

Ctrl+I