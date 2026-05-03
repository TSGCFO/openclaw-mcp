---
source_url: https://docs.openclaw.ai/ar/channels/channel-routing
title: "\u062a\u0648\u062c\u064a\u0647 \u0627\u0644\u0642\u0646\u0648\u0627\u062a - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/channel-routing#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

توجيه القنوات

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [القنوات والتوجيه](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%A7%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D9%88%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)
- [المصطلحات الأساسية](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%A7%D9%84%D9%85%D8%B5%D8%B7%D9%84%D8%AD%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)
- [بادئات الأهداف الصادرة](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%A8%D8%A7%D8%AF%D8%A6%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1%D8%A9)
- [أشكال مفاتيح الجلسات (أمثلة)](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%A3%D8%B4%D9%83%D8%A7%D9%84-%D9%85%D9%81%D8%A7%D8%AA%D9%8A%D8%AD-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-%D8%A3%D9%85%D8%AB%D9%84%D8%A9)
- [تثبيت مسار الرسائل المباشرة الرئيسية](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA-%D9%85%D8%B3%D8%A7%D8%B1-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9-%D8%A7%D9%84%D8%B1%D8%A6%D9%8A%D8%B3%D9%8A%D8%A9)
- [تسجيل الوارد المحمي](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%AA%D8%B3%D8%AC%D9%8A%D9%84-%D8%A7%D9%84%D9%88%D8%A7%D8%B1%D8%AF-%D8%A7%D9%84%D9%85%D8%AD%D9%85%D9%8A)
- [قواعد التوجيه (كيف يتم اختيار وكيل)](https://docs.openclaw.ai/ar/channels/channel-routing#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D9%83%D9%8A%D9%81-%D9%8A%D8%AA%D9%85-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1-%D9%88%D9%83%D9%8A%D9%84)
- [مجموعات البث (تشغيل عدة وكلاء)](https://docs.openclaw.ai/ar/channels/channel-routing#%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D8%AB-%D8%AA%D8%B4%D8%BA%D9%8A%D9%84-%D8%B9%D8%AF%D8%A9-%D9%88%D9%83%D9%84%D8%A7%D8%A1)
- [نظرة عامة على الإعداد](https://docs.openclaw.ai/ar/channels/channel-routing#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9-%D8%B9%D9%84%D9%89-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [تخزين الجلسات](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)
- [سلوك WebChat](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%B3%D9%84%D9%88%D9%83-webchat)
- [سياق الرد](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%B3%D9%8A%D8%A7%D9%82-%D8%A7%D9%84%D8%B1%D8%AF)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/channel-routing#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%A7%D9%84%D9%82%D9%86%D9%88%D8%A7%D8%AA-%D9%88%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)  القنوات والتوجيه

يوجّه OpenClaw الردود **إلى القناة نفسها التي أتت منها الرسالة**. لا
يختار النموذج قناة؛ فالتوجيه حتمي وتتحكم فيه إعدادات المضيف.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%A7%D9%84%D9%85%D8%B5%D8%B7%D9%84%D8%AD%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)  المصطلحات الأساسية

- **القناة**: `telegram`، `whatsapp`، `discord`، `irc`، `googlechat`، `slack`، `signal`، `imessage`، `line`، إضافة إلى قنوات Plugin. `webchat` هي قناة واجهة WebChat الداخلية وليست قناة صادرة قابلة للتكوين.
- **AccountId**: نسخة حساب لكل قناة (عند الدعم).
- حساب القناة الافتراضي الاختياري: يختار `channels.<channel>.defaultAccount`
الحساب المستخدم عندما لا يحدد مسار صادر `accountId`.

  - في إعدادات الحسابات المتعددة، عيّن افتراضيا صريحا (`defaultAccount` أو `accounts.default`) عندما يكون حسابان أو أكثر مهيأين. من دونه، قد يختار توجيه الرجوع أول معرّف حساب مطبّع.
- **AgentId**: مساحة عمل معزولة \+ مخزن جلسة (“دماغ”).
- **SessionKey**: مفتاح الحاوية المستخدم لتخزين السياق والتحكم في التزامن.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%A8%D8%A7%D8%AF%D8%A6%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D9%87%D8%AF%D8%A7%D9%81-%D8%A7%D9%84%D8%B5%D8%A7%D8%AF%D8%B1%D8%A9)  بادئات الأهداف الصادرة

قد تتضمن الأهداف الصادرة الصريحة بادئة مزود، مثل `telegram:123` أو `tg:123`. يتعامل المركز مع تلك البادئة كتلميح لاختيار القناة فقط عندما تكون القناة المحددة `last` أو غير محلولة بطريقة أخرى، وفقط عندما يعلن Plugin المحمّل عن تلك البادئة. إذا كان المستدعي قد حدد قناة صريحة بالفعل، فيجب أن تطابق بادئة المزود تلك القناة؛ تفشل التركيبات العابرة للقنوات، مثل تسليم WhatsApp إلى `telegram:123`، قبل تطبيع الهدف الخاص بــ Plugin.تبقى بادئات نوع الهدف والخدمة مثل `channel:<id>`، و`user:<id>`، و`room:<id>`، و`thread:<id>`، و`imessage:<handle>`، و`sms:<number>` داخل قواعد القناة المحددة. وهي لا تختار المزود بذاتها.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%A3%D8%B4%D9%83%D8%A7%D9%84-%D9%85%D9%81%D8%A7%D8%AA%D9%8A%D8%AD-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-%D8%A3%D9%85%D8%AB%D9%84%D8%A9)  أشكال مفاتيح الجلسات (أمثلة)

تندمج الرسائل المباشرة في جلسة الوكيل **الرئيسية** افتراضيا:

- `agent:<agentId>:<mainKey>` (الافتراضي: `agent:main:main`)

حتى عندما تتم مشاركة سجل محادثة الرسائل المباشرة مع الرئيسي، تستخدم سياسة الصندوق المعزول
والأدوات مفتاح تشغيل محادثة مباشرة مشتقا لكل حساب للرسائل المباشرة الخارجية
حتى لا تعامل الرسائل الناشئة من القنوات مثل عمليات تشغيل الجلسة الرئيسية المحلية.تظل المجموعات والقنوات معزولة لكل قناة:

- المجموعات: `agent:<agentId>:<channel>:group:<id>`
- القنوات/الغرف: `agent:<agentId>:<channel>:channel:<id>`

الخيوط:

- تضيف خيوط Slack/Discord اللاحقة `:thread:<threadId>` إلى مفتاح الأساس.
- تضمّن مواضيع منتديات Telegram اللاحقة `:topic:<topicId>` في مفتاح المجموعة.

أمثلة:

- `agent:main:telegram:group:-1001234567890:topic:42`
- `agent:main:discord:channel:123456:thread:987654`

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA-%D9%85%D8%B3%D8%A7%D8%B1-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9-%D8%A7%D9%84%D8%B1%D8%A6%D9%8A%D8%B3%D9%8A%D8%A9)  تثبيت مسار الرسائل المباشرة الرئيسية

عندما يكون `session.dmScope` هو `main`، قد تشارك الرسائل المباشرة جلسة رئيسية واحدة.
لمنع استبدال `lastRoute` الخاص بالجلسة برسائل مباشرة من غير المالك،
يستنتج OpenClaw مالكا مثبتا من `allowFrom` عندما تكون كل هذه الشروط صحيحة:

- يحتوي `allowFrom` على إدخال واحد فقط غير شامل.
- يمكن تطبيع الإدخال إلى معرّف مرسل ملموس لتلك القناة.
- لا يطابق مرسل الرسالة المباشرة الواردة ذلك المالك المثبت.

في حالة عدم التطابق هذه، يظل OpenClaw يسجل بيانات تعريف الجلسة الواردة، لكنه
يتخطى تحديث `lastRoute` في الجلسة الرئيسية.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%AA%D8%B3%D8%AC%D9%8A%D9%84-%D8%A7%D9%84%D9%88%D8%A7%D8%B1%D8%AF-%D8%A7%D9%84%D9%85%D8%AD%D9%85%D9%8A)  تسجيل الوارد المحمي

يمكن لــ Plugins القنوات وسم سجل جلسة واردة بأنه `createIfMissing: false`
عندما يجب ألا ينشئ مسار محمي جلسة OpenClaw جديدة. في هذا الوضع،
قد يحدّث OpenClaw بيانات التعريف و`lastRoute` لجلسة موجودة، لكنه
لا ينشئ إدخال جلسة مخصصا للمسار فقط لمجرد رصد رسالة.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D9%83%D9%8A%D9%81-%D9%8A%D8%AA%D9%85-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1-%D9%88%D9%83%D9%8A%D9%84)  قواعد التوجيه (كيف يتم اختيار وكيل)

يختار التوجيه **وكيلا واحدا** لكل رسالة واردة:

1. **تطابق النظير الدقيق** (`bindings` مع `peer.kind` \+ `peer.id`).
2. **تطابق النظير الأب** (توريث الخيط).
3. **تطابق النقابة \+ الأدوار** (Discord) عبر `guildId` \+ `roles`.
4. **تطابق النقابة** (Discord) عبر `guildId`.
5. **تطابق الفريق** (Slack) عبر `teamId`.
6. **تطابق الحساب** (`accountId` على القناة).
7. **تطابق القناة** (أي حساب على تلك القناة، `accountId: "*"`).
8. **الوكيل الافتراضي** (`agents.list[].default`، وإلا أول إدخال في القائمة، مع الرجوع إلى `main`).

عندما يتضمن ربط واحد عدة حقول مطابقة (`peer`، و`guildId`، و`teamId`، و`roles`)، **يجب أن تطابق كل الحقول المقدمة** حتى يطبق ذلك الربط.يحدد الوكيل المطابق مساحة العمل ومخزن الجلسة المستخدمين.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D8%A7%D9%84%D8%A8%D8%AB-%D8%AA%D8%B4%D8%BA%D9%8A%D9%84-%D8%B9%D8%AF%D8%A9-%D9%88%D9%83%D9%84%D8%A7%D8%A1)  مجموعات البث (تشغيل عدة وكلاء)

تتيح لك مجموعات البث تشغيل **عدة وكلاء** للنظير نفسه **عندما يرد OpenClaw عادة** (مثلا: في مجموعات WhatsApp، بعد بوابة الذكر/التفعيل).الإعداد:

```
{
  broadcast: {
    strategy: "parallel",
    "120363403215116621@g.us": ["alfred", "baerbel"],
    "+15555550123": ["support", "logger"],
  },
}
```

انظر: [مجموعات البث](https://docs.openclaw.ai/ar/channels/broadcast-groups).

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9-%D8%B9%D9%84%D9%89-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  نظرة عامة على الإعداد

- `agents.list`: تعريفات الوكلاء المسماة (مساحة العمل، النموذج، إلخ).
- `bindings`: ربط القنوات/الحسابات/النظراء الواردة بالوكلاء.

مثال:

```
{
  agents: {
    list: [{ id: "support", name: "Support", workspace: "~/.openclaw/workspace-support" }],
  },
  bindings: [\
    { match: { channel: "slack", teamId: "T123" }, agentId: "support" },\
    { match: { channel: "telegram", peer: { kind: "group", id: "-100123" } }, agentId: "support" },\
  ],
}
```

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)  تخزين الجلسات

تعيش مخازن الجلسات تحت دليل الحالة (الافتراضي `~/.openclaw`):

- `~/.openclaw/agents/<agentId>/sessions/sessions.json`
- تعيش سجلات JSONL بجانب المخزن

يمكنك تجاوز مسار المخزن عبر `session.store` وقوالب `{agentId}`.يفحص اكتشاف جلسات Gateway وACP أيضا مخازن الوكلاء المدعومة بالقرص تحت
جذر `agents/` الافتراضي وتحت جذور `session.store` المقولبة. يجب أن
تبقى المخازن المكتشفة داخل جذر الوكيل المحلول وأن تستخدم ملف
`sessions.json` عاديا. يتم تجاهل الروابط الرمزية والمسارات الخارجة عن الجذر.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%B3%D9%84%D9%88%D9%83-webchat)  سلوك WebChat

يتصل WebChat بــ **الوكيل المحدد** ويستخدم جلسة الوكيل الرئيسية افتراضيا.
وبسبب ذلك، يتيح لك WebChat رؤية السياق العابر للقنوات لذلك
الوكيل في مكان واحد.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%B3%D9%8A%D8%A7%D9%82-%D8%A7%D9%84%D8%B1%D8%AF)  سياق الرد

تتضمن الردود الواردة:

- `ReplyToId`، و`ReplyToBody`، و`ReplyToSender` عند توفرها.
- يضاف السياق المقتبس إلى `Body` ككتلة `[Replying to ...]`.

هذا متسق عبر القنوات.

## [​](https://docs.openclaw.ai/ar/channels/channel-routing\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [المجموعات](https://docs.openclaw.ai/ar/channels/groups)
- [مجموعات البث](https://docs.openclaw.ai/ar/channels/broadcast-groups)
- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing)

[Broadcast groups](https://docs.openclaw.ai/ar/channels/broadcast-groups) [تحليل موقع القناة](https://docs.openclaw.ai/ar/channels/location)

Ctrl+I