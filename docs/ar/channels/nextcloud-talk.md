---
source_url: https://docs.openclaw.ai/ar/channels/nextcloud-talk
title: "Nextcloud Talk - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/nextcloud-talk#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

Nextcloud Talk

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [Plugin مُضمَّن](https://docs.openclaw.ai/ar/channels/nextcloud-talk#plugin-%D9%85%D9%8F%D8%B6%D9%85%D9%91%D9%8E%D9%86)
- [إعداد سريع (للمبتدئين)](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9-%D9%84%D9%84%D9%85%D8%A8%D8%AA%D8%AF%D8%A6%D9%8A%D9%86)
- [ملاحظات](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)
- [التحكم في الوصول (الرسائل المباشرة)](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9)
- [الغرف (المجموعات)](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D8%A7%D9%84%D8%BA%D8%B1%D9%81-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)
- [القدرات](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D8%A7%D9%84%D9%82%D8%AF%D8%B1%D8%A7%D8%AA)
- [مرجع الإعداد (Nextcloud Talk)](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-nextcloud-talk)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/nextcloud-talk#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

الحالة: Plugin مُضمَّن (روبوت Webhook). الرسائل المباشرة، والغرف، والتفاعلات، ورسائل Markdown مدعومة.

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#plugin-%D9%85%D9%8F%D8%B6%D9%85%D9%91%D9%8E%D9%86)  Plugin مُضمَّن

يأتي Nextcloud Talk بوصفه Plugin مُضمَّنًا في إصدارات OpenClaw الحالية، لذلك
لا تحتاج البُنى المعبأة العادية إلى تثبيت منفصل.إذا كنت تستخدم بناءً أقدم أو تثبيتًا مخصصًا يستبعد Nextcloud Talk،
فثبّت حزمة npm حالية عند نشرها:التثبيت عبر CLI (سجل npm، عند وجود حزمة حالية):

```
openclaw plugins install @openclaw/nextcloud-talk
```

إذا أبلغ npm أن الحزمة المملوكة لـ OpenClaw مهملة، فاستخدم بناء OpenClaw
معبأً حاليًا أو مسار النسخة المحلية إلى أن تُنشر حزمة npm أحدث.نسخة محلية (عند التشغيل من مستودع git):

```
openclaw plugins install ./path/to/local/nextcloud-talk-plugin
```

التفاصيل: [Plugins](https://docs.openclaw.ai/ar/tools/plugin)

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9-%D9%84%D9%84%D9%85%D8%A8%D8%AA%D8%AF%D8%A6%D9%8A%D9%86)  إعداد سريع (للمبتدئين)

1. تأكد من توفر Plugin الخاص بـ Nextcloud Talk.   - إصدارات OpenClaw المعبأة الحالية تتضمنه بالفعل.
   - يمكن للتثبيتات الأقدم/المخصصة إضافته يدويًا بالأوامر أعلاه.
2. على خادم Nextcloud لديك، أنشئ روبوتًا:














```
./occ talk:bot:install "OpenClaw" "<shared-secret>" "<webhook-url>" --feature reaction
```

3. فعّل الروبوت في إعدادات الغرفة المستهدفة.
4. اضبط OpenClaw:

   - الإعداد: `channels.nextcloud-talk.baseUrl` \+ `channels.nextcloud-talk.botSecret`
   - أو متغير البيئة: `NEXTCLOUD_TALK_BOT_SECRET` (الحساب الافتراضي فقط)

إعداد CLI:

```
openclaw channels add --channel nextcloud-talk \
  --url https://cloud.example.com \
  --token "<shared-secret>"
```

الحقول الصريحة المكافئة:

```
openclaw channels add --channel nextcloud-talk \
  --base-url https://cloud.example.com \
  --secret "<shared-secret>"
```

سر مدعوم بملف:

```
openclaw channels add --channel nextcloud-talk \
  --base-url https://cloud.example.com \
  --secret-file /path/to/nextcloud-talk-secret
```

5. أعد تشغيل Gateway (أو أكمل الإعداد).

إعداد أدنى:

```
{
  channels: {
    "nextcloud-talk": {
      enabled: true,
      baseUrl: "https://cloud.example.com",
      botSecret: "shared-secret",
      dmPolicy: "pairing",
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)  ملاحظات

- لا تستطيع الروبوتات بدء الرسائل المباشرة. يجب أن يراسل المستخدم الروبوت أولًا.
- يجب أن يكون عنوان Webhook URL قابلًا للوصول بواسطة Gateway؛ عيّن `webhookPublicUrl` إذا كان خلف وكيل.
- رفع الوسائط غير مدعوم بواسطة واجهة API الخاصة بالروبوت؛ تُرسل الوسائط على شكل عناوين URL.
- لا تميّز حمولة Webhook بين الرسائل المباشرة والغرف؛ عيّن `apiUser` \+ `apiPassword` لتمكين الاستعلام عن نوع الغرفة (وإلا فستُعامل الرسائل المباشرة كغرف).

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9)  التحكم في الوصول (الرسائل المباشرة)

- الافتراضي: `channels.nextcloud-talk.dmPolicy = "pairing"`. يحصل المرسلون غير المعروفين على رمز إقران.
- الموافقة عبر:
  - `openclaw pairing list nextcloud-talk`
  - `openclaw pairing approve nextcloud-talk <CODE>`
- الرسائل المباشرة العامة: `channels.nextcloud-talk.dmPolicy="open"` بالإضافة إلى `channels.nextcloud-talk.allowFrom=["*"]`.
- يطابق `allowFrom` معرّفات مستخدمي Nextcloud فقط؛ تُتجاهل أسماء العرض.

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D8%A7%D9%84%D8%BA%D8%B1%D9%81-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)  الغرف (المجموعات)

- الافتراضي: `channels.nextcloud-talk.groupPolicy = "allowlist"` (مقيّد بالإشارة).
- أضف الغرف إلى قائمة السماح باستخدام `channels.nextcloud-talk.rooms`:

```
{
  channels: {
    "nextcloud-talk": {
      rooms: {
        "room-token": { requireMention: true },
      },
    },
  },
}
```

- لعدم السماح بأي غرف، أبقِ قائمة السماح فارغة أو عيّن `channels.nextcloud-talk.groupPolicy="disabled"`.

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D8%A7%D9%84%D9%82%D8%AF%D8%B1%D8%A7%D8%AA)  القدرات

| الميزة | الحالة |
| --- | --- |
| الرسائل المباشرة | مدعومة |
| الغرف | مدعومة |
| سلاسل المحادثات | غير مدعومة |
| الوسائط | عناوين URL فقط |
| التفاعلات | مدعومة |
| الأوامر الأصلية | غير مدعومة |

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-nextcloud-talk)  مرجع الإعداد (Nextcloud Talk)

الإعداد الكامل: [الإعداد](https://docs.openclaw.ai/ar/gateway/configuration)خيارات المزوّد:

- `channels.nextcloud-talk.enabled`: تمكين/تعطيل بدء تشغيل القناة.
- `channels.nextcloud-talk.baseUrl`: عنوان URL لمثيل Nextcloud.
- `channels.nextcloud-talk.botSecret`: السر المشترك للروبوت.
- `channels.nextcloud-talk.botSecretFile`: مسار سر في ملف عادي. تُرفض الروابط الرمزية.
- `channels.nextcloud-talk.apiUser`: مستخدم API للاستعلام عن الغرف (اكتشاف الرسائل المباشرة).
- `channels.nextcloud-talk.apiPassword`: كلمة مرور API/التطبيق للاستعلام عن الغرف.
- `channels.nextcloud-talk.apiPasswordFile`: مسار ملف كلمة مرور API.
- `channels.nextcloud-talk.webhookPort`: منفذ مستمع Webhook (الافتراضي: 8788).
- `channels.nextcloud-talk.webhookHost`: مضيف Webhook (الافتراضي: 0.0.0.0).
- `channels.nextcloud-talk.webhookPath`: مسار Webhook (الافتراضي: /nextcloud-talk-webhook).
- `channels.nextcloud-talk.webhookPublicUrl`: عنوان URL لـ Webhook يمكن الوصول إليه خارجيًا.
- `channels.nextcloud-talk.dmPolicy`: `pairing | allowlist | open | disabled`.
- `channels.nextcloud-talk.allowFrom`: قائمة سماح الرسائل المباشرة (معرّفات المستخدمين). يتطلب `open` وجود `"*"`.
- `channels.nextcloud-talk.groupPolicy`: `allowlist | open | disabled`.
- `channels.nextcloud-talk.groupAllowFrom`: قائمة سماح المجموعات (معرّفات المستخدمين).
- `channels.nextcloud-talk.rooms`: إعدادات وقائمة سماح لكل غرفة.
- `channels.nextcloud-talk.historyLimit`: حد سجل المجموعات (0 يعطّله).
- `channels.nextcloud-talk.dmHistoryLimit`: حد سجل الرسائل المباشرة (0 يعطّله).
- `channels.nextcloud-talk.dms`: تجاوزات لكل رسالة مباشرة (historyLimit).
- `channels.nextcloud-talk.textChunkLimit`: حجم مقطع النص الصادر (أحرف).
- `channels.nextcloud-talk.chunkMode`: `length` (الافتراضي) أو `newline` للتقسيم عند الأسطر الفارغة (حدود الفقرات) قبل التقسيم حسب الطول.
- `channels.nextcloud-talk.blockStreaming`: تعطيل بث الكتل لهذه القناة.
- `channels.nextcloud-talk.blockStreamingCoalesce`: ضبط تجميع بث الكتل.
- `channels.nextcloud-talk.mediaMaxMb`: حد الوسائط الواردة (MB).

## [​](https://docs.openclaw.ai/ar/channels/nextcloud-talk\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — كل القنوات المدعومة
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الإقران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك محادثات المجموعات والتقييد بالإشارة
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه جلسات الرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Mattermost](https://docs.openclaw.ai/ar/channels/mattermost) [Nostr](https://docs.openclaw.ai/ar/channels/nostr)

Ctrl+I