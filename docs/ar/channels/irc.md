---
source_url: https://docs.openclaw.ai/ar/channels/irc
title: "IRC - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/irc#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

IRC

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [البدء السريع](https://docs.openclaw.ai/ar/channels/irc#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [الإعدادات الأمنية الافتراضية](https://docs.openclaw.ai/ar/channels/irc#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%85%D9%86%D9%8A%D8%A9-%D8%A7%D9%84%D8%A7%D9%81%D8%AA%D8%B1%D8%A7%D8%B6%D9%8A%D8%A9)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/irc#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [مشكلة شائعة: allowFrom مخصّصة للرسائل الخاصة، وليست للقنوات](https://docs.openclaw.ai/ar/channels/irc#%D9%85%D8%B4%D9%83%D9%84%D8%A9-%D8%B4%D8%A7%D8%A6%D8%B9%D8%A9-allowfrom-%D9%85%D8%AE%D8%B5%D9%91%D8%B5%D8%A9-%D9%84%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9%D8%8C-%D9%88%D9%84%D9%8A%D8%B3%D8%AA-%D9%84%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA)
- [تفعيل الردود (الإشارات)](https://docs.openclaw.ai/ar/channels/irc#%D8%AA%D9%81%D8%B9%D9%8A%D9%84-%D8%A7%D9%84%D8%B1%D8%AF%D9%88%D8%AF-%D8%A7%D9%84%D8%A5%D8%B4%D8%A7%D8%B1%D8%A7%D8%AA)
- [ملاحظة أمنية (مستحسنة للقنوات العامة)](https://docs.openclaw.ai/ar/channels/irc#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A9-%D8%A3%D9%85%D9%86%D9%8A%D8%A9-%D9%85%D8%B3%D8%AA%D8%AD%D8%B3%D9%86%D8%A9-%D9%84%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D8%B9%D8%A7%D9%85%D8%A9)
- [الأدوات نفسها للجميع في القناة](https://docs.openclaw.ai/ar/channels/irc#%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D9%86%D9%81%D8%B3%D9%87%D8%A7-%D9%84%D9%84%D8%AC%D9%85%D9%8A%D8%B9-%D9%81%D9%8A-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9)
- [أدوات مختلفة لكل مرسِل (المالك يحصل على صلاحيات أكبر)](https://docs.openclaw.ai/ar/channels/irc#%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D9%85%D8%AE%D8%AA%D9%84%D9%81%D8%A9-%D9%84%D9%83%D9%84-%D9%85%D8%B1%D8%B3%D9%90%D9%84-%D8%A7%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%8A%D8%AD%D8%B5%D9%84-%D8%B9%D9%84%D9%89-%D8%B5%D9%84%D8%A7%D8%AD%D9%8A%D8%A7%D8%AA-%D8%A3%D9%83%D8%A8%D8%B1)
- [NickServ](https://docs.openclaw.ai/ar/channels/irc#nickserv)
- [متغيرات البيئة](https://docs.openclaw.ai/ar/channels/irc#%D9%85%D8%AA%D8%BA%D9%8A%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D9%8A%D8%A6%D8%A9)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/irc#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/irc#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

استخدم IRC عندما تريد OpenClaw في القنوات التقليدية (`#room`) والرسائل الخاصة.
يأتي IRC على شكل Plugin مضمّن، لكن يتم تكوينه في الإعداد الرئيسي تحت `channels.irc`.

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  البدء السريع

1. فعّل إعداد IRC في `~/.openclaw/openclaw.json`.
2. اضبط على الأقل:

```
{
  channels: {
    irc: {
      enabled: true,
      host: "irc.example.com",
      port: 6697,
      tls: true,
      nick: "openclaw-bot",
      channels: ["#openclaw"],
    },
  },
}
```

يُفضَّل استخدام خادم IRC خاص لتنسيق البوت. وإذا كنت تستخدم عمدًا شبكة IRC عامة، فمن الخيارات الشائعة Libera.Chat وOFTC وSnoonet. تجنّب القنوات العامة المتوقعة لحركة المرور الخلفية الخاصة بالبوت أو السرب.

3. ابدأ/أعد تشغيل Gateway:

```
openclaw gateway run
```

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%85%D9%86%D9%8A%D8%A9-%D8%A7%D9%84%D8%A7%D9%81%D8%AA%D8%B1%D8%A7%D8%B6%D9%8A%D8%A9)  الإعدادات الأمنية الافتراضية

- القيمة الافتراضية لـ `channels.irc.dmPolicy` هي `"pairing"`.
- القيمة الافتراضية لـ `channels.irc.groupPolicy` هي `"allowlist"`.
- عند استخدام `groupPolicy="allowlist"`، اضبط `channels.irc.groups` لتعريف القنوات المسموح بها.
- استخدم TLS (`channels.irc.tls=true`) ما لم تكن تقبل عمدًا بالنقل النصي غير المشفر.

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

هناك “بوابتان” منفصلتان لقنوات IRC:

1. **الوصول إلى القناة** (`groupPolicy` \+ `groups`): ما إذا كان البوت يقبل الرسائل من القناة أصلًا.
2. **وصول المُرسِل** (`groupAllowFrom` / `groups["#channel"].allowFrom` الخاصة بكل قناة): من المسموح له بتفعيل البوت داخل تلك القناة.

مفاتيح الإعداد:

- قائمة السماح للرسائل الخاصة (وصول مرسِل الرسائل الخاصة): `channels.irc.allowFrom`
- قائمة السماح لمرسلي المجموعات (وصول مرسِل القناة): `channels.irc.groupAllowFrom`
- عناصر التحكم الخاصة بكل قناة (القناة \+ المرسِل \+ قواعد الإشارة): `channels.irc.groups["#channel"]`
- تسمح `channels.irc.groupPolicy="open"` بالقنوات غير المكوّنة ( **لكنها تظل مقيّدة بالإشارة افتراضيًا**)

يجب أن تستخدم إدخالات قائمة السماح هويات مرسلين ثابتة (`nick!user@host`).
المطابقة باستخدام الاسم المختصر فقط قابلة للتغيير، ولا يتم تفعيلها إلا عندما تكون `channels.irc.dangerouslyAllowNameMatching: true`.

### [​](https://docs.openclaw.ai/ar/channels/irc\#%D9%85%D8%B4%D9%83%D9%84%D8%A9-%D8%B4%D8%A7%D8%A6%D8%B9%D8%A9-allowfrom-%D9%85%D8%AE%D8%B5%D9%91%D8%B5%D8%A9-%D9%84%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9%D8%8C-%D9%88%D9%84%D9%8A%D8%B3%D8%AA-%D9%84%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA)  مشكلة شائعة: `allowFrom` مخصّصة للرسائل الخاصة، وليست للقنوات

إذا رأيت سجلات مثل:

- `irc: drop group sender alice!ident@host (policy=allowlist)`

…فهذا يعني أن المرسِل غير مسموح له في رسائل **المجموعة/القناة**. أصلح ذلك بإحدى الطريقتين:

- ضبط `channels.irc.groupAllowFrom` (عام لكل القنوات)، أو
- ضبط قوائم سماح المرسلين لكل قناة على حدة: `channels.irc.groups["#channel"].allowFrom`

مثال (السماح لأي شخص في `#tuirc-dev` بالتحدث إلى البوت):

```
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": { allowFrom: ["*"] },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%AA%D9%81%D8%B9%D9%8A%D9%84-%D8%A7%D9%84%D8%B1%D8%AF%D9%88%D8%AF-%D8%A7%D9%84%D8%A5%D8%B4%D8%A7%D8%B1%D8%A7%D8%AA)  تفعيل الردود (الإشارات)

حتى إذا كانت القناة مسموحًا بها (عبر `groupPolicy` \+ `groups`) وكان المرسِل مسموحًا به، فإن OpenClaw يستخدم افتراضيًا **تقييد الإشارات** في سياقات المجموعات.هذا يعني أنك قد ترى سجلات مثل `drop channel … (missing-mention)` ما لم تتضمن الرسالة نمط إشارة يطابق البوت.لجعل البوت يرد في قناة IRC **من دون الحاجة إلى إشارة**، عطّل تقييد الإشارات لتلك القناة:

```
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": {
          requireMention: false,
          allowFrom: ["*"],
        },
      },
    },
  },
}
```

أو للسماح **لكل** قنوات IRC (من دون قائمة سماح لكل قناة) مع الرد أيضًا من دون إشارات:

```
{
  channels: {
    irc: {
      groupPolicy: "open",
      groups: {
        "*": { requireMention: false, allowFrom: ["*"] },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A9-%D8%A3%D9%85%D9%86%D9%8A%D8%A9-%D9%85%D8%B3%D8%AA%D8%AD%D8%B3%D9%86%D8%A9-%D9%84%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D8%B9%D8%A7%D9%85%D8%A9)  ملاحظة أمنية (مستحسنة للقنوات العامة)

إذا سمحت باستخدام `allowFrom: ["*"]` في قناة عامة، يمكن لأي شخص توجيه مطالبات إلى البوت.
ولتقليل المخاطر، قيّد الأدوات لتلك القناة.

### [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D9%86%D9%81%D8%B3%D9%87%D8%A7-%D9%84%D9%84%D8%AC%D9%85%D9%8A%D8%B9-%D9%81%D9%8A-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9)  الأدوات نفسها للجميع في القناة

```
{
  channels: {
    irc: {
      groups: {
        "#tuirc-dev": {
          allowFrom: ["*"],
          tools: {
            deny: ["group:runtime", "group:fs", "gateway", "nodes", "cron", "browser"],
          },
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D9%85%D8%AE%D8%AA%D9%84%D9%81%D8%A9-%D9%84%D9%83%D9%84-%D9%85%D8%B1%D8%B3%D9%90%D9%84-%D8%A7%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%8A%D8%AD%D8%B5%D9%84-%D8%B9%D9%84%D9%89-%D8%B5%D9%84%D8%A7%D8%AD%D9%8A%D8%A7%D8%AA-%D8%A3%D9%83%D8%A8%D8%B1)  أدوات مختلفة لكل مرسِل (المالك يحصل على صلاحيات أكبر)

استخدم `toolsBySender` لتطبيق سياسة أكثر صرامة على `"*"` وسياسة أقل صرامة على اسمك:

```
{
  channels: {
    irc: {
      groups: {
        "#tuirc-dev": {
          allowFrom: ["*"],
          toolsBySender: {
            "*": {
              deny: ["group:runtime", "group:fs", "gateway", "nodes", "cron", "browser"],
            },
            "id:eigen": {
              deny: ["gateway", "nodes", "cron"],
            },
          },
        },
      },
    },
  },
}
```

ملاحظات:

- يجب أن تستخدم مفاتيح `toolsBySender` البادئة `id:` لقيم هوية مرسل IRC:
`id:eigen` أو `id:eigen!~eigen@174.127.248.171` لمطابقة أقوى.
- لا تزال المفاتيح القديمة غير المسبوقة ببادئة مقبولة وتُطابَق على أنها `id:` فقط.
- تفوز أول سياسة مرسل مطابقة؛ وتمثل `"*"` بديل wildcard.

لمزيد من المعلومات حول الوصول إلى المجموعات مقابل تقييد الإشارات (وكيفية تفاعلهما)، راجع: [/channels/groups](https://docs.openclaw.ai/ar/channels/groups).

## [​](https://docs.openclaw.ai/ar/channels/irc\#nickserv)  NickServ

للتعريف باستخدام NickServ بعد الاتصال:

```
{
  channels: {
    irc: {
      nickserv: {
        enabled: true,
        service: "NickServ",
        password: "your-nickserv-password",
      },
    },
  },
}
```

تسجيل اختياري لمرة واحدة عند الاتصال:

```
{
  channels: {
    irc: {
      nickserv: {
        register: true,
        registerEmail: "bot@example.com",
      },
    },
  },
}
```

عطّل `register` بعد تسجيل الاسم لتجنّب محاولات REGISTER المتكررة.

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D9%85%D8%AA%D8%BA%D9%8A%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D9%8A%D8%A6%D8%A9)  متغيرات البيئة

يدعم الحساب الافتراضي:

- `IRC_HOST`
- `IRC_PORT`
- `IRC_TLS`
- `IRC_NICK`
- `IRC_USERNAME`
- `IRC_REALNAME`
- `IRC_PASSWORD`
- `IRC_CHANNELS` (مفصولة بفواصل)
- `IRC_NICKSERV_PASSWORD`
- `IRC_NICKSERV_REGISTER_EMAIL`

لا يمكن ضبط `IRC_HOST` من ملف `.env` الخاص بمساحة العمل؛ راجع [ملفات `.env` الخاصة بمساحة العمل](https://docs.openclaw.ai/ar/gateway/security).

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

- إذا كان البوت يتصل لكنه لا يرد أبدًا في القنوات، فتحقق من `channels.irc.groups` **وكذلك** مما إذا كان تقييد الإشارات يسقط الرسائل (`missing-mention`). إذا كنت تريد أن يرد من دون تنبيهات، فاضبط `requireMention:false` للقناة.
- إذا فشل تسجيل الدخول، فتحقق من توفر الاسم المستعار وكلمة مرور الخادم.
- إذا فشل TLS على شبكة مخصصة، فتحقق من إعدادات المضيف/المنفذ والشهادة.

## [​](https://docs.openclaw.ai/ar/channels/irc\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — جميع القنوات المدعومة
- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل الخاصة وتدفق الاقتران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك الدردشة الجماعية وتقييد الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[قواعد الإشعارات الفورية في Matrix للمعاينات الصامتة](https://docs.openclaw.ai/ar/channels/matrix-push-rules) [Mattermost](https://docs.openclaw.ai/ar/channels/mattermost)

Ctrl+I