---
source_url: https://docs.openclaw.ai/ar/channels/matrix-push-rules
title: "\u0642\u0648\u0627\u0639\u062f \u0627\u0644\u0625\u0634\u0639\u0627\u0631\u0627\u062a \u0627\u0644\u0641\u0648\u0631\u064a\u0629 \u0641\u064a Matrix \u0644\u0644\u0645\u0639\u0627\u064a\u0646\u0627\u062a \u0627\u0644\u0635\u0627\u0645\u062a\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/matrix-push-rules#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Mainstream messaging

قواعد الإشعارات الفورية في Matrix للمعاينات الصامتة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [المتطلبات الأساسية](https://docs.openclaw.ai/ar/channels/matrix-push-rules#%D8%A7%D9%84%D9%85%D8%AA%D8%B7%D9%84%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)
- [الخطوات](https://docs.openclaw.ai/ar/channels/matrix-push-rules#%D8%A7%D9%84%D8%AE%D8%B7%D9%88%D8%A7%D8%AA)
- [ملاحظات تعدد البوتات](https://docs.openclaw.ai/ar/channels/matrix-push-rules#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%AA%D8%B9%D8%AF%D8%AF-%D8%A7%D9%84%D8%A8%D9%88%D8%AA%D8%A7%D8%AA)
- [ملاحظات الخادم المنزلي](https://docs.openclaw.ai/ar/channels/matrix-push-rules#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%AE%D8%A7%D8%AF%D9%85-%D8%A7%D9%84%D9%85%D9%86%D8%B2%D9%84%D9%8A)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/matrix-push-rules#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

عندما تكون `channels.matrix.streaming` هي `"quiet"`، يعدّل OpenClaw حدث معاينة واحدًا في مكانه ويميّز التعديل النهائي بعلامة محتوى مخصصة. لا ترسل عملاء Matrix إشعارًا بشأن التعديل النهائي إلا إذا طابقت قاعدة دفع لكل مستخدم تلك العلامة. هذه الصفحة مخصصة للمشغلين الذين يستضيفون Matrix ذاتيًا ويريدون تثبيت تلك القاعدة لكل حساب مستلِم.إذا كنت تريد سلوك إشعارات Matrix الافتراضي فقط، فاستخدم `streaming: "partial"` أو اترك البث متوقفًا. راجع [إعداد قناة Matrix](https://docs.openclaw.ai/ar/channels/matrix#streaming-previews).

## [​](https://docs.openclaw.ai/ar/channels/matrix-push-rules\#%D8%A7%D9%84%D9%85%D8%AA%D8%B7%D9%84%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)  المتطلبات الأساسية

- المستخدم المستلِم = الشخص الذي يجب أن يتلقى الإشعار
- مستخدم البوت = حساب Matrix الخاص بـ OpenClaw الذي يرسل الرد
- استخدم رمز وصول المستخدم المستلِم لاستدعاءات API أدناه
- طابِق `sender` في قاعدة الدفع مع MXID الكامل لمستخدم البوت
- يجب أن يكون لدى حساب المستلِم دافعات تعمل بالفعل — لا تعمل قواعد المعاينة الهادئة إلا عندما يكون تسليم دفع Matrix العادي سليمًا

## [​](https://docs.openclaw.ai/ar/channels/matrix-push-rules\#%D8%A7%D9%84%D8%AE%D8%B7%D9%88%D8%A7%D8%AA)  الخطوات

1

[Navigate to header](https://docs.openclaw.ai/ar/channels/matrix-push-rules#)

تكوين المعاينات الهادئة

```
{
  channels: {
    matrix: {
      streaming: "quiet",
    },
  },
}
```

2

[Navigate to header](https://docs.openclaw.ai/ar/channels/matrix-push-rules#)

الحصول على رمز وصول المستلِم

أعد استخدام رمز جلسة عميل موجود عندما يكون ذلك ممكنًا. لإنشاء رمز جديد:

```
curl -sS -X POST \
  "https://matrix.example.org/_matrix/client/v3/login" \
  -H "Content-Type: application/json" \
  --data '{
    "type": "m.login.password",
    "identifier": { "type": "m.id.user", "user": "@alice:example.org" },
    "password": "REDACTED"
  }'
```

3

[Navigate to header](https://docs.openclaw.ai/ar/channels/matrix-push-rules#)

التحقق من وجود الدافعات

```
curl -sS \
  -H "Authorization: Bearer $USER_ACCESS_TOKEN" \
  "https://matrix.example.org/_matrix/client/v3/pushers"
```

إذا لم تعد أي دافعات، فأصلح تسليم دفع Matrix العادي لهذا الحساب قبل المتابعة.

4

[Navigate to header](https://docs.openclaw.ai/ar/channels/matrix-push-rules#)

تثبيت قاعدة الدفع التجاوزية

يميّز OpenClaw تعديلات معاينة النص فقط النهائية باستخدام `content["com.openclaw.finalized_preview"] = true`. ثبّت قاعدة تطابق تلك العلامة بالإضافة إلى MXID الخاص بالبوت كمرسل:

```
curl -sS -X PUT \
  "https://matrix.example.org/_matrix/client/v3/pushrules/global/override/openclaw-finalized-preview-botname" \
  -H "Authorization: Bearer $USER_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  --data '{
    "conditions": [\
      { "kind": "event_match", "key": "type", "pattern": "m.room.message" },\
      {\
        "kind": "event_property_is",\
        "key": "content.m\\.relates_to.rel_type",\
        "value": "m.replace"\
      },\
      {\
        "kind": "event_property_is",\
        "key": "content.com\\.openclaw\\.finalized_preview",\
        "value": true\
      },\
      { "kind": "event_match", "key": "sender", "pattern": "@bot:example.org" }\
    ],
    "actions": [\
      "notify",\
      { "set_tweak": "sound", "value": "default" },\
      { "set_tweak": "highlight", "value": false }\
    ]
  }'
```

استبدل قبل التشغيل:

- `https://matrix.example.org`: عنوان URL الأساسي للخادم المنزلي لديك
- `$USER_ACCESS_TOKEN`: رمز وصول المستخدم المستلِم
- `openclaw-finalized-preview-botname`: معرّف قاعدة فريد لكل بوت ولكل مستلِم (النمط: `openclaw-finalized-preview-<botname>`)
- `@bot:example.org`: MXID بوت OpenClaw لديك، وليس MXID المستلِم

5

[Navigate to header](https://docs.openclaw.ai/ar/channels/matrix-push-rules#)

التحقق

```
curl -sS \
  -H "Authorization: Bearer $USER_ACCESS_TOKEN" \
  "https://matrix.example.org/_matrix/client/v3/pushrules/global/override/openclaw-finalized-preview-botname"
```

بعد ذلك اختبر ردًا مبثوثًا. في الوضع الهادئ، تعرض الغرفة معاينة مسودة هادئة وترسل إشعارًا مرة واحدة عند انتهاء الكتلة أو الدور.

لإزالة القاعدة لاحقًا، نفّذ `DELETE` على عنوان URL نفسه للقاعدة باستخدام رمز المستلِم.

## [​](https://docs.openclaw.ai/ar/channels/matrix-push-rules\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%AA%D8%B9%D8%AF%D8%AF-%D8%A7%D9%84%D8%A8%D9%88%D8%AA%D8%A7%D8%AA)  ملاحظات تعدد البوتات

تُفهرس قواعد الدفع بواسطة `ruleId`: إعادة تشغيل `PUT` على المعرّف نفسه تحدّث قاعدة واحدة. لعدة بوتات OpenClaw ترسل إشعارات إلى المستلِم نفسه، أنشئ قاعدة واحدة لكل بوت مع مطابقة مرسل مميزة.تُدرج قواعد `override` الجديدة المعرّفة من المستخدم قبل قواعد الكبت الافتراضية، لذلك لا حاجة إلى معامل ترتيب إضافي. تؤثر القاعدة فقط في تعديلات معاينة النص فقط التي يمكن إنهاؤها في مكانها؛ أما بدائل الوسائط وبدائل المعاينات القديمة فتستخدم تسليم Matrix العادي.

## [​](https://docs.openclaw.ai/ar/channels/matrix-push-rules\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D8%AE%D8%A7%D8%AF%D9%85-%D8%A7%D9%84%D9%85%D9%86%D8%B2%D9%84%D9%8A)  ملاحظات الخادم المنزلي

Synapse

لا يلزم أي تغيير خاص في `homeserver.yaml`. إذا كانت إشعارات Matrix العادية تصل بالفعل إلى هذا المستخدم، فإن رمز المستلِم + استدعاء `pushrules` أعلاه هو خطوة الإعداد الرئيسية.إذا كنت تشغّل Synapse خلف وكيل عكسي أو عمال، فتأكد من أن `/_matrix/client/.../pushrules/` يصل إلى Synapse بشكل صحيح. يتولى العملية الرئيسية أو `synapse.app.pusher` / عمال الدفع المكوّنون تسليم الدفع — تأكد من أنها سليمة.تستخدم القاعدة شرط قاعدة الدفع `event_property_is` ‏(MSC3758، قاعدة دفع v1.10)، الذي أُضيف إلى Synapse في 2023. تقبل إصدارات Synapse الأقدم استدعاء `PUT pushrules/...` لكنها لا تطابق الشرط بصمت مطلقًا — حدّث Synapse إذا لم يصل أي إشعار عند تعديل معاينة نهائي.

Tuwunel

التدفق نفسه كما في Synapse؛ لا يلزم أي تكوين خاص بـ Tuwunel لعلامة المعاينة النهائية.إذا اختفت الإشعارات أثناء نشاط المستخدم على جهاز آخر، فتحقق مما إذا كان `suppress_push_when_active` مفعّلًا. أضاف Tuwunel هذا الخيار في 1.4.2 (سبتمبر 2025)، ويمكنه كبت عمليات الدفع عمدًا إلى الأجهزة الأخرى أثناء نشاط أحد الأجهزة.

## [​](https://docs.openclaw.ai/ar/channels/matrix-push-rules\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [إعداد قناة Matrix](https://docs.openclaw.ai/ar/channels/matrix)
- [مفاهيم البث](https://docs.openclaw.ai/ar/concepts/streaming)

[ترحيل Matrix](https://docs.openclaw.ai/ar/channels/matrix-migration) [IRC](https://docs.openclaw.ai/ar/channels/irc)

Ctrl+I