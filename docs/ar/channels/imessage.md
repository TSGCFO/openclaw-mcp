---
source_url: https://docs.openclaw.ai/ar/channels/imessage
title: "iMessage - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/imessage#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Mainstream messaging

iMessage

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [الإعداد السريع](https://docs.openclaw.ai/ar/channels/imessage#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [المتطلبات والأذونات (macOS)](https://docs.openclaw.ai/ar/channels/imessage#%D8%A7%D9%84%D9%85%D8%AA%D8%B7%D9%84%D8%A8%D8%A7%D8%AA-%D9%88%D8%A7%D9%84%D8%A3%D8%B0%D9%88%D9%86%D8%A7%D8%AA-macos)
- [التحكم في الوصول والتوجيه](https://docs.openclaw.ai/ar/channels/imessage#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D9%88%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)
- [روابط محادثات ACP](https://docs.openclaw.ai/ar/channels/imessage#%D8%B1%D9%88%D8%A7%D8%A8%D8%B7-%D9%85%D8%AD%D8%A7%D8%AF%D8%AB%D8%A7%D8%AA-acp)
- [أنماط النشر](https://docs.openclaw.ai/ar/channels/imessage#%D8%A3%D9%86%D9%85%D8%A7%D8%B7-%D8%A7%D9%84%D9%86%D8%B4%D8%B1)
- [الوسائط، والتقسيم، وأهداف التسليم](https://docs.openclaw.ai/ar/channels/imessage#%D8%A7%D9%84%D9%88%D8%B3%D8%A7%D8%A6%D8%B7%D8%8C-%D9%88%D8%A7%D9%84%D8%AA%D9%82%D8%B3%D9%8A%D9%85%D8%8C-%D9%88%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85)
- [كتابة الإعدادات](https://docs.openclaw.ai/ar/channels/imessage#%D9%83%D8%AA%D8%A7%D8%A8%D8%A9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/imessage#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [مؤشرات مرجع الإعدادات](https://docs.openclaw.ai/ar/channels/imessage#%D9%85%D8%A4%D8%B4%D8%B1%D8%A7%D8%AA-%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/imessage#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

لعمليات نشر iMessage الجديدة، استخدم [BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles).تكامل `imsg` قديم وقد تتم إزالته في إصدار مستقبلي.

الحالة: تكامل CLI خارجي قديم. يشغّل Gateway الأمر `imsg rpc` ويتواصل عبر JSON-RPC على stdio (بدون خادم daemon/منفذ منفصل).

[**BlueBubbles (موصى به)** \\
\\
مسار iMessage المفضل للإعدادات الجديدة.](https://docs.openclaw.ai/ar/channels/bluebubbles)

[**الإقران** \\
\\
رسائل iMessage المباشرة تستخدم وضع الإقران افتراضياً.](https://docs.openclaw.ai/ar/channels/pairing)

[**مرجع الإعدادات** \\
\\
مرجع كامل لحقول iMessage.](https://docs.openclaw.ai/ar/gateway/config-channels#imessage)

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  الإعداد السريع

- Mac محلي (المسار السريع)

- Mac بعيد عبر SSH


1

[Navigate to header](https://docs.openclaw.ai/ar/channels/imessage#)

ثبّت imsg وتحقق منه

```
brew install steipete/tap/imsg
imsg rpc --help
```

2

[Navigate to header](https://docs.openclaw.ai/ar/channels/imessage#)

اضبط OpenClaw

```
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "/usr/local/bin/imsg",
      dbPath: "/Users/user/Library/Messages/chat.db",
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/ar/channels/imessage#)

ابدأ Gateway

```
openclaw gateway
```

4

[Navigate to header](https://docs.openclaw.ai/ar/channels/imessage#)

وافق على إقران أول رسالة مباشرة (dmPolicy الافتراضي)

```
openclaw pairing list imessage
openclaw pairing approve imessage <CODE>
```

تنتهي صلاحية طلبات الإقران بعد ساعة واحدة.

يتطلب OpenClaw فقط `cliPath` متوافقاً مع stdio، لذلك يمكنك توجيه `cliPath` إلى سكربت غلاف يستخدم SSH إلى Mac بعيد ويشغّل `imsg`.

```
#!/usr/bin/env bash
exec ssh -T gateway-host imsg "$@"
```

الإعداد الموصى به عند تفعيل المرفقات:

```
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "~/.openclaw/scripts/imsg-ssh",
      remoteHost: "user@gateway-host", // used for SCP attachment fetches
      includeAttachments: true,
      // Optional: override allowed attachment roots.
      // Defaults include /Users/*/Library/Messages/Attachments
      attachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      remoteAttachmentRoots: ["/Users/*/Library/Messages/Attachments"],
    },
  },
}
```

إذا لم يتم ضبط `remoteHost`، يحاول OpenClaw اكتشافه تلقائياً عن طريق تحليل سكربت غلاف SSH.
يجب أن يكون `remoteHost` بالشكل `host` أو `user@host` (بدون مسافات أو خيارات SSH).
يستخدم OpenClaw فحصاً صارماً لمفتاح المضيف مع SCP، لذلك يجب أن يكون مفتاح مضيف الترحيل موجوداً مسبقاً في `~/.ssh/known_hosts`.
يتم التحقق من مسارات المرفقات مقابل الجذور المسموح بها (`attachmentRoots` / `remoteAttachmentRoots`).

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A7%D9%84%D9%85%D8%AA%D8%B7%D9%84%D8%A8%D8%A7%D8%AA-%D9%88%D8%A7%D9%84%D8%A3%D8%B0%D9%88%D9%86%D8%A7%D8%AA-macos)  المتطلبات والأذونات (macOS)

- يجب تسجيل الدخول إلى Messages على Mac الذي يشغّل `imsg`.
- يلزم Full Disk Access لسياق العملية الذي يشغّل OpenClaw/`imsg` (للوصول إلى قاعدة بيانات Messages).
- يلزم إذن Automation لإرسال الرسائل عبر Messages.app.

تُمنح الأذونات لكل سياق عملية. إذا كان Gateway يعمل بدون واجهة (LaunchAgent/SSH)، فشغّل أمراً تفاعلياً لمرة واحدة في السياق نفسه لتشغيل المطالبات:

```
imsg chats --limit 1
# or
imsg send <handle> "test"
```

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84-%D9%88%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)  التحكم في الوصول والتوجيه

- سياسة الرسائل المباشرة

- سياسة المجموعات \+ الإشارات

- الجلسات والردود الحتمية


يتحكم `channels.imessage.dmPolicy` في الرسائل المباشرة:

- `pairing` (افتراضي)
- `allowlist`
- `open` (يتطلب أن يتضمن `allowFrom` القيمة `"*"`)
- `disabled`

حقل قائمة السماح: `channels.imessage.allowFrom`.يمكن أن تكون إدخالات قائمة السماح معرّفات أو أهداف دردشة (`chat_id:*`، `chat_guid:*`، `chat_identifier:*`).

يتحكم `channels.imessage.groupPolicy` في معالجة المجموعات:

- `allowlist` (الافتراضي عند ضبطه)
- `open`
- `disabled`

قائمة السماح لمرسلي المجموعات: `channels.imessage.groupAllowFrom`.الرجوع أثناء التشغيل: إذا لم يتم ضبط `groupAllowFrom`، ترجع فحوصات مرسل مجموعة iMessage إلى `allowFrom` عند توفره.
ملاحظة وقت التشغيل: إذا كان `channels.imessage` مفقوداً بالكامل، يرجع وقت التشغيل إلى `groupPolicy="allowlist"` ويسجّل تحذيراً (حتى إذا كان `channels.defaults.groupPolicy` مضبوطاً).بوابة الإشارات للمجموعات:

- لا يحتوي iMessage على بيانات وصفية أصلية للإشارات
- يستخدم اكتشاف الإشارات أنماط regex (`agents.list[].groupChat.mentionPatterns`، مع الرجوع إلى `messages.groupChat.mentionPatterns`)
- بدون أنماط مضبوطة، لا يمكن فرض بوابة الإشارات

يمكن لأوامر التحكم من المرسلين المصرح لهم تجاوز بوابة الإشارات في المجموعات.

- تستخدم الرسائل المباشرة التوجيه المباشر؛ وتستخدم المجموعات توجيه المجموعات.
- مع `session.dmScope=main` الافتراضي، تندمج رسائل iMessage المباشرة في الجلسة الرئيسية للوكيل.
- جلسات المجموعات معزولة (`agent:<agentId>:imessage:group:<chat_id>`).
- يتم توجيه الردود مرة أخرى إلى iMessage باستخدام بيانات القناة/الهدف الأصلية.

سلوك سلاسل المحادثات الشبيهة بالمجموعات:قد تصل بعض سلاسل iMessage متعددة المشاركين مع `is_group=false`.
إذا كان ذلك `chat_id` مضبوطاً صراحةً ضمن `channels.imessage.groups`، يتعامل معه OpenClaw كحركة مرور جماعية (بوابة المجموعات + عزل جلسات المجموعات).

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%B1%D9%88%D8%A7%D8%A8%D8%B7-%D9%85%D8%AD%D8%A7%D8%AF%D8%AB%D8%A7%D8%AA-acp)  روابط محادثات ACP

يمكن أيضاً ربط دردشات iMessage القديمة بجلسات ACP.تدفق سريع للمشغل:

- شغّل `/acp spawn codex --bind here` داخل الرسالة المباشرة أو دردشة المجموعة المسموح بها.
- تُوجَّه الرسائل المستقبلية في محادثة iMessage نفسها إلى جلسة ACP التي تم إنشاؤها.
- يعيد `/new` و`/reset` ضبط جلسة ACP المرتبطة نفسها في مكانها.
- يغلق `/acp close` جلسة ACP ويزيل الربط.

تُدعم الروابط الدائمة المضبوطة من خلال إدخالات `bindings[]` على المستوى الأعلى مع `type: "acp"` و`match.channel: "imessage"`.يمكن أن يستخدم `match.peer.id`:

- معرّف رسالة مباشرة موحّد مثل `+15555550123` أو `user@example.com`
- `chat_id:<id>` (موصى به لروابط المجموعات المستقرة)
- `chat_guid:<guid>`
- `chat_identifier:<identifier>`

مثال:

```
{
  agents: {
    list: [\
      {\
        id: "codex",\
        runtime: {\
          type: "acp",\
          acp: { agent: "codex", backend: "acpx", mode: "persistent" },\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "imessage",\
        accountId: "default",\
        peer: { kind: "group", id: "chat_id:123" },\
      },\
      acp: { label: "codex-group" },\
    },\
  ],
}
```

راجع [وكلاء ACP](https://docs.openclaw.ai/ar/tools/acp-agents) لسلوك ربط ACP المشترك.

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A3%D9%86%D9%85%D8%A7%D8%B7-%D8%A7%D9%84%D9%86%D8%B4%D8%B1)  أنماط النشر

مستخدم macOS مخصص للبوت (هوية iMessage منفصلة)

استخدم Apple ID مخصصاً ومستخدم macOS مخصصاً بحيث تكون حركة مرور البوت معزولة عن ملف Messages الشخصي الخاص بك.التدفق المعتاد:

1. أنشئ/سجّل الدخول إلى مستخدم macOS مخصص.
2. سجّل الدخول إلى Messages باستخدام Apple ID الخاص بالبوت في ذلك المستخدم.
3. ثبّت `imsg` في ذلك المستخدم.
4. أنشئ غلاف SSH حتى يتمكن OpenClaw من تشغيل `imsg` في سياق ذلك المستخدم.
5. وجّه `channels.imessage.accounts.<id>.cliPath` و`.dbPath` إلى ملف ذلك المستخدم الشخصي.

قد يتطلب التشغيل الأول موافقات واجهة رسومية (Automation + Full Disk Access) في جلسة مستخدم البوت.

Mac بعيد عبر Tailscale (مثال)

البنية الشائعة:

- يعمل Gateway على Linux/VM
- يعمل iMessage + `imsg` على Mac في tailnet الخاص بك
- يستخدم غلاف `cliPath` SSH لتشغيل `imsg`
- يفعّل `remoteHost` جلب المرفقات عبر SCP

مثال:

```
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "~/.openclaw/scripts/imsg-ssh",
      remoteHost: "bot@mac-mini.tailnet-1234.ts.net",
      includeAttachments: true,
      dbPath: "/Users/bot/Library/Messages/chat.db",
    },
  },
}
```

```
#!/usr/bin/env bash
exec ssh -T bot@mac-mini.tailnet-1234.ts.net imsg "$@"
```

استخدم مفاتيح SSH حتى يكون كل من SSH وSCP غير تفاعليين.
تأكد أولاً من أن مفتاح المضيف موثوق به (على سبيل المثال `ssh bot@mac-mini.tailnet-1234.ts.net`) حتى تتم تعبئة `known_hosts`.

نمط الحسابات المتعددة

يدعم iMessage إعداداً لكل حساب ضمن `channels.imessage.accounts`.يمكن لكل حساب تجاوز حقول مثل `cliPath`، و`dbPath`، و`allowFrom`، و`groupPolicy`، و`mediaMaxMb`، وإعدادات السجل، وقوائم السماح لجذور المرفقات.

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A7%D9%84%D9%88%D8%B3%D8%A7%D8%A6%D8%B7%D8%8C-%D9%88%D8%A7%D9%84%D8%AA%D9%82%D8%B3%D9%8A%D9%85%D8%8C-%D9%88%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85)  الوسائط، والتقسيم، وأهداف التسليم

المرفقات والوسائط

- استيعاب المرفقات الواردة اختياري: `channels.imessage.includeAttachments`
- يمكن جلب مسارات المرفقات البعيدة عبر SCP عند ضبط `remoteHost`
- يجب أن تطابق مسارات المرفقات الجذور المسموح بها:
  - `channels.imessage.attachmentRoots` (محلي)
  - `channels.imessage.remoteAttachmentRoots` (وضع SCP البعيد)
  - نمط الجذر الافتراضي: `/Users/*/Library/Messages/Attachments`
- يستخدم SCP فحصاً صارماً لمفتاح المضيف (`StrictHostKeyChecking=yes`)
- يستخدم حجم الوسائط الصادرة `channels.imessage.mediaMaxMb` (الافتراضي 16 MB)

تقسيم الصادر

- حد تقسيم النص: `channels.imessage.textChunkLimit` (الافتراضي 4000)
- وضع التقسيم: `channels.imessage.chunkMode`
  - `length` (افتراضي)
  - `newline` (تقسيم يعطي الأولوية للفقرات)

تنسيقات العنونة

الأهداف الصريحة المفضلة:

- `chat_id:123` (موصى به للتوجيه المستقر)
- `chat_guid:...`
- `chat_identifier:...`

أهداف المعرّفات مدعومة أيضاً:

- `imessage:+1555...`
- `sms:+1555...`
- `user@example.com`

```
imsg chats --limit 20
```

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D9%83%D8%AA%D8%A7%D8%A8%D8%A9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  كتابة الإعدادات

يسمح iMessage بكتابة الإعدادات التي تبدأها القناة افتراضياً (لأجل `/config set|unset` عندما تكون `commands.config: true`).للتعطيل:

```
{
  channels: {
    imessage: {
      configWrites: false,
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

لم يتم العثور على imsg أو RPC غير مدعوم

تحقق من الثنائي ودعم RPC:

```
imsg rpc --help
openclaw channels status --probe
```

إذا أبلغ الفحص أن RPC غير مدعوم، فحدّث `imsg`.

يتم تجاهل الرسائل المباشرة

تحقق من:

- `channels.imessage.dmPolicy`
- `channels.imessage.allowFrom`
- موافقات الإقران (`openclaw pairing list imessage`)

يتم تجاهل رسائل المجموعات

تحقق من:

- `channels.imessage.groupPolicy`
- `channels.imessage.groupAllowFrom`
- سلوك قائمة السماح في `channels.imessage.groups`
- إعداد أنماط الإشارة (`agents.list[].groupChat.mentionPatterns`)

تفشل المرفقات البعيدة

تحقق من:

- `channels.imessage.remoteHost`
- `channels.imessage.remoteAttachmentRoots`
- مصادقة مفتاح SSH/SCP من مضيف Gateway
- وجود مفتاح المضيف في `~/.ssh/known_hosts` على مضيف Gateway
- قابلية قراءة المسار البعيد على Mac الذي يشغّل Messages

تم تفويت مطالبات أذونات macOS

أعد التشغيل في طرفية واجهة رسومية تفاعلية في سياق المستخدم/الجلسة نفسه ووافق على المطالبات:

```
imsg chats --limit 1
imsg send <handle> "test"
```

تأكد من منح Full Disk Access + Automation لسياق العملية الذي يشغّل OpenClaw/`imsg`.

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D9%85%D8%A4%D8%B4%D8%B1%D8%A7%D8%AA-%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  مؤشرات مرجع الإعدادات

- [مرجع الإعدادات \- iMessage](https://docs.openclaw.ai/ar/gateway/config-channels#imessage)
- [إعدادات Gateway](https://docs.openclaw.ai/ar/gateway/configuration)
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing)
- [BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles)

## [​](https://docs.openclaw.ai/ar/channels/imessage\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — جميع القنوات المدعومة
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الإقران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك دردشة المجموعات وبوابة الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Google Chat](https://docs.openclaw.ai/ar/channels/googlechat) [BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles)

Ctrl+I