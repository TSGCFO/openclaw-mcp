---
source_url: https://docs.openclaw.ai/ar/channels/pairing
title: "\u0627\u0644\u0627\u0642\u062a\u0631\u0627\u0646 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/pairing#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

الاقتران

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [1) إقران الرسائل الخاصة (وصول الدردشة الواردة)](https://docs.openclaw.ai/ar/channels/pairing#1-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9-%D9%88%D8%B5%D9%88%D9%84-%D8%A7%D9%84%D8%AF%D8%B1%D8%AF%D8%B4%D8%A9-%D8%A7%D9%84%D9%88%D8%A7%D8%B1%D8%AF%D8%A9)
- [الموافقة على مرسل](https://docs.openclaw.ai/ar/channels/pairing#%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%B9%D9%84%D9%89-%D9%85%D8%B1%D8%B3%D9%84)
- [مجموعات المرسلين القابلة لإعادة الاستخدام](https://docs.openclaw.ai/ar/channels/pairing#%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B1%D8%B3%D9%84%D9%8A%D9%86-%D8%A7%D9%84%D9%82%D8%A7%D8%A8%D9%84%D8%A9-%D9%84%D8%A5%D8%B9%D8%A7%D8%AF%D8%A9-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)
- [مكان وجود الحالة](https://docs.openclaw.ai/ar/channels/pairing#%D9%85%D9%83%D8%A7%D9%86-%D9%88%D8%AC%D9%88%D8%AF-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9)
- [2) إقران جهاز Node (iOS/Android/macOS/العُقد بلا واجهة)](https://docs.openclaw.ai/ar/channels/pairing#2-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%AC%D9%87%D8%A7%D8%B2-node-ios%2Fandroid%2Fmacos%2F%D8%A7%D9%84%D8%B9%D9%8F%D9%82%D8%AF-%D8%A8%D9%84%D8%A7-%D9%88%D8%A7%D8%AC%D9%87%D8%A9)
- [الإقران عبر Telegram (موصى به لـ iOS)](https://docs.openclaw.ai/ar/channels/pairing#%D8%A7%D9%84%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%B9%D8%A8%D8%B1-telegram-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87-%D9%84%D9%80-ios)
- [الموافقة على جهاز Node](https://docs.openclaw.ai/ar/channels/pairing#%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%B9%D9%84%D9%89-%D8%AC%D9%87%D8%A7%D8%B2-node)
- [موافقة تلقائية اختيارية لـ Node حسب CIDR موثوق](https://docs.openclaw.ai/ar/channels/pairing#%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D8%A9-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1%D9%8A%D8%A9-%D9%84%D9%80-node-%D8%AD%D8%B3%D8%A8-cidr-%D9%85%D9%88%D8%AB%D9%88%D9%82)
- [تخزين حالة إقران Node](https://docs.openclaw.ai/ar/channels/pairing#%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D8%AD%D8%A7%D9%84%D8%A9-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-node)
- [ملاحظات](https://docs.openclaw.ai/ar/channels/pairing#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)
- [مستندات ذات صلة](https://docs.openclaw.ai/ar/channels/pairing#%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

«الإقران» هو خطوة الموافقة الصريحة على الوصول في OpenClaw.
يُستخدم في موضعين:

1. **إقران الرسائل الخاصة** (من يُسمح له بالتحدث إلى البوت)
2. **إقران Node** (ما الأجهزة/العُقد التي يُسمح لها بالانضمام إلى شبكة Gateway)

سياق الأمان: [الأمان](https://docs.openclaw.ai/ar/gateway/security)

## [​](https://docs.openclaw.ai/ar/channels/pairing\#1-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%AE%D8%A7%D8%B5%D8%A9-%D9%88%D8%B5%D9%88%D9%84-%D8%A7%D9%84%D8%AF%D8%B1%D8%AF%D8%B4%D8%A9-%D8%A7%D9%84%D9%88%D8%A7%D8%B1%D8%AF%D8%A9)  1) إقران الرسائل الخاصة (وصول الدردشة الواردة)

عند تكوين قناة بسياسة رسائل خاصة `pairing`، يحصل المرسلون غير المعروفين على رمز قصير ولا تتم **معالجة** رسالتهم حتى توافق عليها.سياسات الرسائل الخاصة الافتراضية موثقة في: [الأمان](https://docs.openclaw.ai/ar/gateway/security)`dmPolicy: "open"` تكون عامة فقط عندما تتضمن قائمة السماح الفعلية للرسائل الخاصة `"*"`.
يتطلب الإعداد والتحقق وجود حرف البدل هذا لتكوينات الفتح العام. إذا كانت الحالة الموجودة
تحتوي على `open` مع إدخالات `allowFrom` محددة، فسيظل وقت التشغيل يقبل
هؤلاء المرسلين فقط، ولا توسّع موافقات مخزن الإقران وصول `open`.رموز الإقران:

- 8 أحرف، كبيرة، بلا أحرف ملتبسة (`0O1I`).
- **تنتهي صلاحيتها بعد ساعة واحدة**. لا يرسل البوت رسالة الإقران إلا عند إنشاء طلب جديد (تقريبًا مرة واحدة في الساعة لكل مرسل).
- تُحد طلبات إقران الرسائل الخاصة المعلقة إلى **3 لكل قناة** افتراضيًا؛ يتم تجاهل الطلبات الإضافية حتى تنتهي صلاحية أحدها أو تتم الموافقة عليه.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%B9%D9%84%D9%89-%D9%85%D8%B1%D8%B3%D9%84)  الموافقة على مرسل

```
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

إذا لم يكن مالك الأوامر مكوّنًا بعد، فإن الموافقة على رمز إقران الرسائل الخاصة تهيئ أيضًا
`commands.ownerAllowFrom` إلى المرسل الموافق عليه، مثل `telegram:123456789`.
يمنح ذلك إعدادات المرة الأولى مالكًا صريحًا للأوامر ذات الامتيازات ومطالبات موافقة التنفيذ.
بعد وجود مالك، تمنح موافقات الإقران اللاحقة وصول الرسائل الخاصة فقط؛ ولا تضيف مزيدًا من المالكين.القنوات المدعومة: `bluebubbles`, `discord`, `feishu`, `googlechat`, `imessage`, `irc`, `line`, `matrix`, `mattermost`, `msteams`, `nextcloud-talk`, `nostr`, `openclaw-weixin`, `signal`, `slack`, `synology-chat`, `telegram`, `twitch`, `whatsapp`, `zalo`, `zalouser`.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B1%D8%B3%D9%84%D9%8A%D9%86-%D8%A7%D9%84%D9%82%D8%A7%D8%A8%D9%84%D8%A9-%D9%84%D8%A5%D8%B9%D8%A7%D8%AF%D8%A9-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)  مجموعات المرسلين القابلة لإعادة الاستخدام

استخدم `accessGroups` على المستوى الأعلى عندما ينبغي أن تنطبق مجموعة المرسلين الموثوقين نفسها على
قنوات رسائل متعددة أو على قوائم السماح للرسائل الخاصة والمجموعات معًا.تستخدم المجموعات الثابتة `type: "message.senders"` وتُشار إليها باستخدام
`accessGroup:<name>` من قوائم السماح للقنوات:

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
  channels: {
    telegram: { dmPolicy: "allowlist", allowFrom: ["accessGroup:operators"] },
    whatsapp: { groupPolicy: "allowlist", groupAllowFrom: ["accessGroup:operators"] },
  },
}
```

مجموعات الوصول موثقة بالتفصيل هنا: [مجموعات الوصول](https://docs.openclaw.ai/ar/channels/access-groups)

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D9%85%D9%83%D8%A7%D9%86-%D9%88%D8%AC%D9%88%D8%AF-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9)  مكان وجود الحالة

مخزنة ضمن `~/.openclaw/credentials/`:

- الطلبات المعلقة: `<channel>-pairing.json`
- مخزن قائمة السماح الموافق عليها:
  - الحساب الافتراضي: `<channel>-allowFrom.json`
  - الحساب غير الافتراضي: `<channel>-<accountId>-allowFrom.json`

سلوك تحديد نطاق الحساب:

- تقرأ الحسابات غير الافتراضية ملف قائمة السماح المحدد النطاق الخاص بها وتكتب إليه فقط.
- يستخدم الحساب الافتراضي ملف قائمة السماح غير المحدد النطاق على مستوى القناة.

عامل هذه الملفات على أنها حساسة (فهي تتحكم في الوصول إلى مساعدك).

مخزن قائمة السماح بالإقران مخصص لوصول الرسائل الخاصة. تخويل المجموعات منفصل.
لا تؤدي الموافقة على رمز إقران الرسائل الخاصة تلقائيًا إلى السماح لذلك المرسل بتشغيل أوامر المجموعات
أو التحكم في البوت داخل المجموعات. تمهيد المالك الأول هو حالة تكوين منفصلة
في `commands.ownerAllowFrom`، ولا يزال تسليم دردشة المجموعات يتبع
قوائم السماح للمجموعات الخاصة بالقناة (على سبيل المثال `groupAllowFrom` أو `groups` أو التجاوزات لكل مجموعة
أو لكل موضوع بحسب القناة).

## [​](https://docs.openclaw.ai/ar/channels/pairing\#2-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%AC%D9%87%D8%A7%D8%B2-node-ios/android/macos/%D8%A7%D9%84%D8%B9%D9%8F%D9%82%D8%AF-%D8%A8%D9%84%D8%A7-%D9%88%D8%A7%D8%AC%D9%87%D8%A9)  2) إقران جهاز Node (iOS/Android/macOS/العُقد بلا واجهة)

تتصل Nodes بـ Gateway بصفتها **أجهزة** مع `role: node`. ينشئ Gateway
طلب إقران جهاز يجب الموافقة عليه.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D8%A7%D9%84%D8%A5%D9%82%D8%B1%D8%A7%D9%86-%D8%B9%D8%A8%D8%B1-telegram-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87-%D9%84%D9%80-ios)  الإقران عبر Telegram (موصى به لـ iOS)

إذا كنت تستخدم Plugin `device-pair`، يمكنك إجراء إقران الجهاز لأول مرة بالكامل من Telegram:

1. في Telegram، راسل البوت: `/pair`
2. يرد البوت برسالتين: رسالة تعليمات ورسالة **رمز إعداد** منفصلة (يسهل نسخها/لصقها في Telegram).
3. على هاتفك، افتح تطبيق OpenClaw على iOS ← الإعدادات ← Gateway.
4. الصق رمز الإعداد واتصل.
5. مرة أخرى في Telegram: `/pair pending` (راجع معرّفات الطلبات والدور والنطاقات)، ثم وافق.

رمز الإعداد هو حمولة JSON مشفرة بـ base64 تحتوي على:

- `url`: عنوان URL لـ WebSocket الخاص بـ Gateway (`ws://...` أو `wss://...`)
- `bootstrapToken`: رمز تمهيد قصير العمر لجهاز واحد يُستخدم لمصافحة الإقران الأولية

يحمل رمز التمهيد هذا ملف تعريف تمهيد الإقران المدمج:

- يبقى رمز `node` الأساسي المُسلّم `scopes: []`
- يبقى أي رمز `operator` مُسلّم محدودًا بقائمة سماح التمهيد:
`operator.approvals`, `operator.read`, `operator.talk.secrets`, `operator.write`
- فحوص النطاق في التمهيد مسبوقة بالدور، وليست مجموعة نطاقات مسطحة واحدة:
إدخالات نطاق المشغل لا تلبي إلا طلبات المشغل، ويجب على الأدوار غير المشغلة
أن تظل تطلب النطاقات تحت بادئة دورها الخاصة
- يظل تدوير/إبطال الرموز لاحقًا محدودًا بكل من عقد الدور الموافق عليه للجهاز
ونطاقات المشغل لجلسة المستدعي

عامل رمز الإعداد ككلمة مرور أثناء صلاحيته.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D8%A7%D9%84%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%B9%D9%84%D9%89-%D8%AC%D9%87%D8%A7%D8%B2-node)  الموافقة على جهاز Node

```
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
```

إذا أعاد الجهاز نفسه المحاولة بتفاصيل مصادقة مختلفة (على سبيل المثال دور/نطاقات/مفتاح عام مختلف)،
فسيتم استبدال الطلب المعلق السابق وإنشاء `requestId` جديد.

لا يحصل الجهاز المقترن مسبقًا على وصول أوسع بصمت. إذا أعاد الاتصال طالبًا نطاقات أكثر أو دورًا أوسع، يحتفظ OpenClaw بالموافقة الحالية كما هي وينشئ طلب ترقية معلقًا جديدًا. استخدم `openclaw devices list` لمقارنة الوصول الموافق عليه حاليًا مع الوصول المطلوب حديثًا قبل الموافقة.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D9%85%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D8%A9-%D8%A7%D8%AE%D8%AA%D9%8A%D8%A7%D8%B1%D9%8A%D8%A9-%D9%84%D9%80-node-%D8%AD%D8%B3%D8%A8-cidr-%D9%85%D9%88%D8%AB%D9%88%D9%82)  موافقة تلقائية اختيارية لـ Node حسب CIDR موثوق

يبقى إقران الأجهزة يدويًا افتراضيًا. لشبكات Node المحكومة بإحكام،
يمكنك الاشتراك في الموافقة التلقائية على Node لأول مرة باستخدام CIDR صريحة أو عناوين IP دقيقة:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

ينطبق هذا فقط على طلبات إقران `role: node` الجديدة من دون
نطاقات مطلوبة. لا يزال عملاء المشغل والمتصفح وواجهة التحكم وWebChat يتطلبون موافقة يدوية.
ولا تزال تغييرات الدور والنطاق والبيانات الوصفية والمفتاح العام تتطلب موافقة يدوية.

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D8%AD%D8%A7%D9%84%D8%A9-%D8%A5%D9%82%D8%B1%D8%A7%D9%86-node)  تخزين حالة إقران Node

مخزنة ضمن `~/.openclaw/devices/`:

- `pending.json` (قصير العمر؛ تنتهي صلاحية الطلبات المعلقة)
- `paired.json` (الأجهزة المقترنة \+ الرموز)

### [​](https://docs.openclaw.ai/ar/channels/pairing\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA)  ملاحظات

- واجهة API القديمة `node.pair.*` ‏(CLI: `openclaw nodes pending|approve|reject|remove|rename`) هي
مخزن إقران منفصل مملوك لـ Gateway. لا تزال Nodes عبر WS تتطلب إقران الأجهزة.
- سجل الإقران هو مصدر الحقيقة الدائم للأدوار الموافق عليها. تبقى
رموز الأجهزة النشطة محدودة بمجموعة الأدوار الموافق عليها؛ ولا ينشئ إدخال رمز شارد
خارج الأدوار الموافق عليها وصولًا جديدًا.

## [​](https://docs.openclaw.ai/ar/channels/pairing\#%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  مستندات ذات صلة

- نموذج الأمان \+ حقن المطالبات: [الأمان](https://docs.openclaw.ai/ar/gateway/security)
- التحديث بأمان (تشغيل الطبيب): [التحديث](https://docs.openclaw.ai/ar/install/updating)
- تكوينات القنوات:
  - Telegram: [Telegram](https://docs.openclaw.ai/ar/channels/telegram)
  - WhatsApp: [WhatsApp](https://docs.openclaw.ai/ar/channels/whatsapp)
  - Signal: [Signal](https://docs.openclaw.ai/ar/channels/signal)
  - BlueBubbles (iMessage): [BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles)
  - iMessage (قديم): [iMessage](https://docs.openclaw.ai/ar/channels/imessage)
  - Discord: [Discord](https://docs.openclaw.ai/ar/channels/discord)
  - Slack: [Slack](https://docs.openclaw.ai/ar/channels/slack)

[Zalo الشخصي](https://docs.openclaw.ai/ar/channels/zalouser) [مجموعات الوصول](https://docs.openclaw.ai/ar/channels/access-groups)

Ctrl+I