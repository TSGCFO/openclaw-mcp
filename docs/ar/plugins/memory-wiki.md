---
source_url: https://docs.openclaw.ai/ar/plugins/memory-wiki
title: "\u0648\u064a\u0643\u064a \u0627\u0644\u0630\u0627\u0643\u0631\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/plugins/memory-wiki#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Plugins

ويكي الذاكرة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [ما يضيفه](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D9%85%D8%A7-%D9%8A%D8%B6%D9%8A%D9%81%D9%87)
- [كيف يتكامل مع الذاكرة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D9%83%D9%8A%D9%81-%D9%8A%D8%AA%D9%83%D8%A7%D9%85%D9%84-%D9%85%D8%B9-%D8%A7%D9%84%D8%B0%D8%A7%D9%83%D8%B1%D8%A9)
- [النمط الهجين الموصى به](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A7%D9%84%D9%86%D9%85%D8%B7-%D8%A7%D9%84%D9%87%D8%AC%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)
- [أوضاع الخزنة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A3%D9%88%D8%B6%D8%A7%D8%B9-%D8%A7%D9%84%D8%AE%D8%B2%D9%86%D8%A9)
- [isolated](https://docs.openclaw.ai/ar/plugins/memory-wiki#isolated)
- [bridge](https://docs.openclaw.ai/ar/plugins/memory-wiki#bridge)
- [unsafe-local](https://docs.openclaw.ai/ar/plugins/memory-wiki#unsafe-local)
- [تخطيط الخزنة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%AA%D8%AE%D8%B7%D9%8A%D8%B7-%D8%A7%D9%84%D8%AE%D8%B2%D9%86%D8%A9)
- [المطالبات المنظمة والأدلة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A7%D9%84%D9%85%D8%B7%D8%A7%D9%84%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%86%D8%B8%D9%85%D8%A9-%D9%88%D8%A7%D9%84%D8%A3%D8%AF%D9%84%D8%A9)
- [بيانات وصفية للكيانات موجهة للوكلاء](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D9%88%D8%B5%D9%81%D9%8A%D8%A9-%D9%84%D9%84%D9%83%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D9%85%D9%88%D8%AC%D9%87%D8%A9-%D9%84%D9%84%D9%88%D9%83%D9%84%D8%A7%D8%A1)
- [مسار التجميع](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D9%85%D8%B3%D8%A7%D8%B1-%D8%A7%D9%84%D8%AA%D8%AC%D9%85%D9%8A%D8%B9)
- [لوحات المعلومات وتقارير الصحة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D9%84%D9%88%D8%AD%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B9%D9%84%D9%88%D9%85%D8%A7%D8%AA-%D9%88%D8%AA%D9%82%D8%A7%D8%B1%D9%8A%D8%B1-%D8%A7%D9%84%D8%B5%D8%AD%D8%A9)
- [البحث والاسترجاع](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A7%D9%84%D8%A8%D8%AD%D8%AB-%D9%88%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%B1%D8%AC%D8%A7%D8%B9)
- [أدوات الوكيل](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%88%D9%83%D9%8A%D9%84)
- [سلوك المطالبة والسياق](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D9%85%D8%B7%D8%A7%D9%84%D8%A8%D8%A9-%D9%88%D8%A7%D9%84%D8%B3%D9%8A%D8%A7%D9%82)
- [الإعداد](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [مثال: QMD + وضع الجسر](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D9%85%D8%AB%D8%A7%D9%84-qmd-%2B-%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D8%AC%D8%B3%D8%B1)
- [CLI](https://docs.openclaw.ai/ar/plugins/memory-wiki#cli)
- [دعم Obsidian](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%AF%D8%B9%D9%85-obsidian)
- [سير العمل الموصى به](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%B3%D9%8A%D8%B1-%D8%A7%D9%84%D8%B9%D9%85%D9%84-%D8%A7%D9%84%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)
- [المستندات ذات الصلة](https://docs.openclaw.ai/ar/plugins/memory-wiki#%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%A7%D9%84%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`memory-wiki` هو Plugin مضمّن يحوّل الذاكرة الدائمة إلى خزنة معرفة
مجمّعة.إنه **لا** يستبدل Plugin الذاكرة النشطة. يظل Plugin الذاكرة النشطة
مسؤولًا عن الاستدعاء، والترقية، والفهرسة، وDreaming. يقف `memory-wiki` إلى جانبه
ويجمّع المعرفة الدائمة في ويكي قابلة للتنقل مع صفحات حتمية،
ومطالبات منظمة، ومصدرية، ولوحات معلومات، وملخصات قابلة للقراءة آليًا.استخدمه عندما تريد أن تتصرف الذاكرة كطبقة معرفة مُعتنى بها أكثر،
وبدرجة أقل ككومة من ملفات Markdown.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D9%85%D8%A7-%D9%8A%D8%B6%D9%8A%D9%81%D9%87)  ما يضيفه

- خزنة ويكي مخصصة بتخطيط صفحات حتمي
- بيانات وصفية منظمة للمطالبات والأدلة، وليس نثرًا فقط
- مصدرية على مستوى الصفحة، وثقة، وتناقضات، وأسئلة مفتوحة
- ملخصات مجمّعة لمستهلكي الوكلاء/وقت التشغيل
- أدوات بحث/جلب/تطبيق/تدقيق أصلية للويكي
- وضع جسر اختياري يستورد القطع العامة من Plugin الذاكرة النشطة
- وضع عرض اختياري مناسب لـ Obsidian وتكامل CLI

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D9%83%D9%8A%D9%81-%D9%8A%D8%AA%D9%83%D8%A7%D9%85%D9%84-%D9%85%D8%B9-%D8%A7%D9%84%D8%B0%D8%A7%D9%83%D8%B1%D8%A9)  كيف يتكامل مع الذاكرة

فكّر في التقسيم بهذا الشكل:

| الطبقة | تملك |
| --- | --- |
| Plugin الذاكرة النشطة (`memory-core`، QMD، Honcho، إلخ.) | الاستدعاء، والبحث الدلالي، والترقية، وDreaming، ووقت تشغيل الذاكرة |
| `memory-wiki` | صفحات الويكي المجمّعة، والتوليفات الغنية بالمصدرية، ولوحات المعلومات، وبحث/جلب/تطبيق خاص بالويكي |

إذا كشف Plugin الذاكرة النشطة عن قطع استدعاء مشتركة، يستطيع OpenClaw البحث
في الطبقتين معًا بتمريرة واحدة باستخدام `memory_search corpus=all`.عندما تحتاج إلى ترتيب خاص بالويكي، أو مصدرية، أو وصول مباشر إلى الصفحات،
استخدم الأدوات الأصلية للويكي بدلًا من ذلك.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A7%D9%84%D9%86%D9%85%D8%B7-%D8%A7%D9%84%D9%87%D8%AC%D9%8A%D9%86-%D8%A7%D9%84%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)  النمط الهجين الموصى به

الافتراضي القوي للإعدادات المحلية أولًا هو:

- QMD كخلفية الذاكرة النشطة للاستدعاء والبحث الدلالي الواسع
- `memory-wiki` في وضع `bridge` لصفحات المعرفة الدائمة المولّفة

ينجح هذا التقسيم جيدًا لأن كل طبقة تظل مركزة:

- يحافظ QMD على قابلية البحث في الملاحظات الخام، وصادرات الجلسات، والمجموعات الإضافية
- يجمّع `memory-wiki` الكيانات الثابتة، والمطالبات، ولوحات المعلومات، وصفحات المصادر

قاعدة عملية:

- استخدم `memory_search` عندما تريد تمريرة استدعاء واسعة واحدة عبر الذاكرة
- استخدم `wiki_search` و`wiki_get` عندما تريد نتائج ويكي واعية بالمصدرية
- استخدم `memory_search corpus=all` عندما تريد أن يمتد البحث المشترك عبر الطبقتين

إذا أبلغ وضع الجسر عن صفر قطع مصدّرة، فهذا يعني أن Plugin الذاكرة النشطة لا
يكشف حاليًا عن مدخلات جسر عامة بعد. شغّل `openclaw wiki doctor` أولًا،
ثم أكّد أن Plugin الذاكرة النشطة يدعم القطع العامة.عندما يكون وضع الجسر نشطًا ويكون `bridge.readMemoryArtifacts` مفعّلًا،
تقرأ `openclaw wiki status`، و`openclaw wiki doctor`، و`openclaw wiki bridge import` عبر Gateway الجاري. يحافظ ذلك على اتساق فحوصات جسر CLI
مع سياق Plugin ذاكرة وقت التشغيل. إذا كان الجسر معطلًا أو كانت قراءات القطع
متوقفة، فستحافظ تلك الأوامر على سلوكها المحلي/غير المتصل.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A3%D9%88%D8%B6%D8%A7%D8%B9-%D8%A7%D9%84%D8%AE%D8%B2%D9%86%D8%A9)  أوضاع الخزنة

يدعم `memory-wiki` ثلاثة أوضاع خزنة:

### [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#isolated)  `isolated`

خزنة خاصة، ومصادر خاصة، ولا اعتماد على `memory-core`.استخدم هذا عندما تريد أن تكون الويكي مخزن معرفة منسقًا مستقلًا.

### [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#bridge)  `bridge`

يقرأ قطع الذاكرة العامة وأحداث الذاكرة من Plugin الذاكرة النشطة
عبر وصلات Plugin SDK العامة.استخدم هذا عندما تريد أن تجمع الويكي قطع Plugin الذاكرة المصدّرة
وتنظمها دون الدخول إلى التفاصيل الداخلية الخاصة للـ Plugin.يمكن لوضع الجسر فهرسة:

- قطع الذاكرة المصدّرة
- تقارير الأحلام
- الملاحظات اليومية
- ملفات جذر الذاكرة
- سجلات أحداث الذاكرة

### [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#unsafe-local)  `unsafe-local`

مخرج صريح للجهاز نفسه للمسارات المحلية الخاصة.هذا الوضع تجريبي وغير قابل للنقل عمدًا. استخدمه فقط عندما
تفهم حد الثقة وتحتاج تحديدًا إلى وصول محلي لنظام الملفات لا يستطيع
وضع الجسر توفيره.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%AA%D8%AE%D8%B7%D9%8A%D8%B7-%D8%A7%D9%84%D8%AE%D8%B2%D9%86%D8%A9)  تخطيط الخزنة

يهيئ Plugin خزنة بهذا الشكل:

```
<vault>/
  AGENTS.md
  WIKI.md
  index.md
  inbox.md
  entities/
  concepts/
  syntheses/
  sources/
  reports/
  _attachments/
  _views/
  .openclaw-wiki/
```

يبقى المحتوى المُدار داخل الكتل المولدة. تُحفظ كتل الملاحظات البشرية.مجموعات الصفحات الرئيسية هي:

- `sources/` للمواد الخام المستوردة والصفحات المدعومة بالجسر
- `entities/` للأشياء الدائمة، والأشخاص، والأنظمة، والمشاريع، والكائنات
- `concepts/` للأفكار، والتجريدات، والأنماط، والسياسات
- `syntheses/` للملخصات المجمّعة والتجميعات المُعتنى بها
- `reports/` للوحات المعلومات المولدة

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A7%D9%84%D9%85%D8%B7%D8%A7%D9%84%D8%A8%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D9%86%D8%B8%D9%85%D8%A9-%D9%88%D8%A7%D9%84%D8%A3%D8%AF%D9%84%D8%A9)  المطالبات المنظمة والأدلة

يمكن للصفحات حمل frontmatter منظمة باسم `claims`، وليس نصًا حرًا فقط.يمكن لكل مطالبة أن تتضمن:

- `id`
- `text`
- `status`
- `confidence`
- `evidence[]`
- `updatedAt`

يمكن لإدخالات الأدلة أن تتضمن:

- `kind`
- `sourceId`
- `path`
- `lines`
- `weight`
- `confidence`
- `privacyTier`
- `note`
- `updatedAt`

هذا ما يجعل الويكي تتصرف كطبقة اعتقاد أكثر من كونها مستودع ملاحظات
سلبيًا. يمكن تتبع المطالبات، وتسجيل درجاتها، ومنازعتها، وحلها بالعودة إلى المصادر.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D9%88%D8%B5%D9%81%D9%8A%D8%A9-%D9%84%D9%84%D9%83%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D9%85%D9%88%D8%AC%D9%87%D8%A9-%D9%84%D9%84%D9%88%D9%83%D9%84%D8%A7%D8%A1)  بيانات وصفية للكيانات موجهة للوكلاء

يمكن لصفحات الكيانات أيضًا حمل بيانات وصفية للتوجيه لاستخدام الوكيل. هذه
frontmatter عامة، لذا تعمل مع الأشخاص، والفرق، والأنظمة، والمشاريع، أو أي
نوع كيان آخر.تشمل الحقول الشائعة:

- `entityType`: مثل `person`، أو `team`، أو `system`، أو `project`
- `canonicalId`: مفتاح هوية ثابت يُستخدم عبر الأسماء البديلة وعمليات الاستيراد
- `aliases`: أسماء أو معرّفات أو تسميات ينبغي أن تُحل إلى الصفحة نفسها
- `privacyTier`: `public`، أو `local-private`، أو `sensitive`، أو `confirm-before-use`
- `bestUsedFor` / `notEnoughFor`: تلميحات توجيه مضغوطة
- `lastRefreshedAt`: طابع زمني لتحديث المصدر منفصل عن وقت تحرير الصفحة
- `personCard`: بطاقة توجيه اختيارية خاصة بالشخص مع المعرّفات، والشبكات الاجتماعية،
ورسائل البريد الإلكتروني، والمنطقة الزمنية، والمسار، وما يُطلب من أجله، وما يجب تجنب طلبه، والثقة، والخصوصية
- `relationships`: حواف مصنفة إلى صفحات ذات صلة مع الهدف، والنوع، والوزن،
والثقة، ونوع الدليل، وطبقة الخصوصية، والملاحظة

بالنسبة إلى ويكي أشخاص، ينبغي للوكيل عادةً أن يبدأ من
`reports/person-agent-directory.md`، ثم يفتح صفحة الشخص باستخدام `wiki_get`
قبل استخدام تفاصيل الاتصال أو الحقائق المستنتجة.مثال:

```
pageType: entity
entityType: person
id: entity.brad-groux
canonicalId: maintainer.brad-groux
aliases:
  - Brad
  - bgroux
privacyTier: local-private
bestUsedFor:
  - Microsoft Teams and Azure routing
notEnoughFor:
  - legal approval
lastRefreshedAt: "2026-04-29T00:00:00.000Z"
personCard:
  handles:
    - "@bgroux"
  socials:
    - "https://x.example/bgroux"
  emails:
    - brad@example.com
  timezone: America/Chicago
  lane: Microsoft ecosystem
  askFor:
    - Teams rollout questions
  avoidAskingFor:
    - unrelated billing decisions
  confidence: 0.8
  privacyTier: confirm-before-use
relationships:
  - targetId: entity.alice
    targetTitle: Alice
    kind: collaborates-with
    confidence: 0.7
    evidenceKind: discrawl-stat
claims:
  - id: claim.brad.teams
    text: Brad is useful for Microsoft Teams routing.
    status: supported
    confidence: 0.9
    evidence:
      - kind: maintainer-whois
        sourceId: source.maintainers
        privacyTier: local-private
```

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D9%85%D8%B3%D8%A7%D8%B1-%D8%A7%D9%84%D8%AA%D8%AC%D9%85%D9%8A%D8%B9)  مسار التجميع

تقرأ خطوة التجميع صفحات الويكي، وتطبّع الملخصات، وتصدر قطعًا ثابتة
موجهة للآلة ضمن:

- `.openclaw-wiki/cache/agent-digest.json`
- `.openclaw-wiki/cache/claims.jsonl`

توجد هذه الملخصات حتى لا تضطر الوكلاء وشيفرة وقت التشغيل إلى كشط صفحات
Markdown.تدعم المخرجات المجمّعة أيضًا:

- فهرسة الويكي في التمريرة الأولى لتدفقات البحث/الجلب
- البحث بمعرّف المطالبة رجوعًا إلى الصفحات المالكة
- ملاحق مطالبات مضغوطة
- توليد التقارير/لوحات المعلومات

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D9%84%D9%88%D8%AD%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B9%D9%84%D9%88%D9%85%D8%A7%D8%AA-%D9%88%D8%AA%D9%82%D8%A7%D8%B1%D9%8A%D8%B1-%D8%A7%D9%84%D8%B5%D8%AD%D8%A9)  لوحات المعلومات وتقارير الصحة

عندما يكون `render.createDashboards` مفعّلًا، يحافظ التجميع على لوحات المعلومات ضمن
`reports/`.تشمل التقارير المضمّنة:

- `reports/open-questions.md`
- `reports/contradictions.md`
- `reports/low-confidence.md`
- `reports/claim-health.md`
- `reports/stale-pages.md`
- `reports/person-agent-directory.md`
- `reports/relationship-graph.md`
- `reports/provenance-coverage.md`
- `reports/privacy-review.md`

تتتبع هذه التقارير أشياء مثل:

- عناقيد ملاحظات التناقض
- عناقيد المطالبات المتنافسة
- المطالبات التي تفتقد أدلة منظمة
- الصفحات والمطالبات منخفضة الثقة
- الحداثة القديمة أو المجهولة
- الصفحات التي تحتوي على أسئلة غير محلولة
- بطاقات توجيه الأشخاص/الكيانات
- حواف العلاقات المنظمة
- تغطية فئات الأدلة
- طبقات الخصوصية غير العامة التي تحتاج إلى مراجعة قبل الاستخدام

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A7%D9%84%D8%A8%D8%AD%D8%AB-%D9%88%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%B1%D8%AC%D8%A7%D8%B9)  البحث والاسترجاع

يدعم `memory-wiki` خلفيتي بحث:

- `shared`: استخدم تدفق بحث الذاكرة المشترك عندما يكون متاحًا
- `local`: ابحث في الويكي محليًا

ويدعم أيضًا ثلاث مجموعات نصية:

- `wiki`
- `memory`
- `all`

سلوك مهم:

- يستخدم `wiki_search` و`wiki_get` الملخصات المجمّعة كتمريرة أولى عندما يكون ذلك ممكنًا
- يمكن لمعرّفات المطالبات أن تُحل رجوعًا إلى الصفحة المالكة
- تؤثر المطالبات المتنازع عليها/القديمة/الحديثة في الترتيب
- يمكن لتسميات المصدرية أن تبقى في النتائج
- يمكن لوضع البحث ترجيح الترتيب للعثور على الأشخاص، أو توجيه الأسئلة، أو أدلة
المصادر، أو المطالبات الخام

قاعدة عملية:

- استخدم `memory_search corpus=all` لتمريرة استدعاء واسعة واحدة
- استخدم `wiki_search` \+ `wiki_get` عندما تهتم بالترتيب الخاص بالويكي،
أو المصدرية، أو بنية الاعتقاد على مستوى الصفحة

أوضاع البحث:

- `auto`: افتراضي متوازن
- `find-person`: يعزز الكيانات الشبيهة بالأشخاص، والأسماء البديلة، والمعرّفات، والشبكات الاجتماعية، و
المعرّفات القانونية
- `route-question`: يعزز بطاقات الوكلاء، وتلميحات ما يُطلب من أجله، وتلميحات أفضل استخدام، و
سياق العلاقات
- `source-evidence`: يعزز صفحات المصادر والبيانات الوصفية المنظمة للأدلة
- `raw-claim`: يعزز المطالبات المنظمة المطابقة ويعيد بيانات المطالبة/الدليل
الوصفية في النتائج

عندما تطابق نتيجة مطالبة منظمة، يستطيع `wiki_search` إرجاع
`matchedClaimId`، و`matchedClaimStatus`، و`matchedClaimConfidence`،
و`evidenceKinds`، و`evidenceSourceIds` في حمولة التفاصيل الخاصة به. يتضمن خرج النص
أيضًا أسطر `Claim:` و`Evidence:` مضغوطة عند توفرها.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%88%D9%83%D9%8A%D9%84)  أدوات الوكيل

يسجل Plugin هذه الأدوات:

- `wiki_status`
- `wiki_search`
- `wiki_get`
- `wiki_apply`
- `wiki_lint`

ما تفعله:

- `wiki_status`: وضع الخزنة الحالي، والصحة، وتوفر Obsidian CLI
- `wiki_search`: يبحث في صفحات الويكي، وعند تهيئته، في مجموعات الذاكرة المشتركة؛
يقبل `mode` للعثور على الأشخاص، أو توجيه الأسئلة، أو أدلة المصادر، أو التعمق في
المطالبة الخام
- `wiki_get`: يقرأ صفحة ويكي بحسب المعرّف/المسار أو يعود إلى مجموعة الذاكرة المشتركة
- `wiki_apply`: طفرات توليف/بيانات وصفية ضيقة دون جراحة صفحات حرة
- `wiki_lint`: فحوصات بنيوية، وفجوات المصدرية، والتناقضات، والأسئلة المفتوحة

يسجل Plugin أيضًا ملحق مجموعة ذاكرة غير حصري، بحيث يستطيع
`memory_search` و`memory_get` المشتركان الوصول إلى الويكي عندما يدعم Plugin الذاكرة النشطة
اختيار المجموعة.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D9%85%D8%B7%D8%A7%D9%84%D8%A8%D8%A9-%D9%88%D8%A7%D9%84%D8%B3%D9%8A%D8%A7%D9%82)  سلوك المطالبة والسياق

عندما يكون `context.includeCompiledDigestPrompt` مفعّلًا، تلحق أقسام مطالبة الذاكرة
لقطة مجمّعة مضغوطة من `agent-digest.json`.هذه اللقطة صغيرة وعالية القيمة عمدًا:

- الصفحات الأعلى فقط
- المطالبات الأعلى فقط
- عدد التناقضات
- عدد الأسئلة
- مؤهلات الثقة/الحداثة

هذا اختياري لأنه يغيّر شكل المطالبة ويفيد أساسًا محركات السياق
أو تجميع المطالبات القديم الذي يستهلك ملاحق الذاكرة صراحةً.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  الإعداد

ضع الإعداد ضمن `plugins.entries.memory-wiki.config`:

```
{
  plugins: {
    entries: {
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "isolated",
          vault: {
            path: "~/.openclaw/wiki/main",
            renderMode: "obsidian",
          },
          obsidian: {
            enabled: true,
            useOfficialCli: true,
            vaultName: "OpenClaw Wiki",
            openAfterWrites: false,
          },
          bridge: {
            enabled: false,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          ingest: {
            autoCompile: true,
            maxConcurrentJobs: 1,
            allowUrlIngest: true,
          },
          search: {
            backend: "shared",
            corpus: "wiki",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
          render: {
            preserveHumanBlocks: true,
            createBacklinks: true,
            createDashboards: true,
          },
        },
      },
    },
  },
}
```

المفاتيح الرئيسية:

- `vaultMode`: `isolated` أو `bridge` أو `unsafe-local`
- `vault.renderMode`: `native` أو `obsidian`
- `bridge.readMemoryArtifacts`: استيراد العناصر العامة الخاصة بـ Plugin الذاكرة النشطة
- `bridge.followMemoryEvents`: تضمين سجلات الأحداث في وضع الجسر
- `search.backend`: `shared` أو `local`
- `search.corpus`: `wiki` أو `memory` أو `all`
- `context.includeCompiledDigestPrompt`: إلحاق لقطة موجزة مضغوطة بأقسام موجه الذاكرة
- `render.createBacklinks`: إنشاء كتل مرتبطة حتمية
- `render.createDashboards`: إنشاء صفحات لوحات المعلومات

### [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D9%85%D8%AB%D8%A7%D9%84-qmd-+-%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D8%AC%D8%B3%D8%B1)  مثال: QMD + وضع الجسر

استخدم هذا عندما تريد QMD للاستدعاء و`memory-wiki` لطبقة معرفة مصانة:

```
{
  memory: {
    backend: "qmd",
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "bridge",
          bridge: {
            enabled: true,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          search: {
            backend: "shared",
            corpus: "all",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
        },
      },
    },
  },
}
```

يحافظ هذا على:

- تولي QMD مسؤولية استدعاء الذاكرة النشطة
- تركيز `memory-wiki` على الصفحات المجمعة ولوحات المعلومات
- بقاء شكل الموجه دون تغيير حتى تفعّل موجزات التجميع عمدا

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#cli)  CLI

يوفر `memory-wiki` أيضا سطح CLI على المستوى الأعلى:

```
openclaw wiki status
openclaw wiki doctor
openclaw wiki init
openclaw wiki ingest ./notes/alpha.md
openclaw wiki compile
openclaw wiki lint
openclaw wiki search "alpha"
openclaw wiki get entity.alpha
openclaw wiki apply synthesis "Alpha Summary" --body "..." --source-id source.alpha
openclaw wiki bridge import
openclaw wiki obsidian status
```

راجع [CLI: wiki](https://docs.openclaw.ai/ar/cli/wiki) للاطلاع على مرجع الأوامر الكامل.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%AF%D8%B9%D9%85-obsidian)  دعم Obsidian

عندما يكون `vault.renderMode` هو `obsidian`، يكتب Plugin Markdown ملائما لـ Obsidian ويمكنه اختياريا استخدام CLI الرسمي لـ `obsidian`.تشمل مسارات العمل المدعومة:

- فحص الحالة
- البحث في المخزن
- فتح صفحة
- استدعاء أمر Obsidian
- الانتقال إلى الملاحظة اليومية

هذا اختياري. تظل الويكي تعمل في الوضع الأصلي دون Obsidian.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%B3%D9%8A%D8%B1-%D8%A7%D9%84%D8%B9%D9%85%D9%84-%D8%A7%D9%84%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)  سير العمل الموصى به

1. أبق Plugin الذاكرة النشطة لديك للاستدعاء/الترقية/Dreaming.
2. فعّل `memory-wiki`.
3. ابدأ بوضع `isolated` ما لم تكن تريد وضع الجسر صراحة.
4. استخدم `wiki_search` / `wiki_get` عندما تكون مصادر المعلومات مهمة.
5. استخدم `wiki_apply` للتجميعات المحددة أو تحديثات البيانات الوصفية.
6. شغّل `wiki_lint` بعد التغييرات المهمة.
7. فعّل لوحات المعلومات إذا كنت تريد رؤية المحتوى القديم أو التناقضات.

## [​](https://docs.openclaw.ai/ar/plugins/memory-wiki\#%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%A7%D9%84%D8%B5%D9%84%D8%A9)  المستندات ذات الصلة

- [نظرة عامة على الذاكرة](https://docs.openclaw.ai/ar/concepts/memory)
- [CLI: الذاكرة](https://docs.openclaw.ai/ar/cli/memory)
- [CLI: wiki](https://docs.openclaw.ai/ar/cli/wiki)
- [نظرة عامة على Plugin SDK](https://docs.openclaw.ai/ar/plugins/sdk-overview)

[Voice call](https://docs.openclaw.ai/ar/plugins/voice-call) [Memory LanceDB](https://docs.openclaw.ai/ar/plugins/memory-lancedb)

Ctrl+I