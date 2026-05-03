---
source_url: https://docs.openclaw.ai/ar/channels/feishu
title: "Feishu - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/feishu#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Regional platforms

Feishu

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [فيشو / لارك](https://docs.openclaw.ai/ar/channels/feishu#%D9%81%D9%8A%D8%B4%D9%88-%2F-%D9%84%D8%A7%D8%B1%D9%83)
- [البدء السريع](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [الرسائل المباشرة](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9)
- [دردشات المجموعات](https://docs.openclaw.ai/ar/channels/feishu#%D8%AF%D8%B1%D8%AF%D8%B4%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)
- [أمثلة تكوين المجموعات](https://docs.openclaw.ai/ar/channels/feishu#%D8%A3%D9%85%D8%AB%D9%84%D8%A9-%D8%AA%D9%83%D9%88%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)
- [السماح لكل المجموعات، من دون طلب @mention](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D9%84%D9%83%D9%84-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA%D8%8C-%D9%85%D9%86-%D8%AF%D9%88%D9%86-%D8%B7%D9%84%D8%A8-%40mention)
- [السماح لكل المجموعات، مع استمرار طلب @mention](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D9%84%D9%83%D9%84-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA%D8%8C-%D9%85%D8%B9-%D8%A7%D8%B3%D8%AA%D9%85%D8%B1%D8%A7%D8%B1-%D8%B7%D9%84%D8%A8-%40mention)
- [السماح بمجموعات محددة فقط](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D8%A8%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D9%85%D8%AD%D8%AF%D8%AF%D8%A9-%D9%81%D9%82%D8%B7)
- [تقييد المرسلين داخل مجموعة](https://docs.openclaw.ai/ar/channels/feishu#%D8%AA%D9%82%D9%8A%D9%8A%D8%AF-%D8%A7%D9%84%D9%85%D8%B1%D8%B3%D9%84%D9%8A%D9%86-%D8%AF%D8%A7%D8%AE%D9%84-%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A9)
- [الحصول على معرّفات المجموعة/المستخدم](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%AD%D8%B5%D9%88%D9%84-%D8%B9%D9%84%D9%89-%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A9%2F%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D8%AE%D8%AF%D9%85)
- [معرّفات المجموعات (chat\_id، الصيغة: oc\_xxx)](https://docs.openclaw.ai/ar/channels/feishu#%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-chat_id%D8%8C-%D8%A7%D9%84%D8%B5%D9%8A%D8%BA%D8%A9-oc_xxx)
- [معرّفات المستخدمين (open\_id، الصيغة: ou\_xxx)](https://docs.openclaw.ai/ar/channels/feishu#%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D8%AE%D8%AF%D9%85%D9%8A%D9%86-open_id%D8%8C-%D8%A7%D9%84%D8%B5%D9%8A%D8%BA%D8%A9-ou_xxx)
- [الأوامر الشائعة](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%B4%D8%A7%D8%A6%D8%B9%D8%A9)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [البوت لا يرد في دردشات المجموعات](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A8%D9%88%D8%AA-%D9%84%D8%A7-%D9%8A%D8%B1%D8%AF-%D9%81%D9%8A-%D8%AF%D8%B1%D8%AF%D8%B4%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)
- [البوت لا يتلقى الرسائل](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A8%D9%88%D8%AA-%D9%84%D8%A7-%D9%8A%D8%AA%D9%84%D9%82%D9%89-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)
- [تسرب App Secret](https://docs.openclaw.ai/ar/channels/feishu#%D8%AA%D8%B3%D8%B1%D8%A8-app-secret)
- [التكوين المتقدم](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D8%AA%D9%82%D8%AF%D9%85)
- [حسابات متعددة](https://docs.openclaw.ai/ar/channels/feishu#%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)
- [حدود الرسائل](https://docs.openclaw.ai/ar/channels/feishu#%D8%AD%D8%AF%D9%88%D8%AF-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)
- [البث](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A8%D8%AB)
- [تحسين الحصة](https://docs.openclaw.ai/ar/channels/feishu#%D8%AA%D8%AD%D8%B3%D9%8A%D9%86-%D8%A7%D9%84%D8%AD%D8%B5%D8%A9)
- [جلسات ACP](https://docs.openclaw.ai/ar/channels/feishu#%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-acp)
- [ربط ACP مستمر](https://docs.openclaw.ai/ar/channels/feishu#%D8%B1%D8%A8%D8%B7-acp-%D9%85%D8%B3%D8%AA%D9%85%D8%B1)
- [إنشاء ACP من الدردشة](https://docs.openclaw.ai/ar/channels/feishu#%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-acp-%D9%85%D9%86-%D8%A7%D9%84%D8%AF%D8%B1%D8%AF%D8%B4%D8%A9)
- [توجيه عدة وكلاء](https://docs.openclaw.ai/ar/channels/feishu#%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D8%B9%D8%AF%D8%A9-%D9%88%D9%83%D9%84%D8%A7%D8%A1)
- [مرجع التكوين](https://docs.openclaw.ai/ar/channels/feishu#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86)
- [أنواع الرسائل المدعومة](https://docs.openclaw.ai/ar/channels/feishu#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%AF%D8%B9%D9%88%D9%85%D8%A9)
- [الاستقبال](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D9%82%D8%A8%D8%A7%D9%84)
- [الإرسال](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%A5%D8%B1%D8%B3%D8%A7%D9%84)
- [السلاسل والردود](https://docs.openclaw.ai/ar/channels/feishu#%D8%A7%D9%84%D8%B3%D9%84%D8%A7%D8%B3%D9%84-%D9%88%D8%A7%D9%84%D8%B1%D8%AF%D9%88%D8%AF)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/feishu#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ar/channels/feishu\#%D9%81%D9%8A%D8%B4%D9%88-/-%D9%84%D8%A7%D8%B1%D9%83)  فيشو / لارك

Feishu/Lark هي منصة تعاون شاملة تتيح للفرق الدردشة ومشاركة المستندات وإدارة التقويمات وإنجاز العمل معا.**الحالة:** جاهز للإنتاج للرسائل المباشرة مع البوت ودردشات المجموعات. WebSocket هو الوضع الافتراضي؛ ووضع webhook اختياري.

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  البدء السريع

يتطلب OpenClaw 2026.4.25 أو أحدث. شغل `openclaw --version` للتحقق. حدّث باستخدام `openclaw update`.

1

[Navigate to header](https://docs.openclaw.ai/ar/channels/feishu#)

شغّل معالج إعداد القناة

```
openclaw channels login --channel feishu
```

امسح رمز QR باستخدام تطبيق Feishu/Lark على هاتفك لإنشاء بوت Feishu/Lark تلقائيا.

2

[Navigate to header](https://docs.openclaw.ai/ar/channels/feishu#)

بعد اكتمال الإعداد، أعد تشغيل Gateway لتطبيق التغييرات

```
openclaw gateway restart
```

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1%D8%A9)  الرسائل المباشرة

اضبط `dmPolicy` للتحكم في من يمكنه إرسال رسالة مباشرة إلى البوت:

- `"pairing"` — يتلقى المستخدمون غير المعروفين رمز اقتران؛ وافق عليه عبر CLI
- `"allowlist"` — لا يمكن الدردشة إلا للمستخدمين المدرجين في `allowFrom` (الافتراضي: مالك البوت فقط)
- `"open"` — السماح بالرسائل المباشرة العامة فقط عندما يتضمن `allowFrom` القيمة `"*"`؛ ومع الإدخالات المقيّدة، لا يمكن الدردشة إلا للمستخدمين المطابقين
- `"disabled"` — تعطيل كل الرسائل المباشرة

**الموافقة على طلب اقتران:**

```
openclaw pairing list feishu
openclaw pairing approve feishu <CODE>
```

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AF%D8%B1%D8%AF%D8%B4%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)  دردشات المجموعات

**سياسة المجموعة** (`channels.feishu.groupPolicy`):

| القيمة | السلوك |
| --- | --- |
| `"open"` | الرد على كل الرسائل في المجموعات |
| `"allowlist"` | الرد فقط على المجموعات في `groupAllowFrom` أو المكوّنة صراحة ضمن `groups.<chat_id>` |
| `"disabled"` | تعطيل كل رسائل المجموعات؛ لا تتجاوز إدخالات `groups.<chat_id>` الصريحة ذلك |

الافتراضي: `allowlist`**متطلب الإشارة** (`channels.feishu.requireMention`):

- `true` — يتطلب @mention (الافتراضي)
- `false` — الرد من دون @mention
- تجاوز لكل مجموعة: `channels.feishu.groups.<chat_id>.requireMention`
- لا تُعامل إشارات البث فقط `@all` و`@_all` كإشارات إلى البوت. الرسالة التي تشير إلى كل من `@all` والبوت مباشرة ما زالت تُحتسب كإشارة إلى البوت.

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A3%D9%85%D8%AB%D9%84%D8%A9-%D8%AA%D9%83%D9%88%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)  أمثلة تكوين المجموعات

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D9%84%D9%83%D9%84-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA%D8%8C-%D9%85%D9%86-%D8%AF%D9%88%D9%86-%D8%B7%D9%84%D8%A8-@mention)  السماح لكل المجموعات، من دون طلب @mention

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D9%84%D9%83%D9%84-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA%D8%8C-%D9%85%D8%B9-%D8%A7%D8%B3%D8%AA%D9%85%D8%B1%D8%A7%D8%B1-%D8%B7%D9%84%D8%A8-@mention)  السماح لكل المجموعات، مع استمرار طلب @mention

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
      requireMention: true,
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%B3%D9%85%D8%A7%D8%AD-%D8%A8%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-%D9%85%D8%AD%D8%AF%D8%AF%D8%A9-%D9%81%D9%82%D8%B7)  السماح بمجموعات محددة فقط

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      // Group IDs look like: oc_xxx
      groupAllowFrom: ["oc_xxx", "oc_yyy"],
    },
  },
}
```

في وضع `allowlist`، يمكنك أيضا قبول مجموعة بإضافة إدخال `groups.<chat_id>` صريح. لا تتجاوز الإدخالات الصريحة `groupPolicy: "disabled"`. تضبط الإعدادات الافتراضية ذات أحرف البدل ضمن `groups.*` المجموعات المطابقة، لكنها لا تقبل المجموعات بذاتها.

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groups: {
        oc_xxx: {
          requireMention: false,
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AA%D9%82%D9%8A%D9%8A%D8%AF-%D8%A7%D9%84%D9%85%D8%B1%D8%B3%D9%84%D9%8A%D9%86-%D8%AF%D8%A7%D8%AE%D9%84-%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A9)  تقييد المرسلين داخل مجموعة

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["oc_xxx"],
      groups: {
        oc_xxx: {
          // User open_ids look like: ou_xxx
          allowFrom: ["ou_user1", "ou_user2"],
        },
      },
    },
  },
}
```

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%AD%D8%B5%D9%88%D9%84-%D8%B9%D9%84%D9%89-%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A9/%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D8%AE%D8%AF%D9%85)  الحصول على معرّفات المجموعة/المستخدم

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA-chat_id%D8%8C-%D8%A7%D9%84%D8%B5%D9%8A%D8%BA%D8%A9-oc_xxx)  معرّفات المجموعات (`chat_id`، الصيغة: `oc_xxx`)

افتح المجموعة في Feishu/Lark، وانقر أيقونة القائمة في الزاوية العلوية اليمنى، وانتقل إلى **الإعدادات**. يكون معرّف المجموعة (`chat_id`) مدرجا في صفحة الإعدادات.![الحصول على معرّف المجموعة](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=1c9b41e1f9743621dfdd3abf7e952405)

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D9%85%D8%B9%D8%B1%D9%91%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D8%AE%D8%AF%D9%85%D9%8A%D9%86-open_id%D8%8C-%D8%A7%D9%84%D8%B5%D9%8A%D8%BA%D8%A9-ou_xxx)  معرّفات المستخدمين (`open_id`، الصيغة: `ou_xxx`)

ابدأ تشغيل Gateway، وأرسل رسالة مباشرة إلى البوت، ثم تحقق من السجلات:

```
openclaw logs --follow
```

ابحث عن `open_id` في مخرجات السجل. يمكنك أيضا التحقق من طلبات الاقتران المعلّقة:

```
openclaw pairing list feishu
```

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%B4%D8%A7%D8%A6%D8%B9%D8%A9)  الأوامر الشائعة

| الأمر | الوصف |
| --- | --- |
| `/status` | عرض حالة البوت |
| `/reset` | إعادة تعيين الجلسة الحالية |
| `/model` | عرض نموذج الذكاء الاصطناعي أو تبديله |

لا يدعم Feishu/Lark قوائم أوامر الشرطة المائلة الأصلية، لذا أرسل هذه الأوامر كرسائل نصية عادية.

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A8%D9%88%D8%AA-%D9%84%D8%A7-%D9%8A%D8%B1%D8%AF-%D9%81%D9%8A-%D8%AF%D8%B1%D8%AF%D8%B4%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AC%D9%85%D9%88%D8%B9%D8%A7%D8%AA)  البوت لا يرد في دردشات المجموعات

1. تأكد من إضافة البوت إلى المجموعة
2. تأكد من استخدام @mention للإشارة إلى البوت (مطلوب افتراضيا)
3. تحقق من أن `groupPolicy` ليست `"disabled"`
4. تحقق من السجلات: `openclaw logs --follow`

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A8%D9%88%D8%AA-%D9%84%D8%A7-%D9%8A%D8%AA%D9%84%D9%82%D9%89-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)  البوت لا يتلقى الرسائل

1. تأكد من نشر البوت والموافقة عليه في Feishu Open Platform / Lark Developer
2. تأكد من أن اشتراك الأحداث يتضمن `im.message.receive_v1`
3. تأكد من تحديد **الاتصال المستمر** (WebSocket)
4. تأكد من منح كل نطاقات الأذونات المطلوبة
5. تأكد من أن Gateway قيد التشغيل: `openclaw gateway status`
6. تحقق من السجلات: `openclaw logs --follow`

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AA%D8%B3%D8%B1%D8%A8-app-secret)  تسرب App Secret

1. أعد تعيين App Secret في Feishu Open Platform / Lark Developer
2. حدّث القيمة في التكوين لديك
3. أعد تشغيل Gateway: `openclaw gateway restart`

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D8%AA%D9%82%D8%AF%D9%85)  التكوين المتقدم

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AD%D8%B3%D8%A7%D8%A8%D8%A7%D8%AA-%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9)  حسابات متعددة

```
{
  channels: {
    feishu: {
      defaultAccount: "main",
      accounts: {
        main: {
          appId: "cli_xxx",
          appSecret: "xxx",
          name: "Primary bot",
          tts: {
            providers: {
              openai: { voice: "shimmer" },
            },
          },
        },
        backup: {
          appId: "cli_yyy",
          appSecret: "yyy",
          name: "Backup bot",
          enabled: false,
        },
      },
    },
  },
}
```

يتحكم `defaultAccount` في الحساب المستخدم عندما لا تحدد واجهات API الصادرة `accountId`.
يستخدم `accounts.<id>.tts` الشكل نفسه مثل `messages.tts` ويدمج بعمق فوق
تكوين TTS العام، بحيث يمكن لإعدادات Feishu متعددة البوتات إبقاء بيانات اعتماد
الموفر المشتركة عامة مع تجاوز الصوت أو النموذج أو الشخصية أو الوضع التلقائي فقط
لكل حساب.

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AD%D8%AF%D9%88%D8%AF-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)  حدود الرسائل

- `textChunkLimit` — حجم مقطع النص الصادر (الافتراضي: `2000` حرف)
- `mediaMaxMb` — حد رفع/تنزيل الوسائط (الافتراضي: `30` ميغابايت)

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A8%D8%AB)  البث

يدعم Feishu/Lark الردود المتدفقة عبر البطاقات التفاعلية. عند التمكين، يحدّث البوت البطاقة في الوقت الفعلي أثناء توليد النص.

```
{
  channels: {
    feishu: {
      streaming: true, // enable streaming card output (default: true)
      blockStreaming: true, // enable block-level streaming (default: true)
    },
  },
}
```

اضبط `streaming: false` لإرسال الرد الكامل في رسالة واحدة.

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AA%D8%AD%D8%B3%D9%8A%D9%86-%D8%A7%D9%84%D8%AD%D8%B5%D8%A9)  تحسين الحصة

قلل عدد استدعاءات Feishu/Lark API باستخدام علامتين اختياريتين:

- `typingIndicator` (الافتراضي `true`): اضبطه على `false` لتخطي استدعاءات تفاعل الكتابة
- `resolveSenderNames` (الافتراضي `true`): اضبطه على `false` لتخطي عمليات البحث عن ملف المرسل الشخصي

```
{
  channels: {
    feishu: {
      typingIndicator: false,
      resolveSenderNames: false,
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-acp)  جلسات ACP

يدعم Feishu/Lark ‏ACP للرسائل المباشرة ورسائل سلاسل المجموعات. يكون ACP في Feishu/Lark قائما على الأوامر النصية — لا توجد قوائم أوامر شرطة مائلة أصلية، لذا استخدم رسائل `/acp ...` مباشرة في المحادثة.

#### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%B1%D8%A8%D8%B7-acp-%D9%85%D8%B3%D8%AA%D9%85%D8%B1)  ربط ACP مستمر

```
{
  agents: {
    list: [\
      {\
        id: "codex",\
        runtime: {\
          type: "acp",\
          acp: {\
            agent: "codex",\
            backend: "acpx",\
            mode: "persistent",\
            cwd: "/workspace/openclaw",\
          },\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "direct", id: "ou_1234567890" },\
      },\
    },\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "group", id: "oc_group_chat:topic:om_topic_root" },\
      },\
      acp: { label: "codex-feishu-topic" },\
    },\
  ],
}
```

#### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A5%D9%86%D8%B4%D8%A7%D8%A1-acp-%D9%85%D9%86-%D8%A7%D9%84%D8%AF%D8%B1%D8%AF%D8%B4%D8%A9)  إنشاء ACP من الدردشة

في رسالة مباشرة أو سلسلة Feishu/Lark:

```
/acp spawn codex --thread here
```

تعمل `--thread here` للرسائل المباشرة ورسائل سلاسل Feishu/Lark. يتم توجيه رسائل المتابعة في المحادثة المرتبطة مباشرة إلى جلسة ACP تلك.

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D8%B9%D8%AF%D8%A9-%D9%88%D9%83%D9%84%D8%A7%D8%A1)  توجيه عدة وكلاء

استخدم `bindings` لتوجيه رسائل Feishu/Lark المباشرة أو المجموعات إلى وكلاء مختلفين.

```
{
  agents: {
    list: [\
      { id: "main" },\
      { id: "agent-a", workspace: "/home/user/agent-a" },\
      { id: "agent-b", workspace: "/home/user/agent-b" },\
    ],
  },
  bindings: [\
    {\
      agentId: "agent-a",\
      match: {\
        channel: "feishu",\
        peer: { kind: "direct", id: "ou_xxx" },\
      },\
    },\
    {\
      agentId: "agent-b",\
      match: {\
        channel: "feishu",\
        peer: { kind: "group", id: "oc_zzz" },\
      },\
    },\
  ],
}
```

حقول التوجيه:

- `match.channel`: `"feishu"`
- `match.peer.kind`: `"direct"` (رسالة مباشرة) أو `"group"` (دردشة مجموعة)
- `match.peer.id`: معرّف Open ID للمستخدم (`ou_xxx`) أو معرّف المجموعة (`oc_xxx`)

راجع [الحصول على معرّفات المجموعة/المستخدم](https://docs.openclaw.ai/ar/channels/feishu#get-groupuser-ids) للحصول على نصائح البحث.

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86)  مرجع التكوين

التكوين الكامل: [تكوين Gateway](https://docs.openclaw.ai/ar/gateway/configuration)

| الإعداد | الوصف | الافتراضي |
| --- | --- | --- |
| `channels.feishu.enabled` | تفعيل/تعطيل القناة | `true` |
| `channels.feishu.domain` | نطاق API (`feishu` أو `lark`) | `feishu` |
| `channels.feishu.connectionMode` | نقل الأحداث (`websocket` أو `webhook`) | `websocket` |
| `channels.feishu.defaultAccount` | الحساب الافتراضي للتوجيه الصادر | `default` |
| `channels.feishu.verificationToken` | مطلوب لوضع Webhook | — |
| `channels.feishu.encryptKey` | مطلوب لوضع Webhook | — |
| `channels.feishu.webhookPath` | مسار توجيه Webhook | `/feishu/events` |
| `channels.feishu.webhookHost` | مضيف ربط Webhook | `127.0.0.1` |
| `channels.feishu.webhookPort` | منفذ ربط Webhook | `3000` |
| `channels.feishu.accounts.<id>.appId` | معرّف التطبيق | — |
| `channels.feishu.accounts.<id>.appSecret` | سر التطبيق | — |
| `channels.feishu.accounts.<id>.domain` | تجاوز النطاق لكل حساب | `feishu` |
| `channels.feishu.accounts.<id>.tts` | تجاوز TTS لكل حساب | `messages.tts` |
| `channels.feishu.dmPolicy` | سياسة الرسائل المباشرة | `allowlist` |
| `channels.feishu.allowFrom` | قائمة السماح للرسائل المباشرة (قائمة open\_id) | \[BotOwnerId\] |
| `channels.feishu.groupPolicy` | سياسة المجموعة | `allowlist` |
| `channels.feishu.groupAllowFrom` | قائمة السماح للمجموعات | — |
| `channels.feishu.requireMention` | اشتراط @mention في المجموعات | `true` |
| `channels.feishu.groups.<chat_id>.requireMention` | تجاوز @mention لكل مجموعة؛ تقبل المعرّفات الصريحة المجموعة أيضا في وضع قائمة السماح | موروث |
| `channels.feishu.groups.<chat_id>.enabled` | تفعيل/تعطيل مجموعة محددة | `true` |
| `channels.feishu.textChunkLimit` | حجم مقطع الرسالة | `2000` |
| `channels.feishu.mediaMaxMb` | حد حجم الوسائط | `30` |
| `channels.feishu.streaming` | إخراج البطاقات المتدفقة | `true` |
| `channels.feishu.blockStreaming` | التدفق على مستوى الكتلة | `true` |
| `channels.feishu.typingIndicator` | إرسال تفاعلات الكتابة | `true` |
| `channels.feishu.resolveSenderNames` | حل أسماء عرض المرسلين | `true` |

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D9%85%D8%AF%D8%B9%D9%88%D9%85%D8%A9)  أنواع الرسائل المدعومة

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D9%82%D8%A8%D8%A7%D9%84)  الاستقبال

- ✅ نص
- ✅ نص منسق (منشور)
- ✅ صور
- ✅ ملفات
- ✅ صوت
- ✅ فيديو/وسائط
- ✅ ملصقات

تُطبَّع رسائل Feishu/Lark الصوتية الواردة كعناصر نائبة للوسائط بدلا من
ملف JSON خام يحتوي على `file_key`. عند تكوين `tools.media.audio`، يقوم OpenClaw
بتنزيل مورد الملاحظة الصوتية وتشغيل النسخ الصوتي المشترك قبل دورة الوكيل، بحيث
يتلقى الوكيل نص الكلام المنسوخ. إذا ضمّن Feishu نص النسخ مباشرة في حمولة الصوت،
فسيُستخدم ذلك النص دون استدعاء ASR آخر. من دون مزود نسخ صوتي، سيظل الوكيل يتلقى
عنصرًا نائبًا `<media:audio>` مع المرفق المحفوظ، وليس حمولة مورد Feishu الخام.

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%A5%D8%B1%D8%B3%D8%A7%D9%84)  الإرسال

- ✅ نص
- ✅ صور
- ✅ ملفات
- ✅ صوت
- ✅ فيديو/وسائط
- ✅ بطاقات تفاعلية (بما في ذلك تحديثات التدفق)
- ⚠️ نص منسق (تنسيق بأسلوب المنشورات؛ لا يدعم كامل إمكانات التأليف في Feishu/Lark)

تستخدم فقاعات Feishu/Lark الصوتية الأصلية نوع رسالة Feishu `audio` وتتطلب
وسائط رفع Ogg/Opus (`file_type: "opus"`). تُرسل وسائط `.opus` و`.ogg` الموجودة
مباشرة كصوت أصلي. تُحوَّل MP3/WAV/M4A وغيرها من تنسيقات الصوت المحتملة إلى
Ogg/Opus بتردد 48kHz باستخدام `ffmpeg` فقط عندما يطلب الرد التسليم الصوتي
(`audioAsVoice` / أداة الرسائل `asVoice`، بما في ذلك ردود الملاحظات الصوتية
عبر TTS). تبقى مرفقات MP3 العادية ملفات عادية. إذا كان `ffmpeg` مفقودا أو
فشل التحويل، يعود OpenClaw إلى مرفق ملف ويسجل السبب.

### [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%A7%D9%84%D8%B3%D9%84%D8%A7%D8%B3%D9%84-%D9%88%D8%A7%D9%84%D8%B1%D8%AF%D9%88%D8%AF)  السلاسل والردود

- ✅ ردود مضمنة
- ✅ ردود ضمن السلاسل
- ✅ تظل ردود الوسائط واعية بالسلسلة عند الرد على رسالة ضمن سلسلة

بالنسبة إلى `groupSessionScope: "group_topic"` و`"group_topic_sender"`، تستخدم
مجموعات المواضيع الأصلية في Feishu/Lark قيمة الحدث `thread_id` (`omt_*`) كمفتاح
جلسة الموضوع الأساسي. أما ردود المجموعات العادية التي يحولها OpenClaw إلى سلاسل
فتستمر في استخدام معرّف رسالة جذر الرد (`om_*`) بحيث تبقى الدورة الأولى ودورة
المتابعة في الجلسة نفسها.

* * *

## [​](https://docs.openclaw.ai/ar/channels/feishu\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — جميع القنوات المدعومة
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة الرسائل المباشرة وتدفق الإقران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك محادثة المجموعة وبوابة الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[بوت QQ](https://docs.openclaw.ai/ar/channels/qqbot) [يوانباو](https://docs.openclaw.ai/ar/channels/yuanbao)

Ctrl+I

![الحصول على معرّف المجموعة](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?w=1100&fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=36df634e2caf2690c29c722f5068b77b)