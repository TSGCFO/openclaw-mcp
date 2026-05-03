---
source_url: https://docs.openclaw.ai/ar/channels/nostr
title: "Nostr - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/nostr#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Developer and self-hosted

Nostr

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [Plugin مضمّن](https://docs.openclaw.ai/ar/channels/nostr#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)
- [عمليات التثبيت الأقدم/المخصصة](https://docs.openclaw.ai/ar/channels/nostr#%D8%B9%D9%85%D9%84%D9%8A%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA-%D8%A7%D9%84%D8%A3%D9%82%D8%AF%D9%85%2F%D8%A7%D9%84%D9%85%D8%AE%D8%B5%D8%B5%D8%A9)
- [إعداد غير تفاعلي](https://docs.openclaw.ai/ar/channels/nostr#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%BA%D9%8A%D8%B1-%D8%AA%D9%81%D8%A7%D8%B9%D9%84%D9%8A)
- [إعداد سريع](https://docs.openclaw.ai/ar/channels/nostr#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9)
- [مرجع الإعدادات](https://docs.openclaw.ai/ar/channels/nostr#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [بيانات تعريف الملف الشخصي](https://docs.openclaw.ai/ar/channels/nostr#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%AA%D8%B9%D8%B1%D9%8A%D9%81-%D8%A7%D9%84%D9%85%D9%84%D9%81-%D8%A7%D9%84%D8%B4%D8%AE%D8%B5%D9%8A)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [سياسات DM](https://docs.openclaw.ai/ar/channels/nostr#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A7%D8%AA-dm)
- [مثال على allowlist](https://docs.openclaw.ai/ar/channels/nostr#%D9%85%D8%AB%D8%A7%D9%84-%D8%B9%D9%84%D9%89-allowlist)
- [تنسيقات المفاتيح](https://docs.openclaw.ai/ar/channels/nostr#%D8%AA%D9%86%D8%B3%D9%8A%D9%82%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%81%D8%A7%D8%AA%D9%8A%D8%AD)
- [المرحلات](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D9%84%D9%85%D8%B1%D8%AD%D9%84%D8%A7%D8%AA)
- [دعم البروتوكول](https://docs.openclaw.ai/ar/channels/nostr#%D8%AF%D8%B9%D9%85-%D8%A7%D9%84%D8%A8%D8%B1%D9%88%D8%AA%D9%88%D9%83%D9%88%D9%84)
- [الاختبار](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1)
- [مرحّل محلي](https://docs.openclaw.ai/ar/channels/nostr#%D9%85%D8%B1%D8%AD%D9%91%D9%84-%D9%85%D8%AD%D9%84%D9%8A)
- [اختبار يدوي](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-%D9%8A%D8%AF%D9%88%D9%8A)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [عدم تلقي الرسائل](https://docs.openclaw.ai/ar/channels/nostr#%D8%B9%D8%AF%D9%85-%D8%AA%D9%84%D9%82%D9%8A-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)
- [عدم إرسال الاستجابات](https://docs.openclaw.ai/ar/channels/nostr#%D8%B9%D8%AF%D9%85-%D8%A5%D8%B1%D8%B3%D8%A7%D9%84-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AC%D8%A7%D8%A8%D8%A7%D8%AA)
- [استجابات مكررة](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D8%B3%D8%AA%D8%AC%D8%A7%D8%A8%D8%A7%D8%AA-%D9%85%D9%83%D8%B1%D8%B1%D8%A9)
- [الأمان](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D9%84%D8%A3%D9%85%D8%A7%D9%86)
- [القيود (MVP)](https://docs.openclaw.ai/ar/channels/nostr#%D8%A7%D9%84%D9%82%D9%8A%D9%88%D8%AF-mvp)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/nostr#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**الحالة:** Plugin مضمّن اختياري (معطّل افتراضيًا حتى تتم تهيئته).Nostr هو بروتوكول لامركزي للشبكات الاجتماعية. تتيح هذه القناة لـ OpenClaw تلقي الرسائل المباشرة المشفرة (DMs) والرد عليها عبر NIP-04.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#plugin-%D9%85%D8%B6%D9%85%D9%91%D9%86)  Plugin مضمّن

توفّر إصدارات OpenClaw الحالية Nostr بوصفه Plugin مضمّنًا، لذلك لا تحتاج الإصدارات المعبّأة العادية إلى تثبيت منفصل.

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%B9%D9%85%D9%84%D9%8A%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA-%D8%A7%D9%84%D8%A3%D9%82%D8%AF%D9%85/%D8%A7%D9%84%D9%85%D8%AE%D8%B5%D8%B5%D8%A9)  عمليات التثبيت الأقدم/المخصصة

- ما زال الإعداد الأولي (`openclaw onboard`) و`openclaw channels add` يعرضان Nostr من كتالوج القنوات المشترك.
- إذا كان بناؤك يستبعد Nostr المضمّن، فثبّت حزمة npm حالية عند نشرها.

```
openclaw plugins install @openclaw/nostr
```

إذا أبلغ npm أن الحزمة المملوكة لـ OpenClaw مهملة، فاستخدم بناء OpenClaw معبّأًا حاليًا أو نسخة محلية حتى تُنشر حزمة npm أحدث.استخدم نسخة محلية (تدفقات عمل التطوير):

```
openclaw plugins install --link <path-to-local-nostr-plugin>
```

أعد تشغيل Gateway بعد تثبيت Plugins أو تمكينها.

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%BA%D9%8A%D8%B1-%D8%AA%D9%81%D8%A7%D8%B9%D9%84%D9%8A)  إعداد غير تفاعلي

```
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY" --relay-urls "wss://relay.damus.io,wss://relay.primal.net"
```

استخدم `--use-env` لإبقاء `NOSTR_PRIVATE_KEY` في البيئة بدلًا من تخزين المفتاح في الإعدادات.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%B3%D8%B1%D9%8A%D8%B9)  إعداد سريع

1. أنشئ زوج مفاتيح Nostr (إذا لزم الأمر):

```
# Using nak
nak key generate
```

2. أضفه إلى الإعدادات:

```
{
  channels: {
    nostr: {
      privateKey: "${NOSTR_PRIVATE_KEY}",
    },
  },
}
```

3. صدّر المفتاح:

```
export NOSTR_PRIVATE_KEY="nsec1..."
```

4. أعد تشغيل Gateway.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D9%85%D8%B1%D8%AC%D8%B9-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  مرجع الإعدادات

| المفتاح | النوع | الافتراضي | الوصف |
| --- | --- | --- | --- |
| `privateKey` | string | مطلوب | مفتاح خاص بتنسيق `nsec` أو hex |
| `relays` | string\[\] | `['wss://relay.damus.io', 'wss://nos.lol']` | عناوين ترحيل URL (WebSocket) |
| `dmPolicy` | string | `pairing` | سياسة وصول DM |
| `allowFrom` | string\[\] | `[]` | مفاتيح pubkeys للمرسلين المسموح لهم |
| `enabled` | boolean | `true` | تمكين/تعطيل القناة |
| `name` | string | - | اسم العرض |
| `profile` | object | - | بيانات تعريف ملف NIP-01 الشخصي |

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%AA%D8%B9%D8%B1%D9%8A%D9%81-%D8%A7%D9%84%D9%85%D9%84%D9%81-%D8%A7%D9%84%D8%B4%D8%AE%D8%B5%D9%8A)  بيانات تعريف الملف الشخصي

تُنشر بيانات الملف الشخصي كحدث NIP-01 `kind:0`. يمكنك إدارتها من Control UI (Channels -> Nostr -> Profile) أو ضبطها مباشرة في الإعدادات.مثال:

```
{
  channels: {
    nostr: {
      privateKey: "${NOSTR_PRIVATE_KEY}",
      profile: {
        name: "openclaw",
        displayName: "OpenClaw",
        about: "Personal assistant DM bot",
        picture: "https://example.com/avatar.png",
        banner: "https://example.com/banner.png",
        website: "https://example.com",
        nip05: "openclaw@example.com",
        lud16: "openclaw@example.com",
      },
    },
  },
}
```

ملاحظات:

- يجب أن تستخدم عناوين URL للملف الشخصي `https://`.
- يؤدي الاستيراد من المرحلات إلى دمج الحقول والحفاظ على التجاوزات المحلية.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A7%D8%AA-dm)  سياسات DM

- **pairing** (الافتراضي): يحصل المرسلون غير المعروفين على رمز إقران.
- **allowlist**: يمكن فقط للمفاتيح pubkeys الموجودة في `allowFrom` إرسال DM.
- **open**: رسائل DM واردة عامة (يتطلب `allowFrom: ["*"]`).
- **disabled**: تجاهل رسائل DM الواردة.

ملاحظات الإنفاذ:

- يتم التحقق من توقيعات الأحداث الواردة قبل سياسة المرسل وفك تشفير NIP-04، لذلك تُرفض الأحداث المزوّرة مبكرًا.
- تُرسل ردود الإقران من دون معالجة نص DM الأصلي.
- تُقيّد رسائل DM الواردة بمعدل محدد، وتُسقط الحمولات كبيرة الحجم قبل فك التشفير.

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D9%85%D8%AB%D8%A7%D9%84-%D8%B9%D9%84%D9%89-allowlist)  مثال على allowlist

```
{
  channels: {
    nostr: {
      privateKey: "${NOSTR_PRIVATE_KEY}",
      dmPolicy: "allowlist",
      allowFrom: ["npub1abc...", "npub1xyz..."],
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%AA%D9%86%D8%B3%D9%8A%D9%82%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%81%D8%A7%D8%AA%D9%8A%D8%AD)  تنسيقات المفاتيح

التنسيقات المقبولة:

- **المفتاح الخاص:**`nsec...` أو hex بطول 64 حرفًا
- **Pubkeys (`allowFrom`):**`npub...` أو hex

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D9%84%D9%85%D8%B1%D8%AD%D9%84%D8%A7%D8%AA)  المرحلات

الافتراضيات: `relay.damus.io` و`nos.lol`.

```
{
  channels: {
    nostr: {
      privateKey: "${NOSTR_PRIVATE_KEY}",
      relays: ["wss://relay.damus.io", "wss://relay.primal.net", "wss://nostr.wine"],
    },
  },
}
```

نصائح:

- استخدم 2-3 مرحلات للتكرار الاحتياطي.
- تجنّب استخدام عدد كبير جدًا من المرحلات (زمن الانتقال، التكرار).
- يمكن للمرحلات المدفوعة تحسين الاعتمادية.
- المرحلات المحلية مناسبة للاختبار (`ws://localhost:7777`).

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%AF%D8%B9%D9%85-%D8%A7%D9%84%D8%A8%D8%B1%D9%88%D8%AA%D9%88%D9%83%D9%88%D9%84)  دعم البروتوكول

| NIP | الحالة | الوصف |
| --- | --- | --- |
| NIP-01 | مدعوم | تنسيق الحدث الأساسي \+ بيانات تعريف الملف الشخصي |
| NIP-04 | مدعوم | رسائل DM مشفرة (`kind:4`) |
| NIP-17 | مخطط له | رسائل DM مغلفة كهدية |
| NIP-44 | مخطط له | تشفير بإصدارات |

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1)  الاختبار

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D9%85%D8%B1%D8%AD%D9%91%D9%84-%D9%85%D8%AD%D9%84%D9%8A)  مرحّل محلي

```
# Start strfry
docker run -p 7777:7777 ghcr.io/hoytech/strfry
```

```
{
  channels: {
    nostr: {
      privateKey: "${NOSTR_PRIVATE_KEY}",
      relays: ["ws://localhost:7777"],
    },
  },
}
```

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-%D9%8A%D8%AF%D9%88%D9%8A)  اختبار يدوي

1. دوّن pubkey الخاص بالبوت (npub) من السجلات.
2. افتح عميل Nostr (Damus، Amethyst، إلخ).
3. أرسل DM إلى pubkey الخاص بالبوت.
4. تحقق من الاستجابة.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%B9%D8%AF%D9%85-%D8%AA%D9%84%D9%82%D9%8A-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)  عدم تلقي الرسائل

- تحقق من أن المفتاح الخاص صالح.
- تأكد من أن عناوين URL للمرحلات قابلة للوصول وتستخدم `wss://` (أو `ws://` للمحلي).
- تأكد من أن `enabled` ليست `false`.
- افحص سجلات Gateway بحثًا عن أخطاء الاتصال بالمرحلات.

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%B9%D8%AF%D9%85-%D8%A5%D8%B1%D8%B3%D8%A7%D9%84-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AC%D8%A7%D8%A8%D8%A7%D8%AA)  عدم إرسال الاستجابات

- تحقق من أن المرحّل يقبل عمليات الكتابة.
- تحقق من الاتصال الصادر.
- راقب حدود معدل المرحّل.

### [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D8%B3%D8%AA%D8%AC%D8%A7%D8%A8%D8%A7%D8%AA-%D9%85%D9%83%D8%B1%D8%B1%D8%A9)  استجابات مكررة

- هذا متوقع عند استخدام مرحلات متعددة.
- تُزال الرسائل المكررة حسب معرّف الحدث؛ ولا يؤدي إلا التسليم الأول إلى تشغيل استجابة.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D9%84%D8%A3%D9%85%D8%A7%D9%86)  الأمان

- لا تلتزم أبدًا بالمفاتيح الخاصة.
- استخدم متغيرات البيئة للمفاتيح.
- فكّر في `allowlist` لبوتات الإنتاج.
- يتم التحقق من التوقيعات قبل سياسة المرسل، وتُنفّذ سياسة المرسل قبل فك التشفير، لذلك تُرفض الأحداث المزوّرة مبكرًا ولا يستطيع المرسلون غير المعروفين فرض عمل تشفير كامل.

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%A7%D9%84%D9%82%D9%8A%D9%88%D8%AF-mvp)  القيود (MVP)

- الرسائل المباشرة فقط (لا توجد محادثات جماعية).
- لا توجد مرفقات وسائط.
- NIP-04 فقط (تغليف NIP-17 كهدية مخطط له).

## [​](https://docs.openclaw.ai/ar/channels/nostr\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels) — جميع القنوات المدعومة
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing) — مصادقة DM وتدفق الإقران
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups) — سلوك الدردشة الجماعية وبوابة الإشارات
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) — توجيه الجلسات للرسائل
- [الأمان](https://docs.openclaw.ai/ar/gateway/security) — نموذج الوصول والتقوية

[Nextcloud Talk](https://docs.openclaw.ai/ar/channels/nextcloud-talk) [Tlon](https://docs.openclaw.ai/ar/channels/tlon)

Ctrl+I