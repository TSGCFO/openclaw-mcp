---
source_url: https://docs.openclaw.ai/ar/channels/tlon
title: "Tlon - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/tlon#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

Tlon

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [Plugin مضمن](https://docs.openclaw.ai/ar/channels/tlon#plugin-%D9%85%D8%B6%D9%85%D9%86)
- [الإعداد](https://docs.openclaw.ai/ar/channels/tlon#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [السفن الخاصة/شبكة LAN](https://docs.openclaw.ai/ar/channels/tlon#%D8%A7%D9%84%D8%B3%D9%81%D9%86-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9%2F%D8%B4%D8%A8%D9%83%D8%A9-lan)
- [قنوات المجموعات](https://docs.openclaw.ai/ar/channels/tlon#%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/tlon#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [نظام المالك والموافقة](https://docs.openclaw.ai/ar/channels/tlon#%D9%86%D8%B8%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%88%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9)
- [إعدادات القبول التلقائي](https://docs.openclaw.ai/ar/channels/tlon#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D8%A8%D9%88%D9%84-%D8%A7%D9%84%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A)
- [أهداف التسليم (CLI/cron)](https://docs.openclaw.ai/ar/channels/tlon#%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-cli%2Fcron)
- [Skill مضمنة](https://docs.openclaw.ai/ar/channels/tlon#skill-%D9%85%D8%B6%D9%85%D9%86%D8%A9)
- [القدرات](https://docs.openclaw.ai/ar/channels/tlon#%D8%A7%D9%84%D9%82%D8%AF%D8%B1%D8%A7%D8%AA)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/tlon#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [مرجع التهيئة](https://docs.openclaw.ai/ar/channels/tlon#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%AA%D9%87%D9%8A%D8%A6%D8%A9)
- [ملاحظات](https://docs.openclaw.ai/ar/channels/tlon#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/tlon#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Tlon هو برنامج مراسلة لامركزي مبني على Urbit. يتصل OpenClaw بسفينة Urbit الخاصة بك ويمكنه
الرد على الرسائل المباشرة ورسائل محادثات المجموعات. تتطلب ردود المجموعات إشارة @ افتراضيًا ويمكن
تقييدها أكثر عبر قوائم السماح.الحالة: Plugin مضمن. الرسائل المباشرة، وإشارات المجموعات، وردود الخيوط، وتنسيق النص الغني، وعمليات
رفع الصور مدعومة. التفاعلات والاستطلاعات غير مدعومة بعد.

## [​](https://docs.openclaw.ai/ar/channels/tlon\#plugin-%D9%85%D8%B6%D9%85%D9%86)  Plugin مضمن

يأتي Tlon بصفته Plugin مضمنًا في إصدارات OpenClaw الحالية، لذلك لا تحتاج الإصدارات
المعبأة العادية إلى تثبيت منفصل.إذا كنت تستخدم إصدارًا أقدم أو تثبيتًا مخصصًا يستبعد Tlon، فثبّت حزمة npm
حالية عند نشر واحدة:التثبيت عبر CLI (سجل npm، عند وجود حزمة حالية):

```
openclaw plugins install @openclaw/tlon
```

إذا أبلغ npm أن الحزمة المملوكة لـ OpenClaw مهملة، فاستخدم إصدار OpenClaw معبأً حاليًا
أو مسار checkout المحلي إلى أن تُنشر حزمة npm أحدث.checkout محلي (عند التشغيل من مستودع git):

```
openclaw plugins install ./path/to/local/tlon-plugin
```

التفاصيل: [Plugins](https://docs.openclaw.ai/ar/tools/plugin)

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  الإعداد

1. تأكد من أن Plugin الخاص بـ Tlon متاح.
   - إصدارات OpenClaw المعبأة الحالية تتضمنه بالفعل.
   - يمكن للتثبيتات الأقدم/المخصصة إضافته يدويًا باستخدام الأوامر أعلاه.
2. اجمع عنوان URL الخاص بسفينتك ورمز تسجيل الدخول.
3. اضبط `channels.tlon`.
4. أعد تشغيل Gateway.
5. أرسل رسالة مباشرة إلى البوت أو أشر إليه في قناة مجموعة.

الحد الأدنى من الإعدادات (حساب واحد):

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

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A7%D9%84%D8%B3%D9%81%D9%86-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9/%D8%B4%D8%A8%D9%83%D8%A9-lan)  السفن الخاصة/شبكة LAN

افتراضيًا، يحظر OpenClaw أسماء المضيفين الداخلية/الخاصة ونطاقات عناوين IP للحماية من SSRF.
إذا كانت سفينتك تعمل على شبكة خاصة (localhost أو عنوان IP على LAN أو اسم مضيف داخلي)،
فيجب عليك تفعيل ذلك صراحةً:

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

ينطبق هذا على عناوين URL مثل:

- `http://localhost:8080`
- `http://192.168.x.x:8080`
- `http://my-ship.local:8080`

⚠️ فعّل هذا فقط إذا كنت تثق بشبكتك المحلية. يعطل هذا الإعداد وسائل الحماية من SSRF
للطلبات إلى عنوان URL الخاص بسفينتك.

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)  قنوات المجموعات

الاكتشاف التلقائي مفعّل افتراضيًا. يمكنك أيضًا تثبيت القنوات يدويًا:

```
{
  channels: {
    tlon: {
      groupChannels: ["chat/~host-ship/general", "chat/~host-ship/support"],
    },
  },
}
```

تعطيل الاكتشاف التلقائي:

```
{
  channels: {
    tlon: {
      autoDiscoverChannels: false,
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

قائمة السماح للرسائل المباشرة (فارغة = لا يُسمح بأي رسائل مباشرة، استخدم `ownerShip` لتدفق الموافقة):

```
{
  channels: {
    tlon: {
      dmAllowlist: ["~zod", "~nec"],
    },
  },
}
```

تفويض المجموعة (مقيّد افتراضيًا):

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

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D9%86%D8%B8%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%88%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9)  نظام المالك والموافقة

عيّن سفينة مالك لتلقي طلبات الموافقة عندما يحاول مستخدمون غير مصرح لهم التفاعل:

```
{
  channels: {
    tlon: {
      ownerShip: "~your-main-ship",
    },
  },
}
```

سفينة المالك **مصرح لها تلقائيًا في كل مكان** — تُقبل دعوات الرسائل المباشرة تلقائيًا
وتُسمح رسائل القنوات دائمًا. لا تحتاج إلى إضافة المالك إلى `dmAllowlist` أو
`defaultAuthorizedShips`.عند تعيين ذلك، يتلقى المالك إشعارات رسائل مباشرة من أجل:

- طلبات الرسائل المباشرة من سفن غير موجودة في قائمة السماح
- الإشارات في القنوات دون تفويض
- طلبات دعوات المجموعات

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D8%A8%D9%88%D9%84-%D8%A7%D9%84%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A)  إعدادات القبول التلقائي

قبول دعوات الرسائل المباشرة تلقائيًا (للسفن الموجودة في dmAllowlist):

```
{
  channels: {
    tlon: {
      autoAcceptDmInvites: true,
    },
  },
}
```

قبول دعوات المجموعات تلقائيًا:

```
{
  channels: {
    tlon: {
      autoAcceptGroupInvites: true,
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-cli/cron)  أهداف التسليم (CLI/cron)

استخدم هذه مع `openclaw message send` أو تسليم cron:

- رسالة مباشرة: `~sampel-palnet` أو `dm/~sampel-palnet`
- مجموعة: `chat/~host-ship/channel` أو `group:~host-ship/channel`

## [​](https://docs.openclaw.ai/ar/channels/tlon\#skill-%D9%85%D8%B6%D9%85%D9%86%D8%A9)  Skill مضمنة

يتضمن Plugin الخاص بـ Tlon مهارة مضمنة ( [`@tloncorp/tlon-skill`](https://github.com/tloncorp/tlon-skill))
توفر وصول CLI إلى عمليات Tlon:

- **جهات الاتصال**: جلب/تحديث الملفات الشخصية، سرد جهات الاتصال
- **القنوات**: السرد، والإنشاء، ونشر الرسائل، وجلب السجل
- **المجموعات**: السرد، والإنشاء، وإدارة الأعضاء
- **الرسائل المباشرة**: إرسال الرسائل، والتفاعل مع الرسائل
- **التفاعلات**: إضافة/إزالة تفاعلات الرموز التعبيرية إلى المنشورات والرسائل المباشرة
- **الإعدادات**: إدارة أذونات Plugin عبر أوامر slash

تكون المهارة متاحة تلقائيًا عند تثبيت Plugin.

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A7%D9%84%D9%82%D8%AF%D8%B1%D8%A7%D8%AA)  القدرات

| الميزة | الحالة |
| --- | --- |
| الرسائل المباشرة | ✅ مدعومة |
| المجموعات/القنوات | ✅ مدعومة (محكومة بالإشارة افتراضيًا) |
| الخيوط | ✅ مدعومة (ردود تلقائية داخل الخيط) |
| النص الغني | ✅ يُحوّل Markdown إلى تنسيق Tlon |
| الصور | ✅ تُرفع إلى تخزين Tlon |
| التفاعلات | ✅ عبر [المهارة المضمنة](https://docs.openclaw.ai/ar/channels/tlon#bundled-skill) |
| الاستطلاعات | ❌ غير مدعومة بعد |
| الأوامر الأصلية | ✅ مدعومة (للمالك فقط افتراضيًا) |

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

شغّل هذا التسلسل أولًا:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
```

الأعطال الشائعة:

- **يتم تجاهل الرسائل المباشرة**: المرسل غير موجود في `dmAllowlist` ولم يتم ضبط `ownerShip` لتدفق الموافقة.
- **يتم تجاهل رسائل المجموعات**: لم تُكتشف القناة أو المرسل غير مصرح له.
- **أخطاء الاتصال**: تحقق من إمكانية الوصول إلى عنوان URL الخاص بالسفينة؛ فعّل `allowPrivateNetwork` للسفن المحلية.
- **أخطاء المصادقة**: تحقق من أن رمز تسجيل الدخول حالي (تتغير الرموز دوريًا).

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%AA%D9%87%D9%8A%D8%A6%D8%A9)  مرجع التهيئة

التهيئة الكاملة: [التهيئة](https://docs.openclaw.ai/ar/gateway/configuration)خيارات المزوّد:

- `channels.tlon.enabled`: تفعيل/تعطيل بدء تشغيل القناة.
- `channels.tlon.ship`: اسم سفينة Urbit للبوت (مثل `~sampel-palnet`).
- `channels.tlon.url`: عنوان URL الخاص بالسفينة (مثل `https://sampel-palnet.tlon.network`).
- `channels.tlon.code`: رمز تسجيل الدخول إلى السفينة.
- `channels.tlon.allowPrivateNetwork`: السماح بعناوين URL الخاصة بـ localhost/LAN (تجاوز SSRF).
- `channels.tlon.ownerShip`: سفينة المالك لنظام الموافقة (مصرح لها دائمًا).
- `channels.tlon.dmAllowlist`: السفن المسموح لها بإرسال رسائل مباشرة (فارغة = لا شيء).
- `channels.tlon.autoAcceptDmInvites`: قبول الرسائل المباشرة تلقائيًا من السفن الموجودة في قائمة السماح.
- `channels.tlon.autoAcceptGroupInvites`: قبول كل دعوات المجموعات تلقائيًا.
- `channels.tlon.autoDiscoverChannels`: اكتشاف قنوات المجموعات تلقائيًا (الافتراضي: true).
- `channels.tlon.groupChannels`: أعشاش القنوات المثبتة يدويًا.
- `channels.tlon.defaultAuthorizedShips`: السفن المصرح لها لكل القنوات.
- `channels.tlon.authorization.channelRules`: قواعد المصادقة لكل قناة.
- `channels.tlon.showModelSignature`: إلحاق اسم النموذج بالرسائل.

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)  ملاحظات

- تتطلب ردود المجموعات إشارة (مثل `~your-bot-ship`) للرد.
- ردود الخيوط: إذا كانت الرسالة الواردة ضمن خيط، يرد OpenClaw داخل الخيط.
- النص الغني: يُحوّل تنسيق Markdown (غامق، مائل، كود، عناوين، قوائم) إلى تنسيق Tlon الأصلي.
- الصور: تُرفع عناوين URL إلى تخزين Tlon وتُضمّن ككتل صور.

## [​](https://docs.openclaw.ai/ar/channels/tlon\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — كل القنوات المدعومة
- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الاقتران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك محادثات المجموعات والتحكم عبر الإشارة
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Nostr](https://docs.openclaw.ai/ar/channels/nostr) [Synology Chat](https://docs.openclaw.ai/ar/channels/synology-chat)

Ctrl+I