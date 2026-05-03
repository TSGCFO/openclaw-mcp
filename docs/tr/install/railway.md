---
source_url: https://docs.openclaw.ai/tr/install/railway
title: "Railway - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/install/railway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Hosting

Railway

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Railway](https://docs.openclaw.ai/tr/install/railway#railway)
- [Hızlı kontrol listesi (yeni kullanıcılar)](https://docs.openclaw.ai/tr/install/railway#h%C4%B1zl%C4%B1-kontrol-listesi-yeni-kullan%C4%B1c%C4%B1lar)
- [Tek tıklamayla dağıtım](https://docs.openclaw.ai/tr/install/railway#tek-t%C4%B1klamayla-da%C4%9F%C4%B1t%C4%B1m)
- [Elde ettikleriniz](https://docs.openclaw.ai/tr/install/railway#elde-ettikleriniz)
- [Gerekli Railway ayarları](https://docs.openclaw.ai/tr/install/railway#gerekli-railway-ayarlar%C4%B1)
- [Public Networking](https://docs.openclaw.ai/tr/install/railway#public-networking)
- [Volume (zorunlu)](https://docs.openclaw.ai/tr/install/railway#volume-zorunlu)
- [Variables](https://docs.openclaw.ai/tr/install/railway#variables)
- [Bir kanal bağlayın](https://docs.openclaw.ai/tr/install/railway#bir-kanal-ba%C4%9Flay%C4%B1n)
- [Yedekler ve geçiş](https://docs.openclaw.ai/tr/install/railway#yedekler-ve-ge%C3%A7i%C5%9F)
- [Sonraki adımlar](https://docs.openclaw.ai/tr/install/railway#sonraki-ad%C4%B1mlar)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/tr/install/railway\#railway)  Railway

OpenClaw’ı Railway üzerinde tek tıklamalı şablonla dağıtın ve web Control UI üzerinden erişin.
Bu, “sunucuda terminal yok” için en kolay yoldur: Gateway’i Railway sizin için çalıştırır.

## [​](https://docs.openclaw.ai/tr/install/railway\#h%C4%B1zl%C4%B1-kontrol-listesi-yeni-kullan%C4%B1c%C4%B1lar)  Hızlı kontrol listesi (yeni kullanıcılar)

1. Aşağıdaki **Railway’de dağıt** düğmesine tıklayın.
2. `/data` konumuna bağlanan bir **Volume** ekleyin.
3. Gerekli **Variables** değerlerini ayarlayın (en azından `OPENCLAW_GATEWAY_PORT` ve `OPENCLAW_GATEWAY_TOKEN`).
4. `8080` portunda **HTTP Proxy**’yi etkinleştirin.
5. `https://<your-railway-domain>/openclaw` adresini açın ve yapılandırılmış paylaşılan gizli anahtarı kullanarak bağlanın. Bu şablon varsayılan olarak `OPENCLAW_GATEWAY_TOKEN` kullanır; bunu parola kimlik doğrulamasıyla değiştirirseniz onun yerine bu parolayı kullanın.

## [​](https://docs.openclaw.ai/tr/install/railway\#tek-t%C4%B1klamayla-da%C4%9F%C4%B1t%C4%B1m)  Tek tıklamayla dağıtım

[Railway’de dağıt](https://railway.com/deploy/clawdbot-railway-template) Dağıtımdan sonra genel URL’nizi **Railway → hizmetiniz → Settings → Domains** içinde bulun.Railway ya:

- size oluşturulmuş bir alan adı verir (çoğunlukla `https://<something>.up.railway.app`), veya
- bir tane bağladıysanız özel alan adınızı kullanır.

Ardından şunu açın:

- `https://<your-railway-domain>/openclaw` — Control UI

## [​](https://docs.openclaw.ai/tr/install/railway\#elde-ettikleriniz)  Elde ettikleriniz

- Barındırılan OpenClaw Gateway + Control UI
- `openclaw.json`,
ajan başına `auth-profiles.json`, kanal/sağlayıcı durumu, oturumlar ve
çalışma alanının yeniden dağıtımlarda kalıcı olması için Railway Volume (`/data`) üzerinden kalıcı depolama

## [​](https://docs.openclaw.ai/tr/install/railway\#gerekli-railway-ayarlar%C4%B1)  Gerekli Railway ayarları

### [​](https://docs.openclaw.ai/tr/install/railway\#public-networking)  Public Networking

Hizmet için **HTTP Proxy**’yi etkinleştirin.

- Port: `8080`

### [​](https://docs.openclaw.ai/tr/install/railway\#volume-zorunlu)  Volume (zorunlu)

Şu konuma bağlanan bir volume ekleyin:

- `/data`

### [​](https://docs.openclaw.ai/tr/install/railway\#variables)  Variables

Hizmet üzerinde şu değişkenleri ayarlayın:

- `OPENCLAW_GATEWAY_PORT=8080` (zorunlu — Public Networking içindeki portla eşleşmelidir)
- `OPENCLAW_GATEWAY_TOKEN` (zorunlu; yönetici sırrı olarak değerlendirin)
- `OPENCLAW_STATE_DIR=/data/.openclaw` (önerilir)
- `OPENCLAW_WORKSPACE_DIR=/data/workspace` (önerilir)

## [​](https://docs.openclaw.ai/tr/install/railway\#bir-kanal-ba%C4%9Flay%C4%B1n)  Bir kanal bağlayın

Kanal kurulum yönergeleri için `/openclaw` üzerindeki Control UI’yi kullanın veya Railway’nin shell’i üzerinden `openclaw onboard` çalıştırın:

- [Telegram](https://docs.openclaw.ai/tr/channels/telegram) (en hızlısı — yalnızca bir bot token’ı)
- [Discord](https://docs.openclaw.ai/tr/channels/discord)
- [Tüm kanallar](https://docs.openclaw.ai/tr/channels)

## [​](https://docs.openclaw.ai/tr/install/railway\#yedekler-ve-ge%C3%A7i%C5%9F)  Yedekler ve geçiş

Durumunuzu, yapılandırmanızı, kimlik doğrulama profillerinizi ve çalışma alanınızı dışa aktarın:

```
openclaw backup create
```

Bu, OpenClaw durumu ile birlikte yapılandırılmış
çalışma alanını da içeren taşınabilir bir yedek arşivi oluşturur. Ayrıntılar için bkz. [Backup](https://docs.openclaw.ai/tr/cli/backup).

## [​](https://docs.openclaw.ai/tr/install/railway\#sonraki-ad%C4%B1mlar)  Sonraki adımlar

- Mesajlaşma kanallarını kurun: [Kanallar](https://docs.openclaw.ai/tr/channels)
- Gateway’i yapılandırın: [Gateway yapılandırması](https://docs.openclaw.ai/tr/gateway/configuration)
- OpenClaw’ı güncel tutun: [Güncelleme](https://docs.openclaw.ai/tr/install/updating)

[Oracle Cloud](https://docs.openclaw.ai/tr/install/oracle) [Raspberry Pi](https://docs.openclaw.ai/tr/install/raspberry-pi)

Ctrl+I