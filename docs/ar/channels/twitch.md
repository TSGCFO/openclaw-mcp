---
source_url: https://docs.openclaw.ai/ar/channels/twitch
title: "Twitch - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/twitch#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

Twitch

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [Plugin مضمّن](https://docs.openclaw.ai/ar/channels/twitch#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)
- [إعداد سريع (للمبتدئين)](https://docs.openclaw.ai/ar/channels/twitch#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9-%D9%84%D9%84%D9%85%D8%A8%D8%AA%D8%AF%D8%A6%D9%8A%D9%86)
- [ما هو](https://docs.openclaw.ai/ar/channels/twitch#%D9%85%D8%A7-%D9%87%D9%88)
- [الإعداد (تفصيلي)](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%AA%D9%81%D8%B5%D9%8A%D9%84%D9%8A)
- [إنشاء بيانات الاعتماد](https://docs.openclaw.ai/ar/channels/twitch#%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF)
- [ضبط البوت](https://docs.openclaw.ai/ar/channels/twitch#%D8%B6%D8%A8%D8%B7-%D8%A7%D9%84%D8%A8%D9%88%D8%AA)
- [التحكم بالوصول (موصى به)](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D8%A8%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)
- [تحديث الرمز (اختياري)](https://docs.openclaw.ai/ar/channels/twitch#%D8%AA%D8%AD%D8%AF%D9%8A%D8%AB-%D8%A7%D9%84%D8%B1%D9%85%D8%B2-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1%D9%8A)
- [دعم الحسابات المتعددة](https://docs.openclaw.ai/ar/channels/twitch#%D8%AF%D8%B9%D9%85-%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)
- [التحكم بالوصول](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D8%A8%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [الإعدادات](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [إعداد الحساب](https://docs.openclaw.ai/ar/channels/twitch#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8)
- [خيارات الموفر](https://docs.openclaw.ai/ar/channels/twitch#%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%88%D9%81%D8%B1)
- [إجراءات الأدوات](https://docs.openclaw.ai/ar/channels/twitch#%D8%A5%D8%AC%D8%B1%D8%A7%D8%A1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA)
- [السلامة والتشغيل](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%B3%D9%84%D8%A7%D9%85%D8%A9-%D9%88%D8%A7%D9%84%D8%AA%D8%B4%D8%BA%D9%8A%D9%84)
- [الحدود](https://docs.openclaw.ai/ar/channels/twitch#%D8%A7%D9%84%D8%AD%D8%AF%D9%88%D8%AF)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/twitch#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

دعم دردشة Twitch عبر اتصال IRC. يتصل OpenClaw كمستخدم Twitch (حساب بوت) لاستقبال الرسائل وإرسالها في القنوات.

## [​](https://docs.openclaw.ai/ar/channels/twitch\#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)  Plugin مضمّن

يأتي Twitch كـ Plugin مضمّن في إصدارات OpenClaw الحالية، لذلك لا تحتاج الحزم العادية إلى تثبيت منفصل.

إذا كنت تستخدم بناءً أقدم أو تثبيتًا مخصصًا يستثني Twitch، فثبّت حزمة npm حالية عند نشر واحدة:

- سجل npm

- نسخة محلية


```
openclaw plugins install @openclaw/twitch
```

```
openclaw plugins install ./path/to/local/twitch-plugin
```

إذا أبلغ npm أن الحزمة المملوكة لـ OpenClaw مهملة، فاستخدم بناء OpenClaw
حاليًا ومعبأً أو مسار النسخة المحلية إلى أن تُنشر حزمة npm أحدث.التفاصيل: [Plugins](https://docs.openclaw.ai/ar/tools/plugin)

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9-%D9%84%D9%84%D9%85%D8%A8%D8%AA%D8%AF%D8%A6%D9%8A%D9%86)  إعداد سريع (للمبتدئين)

1

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

تأكد من توفر Plugin

إصدارات OpenClaw الحالية المعبأة تتضمنه بالفعل. يمكن للتثبيتات الأقدم/المخصصة إضافته يدويًا بالأوامر أعلاه.

2

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

أنشئ حساب بوت Twitch

أنشئ حساب Twitch مخصصًا للبوت (أو استخدم حسابًا موجودًا).

3

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

أنشئ بيانات الاعتماد

استخدم [Twitch Token Generator](https://twitchtokengenerator.com/):

- اختر **Bot Token**
- تحقق من تحديد النطاقين `chat:read` و`chat:write`
- انسخ **Client ID** و **Access Token**

4

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

اعثر على معرف مستخدم Twitch الخاص بك

استخدم [https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/](https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/) لتحويل اسم مستخدم إلى معرف مستخدم Twitch.

5

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

اضبط الرمز

- متغير البيئة: `OPENCLAW_TWITCH_ACCESS_TOKEN=...` (الحساب الافتراضي فقط)
- أو الإعداد: `channels.twitch.accessToken`

إذا ضُبط كلاهما، تكون الأولوية للإعداد (الرجوع إلى متغير البيئة للحساب الافتراضي فقط).

6

[Navigate to header](https://docs.openclaw.ai/ar/channels/twitch#)

ابدأ Gateway

ابدأ Gateway بالقناة المضبوطة.

أضف تحكمًا بالوصول (`allowFrom` أو `allowedRoles`) لمنع المستخدمين غير المصرح لهم من تشغيل البوت. القيمة الافتراضية لـ `requireMention` هي `true`.

إعداد بسيط:

```
{
  channels: {
    twitch: {
      enabled: true,
      username: "openclaw", // Bot's Twitch account
      accessToken: "oauth:abc123...", // OAuth Access Token (or use OPENCLAW_TWITCH_ACCESS_TOKEN env var)
      clientId: "xyz789...", // Client ID from Token Generator
      channel: "vevisk", // Which Twitch channel's chat to join (required)
      allowFrom: ["123456789"], // (recommended) Your Twitch user ID only - get it from https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D9%85%D8%A7-%D9%87%D9%88)  ما هو

- قناة Twitch يملكها Gateway.
- توجيه حتمي: تعود الردود دائمًا إلى Twitch.
- يُربط كل حساب بمفتاح جلسة معزول `agent:<agentId>:twitch:<accountName>`.
- `username` هو حساب البوت (الذي يصادق)، و`channel` هي غرفة الدردشة التي سينضم إليها.

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%AA%D9%81%D8%B5%D9%8A%D9%84%D9%8A)  الإعداد (تفصيلي)

### [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF)  إنشاء بيانات الاعتماد

استخدم [Twitch Token Generator](https://twitchtokengenerator.com/):

- اختر **Bot Token**
- تحقق من تحديد النطاقين `chat:read` و`chat:write`
- انسخ **Client ID** و **Access Token**

لا يلزم تسجيل تطبيق يدويًا. تنتهي صلاحية الرموز بعد عدة ساعات.

### [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%B6%D8%A8%D8%B7-%D8%A7%D9%84%D8%A8%D9%88%D8%AA)  ضبط البوت

- متغير بيئة (الحساب الافتراضي فقط)

- الإعداد


```
OPENCLAW_TWITCH_ACCESS_TOKEN=oauth:abc123...
```

```
{
  channels: {
    twitch: {
      enabled: true,
      username: "openclaw",
      accessToken: "oauth:abc123...",
      clientId: "xyz789...",
      channel: "vevisk",
    },
  },
}
```

إذا ضُبط كل من متغير البيئة والإعداد، تكون الأولوية للإعداد.

### [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D8%A8%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)  التحكم بالوصول (موصى به)

```
{
  channels: {
    twitch: {
      allowFrom: ["123456789"], // (recommended) Your Twitch user ID only
    },
  },
}
```

فضّل `allowFrom` لقائمة سماح صارمة. استخدم `allowedRoles` بدلًا من ذلك إذا أردت وصولًا قائمًا على الأدوار.**الأدوار المتاحة:**`"moderator"`، `"owner"`، `"vip"`، `"subscriber"`، `"all"`.

**لماذا معرفات المستخدم؟** يمكن أن تتغير أسماء المستخدمين، مما يسمح بانتحال الشخصية. معرفات المستخدم دائمة.اعثر على معرف مستخدم Twitch الخاص بك: [https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/](https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/) (حوّل اسم مستخدم Twitch الخاص بك إلى معرف)

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%AA%D8%AD%D8%AF%D9%8A%D8%AB-%D8%A7%D9%84%D8%B1%D9%85%D8%B2-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1%D9%8A)  تحديث الرمز (اختياري)

لا يمكن تحديث الرموز من [Twitch Token Generator](https://twitchtokengenerator.com/) تلقائيًا \- أعد إنشاءها عند انتهاء صلاحيتها.للتحديث التلقائي للرمز، أنشئ تطبيق Twitch الخاص بك في [Twitch Developer Console](https://dev.twitch.tv/console) وأضفه إلى الإعداد:

```
{
  channels: {
    twitch: {
      clientSecret: "your_client_secret",
      refreshToken: "your_refresh_token",
    },
  },
}
```

يحدّث البوت الرموز تلقائيًا قبل انتهاء الصلاحية ويسجل أحداث التحديث.

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%AF%D8%B9%D9%85-%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)  دعم الحسابات المتعددة

استخدم `channels.twitch.accounts` مع رموز مخصصة لكل حساب. راجع [الإعدادات](https://docs.openclaw.ai/ar/gateway/configuration) للنمط المشترك.مثال (حساب بوت واحد في قناتين):

```
{
  channels: {
    twitch: {
      accounts: {
        channel1: {
          username: "openclaw",
          accessToken: "oauth:abc123...",
          clientId: "xyz789...",
          channel: "vevisk",
        },
        channel2: {
          username: "openclaw",
          accessToken: "oauth:def456...",
          clientId: "uvw012...",
          channel: "secondchannel",
        },
      },
    },
  },
}
```

يحتاج كل حساب إلى رمزه الخاص (رمز واحد لكل قناة).

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D8%A8%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم بالوصول

- قائمة سماح معرفات المستخدمين (الأكثر أمانًا)

- قائم على الأدوار

- تعطيل متطلب @mention


```
{
  channels: {
    twitch: {
      accounts: {
        default: {
          allowFrom: ["123456789", "987654321"],
        },
      },
    },
  },
}
```

```
{
  channels: {
    twitch: {
      accounts: {
        default: {
          allowedRoles: ["moderator", "vip"],
        },
      },
    },
  },
}
```

`allowFrom` هي قائمة سماح صارمة. عند ضبطها، يُسمح لمعرفات المستخدمين هذه فقط. إذا أردت وصولًا قائمًا على الأدوار، فاترك `allowFrom` غير مضبوط واضبط `allowedRoles` بدلًا منه.

افتراضيًا، تكون `requireMention` هي `true`. للتعطيل والرد على جميع الرسائل:

```
{
  channels: {
    twitch: {
      accounts: {
        default: {
          requireMention: false,
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

أولًا، شغّل أوامر التشخيص:

```
openclaw doctor
openclaw channels status --probe
```

البوت لا يرد على الرسائل

- **تحقق من التحكم بالوصول:** تأكد من أن معرف المستخدم الخاص بك موجود في `allowFrom`، أو أزل `allowFrom` مؤقتًا واضبط `allowedRoles: ["all"]` للاختبار.
- **تحقق من أن البوت في القناة:** يجب أن ينضم البوت إلى القناة المحددة في `channel`.

مشكلات الرمز

أخطاء “فشل الاتصال” أو المصادقة:

- تحقق من أن `accessToken` هو قيمة رمز وصول OAuth (يبدأ عادةً بالبادئة `oauth:`)
- تحقق من أن الرمز لديه النطاقان `chat:read` و`chat:write`
- إذا كنت تستخدم تحديث الرمز، فتحقق من ضبط `clientSecret` و`refreshToken`

تحديث الرمز لا يعمل

تحقق من السجلات بحثًا عن أحداث التحديث:

```
Using env token source for mybot
Access token refreshed for user 123456 (expires in 14400s)
```

إذا رأيت “token refresh disabled (no refresh token)”:

- تأكد من توفير `clientSecret`
- تأكد من توفير `refreshToken`

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  الإعدادات

### [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%AD%D8%B3%D8%A7%D8%A8)  إعداد الحساب

[​](https://docs.openclaw.ai/ar/channels/twitch#param-username)

username

string

اسم مستخدم البوت.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-access-token)

accessToken

string

رمز وصول OAuth مع `chat:read` و`chat:write`.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-client-id)

clientId

string

Twitch Client ID (من Token Generator أو تطبيقك).

[​](https://docs.openclaw.ai/ar/channels/twitch#param-channel)

channel

string

مطلوب

القناة التي سيتم الانضمام إليها.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-enabled)

enabled

boolean

افتراضي:"true"

فعّل هذا الحساب.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-client-secret)

clientSecret

string

اختياري: للتحديث التلقائي للرمز.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-refresh-token)

refreshToken

string

اختياري: للتحديث التلقائي للرمز.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-expires-in)

expiresIn

number

انتهاء صلاحية الرمز بالثواني.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-obtainment-timestamp)

obtainmentTimestamp

number

الطابع الزمني للحصول على الرمز.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-allow-from)

allowFrom

string\[\]

قائمة سماح معرفات المستخدمين.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-allowed-roles)

allowedRoles

Array<"moderator" \| "owner" \| "vip" \| "subscriber" \| "all">

التحكم بالوصول القائم على الأدوار.

[​](https://docs.openclaw.ai/ar/channels/twitch#param-require-mention)

requireMention

boolean

افتراضي:"true"

يتطلب @mention.

### [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%88%D9%81%D8%B1)  خيارات الموفر

- `channels.twitch.enabled` \- تفعيل/تعطيل بدء تشغيل القناة
- `channels.twitch.username` \- اسم مستخدم البوت (إعداد حساب واحد مبسط)
- `channels.twitch.accessToken` \- رمز وصول OAuth (إعداد حساب واحد مبسط)
- `channels.twitch.clientId` \- Twitch Client ID (إعداد حساب واحد مبسط)
- `channels.twitch.channel` \- القناة التي سيتم الانضمام إليها (إعداد حساب واحد مبسط)
- `channels.twitch.accounts.<accountName>` \- إعداد حسابات متعددة (كل حقول الحساب أعلاه)

مثال كامل:

```
{
  channels: {
    twitch: {
      enabled: true,
      username: "openclaw",
      accessToken: "oauth:abc123...",
      clientId: "xyz789...",
      channel: "vevisk",
      clientSecret: "secret123...",
      refreshToken: "refresh456...",
      allowFrom: ["123456789"],
      allowedRoles: ["moderator", "vip"],
      accounts: {
        default: {
          username: "mybot",
          accessToken: "oauth:abc123...",
          clientId: "xyz789...",
          channel: "your_channel",
          enabled: true,
          clientSecret: "secret123...",
          refreshToken: "refresh456...",
          expiresIn: 14400,
          obtainmentTimestamp: 1706092800000,
          allowFrom: ["123456789", "987654321"],
          allowedRoles: ["moderator"],
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A5%D8%AC%D8%B1%D8%A7%D8%A1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA)  إجراءات الأدوات

يمكن للوكيل استدعاء `twitch` مع الإجراء:

- `send` \- إرسال رسالة إلى قناة

مثال:

```
{
  action: "twitch",
  params: {
    message: "Hello Twitch!",
    to: "#mychannel",
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%B3%D9%84%D8%A7%D9%85%D8%A9-%D9%88%D8%A7%D9%84%D8%AA%D8%B4%D8%BA%D9%8A%D9%84)  السلامة والتشغيل

- **عامل الرموز مثل كلمات المرور** — لا تلتزم الرموز أبدًا إلى git.
- **استخدم التحديث التلقائي للرمز** للبوتات طويلة التشغيل.
- **استخدم قوائم سماح معرفات المستخدمين** بدلًا من أسماء المستخدمين للتحكم بالوصول.
- **راقب السجلات** لأحداث تحديث الرمز وحالة الاتصال.
- **قلّل نطاقات الرموز إلى الحد الأدنى** — اطلب فقط `chat:read` و`chat:write`.
- **إذا علقت**: أعد تشغيل Gateway بعد التأكد من عدم امتلاك أي عملية أخرى للجلسة.

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%A7%D9%84%D8%AD%D8%AF%D9%88%D8%AF)  الحدود

- **500 حرف** لكل رسالة (تُقسّم تلقائيًا عند حدود الكلمات).
- تُزال Markdown قبل التقسيم.
- لا يوجد تحديد معدل (يستخدم حدود المعدل المدمجة في Twitch).

## [​](https://docs.openclaw.ai/ar/channels/twitch\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — كل القنوات المدعومة
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك دردشة المجموعات وبوابة الإشارات
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الإقران
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Synology Chat](https://docs.openclaw.ai/ar/channels/synology-chat) [سطر](https://docs.openclaw.ai/ar/channels/line)

Ctrl+I