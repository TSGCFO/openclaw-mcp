# Other

_5 pages from docs.openclaw.ai — full content preserved._

## Contents

- [OpenClaw - OpenClaw](#openclaw---openclaw)
- [OpenClaw - OpenClaw](#openclaw---openclaw)
- [OpenClaw - OpenClaw](#openclaw---openclaw)
- [OpenClaw - OpenClaw](#openclaw---openclaw)
- [OpenClaw - OpenClaw](#openclaw---openclaw)

---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai/de>_

# OpenClaw 🦞

> _“EXFOLIATE! EXFOLIATE!”_ — Wahrscheinlich ein Weltraumhummer

**Gateway für KI-Agenten auf jedem Betriebssystem über Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo und mehr.**

Senden Sie eine Nachricht und erhalten Sie eine Agentenantwort direkt in Ihre Tasche. Betreiben Sie ein einziges Gateway über integrierte Channels, gebündelte Channel-Plugins, WebChat und mobile Nodes.

[**Erste Schritte** \\
\\
Installieren Sie OpenClaw und starten Sie das Gateway in wenigen Minuten.](https://docs.openclaw.ai/de/start/getting-started)

[**Onboarding ausführen** \\
\\
Geführte Einrichtung mit `openclaw onboard` und Pairing-Abläufen.](https://docs.openclaw.ai/de/start/wizard)

[**Die Control UI öffnen** \\
\\
Starten Sie das Browser-Dashboard für Chat, Konfiguration und Sitzungen.](https://docs.openclaw.ai/web/control-ui)

## Was ist OpenClaw?

OpenClaw ist ein **selbst gehostetes Gateway**, das Ihre bevorzugten Chat-Apps und Channel-Oberflächen — integrierte Channels plus gebündelte oder externe Channel-Plugins wie Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo und mehr — mit KI-Coding-Agenten wie Pi verbindet. Sie betreiben einen einzigen Gateway-Prozess auf Ihrer eigenen Maschine (oder einem Server), und er wird zur Brücke zwischen Ihren Messaging-Apps und einem jederzeit verfügbaren KI-Assistenten.**Für wen ist es gedacht?** Entwickler und Power-User, die einen persönlichen KI-Assistenten möchten, dem sie von überall aus Nachrichten senden können — ohne die Kontrolle über ihre Daten aufzugeben oder sich auf einen gehosteten Dienst zu verlassen.**Was macht es anders?**

- **Selbst gehostet**: läuft auf Ihrer Hardware, nach Ihren Regeln
- **Multi-Channel**: ein Gateway bedient gleichzeitig integrierte Channels plus gebündelte oder externe Channel-Plugins
- **Agent-native**: entwickelt für Coding-Agenten mit Tool-Nutzung, Sitzungen, Speicher und Multi-Agent-Routing
- **Open Source**: MIT-lizenziert, von der Community getragen

**Was benötigen Sie?** Node 24 (empfohlen) oder Node 22 LTS (`22.14+`) zur Kompatibilität, einen API-Key Ihres gewählten Providers und 5 Minuten. Für beste Qualität und Sicherheit verwenden Sie das stärkste Modell der neuesten Generation, das Ihnen zur Verfügung steht.

## So funktioniert es

Das Gateway ist die einzige Quelle der Wahrheit für Sitzungen, Routing und Channel-Verbindungen.

## Hauptfunktionen

[**Multi-Channel-Gateway** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat und mehr mit einem einzigen Gateway-Prozess.](https://docs.openclaw.ai/de/channels)

[**Plugin-Channels** \\
\\
Gebündelte Plugins fügen in normalen aktuellen Releases Matrix, Nostr, Twitch, Zalo und mehr hinzu.](https://docs.openclaw.ai/de/tools/plugin)

[**Multi-Agent-Routing** \\
\\
Isolierte Sitzungen pro Agent, Workspace oder Absender.](https://docs.openclaw.ai/de/concepts/multi-agent)

[**Medienunterstützung** \\
\\
Senden und empfangen Sie Bilder, Audio und Dokumente.](https://docs.openclaw.ai/de/nodes/images)

[**Web-Control-UI** \\
\\
Browser-Dashboard für Chat, Konfiguration, Sitzungen und Nodes.](https://docs.openclaw.ai/web/control-ui)

[**Mobile Nodes** \\
\\
Koppeln Sie iOS- und Android-Nodes für Canvas-, Kamera- und sprachaktivierte Workflows.](https://docs.openclaw.ai/de/nodes)

## Schnellstart

1

[Navigate to header](https://docs.openclaw.ai/de#)

OpenClaw installieren

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/de#)

Onboarding durchführen und den Dienst installieren

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/de#)

Chatten

Öffnen Sie die Control UI in Ihrem Browser und senden Sie eine Nachricht:

```
openclaw dashboard
```

Oder verbinden Sie einen Channel ( [Telegram](https://docs.openclaw.ai/de/channels/telegram) ist am schnellsten) und chatten Sie von Ihrem Telefon aus.

Benötigen Sie die vollständige Installations- und Entwicklungsumgebung? Siehe [Getting Started](https://docs.openclaw.ai/de/start/getting-started).

## Dashboard

Öffnen Sie die browserbasierte Control UI, nachdem das Gateway gestartet wurde.

- Lokaler Standard: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- Remote-Zugriff: [Web surfaces](https://docs.openclaw.ai/web) und [Tailscale](https://docs.openclaw.ai/de/gateway/tailscale)

## Konfiguration (optional)

Die Konfiguration befindet sich unter `~/.openclaw/openclaw.json`.

- Wenn Sie **nichts tun**, verwendet OpenClaw die gebündelte Pi-Binärdatei im RPC-Modus mit Sitzungen pro Absender.
- Wenn Sie es stärker absichern möchten, beginnen Sie mit `channels.whatsapp.allowFrom` und (für Gruppen) Erwähnungsregeln.

Beispiel:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## Hier starten

[**Docs-Hubs** \\
\\
Alle Docs und Leitfäden, nach Anwendungsfall organisiert.](https://docs.openclaw.ai/de/start/hubs)

[**Konfiguration** \\
\\
Zentrale Gateway-Einstellungen, Tokens und Provider-Konfiguration.](https://docs.openclaw.ai/de/gateway/configuration)

[**Remote-Zugriff** \\
\\
SSH- und Tailnet-Zugriffsmuster.](https://docs.openclaw.ai/de/gateway/remote)

[**Channels** \\
\\
Channel-spezifische Einrichtung für Feishu, Microsoft Teams, WhatsApp, Telegram, Discord und mehr.](https://docs.openclaw.ai/de/channels/telegram)

[**Nodes** \\
\\
iOS- und Android-Nodes mit Pairing, Canvas, Kamera und Geräteaktionen.](https://docs.openclaw.ai/de/nodes)

[**Hilfe** \\
\\
Einstiegspunkt für häufige Lösungen und Fehlerbehebung.](https://docs.openclaw.ai/de/help)

## Mehr erfahren

[**Vollständige Funktionsliste** \\
\\
Vollständige Channel-, Routing- und Medienfunktionen.](https://docs.openclaw.ai/de/concepts/features)

[**Multi-Agent-Routing** \\
\\
Workspace-Isolation und Sitzungen pro Agent.](https://docs.openclaw.ai/de/concepts/multi-agent)

[**Sicherheit** \\
\\
Tokens, Allowlists und Sicherheitskontrollen.](https://docs.openclaw.ai/de/gateway/security)

[**Fehlerbehebung** \\
\\
Gateway-Diagnose und häufige Fehler.](https://docs.openclaw.ai/de/gateway/troubleshooting)

[**Über das Projekt und Danksagungen** \\
\\
Projektursprung, Mitwirkende und Lizenz.](https://docs.openclaw.ai/de/reference/credits)

[Showcase](https://docs.openclaw.ai/de/start/showcase)

Ctrl+I

---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai/id>_

# OpenClaw 🦞

> _“EXFOLIATE! EXFOLIATE!”_ — Seekor lobster luar angkasa, mungkin

**Gateway lintas sistem operasi untuk agen AI di Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, dan lainnya.**

Kirim pesan, dapatkan respons agen dari saku Anda. Jalankan satu Gateway di berbagai channel bawaan, plugin channel bawaan, WebChat, dan Node seluler.

[**Get Started** \\
\\
Instal OpenClaw dan jalankan Gateway dalam hitungan menit.](https://docs.openclaw.ai/id/start/getting-started)

[**Run Onboarding** \\
\\
Penyiapan terpandu dengan `openclaw onboard` dan alur pairing.](https://docs.openclaw.ai/id/start/wizard)

[**Open the Control UI** \\
\\
Buka dasbor browser untuk chat, config, dan sesi.](https://docs.openclaw.ai/web/control-ui)

## Apa itu OpenClaw?

OpenClaw adalah **Gateway self-hosted** yang menghubungkan aplikasi chat dan permukaan channel favorit Anda — channel bawaan plus plugin channel bawaan atau eksternal seperti Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, dan lainnya — ke agen coding AI seperti Pi. Anda menjalankan satu proses Gateway di mesin Anda sendiri (atau server), dan proses itu menjadi jembatan antara aplikasi perpesanan Anda dan asisten AI yang selalu tersedia.**Untuk siapa ini?** Developer dan power user yang menginginkan asisten AI pribadi yang bisa mereka kirimi pesan dari mana saja — tanpa kehilangan kendali atas data mereka atau bergantung pada layanan ter-host.**Apa yang membuatnya berbeda?**

- **Self-hosted**: berjalan di perangkat keras Anda, aturan Anda
- **Multi-channel**: satu Gateway melayani channel bawaan plus plugin channel bawaan atau eksternal secara bersamaan
- **Native untuk agen**: dibangun untuk agen coding dengan penggunaan tool, sesi, memori, dan perutean multi-agen
- **Open source**: berlisensi MIT, digerakkan oleh komunitas

**Apa yang Anda perlukan?** Node 24 (disarankan), atau Node 22 LTS (`22.14+`) untuk kompatibilitas, API key dari provider pilihan Anda, dan 5 menit. Untuk kualitas dan keamanan terbaik, gunakan model generasi terbaru terkuat yang tersedia.

## Cara kerjanya

Aplikasi chat + plugin

Gateway

Agen Pi

CLI

Web Control UI

Aplikasi macOS

Node iOS dan Android

Gateway adalah sumber kebenaran tunggal untuk sesi, perutean, dan koneksi channel.

## Kemampuan utama

[**Multi-channel gateway** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat, dan lainnya dengan satu proses Gateway.](https://docs.openclaw.ai/id/channels)

[**Plugin channels** \\
\\
Plugin bawaan menambahkan Matrix, Nostr, Twitch, Zalo, dan lainnya dalam rilis normal saat ini.](https://docs.openclaw.ai/id/tools/plugin)

[**Multi-agent routing** \\
\\
Sesi terisolasi per agen, workspace, atau pengirim.](https://docs.openclaw.ai/id/concepts/multi-agent)

[**Media support** \\
\\
Kirim dan terima gambar, audio, dan dokumen.](https://docs.openclaw.ai/id/nodes/images)

[**Web Control UI** \\
\\
Dasbor browser untuk chat, config, sesi, dan Node.](https://docs.openclaw.ai/web/control-ui)

[**Mobile nodes** \\
\\
Pairing Node iOS dan Android untuk alur kerja Canvas, kamera, dan voice.](https://docs.openclaw.ai/id/nodes)

## Mulai cepat

1

[Navigate to header](https://docs.openclaw.ai/id#)

Install OpenClaw

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/id#)

Onboard and install the service

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/id#)

Chat

Buka Control UI di browser Anda lalu kirim pesan:

```
openclaw dashboard
```

Atau hubungkan sebuah channel ( [Telegram](https://docs.openclaw.ai/id/channels/telegram) adalah yang tercepat) lalu chat dari ponsel Anda.

Perlu penyiapan instalasi dan pengembangan lengkap? Lihat [Getting Started](https://docs.openclaw.ai/id/start/getting-started).

## Dasbor

Buka Control UI di browser setelah Gateway dimulai.

- Default lokal: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- Akses remote: [Web surfaces](https://docs.openclaw.ai/web) dan [Tailscale](https://docs.openclaw.ai/id/gateway/tailscale)

## Konfigurasi (opsional)

Config berada di `~/.openclaw/openclaw.json`.

- Jika Anda **tidak melakukan apa pun**, OpenClaw menggunakan biner Pi bawaan dalam mode RPC dengan sesi per pengirim.
- Jika Anda ingin menguncinya, mulai dari `channels.whatsapp.allowFrom` dan aturan mention (untuk grup).

Contoh:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## Mulai dari sini

[**Docs hubs** \\
\\
Semua dokumen dan panduan, diatur berdasarkan kasus penggunaan.](https://docs.openclaw.ai/id/start/hubs)

[**Configuration** \\
\\
Pengaturan inti Gateway, token, dan config provider.](https://docs.openclaw.ai/id/gateway/configuration)

[**Remote access** \\
\\
Pola akses SSH dan tailnet.](https://docs.openclaw.ai/id/gateway/remote)

[**Channels** \\
\\
Penyiapan khusus channel untuk Feishu, Microsoft Teams, WhatsApp, Telegram, Discord, dan lainnya.](https://docs.openclaw.ai/id/channels/telegram)

[**Nodes** \\
\\
Node iOS dan Android dengan pairing, Canvas, kamera, dan aksi perangkat.](https://docs.openclaw.ai/id/nodes)

[**Help** \\
\\
Perbaikan umum dan titik masuk pemecahan masalah.](https://docs.openclaw.ai/id/help)

## Pelajari lebih lanjut

[**Full feature list** \\
\\
Kemampuan lengkap channel, perutean, dan media.](https://docs.openclaw.ai/id/concepts/features)

[**Multi-agent routing** \\
\\
Isolasi workspace dan sesi per agen.](https://docs.openclaw.ai/id/concepts/multi-agent)

[**Security** \\
\\
Token, allowlist, dan kontrol keamanan.](https://docs.openclaw.ai/id/gateway/security)

[**Troubleshooting** \\
\\
Diagnostik Gateway dan error umum.](https://docs.openclaw.ai/id/gateway/troubleshooting)

[**About and credits** \\
\\
Asal-usul proyek, kontributor, dan lisensi.](https://docs.openclaw.ai/id/reference/credits)

[Showcase](https://docs.openclaw.ai/id/start/showcase)

Ctrl+I

---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai/ko>_

# OpenClaw 🦞

> _“EXFOLIATE! EXFOLIATE!”_ — 아마도 우주 바닷가재

**Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo 등 전반에서 AI 에이전트를 위한 모든 OS용 Gateway.**

메시지를 보내면 주머니 속에서 에이전트 응답을 받을 수 있습니다. 내장 채널, 번들 채널 Plugin, WebChat, 모바일 Node 전반에 걸쳐 하나의 Gateway를 실행하세요.

[**시작하기** \\
\\
OpenClaw을 설치하고 몇 분 안에 Gateway를 실행하세요.](https://docs.openclaw.ai/ko/start/getting-started)

[**온보딩 실행** \\
\\
`openclaw onboard`와 pairing 흐름을 사용한 안내형 설정입니다.](https://docs.openclaw.ai/ko/start/wizard)

[**Control UI 열기** \\
\\
채팅, config, 세션을 위한 브라우저 대시보드를 실행하세요.](https://docs.openclaw.ai/web/control-ui)

## OpenClaw이란?

OpenClaw은 **셀프 호스팅 Gateway** 로, Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo 등의 번들 또는 외부 채널 Plugin과 내장 채널을 Pi 같은 AI 코딩 에이전트에 연결합니다. 사용자는 자신의 머신(또는 서버)에서 단일 Gateway 프로세스를 실행하며, 이 프로세스가 메시징 앱과 항상 사용 가능한 AI 도우미 사이의 브리지 역할을 합니다.**누구를 위한 제품인가요?** 데이터를 계속 제어하고 호스팅 서비스에 의존하지 않으면서, 어디서나 메시지를 보낼 수 있는 개인 AI 도우미를 원하는 개발자와 고급 사용자입니다.**무엇이 다른가요?**

- **셀프 호스팅**: 내 하드웨어, 내 규칙으로 실행
- **멀티채널**: 하나의 Gateway가 내장 채널과 번들 또는 외부 채널 Plugin을 동시에 제공
- **에이전트 네이티브**: 도구 사용, 세션, 메모리, 다중 에이전트 라우팅을 갖춘 코딩 에이전트용으로 설계
- **오픈 소스**: MIT 라이선스, 커뮤니티 중심

**무엇이 필요한가요?** Node 24(권장) 또는 호환성을 위한 Node 22 LTS(`22.14+`), 선택한 provider의 API 키, 그리고 5분이면 됩니다. 최상의 품질과 보안을 위해 사용 가능한 최신 세대의 가장 강력한 모델을 사용하세요.

## 작동 방식

Gateway는 세션, 라우팅, 채널 연결을 위한 단일 진실 공급원입니다.

## 주요 기능

[**멀티채널 Gateway** \\
\\
단일 Gateway 프로세스로 Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat 등을 지원합니다.](https://docs.openclaw.ai/ko/channels)

[**Plugin 채널** \\
\\
번들 Plugin이 현재 일반 릴리스에서 Matrix, Nostr, Twitch, Zalo 등을 추가합니다.](https://docs.openclaw.ai/ko/tools/plugin)

[**다중 에이전트 라우팅** \\
\\
에이전트, 워크스페이스 또는 발신자별로 세션을 격리합니다.](https://docs.openclaw.ai/ko/concepts/multi-agent)

[**미디어 지원** \\
\\
이미지, 오디오, 문서를 보내고 받을 수 있습니다.](https://docs.openclaw.ai/ko/nodes/images)

[**Web Control UI** \\
\\
채팅, config, 세션, Node를 위한 브라우저 대시보드입니다.](https://docs.openclaw.ai/web/control-ui)

[**모바일 Node** \\
\\
Canvas, 카메라, 음성 지원 워크플로를 위한 iOS 및 Android Node를 pairing합니다.](https://docs.openclaw.ai/ko/nodes)

## 빠른 시작

1

[Navigate to header](https://docs.openclaw.ai/ko#)

OpenClaw 설치

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/ko#)

온보딩 및 서비스 설치

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/ko#)

채팅

브라우저에서 Control UI를 열고 메시지를 보내세요:

```
openclaw dashboard
```

또는 채널을 연결하고( [Telegram](https://docs.openclaw.ai/ko/channels/telegram) 이 가장 빠름) 휴대폰에서 채팅하세요.

전체 설치 및 개발 설정이 필요하신가요? [시작하기](https://docs.openclaw.ai/ko/start/getting-started) 를 참고하세요.

## 대시보드

Gateway가 시작되면 브라우저 Control UI를 여세요.

- 로컬 기본값: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- 원격 액세스: [Web 표면](https://docs.openclaw.ai/web) 및 [Tailscale](https://docs.openclaw.ai/ko/gateway/tailscale)

## 구성(선택 사항)

config는 `~/.openclaw/openclaw.json`에 있습니다.

- **아무것도 하지 않으면**, OpenClaw은 번들 Pi 바이너리를 RPC 모드와 발신자별 세션으로 사용합니다.
- 잠그고 싶다면 `channels.whatsapp.allowFrom`과 (그룹의 경우) 멘션 규칙부터 시작하세요.

예시:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## 여기서 시작하세요

[**문서 허브** \\
\\
사용 사례별로 정리된 모든 문서와 가이드입니다.](https://docs.openclaw.ai/ko/start/hubs)

[**구성** \\
\\
핵심 Gateway 설정, 토큰, provider config입니다.](https://docs.openclaw.ai/ko/gateway/configuration)

[**원격 액세스** \\
\\
SSH 및 tailnet 액세스 패턴입니다.](https://docs.openclaw.ai/ko/gateway/remote)

[**채널** \\
\\
Feishu, Microsoft Teams, WhatsApp, Telegram, Discord 등의 채널별 설정입니다.](https://docs.openclaw.ai/ko/channels/telegram)

[**Node** \\
\\
pairing, Canvas, 카메라, 디바이스 작업을 지원하는 iOS 및 Android Node입니다.](https://docs.openclaw.ai/ko/nodes)

[**도움말** \\
\\
일반적인 수정 사항과 문제 해결 시작점입니다.](https://docs.openclaw.ai/ko/help)

## 더 알아보기

[**전체 기능 목록** \\
\\
완전한 채널, 라우팅, 미디어 기능입니다.](https://docs.openclaw.ai/ko/concepts/features)

[**다중 에이전트 라우팅** \\
\\
워크스페이스 격리 및 에이전트별 세션입니다.](https://docs.openclaw.ai/ko/concepts/multi-agent)

[**보안** \\
\\
토큰, 허용 목록, 안전 제어입니다.](https://docs.openclaw.ai/ko/gateway/security)

[**문제 해결** \\
\\
Gateway 진단 및 일반적인 오류입니다.](https://docs.openclaw.ai/ko/gateway/troubleshooting)

[**정보 및 크레딧** \\
\\
프로젝트 기원, 기여자, 라이선스입니다.](https://docs.openclaw.ai/ko/reference/credits)

[쇼케이스](https://docs.openclaw.ai/ko/start/showcase)

Ctrl+I

---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai/uk>_

# OpenClaw 🦞

> _“ВІДЛУЩУЙ! ВІДЛУЩУЙ!”_ — Космічний лобстер, імовірно

**Gateway для AI-агентів на будь-якій ОС у Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo тощо.**

Надішліть повідомлення й отримайте відповідь агента просто зі своєї кишені. Запустіть один Gateway для вбудованих каналів, комплектних channel plugins, WebChat і mobile nodes.

[**Почати** \\
\\
Встановіть OpenClaw і запустіть Gateway за лічені хвилини.](https://docs.openclaw.ai/uk/start/getting-started)

[**Запустити онбординг** \\
\\
Покрокове налаштування за допомогою `openclaw onboard` і процесів сполучення.](https://docs.openclaw.ai/uk/start/wizard)

[**Відкрити Control UI** \\
\\
Запустіть панель керування в браузері для чату, конфігурації та сесій.](https://docs.openclaw.ai/web/control-ui)

## Що таке OpenClaw?

OpenClaw — це **self-hosted Gateway**, який з’єднує ваші улюблені застосунки для чату та поверхні каналів — вбудовані канали, а також комплектні чи зовнішні channel plugins, як-от Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo та інші — з AI coding agents, такими як Pi. Ви запускаєте один процес Gateway на власному комп’ютері (або сервері), і він стає мостом між вашими застосунками для обміну повідомленнями та AI-помічником, який завжди доступний.**Для кого це?** Для розробників і досвідчених користувачів, які хочуть мати персонального AI-помічника, якому можна написати звідусіль, — не втрачаючи контроль над своїми даними й не покладаючись на хостинговий сервіс.**Що робить його особливим?**

- **Self-hosted**: працює на вашому обладнанні, за вашими правилами
- **Багатоканальність**: один Gateway одночасно обслуговує вбудовані канали, а також комплектні чи зовнішні channel plugins
- **Орієнтований на агентів**: створений для coding agents з використанням інструментів, сесіями, пам’яттю та маршрутизацією між кількома агентами
- **Відкритий код**: ліцензія MIT, розвиток спільнотою

**Що вам потрібно?** Node 24 (рекомендовано) або Node 22 LTS (`22.14+`) для сумісності, API-ключ від обраного провайдера та 5 хвилин. Для найкращої якості й безпеки використовуйте найпотужнішу доступну модель останнього покоління.

## Як це працює

Gateway — це єдине джерело істини для сесій, маршрутизації та підключень каналів.

## Ключові можливості

[**Багатоканальний Gateway** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat та інші через один процес Gateway.](https://docs.openclaw.ai/uk/channels)

[**Канали Plugin** \\
\\
Комплектні plugins додають Matrix, Nostr, Twitch, Zalo та інші в типових поточних релізах.](https://docs.openclaw.ai/uk/tools/plugin)

[**Маршрутизація між кількома агентами** \\
\\
Ізольовані сесії для кожного агента, робочого простору або відправника.](https://docs.openclaw.ai/uk/concepts/multi-agent)

[**Підтримка медіа** \\
\\
Надсилайте й отримуйте зображення, аудіо та документи.](https://docs.openclaw.ai/uk/nodes/images)

[**Web Control UI** \\
\\
Панель у браузері для чату, конфігурації, сесій і nodes.](https://docs.openclaw.ai/web/control-ui)

[**Mobile nodes** \\
\\
Підключайте iOS і Android nodes для Canvas, камери та сценаріїв із підтримкою голосу.](https://docs.openclaw.ai/uk/nodes)

## Швидкий старт

1

[Navigate to header](https://docs.openclaw.ai/uk/#)

Встановіть OpenClaw

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/uk/#)

Пройдіть онбординг і встановіть службу

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/uk/#)

Спілкуйтеся

Відкрийте Control UI у браузері та надішліть повідомлення:

```
openclaw dashboard
```

Або підключіть канал ( [Telegram](https://docs.openclaw.ai/uk/channels/telegram) — найшвидший варіант) і спілкуйтеся з телефона.

Потрібні повні інструкції зі встановлення та налаштування середовища розробки? Див. [Getting Started](https://docs.openclaw.ai/uk/start/getting-started).

## Панель керування

Відкрийте browser Control UI після запуску Gateway.

- Локальний варіант за замовчуванням: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- Віддалений доступ: [Web surfaces](https://docs.openclaw.ai/web) і [Tailscale](https://docs.openclaw.ai/uk/gateway/tailscale)

## Конфігурація (необов’язково)

Конфігурація зберігається в `~/.openclaw/openclaw.json`.

- Якщо ви **нічого не робите**, OpenClaw використовує комплектний двійковий файл Pi у режимі RPC із сесіями для кожного відправника.
- Якщо ви хочете обмежити доступ, почніть із `channels.whatsapp.allowFrom` і (для груп) правил згадування.

Приклад:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## Почніть тут

[**Центри документації** \\
\\
Уся документація й посібники, упорядковані за сценаріями використання.](https://docs.openclaw.ai/uk/start/hubs)

[**Конфігурація** \\
\\
Основні параметри Gateway, токени та конфігурація провайдера.](https://docs.openclaw.ai/uk/gateway/configuration)

[**Віддалений доступ** \\
\\
Шаблони доступу через SSH і tailnet.](https://docs.openclaw.ai/uk/gateway/remote)

[**Канали** \\
\\
Налаштування каналів для Feishu, Microsoft Teams, WhatsApp, Telegram, Discord та інших.](https://docs.openclaw.ai/uk/channels/telegram)

[**Nodes** \\
\\
iOS і Android nodes із pairing, Canvas, камерою та діями на пристрої.](https://docs.openclaw.ai/uk/nodes)

[**Довідка** \\
\\
Типові виправлення та стартова точка для усунення несправностей.](https://docs.openclaw.ai/uk/help)

## Дізнайтеся більше

[**Повний список можливостей** \\
\\
Повний перелік можливостей каналів, маршрутизації та роботи з медіа.](https://docs.openclaw.ai/uk/concepts/features)

[**Маршрутизація між кількома агентами** \\
\\
Ізоляція робочих просторів і сесії для кожного агента.](https://docs.openclaw.ai/uk/concepts/multi-agent)

[**Безпека** \\
\\
Токени, allowlists і засоби безпеки.](https://docs.openclaw.ai/uk/gateway/security)

[**Усунення несправностей** \\
\\
Діагностика Gateway і типові помилки.](https://docs.openclaw.ai/uk/gateway/troubleshooting)

[**Про проєкт і подяки** \\
\\
Походження проєкту, учасники та ліцензія.](https://docs.openclaw.ai/uk/reference/credits)

[Вітрина](https://docs.openclaw.ai/uk/start/showcase)

Ctrl+I

---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai/zh-CN>_

# OpenClaw 🦞

> _“蜕皮！蜕皮！”_ — 大概是一只太空龙虾

**适用于 AI 智能体的任意操作系统 Gateway 网关，覆盖 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等渠道。**

发一条消息，就能随时随地收到智能体回复。通过一个 Gateway 网关即可运行内置渠道、内置渠道插件、WebChat 和移动节点。

[**入门指南** \\
\\
安装 OpenClaw，几分钟内启动 Gateway 网关。](https://docs.openclaw.ai/zh-CN/start/getting-started)

[**运行新手引导** \\
\\
使用 `openclaw onboard` 和配对流程进行引导式设置。](https://docs.openclaw.ai/zh-CN/start/wizard)

[**打开控制 UI** \\
\\
启动浏览器仪表板，用于聊天、配置和会话。](https://docs.openclaw.ai/web/control-ui)

## OpenClaw 是什么？

OpenClaw 是一个 **自托管 Gateway 网关**，可将你常用的聊天应用和渠道界面——包括内置渠道，以及 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等内置或外部渠道插件——连接到像 Pi 这样的 AI 编码智能体。你只需在自己的机器上（或服务器上）运行一个 Gateway 网关进程，它就会成为你的消息应用与一个始终在线的 AI 助手之间的桥梁。**它适合谁？** 适合开发者和高级用户，他们希望拥有一个可随时随地发送消息的个人 AI 助手——同时不放弃对自己数据的控制，也不依赖托管服务。**它有什么不同？**

- **自托管**：运行在你的硬件上，按你的规则工作
- **多渠道**：一个 Gateway 网关可同时服务内置渠道以及内置或外部渠道插件
- **智能体原生**：专为编码智能体打造，支持工具使用、会话、记忆和多智能体路由
- **开源**：采用 MIT 许可证，由社区驱动

**你需要什么？** Node 24（推荐），或兼容用的 Node 22 LTS（`22.14+`），你所选提供商的 API 密钥，以及 5 分钟时间。为了获得最佳质量与安全性，请使用当前可用的最新一代最强模型。

## 工作原理

聊天应用 \+ 插件

Gateway 网关

Pi 智能体

CLI

Web 控制 UI

macOS 应用

iOS 和 Android 节点

Gateway 网关是会话、路由和渠道连接的唯一事实来源。

## 关键能力

[**多渠道 Gateway 网关** \\
\\
通过单个 Gateway 网关进程接入 Discord、iMessage、Signal、Slack、Telegram、WhatsApp、WebChat 等更多渠道。](https://docs.openclaw.ai/zh-CN/channels)

[**插件渠道** \\
\\
内置插件可在当前正式版本中添加 Matrix、Nostr、Twitch、Zalo 等更多渠道。](https://docs.openclaw.ai/zh-CN/tools/plugin)

[**多智能体路由** \\
\\
针对每个智能体、工作区或发送者提供隔离的会话。](https://docs.openclaw.ai/zh-CN/concepts/multi-agent)

[**媒体支持** \\
\\
发送和接收图片、音频与文档。](https://docs.openclaw.ai/zh-CN/nodes/images)

[**Web 控制 UI** \\
\\
用于聊天、配置、会话和节点的浏览器仪表板。](https://docs.openclaw.ai/web/control-ui)

[**移动节点** \\
\\
配对 iOS 和 Android 节点，用于 Canvas、相机和支持语音的工作流。](https://docs.openclaw.ai/zh-CN/nodes)

## 快速开始

1

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

安装 OpenClaw

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

执行新手引导并安装服务

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

开始聊天

在浏览器中打开控制 UI 并发送一条消息：

```
openclaw dashboard
```

或连接一个渠道（ [Telegram](https://docs.openclaw.ai/zh-CN/channels/telegram) 最快），然后直接用手机聊天。

需要完整的安装和开发设置？请参阅 [入门指南](https://docs.openclaw.ai/zh-CN/start/getting-started)。

## 仪表板

Gateway 网关启动后，在浏览器中打开控制 UI。

- 本地默认地址： [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- 远程访问： [Web 界面](https://docs.openclaw.ai/web) 和 [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

## 配置（可选）

配置文件位于 `~/.openclaw/openclaw.json`。

- 如果你 **什么都不做**，OpenClaw 会使用内置的 Pi 二进制并以 RPC 模式运行，同时为每个发送者创建独立会话。
- 如果你想进一步收紧权限，建议从 `channels.whatsapp.allowFrom` 和（针对群组的）提及规则开始。

示例：

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## 从这里开始

[**文档中心** \\
\\
按使用场景组织的所有文档和指南。](https://docs.openclaw.ai/zh-CN/start/hubs)

[**配置** \\
\\
核心 Gateway 网关设置、令牌和提供商配置。](https://docs.openclaw.ai/zh-CN/gateway/configuration)

[**远程访问** \\
\\
SSH 和 tailnet 访问模式。](https://docs.openclaw.ai/zh-CN/gateway/remote)

[**渠道** \\
\\
针对 Feishu、Microsoft Teams、WhatsApp、Telegram、Discord 等渠道的专属设置说明。](https://docs.openclaw.ai/zh-CN/channels/telegram)

[**节点** \\
\\
iOS 和 Android 节点，支持配对、Canvas、相机和设备操作。](https://docs.openclaw.ai/zh-CN/nodes)

[**帮助** \\
\\
常见修复方法和故障排除入口。](https://docs.openclaw.ai/zh-CN/help)

## 了解更多

[**完整功能列表** \\
\\
完整的渠道、路由和媒体能力说明。](https://docs.openclaw.ai/zh-CN/concepts/features)

[**多智能体路由** \\
\\
工作区隔离和按智能体划分的会话。](https://docs.openclaw.ai/zh-CN/concepts/multi-agent)

[**安全** \\
\\
令牌、允许列表和安全控制。](https://docs.openclaw.ai/zh-CN/gateway/security)

[**故障排除** \\
\\
Gateway 网关诊断和常见错误。](https://docs.openclaw.ai/zh-CN/gateway/troubleshooting)

[**关于与致谢** \\
\\
项目起源、贡献者和许可证。](https://docs.openclaw.ai/zh-CN/reference/credits)

[展示专区](https://docs.openclaw.ai/zh-CN/start/showcase)

Ctrl+I

---
