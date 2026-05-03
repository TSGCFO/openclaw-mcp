---
source_url: https://docs.openclaw.ai/ar/channels
title: "\u0642\u0646\u0648\u0627\u062a \u0627\u0644\u062f\u0631\u062f\u0634\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Overview

قنوات الدردشة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [ملاحظات التسليم](https://docs.openclaw.ai/ar/channels#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85)
- [القنوات المدعومة](https://docs.openclaw.ai/ar/channels#%D8%A7%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AF%D8%B9%D9%88%D9%85%D8%A9)
- [ملاحظات](https://docs.openclaw.ai/ar/channels#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw يمكنه التحدث إليك عبر أي تطبيق دردشة تستخدمه بالفعل. تتصل كل قناة عبر Gateway.
النص مدعوم في كل مكان؛ أما الوسائط والتفاعلات فتختلف حسب القناة.

## [​](https://docs.openclaw.ai/ar/channels\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85)  ملاحظات التسليم

- يتم تحويل ردود Telegram التي تحتوي على صيغة صور markdown، مثل `![alt](url)`،
إلى ردود وسائط في مسار الإرسال النهائي عندما يكون ذلك ممكنًا.
- يتم توجيه رسائل Slack المباشرة متعددة الأشخاص كدردشات جماعية، لذلك تنطبق سياسة المجموعات وسلوك الإشارات وقواعد جلسات المجموعات على محادثات MPIM.
- إعداد WhatsApp يعمل بالتثبيت عند الطلب: يمكن أن يعرض الإعداد الأولي مسار الإعداد قبل
تثبيت حزمة plugin، ولا يحمّل Gateway وقت تشغيل WhatsApp
إلا عندما تكون القناة نشطة فعليًا.

## [​](https://docs.openclaw.ai/ar/channels\#%D8%A7%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AF%D8%B9%D9%88%D9%85%D8%A9)  القنوات المدعومة

- [BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles) — **موصى به لـ iMessage**؛ يستخدم واجهة BlueBubbles macOS server REST API مع دعم كامل للميزات (plugin مضمّن؛ تعديل، إلغاء إرسال، مؤثرات، تفاعلات، إدارة المجموعات — التعديل معطّل حاليًا على macOS 26 Tahoe).
- [Discord](https://docs.openclaw.ai/ar/channels/discord) — Discord Bot API + Gateway؛ يدعم الخوادم والقنوات والرسائل المباشرة.
- [Feishu](https://docs.openclaw.ai/ar/channels/feishu) — روبوت Feishu/Lark عبر WebSocket (plugin مضمّن).
- [Google Chat](https://docs.openclaw.ai/ar/channels/googlechat) — تطبيق Google Chat API عبر HTTP webhook (plugin قابل للتنزيل).
- [iMessage (القديم)](https://docs.openclaw.ai/ar/channels/imessage) — تكامل macOS القديم عبر imsg CLI (مهمل، استخدم BlueBubbles للإعدادات الجديدة).
- [IRC](https://docs.openclaw.ai/ar/channels/irc) — خوادم IRC الكلاسيكية؛ قنوات + رسائل مباشرة مع عناصر تحكم الاقتران/قائمة السماح.
- [LINE](https://docs.openclaw.ai/ar/channels/line) — روبوت LINE Messaging API (plugin قابل للتنزيل).
- [Matrix](https://docs.openclaw.ai/ar/channels/matrix) — بروتوكول Matrix (plugin قابل للتنزيل).
- [Mattermost](https://docs.openclaw.ai/ar/channels/mattermost) — Bot API + WebSocket؛ قنوات ومجموعات ورسائل مباشرة (plugin قابل للتنزيل).
- [Microsoft Teams](https://docs.openclaw.ai/ar/channels/msteams) — Bot Framework؛ دعم مؤسسي (plugin مضمّن).
- [Nextcloud Talk](https://docs.openclaw.ai/ar/channels/nextcloud-talk) — دردشة مستضافة ذاتيًا عبر Nextcloud Talk (plugin مضمّن).
- [Nostr](https://docs.openclaw.ai/ar/channels/nostr) — رسائل مباشرة لامركزية عبر NIP-04 (plugin مضمّن).
- [QQ Bot](https://docs.openclaw.ai/ar/channels/qqbot) — QQ Bot API؛ دردشة خاصة ودردشة جماعية ووسائط غنية (plugin مضمّن).
- [Signal](https://docs.openclaw.ai/ar/channels/signal) — signal-cli؛ يركز على الخصوصية.
- [Slack](https://docs.openclaw.ai/ar/channels/slack) — Bolt SDK؛ تطبيقات مساحات العمل.
- [Synology Chat](https://docs.openclaw.ai/ar/channels/synology-chat) — Synology NAS Chat عبر webhooks صادرة + واردة (plugin مضمّن).
- [Telegram](https://docs.openclaw.ai/ar/channels/telegram) — Bot API عبر grammY؛ يدعم المجموعات.
- [Tlon](https://docs.openclaw.ai/ar/channels/tlon) — مراسلة مبنية على Urbit (plugin مضمّن).
- [Twitch](https://docs.openclaw.ai/ar/channels/twitch) — دردشة Twitch عبر اتصال IRC (plugin مضمّن).
- [Voice Call](https://docs.openclaw.ai/ar/plugins/voice-call) — اتصالات هاتفية عبر Plivo أو Twilio (plugin، يُثبّت بشكل منفصل).
- [WebChat](https://docs.openclaw.ai/ar/web/webchat) — واجهة Gateway WebChat عبر WebSocket.
- [WeChat](https://docs.openclaw.ai/ar/channels/wechat) — plugin Tencent iLink Bot عبر تسجيل الدخول برمز QR؛ الدردشات الخاصة فقط (plugin خارجي).
- [WhatsApp](https://docs.openclaw.ai/ar/channels/whatsapp) — الأكثر شيوعًا؛ يستخدم Baileys ويتطلب اقتران QR.
- [Yuanbao](https://docs.openclaw.ai/ar/channels/yuanbao) — روبوت Tencent Yuanbao (plugin خارجي).
- [Zalo](https://docs.openclaw.ai/ar/channels/zalo) — Zalo Bot API؛ تطبيق المراسلة الشائع في فيتنام (plugin مضمّن).
- [Zalo Personal](https://docs.openclaw.ai/ar/channels/zalouser) — حساب Zalo شخصي عبر تسجيل الدخول برمز QR (plugin مضمّن).

## [​](https://docs.openclaw.ai/ar/channels\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)  ملاحظات

- يمكن تشغيل القنوات في الوقت نفسه؛ اضبط قنوات متعددة وسيوجّه OpenClaw الرسائل حسب كل دردشة.
- أسرع إعداد يكون عادةً **Telegram** (رمز روبوت بسيط). يتطلب WhatsApp اقتران QR
ويخزّن حالة أكثر على القرص.
- يختلف سلوك المجموعات حسب القناة؛ راجع [المجموعات](https://docs.openclaw.ai/ar/channels/groups).
- يتم فرض اقتران الرسائل المباشرة وقوائم السماح لأغراض السلامة؛ راجع [الأمان](https://docs.openclaw.ai/ar/gateway/security).
- استكشاف الأخطاء وإصلاحها: [استكشاف أخطاء القنوات وإصلاحها](https://docs.openclaw.ai/ar/channels/troubleshooting).
- موفرو النماذج موثقون بشكل منفصل؛ راجع [موفرو النماذج](https://docs.openclaw.ai/ar/providers/models).

[Discord](https://docs.openclaw.ai/ar/channels/discord)

Ctrl+I