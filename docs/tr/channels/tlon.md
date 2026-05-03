---
source_url: https://docs.openclaw.ai/tr/channels/tlon
title: "Tlon - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/channels/tlon#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Developer and self-hosted

Tlon

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Birlikte gelen Plugin](https://docs.openclaw.ai/tr/channels/tlon#birlikte-gelen-plugin)
- [Kurulum](https://docs.openclaw.ai/tr/channels/tlon#kurulum)
- [Özel/LAN ship’leri](https://docs.openclaw.ai/tr/channels/tlon#%C3%B6zel%2Flan-ship%E2%80%99leri)
- [Grup kanalları](https://docs.openclaw.ai/tr/channels/tlon#grup-kanallar%C4%B1)
- [Erişim denetimi](https://docs.openclaw.ai/tr/channels/tlon#eri%C5%9Fim-denetimi)
- [Sahip ve onay sistemi](https://docs.openclaw.ai/tr/channels/tlon#sahip-ve-onay-sistemi)
- [Otomatik kabul ayarları](https://docs.openclaw.ai/tr/channels/tlon#otomatik-kabul-ayarlar%C4%B1)
- [Teslim hedefleri (CLI/cron)](https://docs.openclaw.ai/tr/channels/tlon#teslim-hedefleri-cli%2Fcron)
- [Birlikte gelen skill](https://docs.openclaw.ai/tr/channels/tlon#birlikte-gelen-skill)
- [Yetenekler](https://docs.openclaw.ai/tr/channels/tlon#yetenekler)
- [Sorun giderme](https://docs.openclaw.ai/tr/channels/tlon#sorun-giderme)
- [Yapılandırma başvurusu](https://docs.openclaw.ai/tr/channels/tlon#yap%C4%B1land%C4%B1rma-ba%C5%9Fvurusu)
- [Notlar](https://docs.openclaw.ai/tr/channels/tlon#notlar)
- [İlgili](https://docs.openclaw.ai/tr/channels/tlon#i%CC%87lgili)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Tlon, Urbit üzerinde inşa edilmiş merkeziyetsiz bir mesajlaşma uygulamasıdır. OpenClaw, Urbit ship’inize bağlanır ve
DM’lere ve grup sohbeti mesajlarına yanıt verebilir. Grup yanıtları varsayılan olarak @ mention gerektirir ve
allowlist’ler aracılığıyla daha da kısıtlanabilir.Durum: birlikte gelen Plugin. DM’ler, grup mention’ları, thread yanıtları, zengin metin biçimlendirmesi ve
görsel yüklemeleri desteklenir. Reaksiyonlar ve anketler henüz desteklenmiyor.

## [​](https://docs.openclaw.ai/tr/channels/tlon\#birlikte-gelen-plugin)  Birlikte gelen Plugin

Tlon, mevcut OpenClaw sürümlerinde birlikte gelen bir Plugin olarak sunulur; bu nedenle normal paketlenmiş
derlemeler ayrı bir kurulum gerektirmez.Daha eski bir derlemedeyseniz veya Tlon’u hariç tutan özel bir kurulum kullanıyorsanız,
güncel bir npm paketi kurun:CLI üzerinden kurulum (npm registry):

```
openclaw plugins install @openclaw/tlon
```

Mevcut resmi sürüm etiketini takip etmek için yalın paketi kullanın. Kesin bir
sürümü yalnızca tekrarlanabilir bir kurulum gerektiğinde sabitleyin.Yerel checkout (bir git deposundan çalıştırırken):

```
openclaw plugins install ./path/to/local/tlon-plugin
```

Ayrıntılar: [Plugins](https://docs.openclaw.ai/tr/tools/plugin)

## [​](https://docs.openclaw.ai/tr/channels/tlon\#kurulum)  Kurulum

1. Tlon Plugin’inin kullanılabilir olduğundan emin olun.
   - Mevcut paketlenmiş OpenClaw sürümleri bunu zaten içerir.
   - Daha eski/özel kurulumlar, yukarıdaki komutlarla bunu elle ekleyebilir.
2. Ship URL’nizi ve oturum açma kodunuzu alın.
3. `channels.tlon` yapılandırmasını yapın.
4. Gateway’i yeniden başlatın.
5. Bota DM gönderin veya bir grup kanalında ondan mention ile bahsedin.

Minimal yapılandırma (tek hesap):

```
{
  channels: {
    tlon: {
      enabled: true,
      ship: "~sampel-palnet",
      url: "https://your-ship-host",
      code: "lidlut-tabwed-pillex-ridrup",
      ownerShip: "~your-main-ship", // recommended: your ship, always allowed
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/channels/tlon\#%C3%B6zel/lan-ship%E2%80%99leri)  Özel/LAN ship’leri

Varsayılan olarak OpenClaw, SSRF koruması için özel/dahili host adlarını ve IP aralıklarını engeller.
Ship’iniz özel bir ağda çalışıyorsa (localhost, LAN IP veya dahili hostname),
açıkça etkinleştirmeniz gerekir:

```
{
  channels: {
    tlon: {
      url: "http://localhost:8080",
      allowPrivateNetwork: true,
    },
  },
}
```

Bu, aşağıdaki gibi URL’ler için geçerlidir:

- `http://localhost:8080`
- `http://192.168.x.x:8080`
- `http://my-ship.local:8080`

⚠️ Bunu yalnızca yerel ağınıza güveniyorsanız etkinleştirin. Bu ayar, ship URL’nize yapılan
istekler için SSRF korumalarını devre dışı bırakır.

## [​](https://docs.openclaw.ai/tr/channels/tlon\#grup-kanallar%C4%B1)  Grup kanalları

Otomatik keşif varsayılan olarak etkindir. Kanalları elle de sabitleyebilirsiniz:

```
{
  channels: {
    tlon: {
      groupChannels: ["chat/~host-ship/general", "chat/~host-ship/support"],
    },
  },
}
```

Otomatik keşfi devre dışı bırakın:

```
{
  channels: {
    tlon: {
      autoDiscoverChannels: false,
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/channels/tlon\#eri%C5%9Fim-denetimi)  Erişim denetimi

DM allowlist’i (boş = DM’lere izin verilmez, onay akışı için `ownerShip` kullanın):

```
{
  channels: {
    tlon: {
      dmAllowlist: ["~zod", "~nec"],
    },
  },
}
```

Grup yetkilendirmesi (varsayılan olarak kısıtlı):

```
{
  channels: {
    tlon: {
      defaultAuthorizedShips: ["~zod"],
      authorization: {
        channelRules: {
          "chat/~host-ship/general": {
            mode: "restricted",
            allowedShips: ["~zod", "~nec"],
          },
          "chat/~host-ship/announcements": {
            mode: "open",
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/channels/tlon\#sahip-ve-onay-sistemi)  Sahip ve onay sistemi

Yetkisiz kullanıcılar etkileşim kurmaya çalıştığında onay istekleri almak için bir sahip ship ayarlayın:

```
{
  channels: {
    tlon: {
      ownerShip: "~your-main-ship",
    },
  },
}
```

Sahip ship **her yerde otomatik olarak yetkilidir** — DM davetleri otomatik kabul edilir ve
kanal mesajlarına her zaman izin verilir. Sahibi `dmAllowlist` veya
`defaultAuthorizedShips` içine eklemeniz gerekmez.Ayarlandığında, sahip aşağıdakiler için DM bildirimleri alır:

- Allowlist’te olmayan ship’lerden gelen DM istekleri
- Yetkilendirmesi olmayan kanallardaki mention’lar
- Grup daveti istekleri

## [​](https://docs.openclaw.ai/tr/channels/tlon\#otomatik-kabul-ayarlar%C4%B1)  Otomatik kabul ayarları

DM davetlerini otomatik kabul edin (`dmAllowlist` içindeki ship’ler için):

```
{
  channels: {
    tlon: {
      autoAcceptDmInvites: true,
    },
  },
}
```

Grup davetlerini otomatik kabul edin:

```
{
  channels: {
    tlon: {
      autoAcceptGroupInvites: true,
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/channels/tlon\#teslim-hedefleri-cli/cron)  Teslim hedefleri (CLI/cron)

Bunları `openclaw message send` veya cron teslimiyle kullanın:

- DM: `~sampel-palnet` veya `dm/~sampel-palnet`
- Grup: `chat/~host-ship/channel` veya `group:~host-ship/channel`

## [​](https://docs.openclaw.ai/tr/channels/tlon\#birlikte-gelen-skill)  Birlikte gelen skill

Tlon Plugin’i, Tlon işlemlerine CLI erişimi sağlayan birlikte gelen bir skill
( [`@tloncorp/tlon-skill`](https://github.com/tloncorp/tlon-skill)) içerir:

- **Kişiler**: profilleri alın/güncelleyin, kişileri listeleyin
- **Kanallar**: listeleyin, oluşturun, mesaj gönderin, geçmişi getirin
- **Gruplar**: listeleyin, oluşturun, üyeleri yönetin
- **DM’ler**: mesaj gönderin, mesajlara reaksiyon verin
- **Reaksiyonlar**: gönderilere ve DM’lere emoji reaksiyonları ekleyin/kaldırın
- **Ayarlar**: slash komutları aracılığıyla Plugin izinlerini yönetin

Plugin kurulduğunda skill otomatik olarak kullanılabilir olur.

## [​](https://docs.openclaw.ai/tr/channels/tlon\#yetenekler)  Yetenekler

| Özellik | Durum |
| --- | --- |
| Doğrudan mesajlar | ✅ Desteklenir |
| Gruplar/kanallar | ✅ Desteklenir (varsayılan olarak mention kapılı) |
| Thread’ler | ✅ Desteklenir (thread içinde otomatik yanıtlar) |
| Zengin metin | ✅ Markdown Tlon biçimine dönüştürülür |
| Görseller | ✅ Tlon depolamasına yüklenir |
| Reaksiyonlar | ✅ [birlikte gelen skill](https://docs.openclaw.ai/tr/channels/tlon#bundled-skill) aracılığıyla |
| Anketler | ❌ Henüz desteklenmiyor |
| Yerel komutlar | ✅ Desteklenir (varsayılan olarak yalnızca sahip) |

## [​](https://docs.openclaw.ai/tr/channels/tlon\#sorun-giderme)  Sorun giderme

Önce şu sırayı çalıştırın:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
```

Yaygın hatalar:

- **DM’ler yok sayılıyor**: gönderen `dmAllowlist` içinde değil ve onay akışı için `ownerShip` yapılandırılmamış.
- **Grup mesajları yok sayılıyor**: kanal keşfedilmemiş veya gönderen yetkili değil.
- **Bağlantı hataları**: ship URL’sinin erişilebilir olduğunu kontrol edin; yerel ship’ler için `allowPrivateNetwork` etkinleştirin.
- **Kimlik doğrulama hataları**: oturum açma kodunun güncel olduğunu doğrulayın (kodlar döndürülür).

## [​](https://docs.openclaw.ai/tr/channels/tlon\#yap%C4%B1land%C4%B1rma-ba%C5%9Fvurusu)  Yapılandırma başvurusu

Tam yapılandırma: [Yapılandırma](https://docs.openclaw.ai/tr/gateway/configuration)Sağlayıcı seçenekleri:

- `channels.tlon.enabled`: kanal başlatmayı etkinleştir/devre dışı bırak.
- `channels.tlon.ship`: botun Urbit ship adı (ör. `~sampel-palnet`).
- `channels.tlon.url`: ship URL’si (ör. `https://sampel-palnet.tlon.network`).
- `channels.tlon.code`: ship oturum açma kodu.
- `channels.tlon.allowPrivateNetwork`: localhost/LAN URL’lerine izin ver (SSRF atlatma).
- `channels.tlon.ownerShip`: onay sistemi için sahip ship (her zaman yetkili).
- `channels.tlon.dmAllowlist`: DM göndermesine izin verilen ship’ler (boş = hiçbiri).
- `channels.tlon.autoAcceptDmInvites`: allowlist’teki ship’lerden gelen DM’leri otomatik kabul et.
- `channels.tlon.autoAcceptGroupInvites`: tüm grup davetlerini otomatik kabul et.
- `channels.tlon.autoDiscoverChannels`: grup kanallarını otomatik keşfet (varsayılan: true).
- `channels.tlon.groupChannels`: elle sabitlenmiş kanal yuvaları.
- `channels.tlon.defaultAuthorizedShips`: tüm kanallar için yetkili ship’ler.
- `channels.tlon.authorization.channelRules`: kanal başına kimlik doğrulama kuralları.
- `channels.tlon.showModelSignature`: mesajlara model adını ekle.

## [​](https://docs.openclaw.ai/tr/channels/tlon\#notlar)  Notlar

- Grup yanıtları, yanıt vermek için bir mention gerektirir (ör. `~your-bot-ship`).
- Thread yanıtları: gelen mesaj bir thread içindeyse, OpenClaw thread içinde yanıtlar.
- Zengin metin: Markdown biçimlendirmesi (kalın, italik, kod, başlıklar, listeler) Tlon’un yerel biçimine dönüştürülür.
- Görseller: URL’ler Tlon depolamasına yüklenir ve görsel blokları olarak gömülür.

## [​](https://docs.openclaw.ai/tr/channels/tlon\#i%CC%87lgili)  İlgili

- [Kanallara Genel Bakış](https://docs.openclaw.ai/tr/channels) — desteklenen tüm kanallar
- [Eşleştirme](https://docs.openclaw.ai/tr/channels/pairing) — DM kimlik doğrulaması ve eşleştirme akışı
- [Gruplar](https://docs.openclaw.ai/tr/channels/groups) — grup sohbeti davranışı ve mention kapısı
- [Kanal Yönlendirme](https://docs.openclaw.ai/tr/channels/channel-routing) — mesajlar için oturum yönlendirmesi
- [Güvenlik](https://docs.openclaw.ai/tr/gateway/security) — erişim modeli ve sertleştirme

[Nostr](https://docs.openclaw.ai/tr/channels/nostr) [Synology Chat](https://docs.openclaw.ai/tr/channels/synology-chat)

Ctrl+I