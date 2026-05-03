---
source_url: https://docs.openclaw.ai/ar/channels/group-messages
title: "\u0631\u0633\u0627\u0626\u0644 \u0627\u0644\u0645\u062c\u0645\u0648\u0639\u0627\u062a - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/group-messages#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

رسائل المجموعات

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [التنفيذ الحالي (2025-12-03)](https://docs.openclaw.ai/ar/channels/group-messages#%D8%A7%D9%84%D8%AA%D9%86%D9%81%D9%8A%D8%B0-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D9%8A-2025-12-03)
- [مثال تكوين (WhatsApp)](https://docs.openclaw.ai/ar/channels/group-messages#%D9%85%D8%AB%D8%A7%D9%84-%D8%AA%D9%83%D9%88%D9%8A%D9%86-whatsapp)
- [أمر التفعيل (للمالك فقط)](https://docs.openclaw.ai/ar/channels/group-messages#%D8%A3%D9%85%D8%B1-%D8%A7%D9%84%D8%AA%D9%81%D8%B9%D9%8A%D9%84-%D9%84%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%81%D9%82%D8%B7)
- [كيفية الاستخدام](https://docs.openclaw.ai/ar/channels/group-messages#%D9%83%D9%8A%D9%81%D9%8A%D8%A9-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)
- [الاختبار / التحقق](https://docs.openclaw.ai/ar/channels/group-messages#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-%2F-%D8%A7%D9%84%D8%AA%D8%AD%D9%82%D9%82)
- [اعتبارات معروفة](https://docs.openclaw.ai/ar/channels/group-messages#%D8%A7%D8%B9%D8%AA%D8%A8%D8%A7%D8%B1%D8%A7%D8%AA-%D9%85%D8%B9%D8%B1%D9%88%D9%81%D8%A9)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/group-messages#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

الهدف: السماح لـ Clawd بالوجود في مجموعات WhatsApp، والاستيقاظ فقط عند تنبيهه، وإبقاء ذلك الخيط منفصلاً عن جلسة الرسائل المباشرة الشخصية.

يُستخدم `agents.list[].groupChat.mentionPatterns` أيضاً بواسطة Telegram وDiscord وSlack وiMessage. يركز هذا المستند على السلوك الخاص بـ WhatsApp. لإعدادات متعددة الوكلاء، اضبط `agents.list[].groupChat.mentionPatterns` لكل وكيل، أو استخدم `messages.groupChat.mentionPatterns` كبديل عام.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D8%A7%D9%84%D8%AA%D9%86%D9%81%D9%8A%D8%B0-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D9%8A-2025-12-03)  التنفيذ الحالي (2025-12-03)

- أوضاع التفعيل: `mention` (الافتراضي) أو `always`. يتطلب `mention` تنبيهاً (إشارات WhatsApp @ الحقيقية عبر `mentionedJids`، أو أنماط تعبيرات منتظمة آمنة، أو رقم E.164 الخاص بالبوت في أي مكان داخل النص). يوقظ `always` الوكيل عند كل رسالة، لكن ينبغي أن يرد فقط عندما يستطيع إضافة قيمة مفيدة؛ وإلا فيُرجع رمز الصمت الدقيق `NO_REPLY` / `no_reply`. يمكن ضبط الإعدادات الافتراضية في التكوين (`channels.whatsapp.groups`) وتجاوزها لكل مجموعة عبر `/activation`. عند ضبط `channels.whatsapp.groups`، فإنه يعمل أيضاً كقائمة سماح للمجموعات (أدرج `"*"` للسماح للجميع).
- سياسة المجموعة: يتحكم `channels.whatsapp.groupPolicy` فيما إذا كانت رسائل المجموعة مقبولة (`open|disabled|allowlist`). يستخدم `allowlist` القيمة `channels.whatsapp.groupAllowFrom` (البديل: `channels.whatsapp.allowFrom` الصريح). الافتراضي هو `allowlist` (محظور حتى تضيف المرسلين).
- جلسات لكل مجموعة: تبدو مفاتيح الجلسات مثل `agent:<agentId>:whatsapp:group:<jid>` بحيث تكون أوامر مثل `/verbose on` أو `/trace on` أو `/think high` (المرسلة كرسائل مستقلة) مقصورة على تلك المجموعة؛ وتبقى حالة الرسائل المباشرة الشخصية دون مساس. يتم تخطي Heartbeats لخيوط المجموعات.
- حقن السياق: رسائل المجموعة **المعلقة فقط** (الافتراضي 50) التي _لم_ تشغّل تنفيذاً تُضاف تحت `[Chat messages since your last reply - for context]`، مع سطر التشغيل تحت `[Current message - respond to this]`. الرسائل الموجودة مسبقاً في الجلسة لا تُحقن مجدداً.
- إظهار المرسل: تنتهي كل دفعة مجموعة الآن بـ `[from: Sender Name (+E164)]` كي يعرف Pi من يتحدث.
- المؤقتة/العرض لمرة واحدة: نفك تغليف هذه الرسائل قبل استخراج النص/الإشارات، لذلك تظل التنبيهات داخلها قادرة على التشغيل.
- موجه نظام المجموعة: في أول دور من جلسة مجموعة (وكلما غيّر `/activation` الوضع) نحقن نبذة قصيرة في موجه النظام مثل `You are replying inside the WhatsApp group "<subject>". Group members: Alice (+44...), Bob (+43...), … Activation: trigger-only … Address the specific sender noted in the message context.` إذا لم تكن البيانات الوصفية متاحة، نظل نخبر الوكيل بأنها دردشة مجموعة.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D9%85%D8%AB%D8%A7%D9%84-%D8%AA%D9%83%D9%88%D9%8A%D9%86-whatsapp)  مثال تكوين (WhatsApp)

أضف كتلة `groupChat` إلى `~/.openclaw/openclaw.json` كي تعمل تنبيهات أسماء العرض حتى عندما يزيل WhatsApp الرمز المرئي `@` من متن النص:

```
{
  channels: {
    whatsapp: {
      groups: {
        "*": { requireMention: true },
      },
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: {\
          historyLimit: 50,\
          mentionPatterns: ["@?openclaw", "\\+?15555550123"],\
        },\
      },\
    ],
  },
}
```

ملاحظات:

- التعبيرات المنتظمة غير حساسة لحالة الأحرف وتستخدم ضوابط التعبيرات المنتظمة الآمنة نفسها مثل أسطح تعبيرات التكوين المنتظمة الأخرى؛ يتم تجاهل الأنماط غير الصالحة والتكرار المتداخل غير الآمن.
- ما زال WhatsApp يرسل الإشارات القياسية عبر `mentionedJids` عندما يضغط شخص ما على جهة الاتصال، لذلك نادراً ما تكون الحاجة إلى بديل الرقم، لكنه شبكة أمان مفيدة.

### [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D8%A3%D9%85%D8%B1-%D8%A7%D9%84%D8%AA%D9%81%D8%B9%D9%8A%D9%84-%D9%84%D9%84%D9%85%D8%A7%D9%84%D9%83-%D9%81%D9%82%D8%B7)  أمر التفعيل (للمالك فقط)

استخدم أمر دردشة المجموعة:

- `/activation mention`
- `/activation always`

يمكن فقط لرقم المالك (من `channels.whatsapp.allowFrom`، أو رقم E.164 الخاص بالبوت نفسه عند عدم ضبطه) تغيير هذا. أرسل `/status` كرسالة مستقلة في المجموعة لرؤية وضع التفعيل الحالي.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D9%83%D9%8A%D9%81%D9%8A%D8%A9-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)  كيفية الاستخدام

1. أضف حساب WhatsApp الخاص بك (الحساب الذي يشغّل OpenClaw) إلى المجموعة.
2. قل `@openclaw …` (أو أدرج الرقم). لا يمكن تشغيله إلا للمرسلين الموجودين في قائمة السماح ما لم تضبط `groupPolicy: "open"`.
3. سيتضمن موجه الوكيل سياق المجموعة الحديث بالإضافة إلى علامة `[from: …]` اللاحقة كي يستطيع مخاطبة الشخص الصحيح.
4. تنطبق توجيهات مستوى الجلسة (`/verbose on`، `/trace on`، `/think high`، `/new` أو `/reset`، `/compact`) على جلسة تلك المجموعة فقط؛ أرسلها كرسائل مستقلة كي تُسجّل. تبقى جلسة الرسائل المباشرة الشخصية مستقلة.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-/-%D8%A7%D9%84%D8%AA%D8%AD%D9%82%D9%82)  الاختبار / التحقق

- اختبار يدوي سريع:
  - أرسل تنبيهاً `@openclaw` في المجموعة وتأكد من وجود رد يشير إلى اسم المرسل.
  - أرسل تنبيهاً ثانياً وتحقق من تضمين كتلة السجل ثم مسحها في الدور التالي.
- تحقق من سجلات Gateway (شغّل باستخدام `--verbose`) لرؤية إدخالات `inbound web message` التي تعرض `from: <groupJid>` واللاحقة `[from: …]`.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D8%A7%D8%B9%D8%AA%D8%A8%D8%A7%D8%B1%D8%A7%D8%AA-%D9%85%D8%B9%D8%B1%D9%88%D9%81%D8%A9)  اعتبارات معروفة

- يتم تخطي Heartbeats عمداً للمجموعات لتجنب البث المزعج.
- يستخدم كبت الصدى سلسلة الدفعة المجمعة؛ إذا أرسلت نصاً متطابقاً مرتين دون إشارات، فلن يحصل على استجابة إلا الأول.
- ستظهر إدخالات مخزن الجلسات على شكل `agent:<agentId>:whatsapp:group:<jid>` في مخزن الجلسات (`~/.openclaw/agents/<agentId>/sessions/sessions.json` افتراضياً)؛ يعني غياب الإدخال فقط أن المجموعة لم تشغّل تنفيذاً بعد.
- تتبع مؤشرات الكتابة في المجموعات `agents.defaults.typingMode`. عندما تستخدم الردود المرئية وضع أداة الرسائل فقط الافتراضي، تبدأ الكتابة فوراً افتراضياً كي يرى أعضاء المجموعة أن الوكيل يعمل حتى إذا لم يُنشر رد نهائي تلقائي. يظل تكوين وضع الكتابة الصريح هو الغالب.

## [​](https://docs.openclaw.ai/ar/channels/group-messages\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [المجموعات](https://docs.openclaw.ai/ar/channels/groups)
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing)
- [مجموعات البث](https://docs.openclaw.ai/ar/channels/broadcast-groups)

[مجموعات الوصول](https://docs.openclaw.ai/ar/channels/access-groups) [Groups](https://docs.openclaw.ai/ar/channels/groups)

Ctrl+I