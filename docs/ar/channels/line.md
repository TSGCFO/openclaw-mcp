---
source_url: https://docs.openclaw.ai/ar/channels/line
title: "\u0633\u0637\u0631 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/line#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Regional platforms

سطر

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [التثبيت](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA)
- [الإعداد](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [الإعدادات](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [سلوك الرسائل](https://docs.openclaw.ai/ar/channels/line#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)
- [بيانات القناة (الرسائل الغنية)](https://docs.openclaw.ai/ar/channels/line#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%BA%D9%86%D9%8A%D8%A9)
- [دعم ACP](https://docs.openclaw.ai/ar/channels/line#%D8%AF%D8%B9%D9%85-acp)
- [الوسائط الصادرة](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D9%84%D9%88%D8%B3%D8%A7%D8%A6%D8%B7-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1%D8%A9)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/line#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/line#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

يتصل LINE بـ OpenClaw عبر LINE Messaging API. يعمل Plugin كمستقبِل Webhook
على Gateway ويستخدم رمز الوصول إلى القناة + سر القناة للمصادقة.الحالة: Plugin قابل للتنزيل. الرسائل المباشرة، ومحادثات المجموعات، والوسائط، والمواقع، ورسائل Flex
ورسائل القوالب والردود السريعة مدعومة. التفاعلات والسلاسل
غير مدعومة.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA)  التثبيت

ثبّت LINE قبل إعداد القناة:

```
openclaw plugins install @openclaw/line
```

نسخة محلية من المستودع (عند التشغيل من مستودع git):

```
openclaw plugins install ./path/to/local/line-plugin
```

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  الإعداد

1. أنشئ حساب LINE Developers وافتح وحدة التحكم:
[https://developers.line.biz/console/](https://developers.line.biz/console/)
2. أنشئ (أو اختر) موفّرًا وأضف قناة **Messaging API**.
3. انسخ **رمز الوصول إلى القناة** و **سر القناة** من إعدادات القناة.
4. فعّل **Use webhook** في إعدادات Messaging API.
5. عيّن عنوان URL الخاص بالـ Webhook إلى نقطة نهاية Gateway لديك (يتطلب HTTPS):

```
https://gateway-host/line/webhook
```

يستجيب Gateway لتحقق Webhook من LINE (GET) والأحداث الواردة (POST).
إذا احتجت إلى مسار مخصص، فاضبط `channels.line.webhookPath` أو
`channels.line.accounts.<id>.webhookPath` وحدّث عنوان URL وفقًا لذلك.ملاحظة أمنية:

- يعتمد تحقق توقيع LINE على النص الأساسي (HMAC على النص الأساسي الخام)، لذلك يطبّق OpenClaw حدودًا صارمة على النص الأساسي قبل المصادقة ومهلة زمنية قبل التحقق.
- يعالج OpenClaw أحداث Webhook من بايتات الطلب الخام المتحقق منها. يتم تجاهل قيم `req.body` المحوّلة بواسطة الوسيط العلوي حفاظًا على سلامة التوقيع.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  الإعدادات

إعدادات دنيا:

```
{
  channels: {
    line: {
      enabled: true,
      channelAccessToken: "LINE_CHANNEL_ACCESS_TOKEN",
      channelSecret: "LINE_CHANNEL_SECRET",
      dmPolicy: "pairing",
    },
  },
}
```

متغيرات البيئة (للحساب الافتراضي فقط):

- `LINE_CHANNEL_ACCESS_TOKEN`
- `LINE_CHANNEL_SECRET`

ملفات الرمز/السر:

```
{
  channels: {
    line: {
      tokenFile: "/path/to/line-token.txt",
      secretFile: "/path/to/line-secret.txt",
    },
  },
}
```

يجب أن يشير `tokenFile` و`secretFile` إلى ملفات عادية. يتم رفض الروابط الرمزية.حسابات متعددة:

```
{
  channels: {
    line: {
      accounts: {
        marketing: {
          channelAccessToken: "...",
          channelSecret: "...",
          webhookPath: "/line/marketing",
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

تستخدم الرسائل المباشرة الاقتران افتراضيًا. يتلقى المرسلون غير المعروفين رمز اقتران وتُتجاهل
رسائلهم حتى تتم الموافقة عليهم.

```
openclaw pairing list line
openclaw pairing approve line <CODE>
```

قوائم السماح والسياسات:

- `channels.line.dmPolicy`: `pairing | allowlist | open | disabled`
- `channels.line.allowFrom`: معرّفات مستخدمي LINE المسموح بها للرسائل المباشرة
- `channels.line.groupPolicy`: `allowlist | open | disabled`
- `channels.line.groupAllowFrom`: معرّفات مستخدمي LINE المسموح بها للمجموعات
- التجاوزات لكل مجموعة: `channels.line.groups.<groupId>.allowFrom`
- ملاحظة وقت التشغيل: إذا كان `channels.line` مفقودًا بالكامل، يعود وقت التشغيل إلى `groupPolicy="allowlist"` لفحوصات المجموعات (حتى إذا كان `channels.defaults.groupPolicy` مضبوطًا).

معرّفات LINE حساسة لحالة الأحرف. تبدو المعرّفات الصالحة كما يلي:

- المستخدم: `U` \+ 32 محرفًا سداسيًا عشريًا
- المجموعة: `C` \+ 32 محرفًا سداسيًا عشريًا
- الغرفة: `R` \+ 32 محرفًا سداسيًا عشريًا

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)  سلوك الرسائل

- يُقسّم النص إلى أجزاء عند 5000 محرف.
- تُزال تنسيقات Markdown؛ وتُحوّل كتل التعليمات البرمجية والجداول إلى بطاقات Flex
عندما يكون ذلك ممكنًا.
- تُخزّن استجابات البث مؤقتًا؛ يتلقى LINE أجزاء كاملة مع حركة تحميل
بينما يعمل الوكيل.
- تُقيّد تنزيلات الوسائط بواسطة `channels.line.mediaMaxMb` (الافتراضي 10).
- تُحفظ الوسائط الواردة ضمن `~/.openclaw/media/inbound/` قبل تمريرها
إلى الوكيل، بما يطابق مخزن الوسائط المشترك الذي تستخدمه Plugins القنوات المضمنة الأخرى.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%BA%D9%86%D9%8A%D8%A9)  بيانات القناة (الرسائل الغنية)

استخدم `channelData.line` لإرسال ردود سريعة أو مواقع أو بطاقات Flex أو رسائل قوالب.

```
{
  text: "Here you go",
  channelData: {
    line: {
      quickReplies: ["Status", "Help"],
      location: {
        title: "Office",
        address: "123 Main St",
        latitude: 35.681236,
        longitude: 139.767125,
      },
      flexMessage: {
        altText: "Status card",
        contents: {
          /* Flex payload */
        },
      },
      templateMessage: {
        type: "confirm",
        text: "Proceed?",
        confirmLabel: "Yes",
        confirmData: "yes",
        cancelLabel: "No",
        cancelData: "no",
      },
    },
  },
}
```

يوفر LINE Plugin أيضًا أمر `/card` لإعدادات رسائل Flex المسبقة:

```
/card info "Welcome" "Thanks for joining!"
```

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%AF%D8%B9%D9%85-acp)  دعم ACP

يدعم LINE ارتباطات محادثات ACP (Agent Communication Protocol):

- يربط `/acp spawn <agent> --bind here` محادثة LINE الحالية بجلسة ACP دون إنشاء سلسلة فرعية.
- تعمل ارتباطات ACP المضبوطة وجلسات ACP النشطة المرتبطة بالمحادثات على LINE مثل قنوات المحادثات الأخرى.

راجع [وكلاء ACP](https://docs.openclaw.ai/ar/tools/acp-agents) للحصول على التفاصيل.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D9%84%D9%88%D8%B3%D8%A7%D8%A6%D8%B7-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1%D8%A9)  الوسائط الصادرة

يدعم LINE Plugin إرسال الصور ومقاطع الفيديو وملفات الصوت عبر أداة رسائل الوكيل. تُرسل الوسائط عبر مسار التسليم الخاص بـ LINE مع التعامل المناسب مع المعاينة والتتبع:

- **الصور**: تُرسل كرسائل صور LINE مع إنشاء معاينة تلقائي.
- **مقاطع الفيديو**: تُرسل مع تعامل صريح مع المعاينة ونوع المحتوى.
- **الصوت**: يُرسل كرسائل صوت LINE.

يجب أن تكون عناوين URL للوسائط الصادرة عناوين HTTPS عامة. يتحقق OpenClaw من اسم مضيف الهدف قبل تسليم عنوان URL إلى LINE ويرفض أهداف local loopback وlink-local والشبكات الخاصة.تعود عمليات إرسال الوسائط العامة إلى مسار الصور فقط الموجود عندما لا يكون المسار الخاص بـ LINE متاحًا.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

- **يفشل تحقق Webhook:** تأكد من أن عنوان URL الخاص بالـ Webhook يستخدم HTTPS وأن
`channelSecret` يطابق وحدة تحكم LINE.
- **لا توجد أحداث واردة:** تأكد من أن مسار Webhook يطابق `channels.line.webhookPath`
وأن Gateway قابل للوصول من LINE.
- **أخطاء تنزيل الوسائط:** ارفع `channels.line.mediaMaxMb` إذا تجاوزت الوسائط
الحد الافتراضي.

## [​](https://docs.openclaw.ai/ar/channels/line\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — كل القنوات المدعومة
- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الاقتران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك محادثات المجموعات وبوابة الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Twitch](https://docs.openclaw.ai/ar/channels/twitch) [WeChat](https://docs.openclaw.ai/ar/channels/wechat)

Ctrl+I