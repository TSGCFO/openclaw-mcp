---
source_url: https://docs.openclaw.ai/ar/channels/synology-chat
title: "Synology Chat - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/synology-chat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

Synology Chat

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [Plugin مضمّن](https://docs.openclaw.ai/ar/channels/synology-chat#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)
- [الإعداد السريع](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [متغيرات البيئة](https://docs.openclaw.ai/ar/channels/synology-chat#%D9%85%D8%AA%D8%BA%D9%8A%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D9%8A%D8%A6%D8%A9)
- [سياسة الرسائل المباشرة والتحكم في الوصول](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9-%D9%88%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [التسليم الصادر](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1)
- [الحسابات المتعددة](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)
- [ملاحظات الأمان](https://docs.openclaw.ai/ar/channels/synology-chat#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%85%D8%A7%D9%86)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/synology-chat#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

الحالة: قناة رسائل مباشرة عبر Plugin مضمّن يستخدم Webhook الخاصة بـ Synology Chat.
يقبل Plugin الرسائل الواردة من Webhook الصادرة في Synology Chat ويرسل الردود
عبر Webhook وارد في Synology Chat.

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)  Plugin مضمّن

يأتي Synology Chat كـ Plugin مضمّن في إصدارات OpenClaw الحالية، لذلك لا تحتاج
البُنى المعبأة العادية إلى تثبيت منفصل.إذا كنت تستخدم بنية أقدم أو تثبيتًا مخصصًا يستبعد Synology Chat،
فثبّته يدويًا:التثبيت من نسخة محلية:

```
openclaw plugins install ./path/to/local/synology-chat-plugin
```

التفاصيل: [Plugins](https://docs.openclaw.ai/ar/tools/plugin)

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  الإعداد السريع

1. تأكد من توفر Plugin الخاص بـ Synology Chat.
   - إصدارات OpenClaw المعبأة الحالية تضمنه بالفعل.
   - يمكن للتثبيتات الأقدم/المخصصة إضافته يدويًا من نسخة مصدرية باستخدام الأمر أعلاه.
   - يعرض `openclaw onboard` الآن Synology Chat في قائمة إعداد القنوات نفسها مثل `openclaw channels add`.
   - إعداد غير تفاعلي: `openclaw channels add --channel synology-chat --token <token> --url <incoming-webhook-url>`
2. في تكاملات Synology Chat:
   - أنشئ Webhook واردًا وانسخ عنوان URL الخاص به.
   - أنشئ Webhook صادرًا مع الرمز السري الخاص بك.
3. وجّه عنوان URL الخاص بالـ Webhook الصادر إلى Gateway الخاص بـ OpenClaw:
   - `https://gateway-host/webhook/synology` افتراضيًا.
   - أو `channels.synology-chat.webhookPath` المخصص لديك.
4. أكمل الإعداد في OpenClaw.
   - موجّه: `openclaw onboard`
   - مباشر: `openclaw channels add --channel synology-chat --token <token> --url <incoming-webhook-url>`
5. أعد تشغيل Gateway وأرسل رسالة مباشرة إلى بوت Synology Chat.

تفاصيل مصادقة Webhook:

- يقبل OpenClaw رمز Webhook الصادر من `body.token`، ثم
`?token=...`، ثم الترويسات.
- صيغ الترويسات المقبولة:
  - `x-synology-token`
  - `x-webhook-token`
  - `x-openclaw-token`
  - `Authorization: Bearer <token>`
- الرموز الفارغة أو المفقودة تفشل بإغلاق آمن.

الحد الأدنى من الإعداد:

```
{
  channels: {
    "synology-chat": {
      enabled: true,
      token: "synology-outgoing-token",
      incomingUrl: "https://nas.example.com/webapi/entry.cgi?api=SYNO.Chat.External&method=incoming&version=2&token=...",
      webhookPath: "/webhook/synology",
      dmPolicy: "allowlist",
      allowedUserIds: ["123456"],
      rateLimitPerMinute: 30,
      allowInsecureSsl: false,
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D9%85%D8%AA%D8%BA%D9%8A%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D9%8A%D8%A6%D8%A9)  متغيرات البيئة

للحساب الافتراضي، يمكنك استخدام متغيرات البيئة:

- `SYNOLOGY_CHAT_TOKEN`
- `SYNOLOGY_CHAT_INCOMING_URL`
- `SYNOLOGY_NAS_HOST`
- `SYNOLOGY_ALLOWED_USER_IDS` (مفصولة بفواصل)
- `SYNOLOGY_RATE_LIMIT`
- `OPENCLAW_BOT_NAME`

تتجاوز قيم الإعداد متغيرات البيئة.لا يمكن تعيين `SYNOLOGY_CHAT_INCOMING_URL` من ملف `.env` في مساحة العمل؛ راجع [ملفات `.env` في مساحة العمل](https://docs.openclaw.ai/ar/gateway/security).

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9-%D9%88%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  سياسة الرسائل المباشرة والتحكم في الوصول

- `dmPolicy: "allowlist"` هو الخيار الافتراضي الموصى به.
- يقبل `allowedUserIds` قائمة (أو سلسلة مفصولة بفواصل) من معرّفات مستخدمي Synology.
- في وضع `allowlist`، تُعامل قائمة `allowedUserIds` الفارغة كخطأ في الإعداد ولن يبدأ مسار Webhook (استخدم `dmPolicy: "open"` مع `allowedUserIds: ["*"]` للسماح للجميع).
- يتيح `dmPolicy: "open"` الرسائل المباشرة العامة فقط عندما يتضمن `allowedUserIds` القيمة `"*"`؛ ومع الإدخالات التقييدية، يمكن للمستخدمين المطابقين فقط الدردشة.
- يحظر `dmPolicy: "disabled"` الرسائل المباشرة.
- يبقى ربط مستلم الرد على `user_id` الرقمي المستقر افتراضيًا. يُعد `channels.synology-chat.dangerouslyAllowNameMatching: true` وضع توافق للطوارئ يعيد تمكين البحث باسم المستخدم/اللقب القابل للتغيير لتسليم الرد.
- تعمل موافقات الاقتران مع:
  - `openclaw pairing list synology-chat`
  - `openclaw pairing approve synology-chat <CODE>`

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1)  التسليم الصادر

استخدم معرّفات مستخدمي Synology Chat الرقمية كأهداف.أمثلة:

```
openclaw message send --channel synology-chat --target 123456 --text "Hello from OpenClaw"
openclaw message send --channel synology-chat --target synology-chat:123456 --text "Hello again"
openclaw message send --channel synology-chat --target synology:123456 --text "Short prefix"
```

إرسال الوسائط مدعوم عبر تسليم الملفات المستند إلى URL.
يجب أن تستخدم عناوين URL للملفات الصادرة `http` أو `https`، ويتم رفض أهداف الشبكات الخاصة أو المحظورة بطريقة أخرى قبل أن يمرر OpenClaw عنوان URL إلى Webhook الخاص بـ NAS.

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)  الحسابات المتعددة

تُدعم حسابات Synology Chat المتعددة ضمن `channels.synology-chat.accounts`.
يمكن لكل حساب تجاوز الرمز، وعنوان URL الوارد، ومسار Webhook، وسياسة الرسائل المباشرة، والحدود.
تُعزل جلسات الرسائل المباشرة لكل حساب ومستخدم، لذلك لا يشارك `user_id`
الرقمي نفسه على حسابين مختلفين في Synology حالة النص المنسوخ.
امنح كل حساب مفعّل `webhookPath` مميزًا. يرفض OpenClaw الآن المسارات الدقيقة المكررة
ويرفض بدء الحسابات المسماة التي لا تفعل سوى وراثة مسار Webhook مشترك في إعدادات الحسابات المتعددة.
إذا كنت تحتاج عمدًا إلى الوراثة القديمة لحساب مسمى، فعيّن
`dangerouslyAllowInheritedWebhookPath: true` على ذلك الحساب أو عند `channels.synology-chat`،
لكن المسارات الدقيقة المكررة تظل مرفوضة بإغلاق آمن. فضّل المسارات الصريحة لكل حساب.

```
{
  channels: {
    "synology-chat": {
      enabled: true,
      accounts: {
        default: {
          token: "token-a",
          incomingUrl: "https://nas-a.example.com/...token=...",
        },
        alerts: {
          token: "token-b",
          incomingUrl: "https://nas-b.example.com/...token=...",
          webhookPath: "/webhook/synology-alerts",
          dmPolicy: "allowlist",
          allowedUserIds: ["987654"],
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%85%D8%A7%D9%86)  ملاحظات الأمان

- أبقِ `token` سريًا ودوّره إذا تسرّب.
- أبقِ `allowInsecureSsl: false` ما لم تكن تثق صراحةً بشهادة NAS محلية موقعة ذاتيًا.
- يتم التحقق من طلبات Webhook الواردة بالرمز وتقييد معدلها لكل مرسل.
- تستخدم فحوصات الرمز غير الصالح مقارنة أسرار بزمن ثابت وتفشل بإغلاق آمن.
- فضّل `dmPolicy: "allowlist"` للإنتاج.
- أبقِ `dangerouslyAllowNameMatching` معطلًا ما لم تكن تحتاج صراحةً إلى تسليم الردود القديم المستند إلى اسم المستخدم.
- أبقِ `dangerouslyAllowInheritedWebhookPath` معطلًا ما لم تكن تقبل صراحةً خطر التوجيه عبر مسار مشترك في إعداد حسابات متعددة.

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

- `Missing required fields (token, user_id, text)`:

  - تفتقد حمولة Webhook الصادر أحد الحقول المطلوبة
  - إذا كان Synology يرسل الرمز في الترويسات، فتأكد من أن Gateway/الوكيل يحافظ على تلك الترويسات
- `Invalid token`:

  - سر Webhook الصادر لا يطابق `channels.synology-chat.token`
  - الطلب يصل إلى الحساب/مسار Webhook الخطأ
  - أزال وكيل عكسي ترويسة الرمز قبل وصول الطلب إلى OpenClaw
- `Rate limit exceeded`:

  - قد تؤدي محاولات الرمز غير الصالح الكثيرة جدًا من المصدر نفسه إلى قفل ذلك المصدر مؤقتًا
  - لدى المرسلين المصادق عليهم أيضًا حد معدل رسائل منفصل لكل مستخدم
- `Allowlist is empty. Configure allowedUserIds or use dmPolicy=open with allowedUserIds=["*"].`:

  - تم تمكين `dmPolicy="allowlist"` لكن لم يتم إعداد أي مستخدمين
- `User not authorized`:

  - `user_id` الرقمي للمرسل غير موجود في `allowedUserIds`

## [​](https://docs.openclaw.ai/ar/channels/synology-chat\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — كل القنوات المدعومة
- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الاقتران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك دردشة المجموعات وبوابة الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Tlon](https://docs.openclaw.ai/ar/channels/tlon) [Twitch](https://docs.openclaw.ai/ar/channels/twitch)

Ctrl+I