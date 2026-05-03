---
source_url: https://docs.openclaw.ai/tr/channels
title: "Sohbet kanallar\u0131 - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Overview

Sohbet kanalları

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Teslim notları](https://docs.openclaw.ai/tr/channels#teslim-notlar%C4%B1)
- [Desteklenen kanallar](https://docs.openclaw.ai/tr/channels#desteklenen-kanallar)
- [Notlar](https://docs.openclaw.ai/tr/channels#notlar)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw, halihazırda kullandığınız herhangi bir sohbet uygulamasında sizinle konuşabilir. Her kanal Gateway üzerinden bağlanır.
Metin her yerde desteklenir; medya ve tepkiler kanala göre değişir.

## [​](https://docs.openclaw.ai/tr/channels\#teslim-notlar%C4%B1)  Teslim notları

- `![alt](url)` gibi markdown resim sözdizimi içeren Telegram yanıtları,
mümkün olduğunda son giden yolda medya yanıtlarına dönüştürülür.
- Slack çok kişili DM’leri grup sohbetleri olarak yönlendirilir; bu nedenle grup politikası, bahsetme
davranışı ve grup oturumu kuralları MPIM konuşmaları için geçerlidir.
- WhatsApp kurulumu isteğe bağlı kurulumdur: katılım akışı, Plugin paketi kurulmadan önce kurulum akışını gösterebilir ve Gateway, WhatsApp çalışma zamanını
yalnızca kanal gerçekten etkin olduğunda yükler.

## [​](https://docs.openclaw.ai/tr/channels\#desteklenen-kanallar)  Desteklenen kanallar

- [BlueBubbles](https://docs.openclaw.ai/tr/channels/bluebubbles) — **iMessage için önerilir**; tam özellik desteğiyle BlueBubbles macOS sunucusu REST API’sini kullanır (birlikte gelen Plugin; düzenleme, göndermeyi geri alma, efektler, tepkiler, grup yönetimi — düzenleme şu anda macOS 26 Tahoe’da bozuk).
- [Discord](https://docs.openclaw.ai/tr/channels/discord) — Discord Bot API + Gateway; sunucuları, kanalları ve DM’leri destekler.
- [Feishu](https://docs.openclaw.ai/tr/channels/feishu) — WebSocket üzerinden Feishu/Lark botu (birlikte gelen Plugin).
- [Google Chat](https://docs.openclaw.ai/tr/channels/googlechat) — HTTP Webhook üzerinden Google Chat API uygulaması (indirilebilir Plugin).
- [iMessage (eski)](https://docs.openclaw.ai/tr/channels/imessage) — imsg CLI üzerinden eski macOS entegrasyonu (kullanımdan kaldırıldı, yeni kurulumlar için BlueBubbles kullanın).
- [IRC](https://docs.openclaw.ai/tr/channels/irc) — Klasik IRC sunucuları; eşleştirme/izin verilenler listesi kontrolleriyle kanallar + DM’ler.
- [LINE](https://docs.openclaw.ai/tr/channels/line) — LINE Messaging API botu (indirilebilir Plugin).
- [Matrix](https://docs.openclaw.ai/tr/channels/matrix) — Matrix protokolü (indirilebilir Plugin).
- [Mattermost](https://docs.openclaw.ai/tr/channels/mattermost) — Bot API + WebSocket; kanallar, gruplar, DM’ler (indirilebilir Plugin).
- [Microsoft Teams](https://docs.openclaw.ai/tr/channels/msteams) — Bot Framework; kurumsal destek (birlikte gelen Plugin).
- [Nextcloud Talk](https://docs.openclaw.ai/tr/channels/nextcloud-talk) — Nextcloud Talk üzerinden kendi barındırdığınız sohbet (birlikte gelen Plugin).
- [Nostr](https://docs.openclaw.ai/tr/channels/nostr) — NIP-04 üzerinden merkeziyetsiz DM’ler (birlikte gelen Plugin).
- [QQ Bot](https://docs.openclaw.ai/tr/channels/qqbot) — QQ Bot API; özel sohbet, grup sohbeti ve zengin medya (birlikte gelen Plugin).
- [Signal](https://docs.openclaw.ai/tr/channels/signal) — signal-cli; gizlilik odaklı.
- [Slack](https://docs.openclaw.ai/tr/channels/slack) — Bolt SDK; çalışma alanı uygulamaları.
- [Synology Chat](https://docs.openclaw.ai/tr/channels/synology-chat) — Giden+gelen Webhook’lar üzerinden Synology NAS Chat (birlikte gelen Plugin).
- [Telegram](https://docs.openclaw.ai/tr/channels/telegram) — grammY üzerinden Bot API; grupları destekler.
- [Tlon](https://docs.openclaw.ai/tr/channels/tlon) — Urbit tabanlı mesajlaşma uygulaması (birlikte gelen Plugin).
- [Twitch](https://docs.openclaw.ai/tr/channels/twitch) — IRC bağlantısı üzerinden Twitch sohbeti (birlikte gelen Plugin).
- [Sesli Arama](https://docs.openclaw.ai/tr/plugins/voice-call) — Plivo veya Twilio üzerinden telefon görüşmesi (Plugin, ayrı kurulur).
- [WebChat](https://docs.openclaw.ai/tr/web/webchat) — WebSocket üzerinden Gateway WebChat arayüzü.
- [WeChat](https://docs.openclaw.ai/tr/channels/wechat) — QR oturum açma üzerinden Tencent iLink Bot Plugin’i; yalnızca özel sohbetler (harici Plugin).
- [WhatsApp](https://docs.openclaw.ai/tr/channels/whatsapp) — En popüler; Baileys kullanır ve QR eşleştirmesi gerektirir.
- [Yuanbao](https://docs.openclaw.ai/tr/channels/yuanbao) — Tencent Yuanbao botu (harici Plugin).
- [Zalo](https://docs.openclaw.ai/tr/channels/zalo) — Zalo Bot API; Vietnam’ın popüler mesajlaşma uygulaması (birlikte gelen Plugin).
- [Zalo Personal](https://docs.openclaw.ai/tr/channels/zalouser) — QR oturum açma üzerinden Zalo kişisel hesabı (birlikte gelen Plugin).

## [​](https://docs.openclaw.ai/tr/channels\#notlar)  Notlar

- Kanallar aynı anda çalışabilir; birden fazla kanal yapılandırın, OpenClaw sohbet başına yönlendirme yapar.
- En hızlı kurulum genellikle **Telegram**’dır (basit bot belirteci). WhatsApp QR eşleştirmesi gerektirir ve
diskte daha fazla durum saklar.
- Grup davranışı kanala göre değişir; bkz. [Gruplar](https://docs.openclaw.ai/tr/channels/groups).
- Güvenlik için DM eşleştirmesi ve izin verilenler listeleri zorunlu tutulur; bkz. [Güvenlik](https://docs.openclaw.ai/tr/gateway/security).
- Sorun giderme: [Kanal sorun giderme](https://docs.openclaw.ai/tr/channels/troubleshooting).
- Model sağlayıcıları ayrı olarak belgelenmiştir; bkz. [Model Sağlayıcıları](https://docs.openclaw.ai/tr/providers/models).

[Discord](https://docs.openclaw.ai/tr/channels/discord)

Ctrl+I