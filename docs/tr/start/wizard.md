---
source_url: https://docs.openclaw.ai/tr/start/wizard
title: "\u0130lk kurulum (CLI) - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/start/wizard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

First steps

İlk kurulum (CLI)

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [QuickStart ve Advanced](https://docs.openclaw.ai/tr/start/wizard#quickstart-ve-advanced)
- [Onboarding neyi yapılandırır](https://docs.openclaw.ai/tr/start/wizard#onboarding-neyi-yap%C4%B1land%C4%B1r%C4%B1r)
- [Başka bir agent ekleyin](https://docs.openclaw.ai/tr/start/wizard#ba%C5%9Fka-bir-agent-ekleyin)
- [Tam başvuru](https://docs.openclaw.ai/tr/start/wizard#tam-ba%C5%9Fvuru)
- [İlgili dokümanlar](https://docs.openclaw.ai/tr/start/wizard#i%CC%87lgili-dok%C3%BCmanlar)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

CLI onboarding, OpenClaw’ı macOS, Linux veya Windows üzerinde (WSL2 aracılığıyla; kesinlikle önerilir) kurmanın **önerilen** yoludur.
Tek bir rehberli akışta yerel bir Gateway veya uzak Gateway bağlantısının yanı sıra kanalları, Skills’i
ve çalışma alanı varsayılanlarını yapılandırır.

```
openclaw onboard
```

En hızlı ilk sohbet: Control UI’ı açın (kanal kurulumu gerekmez). `openclaw dashboard`
komutunu çalıştırın ve tarayıcıda sohbet edin. Dokümanlar: [Dashboard](https://docs.openclaw.ai/tr/web/dashboard).

Daha sonra yeniden yapılandırmak için:

```
openclaw configure
openclaw agents add <name>
```

`--json`, etkileşimsiz mod anlamına gelmez. Betikler için `--non-interactive` kullanın.

CLI onboarding, Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search,
Ollama Web Search, Perplexity, SearXNG veya Tavily gibi bir sağlayıcı seçebileceğiniz
bir web arama adımı içerir. Bazı sağlayıcılar API anahtarı gerektirirken diğerleri
anahtarsızdır. Bunu daha sonra `openclaw configure --section web` ile de yapılandırabilirsiniz.
Dokümanlar: [Web araçları](https://docs.openclaw.ai/tr/tools/web).

## [​](https://docs.openclaw.ai/tr/start/wizard\#quickstart-ve-advanced)  QuickStart ve Advanced

Onboarding, **QuickStart** (varsayılanlar) ile **Advanced** (tam denetim) seçenekleriyle başlar.

- QuickStart (varsayılanlar)

- Advanced (tam denetim)


- Yerel gateway (loopback)
- Çalışma alanı varsayılanı (veya mevcut çalışma alanı)
- Gateway bağlantı noktası **18789**
- Gateway kimlik doğrulaması **Token** (loopback üzerinde bile otomatik oluşturulur)
- Yeni yerel kurulumlar için araç ilkesi varsayılanı: `tools.profile: "coding"` (mevcut açık profil korunur)
- DM izolasyonu varsayılanı: yerel onboarding, ayarlanmamışsa `session.dmScope: "per-channel-peer"` yazar. Ayrıntılar: [CLI Kurulum Başvurusu](https://docs.openclaw.ai/tr/start/wizard-cli-reference#outputs-and-internals)
- Tailscale dışa açımı **Kapalı**
- Telegram + WhatsApp DM’leri varsayılan olarak **izin listesi** kullanır (telefon numaranız istenir)

- Her adımı açığa çıkarır (mod, çalışma alanı, gateway, kanallar, daemon, Skills).

## [​](https://docs.openclaw.ai/tr/start/wizard\#onboarding-neyi-yap%C4%B1land%C4%B1r%C4%B1r)  Onboarding neyi yapılandırır

**Yerel mod (varsayılan)** sizi şu adımlardan geçirir:

1. **Model/Kimlik Doğrulama** — Custom Provider dahil desteklenen herhangi bir sağlayıcı/kimlik doğrulama akışını (API anahtarı, OAuth veya sağlayıcıya özgü manuel kimlik doğrulama) seçin
(OpenAI uyumlu, Anthropic uyumlu veya Unknown otomatik algılama). Varsayılan bir model seçin.
Güvenlik notu: Bu agent araç çalıştıracak veya webhook/hooks içeriğini işleyecekse, mevcut en güçlü son nesil modeli tercih edin ve araç ilkesini sıkı tutun. Daha zayıf/eski katmanlar prompt-inject saldırılarına daha açıktır.
Etkileşimsiz çalıştırmalarda, `--secret-input-mode ref` düz metin API anahtarı değerleri yerine kimlik doğrulama profillerinde env destekli referanslar saklar.
Etkileşimsiz `ref` modunda, sağlayıcı env var ayarlanmış olmalıdır; bu env var olmadan satır içi anahtar bayrakları geçirmek hızlıca başarısız olur.
Etkileşimli çalıştırmalarda, gizli referans modunu seçmek, kaydetmeden önce hızlı bir ön kontrol doğrulamasıyla bir ortam değişkenine veya yapılandırılmış bir sağlayıcı referansına (`file` veya `exec`) işaret etmenizi sağlar.
Anthropic için etkileşimli onboarding/configure, tercih edilen yerel yol olarak **Anthropic Claude CLI**’ı ve önerilen üretim yolu olarak **Anthropic API key**’i sunar. Anthropic setup-token da desteklenen bir token-auth yolu olarak kullanılabilir kalır.
2. **Çalışma Alanı** — Agent dosyalarının konumu (varsayılan `~/.openclaw/workspace`). Başlangıç dosyalarını yerleştirir.
3. **Gateway** — Bağlantı noktası, bind adresi, kimlik doğrulama modu, Tailscale dışa açımı.
Etkileşimli token modunda, varsayılan düz metin token depolamasını seçin veya SecretRef’e geçin.
Etkileşimsiz token SecretRef yolu: `--gateway-token-ref-env <ENV_VAR>`.
4. **Kanallar** — BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams, QQ Bot, Signal, Slack, Telegram, WhatsApp ve daha fazlası gibi yerleşik ve paketlenmiş sohbet kanalları.
5. **Daemon** — Bir LaunchAgent (macOS), systemd kullanıcı birimi (Linux/WSL2) veya kullanıcı başına Startup klasörü yedeğiyle yerel Windows Scheduled Task kurar.
Token kimlik doğrulaması bir token gerektiriyorsa ve `gateway.auth.token` SecretRef ile yönetiliyorsa, daemon kurulumu bunu doğrular ancak çözümlenen token’ı supervisor hizmet ortamı meta verilerine kalıcı olarak yazmaz.
Token kimlik doğrulaması bir token gerektiriyorsa ve yapılandırılmış token SecretRef çözümlenemiyorsa, daemon kurulumu uygulanabilir yönlendirmeyle engellenir.
Hem `gateway.auth.token` hem de `gateway.auth.password` yapılandırılmışsa ve `gateway.auth.mode` ayarlanmamışsa, mod açıkça ayarlanana kadar daemon kurulumu engellenir.
6. **Sağlık denetimi** — Gateway’i başlatır ve çalıştığını doğrular.
7. **Skills** — Önerilen Skills’i ve isteğe bağlı bağımlılıkları kurar.

Onboarding’i yeniden çalıştırmak, açıkça **Reset** seçmediğiniz (veya `--reset` geçmediğiniz) sürece hiçbir şeyi silmez.
CLI `--reset` varsayılan olarak yapılandırma, kimlik bilgileri ve oturumları kapsar; çalışma alanını dahil etmek için `--reset-scope full` kullanın.
Yapılandırma geçersizse veya eski anahtarlar içeriyorsa, onboarding önce `openclaw doctor` çalıştırmanızı ister.

**Uzak mod** yalnızca yerel istemciyi başka bir yerdeki Gateway’e bağlanacak şekilde yapılandırır.
Uzak ana makinede hiçbir şey kurmaz veya değiştirmez.

## [​](https://docs.openclaw.ai/tr/start/wizard\#ba%C5%9Fka-bir-agent-ekleyin)  Başka bir agent ekleyin

Kendi çalışma alanı, oturumları ve kimlik doğrulama profilleri olan ayrı bir agent oluşturmak için
`openclaw agents add <name>` kullanın. `--workspace` olmadan çalıştırmak onboarding’i başlatır.Ayarladıkları:

- `agents.list[].name`
- `agents.list[].workspace`
- `agents.list[].agentDir`

Notlar:

- Varsayılan çalışma alanları `~/.openclaw/workspace-<agentId>` düzenini izler.
- Gelen mesajları yönlendirmek için `bindings` ekleyin (onboarding bunu yapabilir).
- Etkileşimsiz bayraklar: `--model`, `--agent-dir`, `--bind`, `--non-interactive`.

## [​](https://docs.openclaw.ai/tr/start/wizard\#tam-ba%C5%9Fvuru)  Tam başvuru

Ayrıntılı adım adım dökümler ve yapılandırma çıktıları için
[CLI Kurulum Başvurusu](https://docs.openclaw.ai/tr/start/wizard-cli-reference) bölümüne bakın.
Etkileşimsiz örnekler için [CLI Otomasyonu](https://docs.openclaw.ai/tr/start/wizard-cli-automation) bölümüne bakın.
RPC ayrıntıları dahil daha derin teknik başvuru için
[Onboarding Başvurusu](https://docs.openclaw.ai/tr/reference/wizard) bölümüne bakın.

## [​](https://docs.openclaw.ai/tr/start/wizard\#i%CC%87lgili-dok%C3%BCmanlar)  İlgili dokümanlar

- CLI komut başvurusu: [`openclaw onboard`](https://docs.openclaw.ai/tr/cli/onboard)
- Onboarding genel bakışı: [Onboarding Genel Bakışı](https://docs.openclaw.ai/tr/start/onboarding-overview)
- macOS uygulaması onboarding: [Onboarding](https://docs.openclaw.ai/tr/start/onboarding)
- Agent ilk çalıştırma ritüeli: [Agent Bootstrapping](https://docs.openclaw.ai/tr/start/bootstrapping)

[Onboarding Overview](https://docs.openclaw.ai/tr/start/onboarding-overview) [Onboarding: macOS App](https://docs.openclaw.ai/tr/start/onboarding)

Ctrl+I