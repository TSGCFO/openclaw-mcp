---
source_url: https://docs.openclaw.ai/ar/channels/qa-channel
title: "\u0642\u0646\u0627\u0629 \u0636\u0645\u0627\u0646 \u0627\u0644\u062c\u0648\u062f\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/qa-channel#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

قناة ضمان الجودة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [ما الذي يفعله](https://docs.openclaw.ai/ar/channels/qa-channel#%D9%85%D8%A7-%D8%A7%D9%84%D8%B0%D9%8A-%D9%8A%D9%81%D8%B9%D9%84%D9%87)
- [الإعدادات](https://docs.openclaw.ai/ar/channels/qa-channel#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [المشغّلات](https://docs.openclaw.ai/ar/channels/qa-channel#%D8%A7%D9%84%D9%85%D8%B4%D8%BA%D9%91%D9%84%D8%A7%D8%AA)
- [ذات صلة](https://docs.openclaw.ai/ar/channels/qa-channel#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`qa-channel` هو ناقل رسائل اصطناعي مضمّن مخصص لضمان جودة OpenClaw الآلي. وهو ليس قناة إنتاجية — بل موجود لاختبار حدود Plugin القناة نفسها التي تستخدمها نواقل النقل الحقيقية، مع إبقاء الحالة حتمية وقابلة للفحص بالكامل.

## [​](https://docs.openclaw.ai/ar/channels/qa-channel\#%D9%85%D8%A7-%D8%A7%D9%84%D8%B0%D9%8A-%D9%8A%D9%81%D8%B9%D9%84%D9%87)  ما الذي يفعله

- قواعد أهداف من فئة Slack:
  - `dm:<user>`
  - `channel:<room>`
  - `group:<room>`
  - `thread:<room>/<thread>`
- تُعرَض محادثات `channel:` و`group:` المشتركة للوكلاء كدورات غرف مجموعة/قناة، بحيث تختبر سياسة التوجيه نفسها للردود المرئية وأداة الرسائل المستخدمة بواسطة Discord وSlack وTelegram ونواقل النقل المشابهة.
- ناقل اصطناعي مدعوم عبر HTTP لحقن الرسائل الواردة، والتقاط النصوص الصادرة، وإنشاء سلاسل النقاش، والتفاعلات، والتعديلات، والحذف، وإجراءات البحث/القراءة.
- مشغّل فحص ذاتي على جانب المضيف يكتب تقرير Markdown إلى `.artifacts/qa-e2e/`.

## [​](https://docs.openclaw.ai/ar/channels/qa-channel\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  الإعدادات

```
{
  "channels": {
    "qa-channel": {
      "baseUrl": "http://127.0.0.1:43123",
      "botUserId": "openclaw",
      "botDisplayName": "OpenClaw QA",
      "allowFrom": ["*"],
      "pollTimeoutMs": 1000
    }
  }
}
```

مفاتيح الحساب:

- `enabled` — مفتاح التفعيل الرئيسي لهذا الحساب.
- `name` — تسمية عرض اختيارية.
- `baseUrl` — عنوان URL للناقل الاصطناعي.
- `botUserId` — معرّف مستخدم البوت بأسلوب Matrix المستخدم في قواعد الأهداف.
- `botDisplayName` — اسم العرض للرسائل الصادرة.
- `pollTimeoutMs` — نافذة انتظار الاستطلاع الطويل. عدد صحيح بين 100 و30000.
- `allowFrom` — قائمة سماح للمرسلين (معرّفات مستخدمين أو `"*"`).
- `defaultTo` — الهدف الاحتياطي عند عدم توفير أي هدف.
- `actions.messages` / `actions.reactions` / `actions.search` / `actions.threads` — ضبط السماح بالأدوات لكل إجراء.

مفاتيح الحسابات المتعددة في المستوى الأعلى:

- `accounts` — سجل لتجاوزات مسماة لكل حساب مفهرسة بمعرّف الحساب.
- `defaultAccount` — معرّف الحساب المفضّل عند تكوين عدة حسابات.

## [​](https://docs.openclaw.ai/ar/channels/qa-channel\#%D8%A7%D9%84%D9%85%D8%B4%D8%BA%D9%91%D9%84%D8%A7%D8%AA)  المشغّلات

الفحص الذاتي على جانب المضيف (يكتب تقرير Markdown تحت `.artifacts/qa-e2e/`):

```
pnpm qa:e2e
```

يمر هذا عبر `qa-lab`، ويبدأ ناقل ضمان الجودة داخل المستودع، ويشغّل شريحة وقت التشغيل المضمّنة `qa-channel`، ثم ينفّذ فحصًا ذاتيًا حتميًا.مجموعة السيناريوهات الكاملة المدعومة بالمستودع:

```
pnpm openclaw qa suite
```

يشغّل السيناريوهات بالتوازي مقابل مسار QA في Gateway. راجع [نظرة عامة على QA](https://docs.openclaw.ai/ar/concepts/qa-e2e-automation) للاطلاع على السيناريوهات، والملفات التعريفية، وأوضاع المزوّد.موقع QA المدعوم بـ Docker (Gateway + واجهة مصحح QA Lab في حزمة واحدة):

```
pnpm qa:lab:up
```

يبني موقع QA، ويبدأ حزمة Gateway + QA Lab المدعومة بـ Docker، ويطبع عنوان URL الخاص بـ QA Lab. من هناك يمكنك اختيار السيناريوهات، وتحديد مسار النموذج، وتشغيل عمليات فردية، ومشاهدة النتائج مباشرة. مصحح QA Lab منفصل عن حزمة Control UI المشحونة.

## [​](https://docs.openclaw.ai/ar/channels/qa-channel\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [نظرة عامة على QA](https://docs.openclaw.ai/ar/concepts/qa-e2e-automation) — البنية الكاملة، ومحولات النقل، وتأليف السيناريوهات
- [Matrix QA](https://docs.openclaw.ai/ar/concepts/qa-matrix) — مثال لمشغّل نقل مباشر يقود قناة حقيقية
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing)
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups)
- [نظرة عامة على القنوات](https://docs.openclaw.ai/ar/channels)

[استكشاف مشكلات القنوات وإصلاحها](https://docs.openclaw.ai/ar/channels/troubleshooting)

Ctrl+I