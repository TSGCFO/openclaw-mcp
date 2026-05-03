---
source_url: https://docs.openclaw.ai/ar/automation/taskflow
title: "\u062a\u062f\u0641\u0642 \u0627\u0644\u0645\u0647\u0627\u0645 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation/taskflow#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Automation and tasks

تدفق المهام

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [متى تستخدم تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow#%D9%85%D8%AA%D9%89-%D8%AA%D8%B3%D8%AA%D8%AE%D8%AF%D9%85-%D8%AA%D8%AF%D9%81%D9%82-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [نمط سير عمل مجدول موثوق](https://docs.openclaw.ai/ar/automation/taskflow#%D9%86%D9%85%D8%B7-%D8%B3%D9%8A%D8%B1-%D8%B9%D9%85%D9%84-%D9%85%D8%AC%D8%AF%D9%88%D9%84-%D9%85%D9%88%D8%AB%D9%88%D9%82)
- [أوضاع المزامنة](https://docs.openclaw.ai/ar/automation/taskflow#%D8%A3%D9%88%D8%B6%D8%A7%D8%B9-%D8%A7%D9%84%D9%85%D8%B2%D8%A7%D9%85%D9%86%D8%A9)
- [الوضع المُدار](https://docs.openclaw.ai/ar/automation/taskflow#%D8%A7%D9%84%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D9%85%D9%8F%D8%AF%D8%A7%D8%B1)
- [الوضع المعكوس](https://docs.openclaw.ai/ar/automation/taskflow#%D8%A7%D9%84%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D9%85%D8%B9%D9%83%D9%88%D8%B3)
- [الحالة الدائمة وتتبع المراجعات](https://docs.openclaw.ai/ar/automation/taskflow#%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9-%D9%88%D8%AA%D8%AA%D8%A8%D8%B9-%D8%A7%D9%84%D9%85%D8%B1%D8%A7%D8%AC%D8%B9%D8%A7%D8%AA)
- [سلوك الإلغاء](https://docs.openclaw.ai/ar/automation/taskflow#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D8%A5%D9%84%D8%BA%D8%A7%D8%A1)
- [أوامر CLI](https://docs.openclaw.ai/ar/automation/taskflow#%D8%A3%D9%88%D8%A7%D9%85%D8%B1-cli)
- [كيف ترتبط التدفقات بالمهام](https://docs.openclaw.ai/ar/automation/taskflow#%D9%83%D9%8A%D9%81-%D8%AA%D8%B1%D8%AA%D8%A8%D8%B7-%D8%A7%D9%84%D8%AA%D8%AF%D9%81%D9%82%D8%A7%D8%AA-%D8%A8%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [ذات صلة](https://docs.openclaw.ai/ar/automation/taskflow#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

تدفق المهام هو ركيزة تنسيق التدفقات التي تقع فوق [المهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks). يدير تدفقات دائمة متعددة الخطوات بحالتها الخاصة، وتتبع المراجعات، ودلالات المزامنة، بينما تبقى المهام الفردية وحدة العمل المنفصل.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D9%85%D8%AA%D9%89-%D8%AA%D8%B3%D8%AA%D8%AE%D8%AF%D9%85-%D8%AA%D8%AF%D9%81%D9%82-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  متى تستخدم تدفق المهام

استخدم تدفق المهام عندما يمتد العمل عبر خطوات متعددة متسلسلة أو متفرعة وتحتاج إلى تتبع دائم للتقدم عبر عمليات إعادة تشغيل Gateway. بالنسبة للعمليات الخلفية المفردة، تكفي [مهمة](https://docs.openclaw.ai/ar/automation/tasks) عادية.

| السيناريو | الاستخدام |
| --- | --- |
| مهمة خلفية واحدة | مهمة عادية |
| خط أنابيب متعدد الخطوات (A ثم B ثم C) | تدفق المهام (مُدار) |
| مراقبة المهام المنشأة خارجيًا | تدفق المهام (معكوس) |
| تذكير لمرة واحدة | مهمة Cron |

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D9%86%D9%85%D8%B7-%D8%B3%D9%8A%D8%B1-%D8%B9%D9%85%D9%84-%D9%85%D8%AC%D8%AF%D9%88%D9%84-%D9%85%D9%88%D8%AB%D9%88%D9%82)  نمط سير عمل مجدول موثوق

بالنسبة إلى سير العمل المتكرر مثل ملخصات معلومات السوق، تعامل مع الجدولة، والتنسيق، وفحوصات الموثوقية كطبقات منفصلة:

1. استخدم [المهام المجدولة](https://docs.openclaw.ai/ar/automation/cron-jobs) للتوقيت.
2. استخدم جلسة cron دائمة عندما يجب أن يبني سير العمل على السياق السابق.
3. استخدم [Lobster](https://docs.openclaw.ai/ar/tools/lobster) للخطوات الحتمية، وبوابات الموافقة، ورموز الاستئناف.
4. استخدم تدفق المهام لتتبع التشغيل متعدد الخطوات عبر المهام الفرعية، والانتظارات، وإعادة المحاولة، وعمليات إعادة تشغيل Gateway.

شكل cron نموذجي:

```
openclaw cron add \
  --name "Market intelligence brief" \
  --cron "0 7 * * 1-5" \
  --tz "America/New_York" \
  --session session:market-intel \
  --message "Run the market-intel Lobster workflow. Verify source freshness before summarizing." \
  --announce \
  --channel slack \
  --to "channel:C1234567890"
```

استخدم `session:<id>` بدلًا من `isolated` عندما يحتاج سير العمل المتكرر إلى سجل مقصود، أو ملخصات تشغيل سابقة، أو سياق ثابت. استخدم `isolated` عندما يجب أن يبدأ كل تشغيل من جديد وتكون كل الحالة المطلوبة صريحة في سير العمل.داخل سير العمل، ضع فحوصات الموثوقية قبل خطوة ملخص LLM:

```
name: market-intel-brief
steps:
  - id: preflight
    command: market-intel check --json
  - id: collect
    command: market-intel collect --json
    stdin: $preflight.json
  - id: summarize
    command: market-intel summarize --json
    stdin: $collect.json
  - id: approve
    command: market-intel deliver --preview
    stdin: $summarize.json
    approval: required
  - id: deliver
    command: market-intel deliver --execute
    stdin: $summarize.json
    condition: $approve.approved
```

فحوصات ما قبل التشغيل الموصى بها:

- توفر المتصفح واختيار الملف الشخصي، على سبيل المثال `openclaw` للحالة المُدارة أو `user` عندما تكون جلسة Chrome مسجلة الدخول مطلوبة. راجع [المتصفح](https://docs.openclaw.ai/ar/tools/browser).
- بيانات اعتماد API والحصة لكل مصدر.
- إمكانية الوصول عبر الشبكة إلى نقاط النهاية المطلوبة.
- الأدوات المطلوبة مفعّلة للوكيل، مثل `lobster`، و`browser`، و`llm-task`.
- وجهة الفشل مهيأة لـ cron بحيث تكون حالات فشل ما قبل التشغيل مرئية. راجع [المهام المجدولة](https://docs.openclaw.ai/ar/automation/cron-jobs#delivery-and-output).

حقول مصدر البيانات الموصى بها لكل عنصر مُجمّع:

```
{
  "sourceUrl": "https://example.com/report",
  "retrievedAt": "2026-04-24T12:00:00Z",
  "asOf": "2026-04-24",
  "title": "Example report",
  "content": "..."
}
```

اجعل سير العمل يرفض العناصر القديمة أو يضع عليها علامة قبل التلخيص. يجب أن تتلقى خطوة LLM فقط JSON منظمًا، ويجب أن يُطلب منها الحفاظ على `sourceUrl`، و`retrievedAt`، و`asOf` في مخرجاتها. استخدم [مهمة LLM](https://docs.openclaw.ai/ar/tools/llm-task) عندما تحتاج إلى خطوة نموذج متحقق من مخططها داخل سير العمل.بالنسبة إلى سير العمل القابل لإعادة الاستخدام من قِبل الفريق أو المجتمع، حزم CLI، وملفات `.lobster`، وأي ملاحظات إعداد كمهارة أو plugin وانشرها عبر [ClawHub](https://docs.openclaw.ai/ar/tools/clawhub). أبقِ حواجز الحماية الخاصة بسير العمل ضمن تلك الحزمة ما لم تكن API الخاصة بالـ plugin تفتقد إلى قدرة عامة مطلوبة.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%A3%D9%88%D8%B6%D8%A7%D8%B9-%D8%A7%D9%84%D9%85%D8%B2%D8%A7%D9%85%D9%86%D8%A9)  أوضاع المزامنة

### [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%A7%D9%84%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D9%85%D9%8F%D8%AF%D8%A7%D8%B1)  الوضع المُدار

يمتلك تدفق المهام دورة الحياة من البداية إلى النهاية. ينشئ المهام كخطوات تدفق، ويدفعها إلى الاكتمال، ويقدّم حالة التدفق تلقائيًا.مثال: تدفق تقرير أسبوعي يقوم بـ (1) جمع البيانات، و(2) إنشاء التقرير، و(3) تسليمه. ينشئ تدفق المهام كل خطوة كمهمة خلفية، وينتظر الاكتمال، ثم ينتقل إلى الخطوة التالية.

```
Flow: weekly-report
  Step 1: gather-data     → task created → succeeded
  Step 2: generate-report → task created → succeeded
  Step 3: deliver         → task created → running
```

### [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%A7%D9%84%D9%88%D8%B6%D8%B9-%D8%A7%D9%84%D9%85%D8%B9%D9%83%D9%88%D8%B3)  الوضع المعكوس

يراقب تدفق المهام المهام المنشأة خارجيًا ويحافظ على تزامن حالة التدفق دون امتلاك إنشاء المهام. يكون هذا مفيدًا عندما تنشأ المهام من وظائف cron، أو أوامر CLI، أو مصادر أخرى وتريد عرضًا موحدًا لتقدمها كتدفق.مثال: ثلاث وظائف cron مستقلة تشكل معًا روتين “عمليات الصباح”. يتتبع تدفق معكوس تقدمها الجماعي دون التحكم في وقت تشغيلها أو كيفية تشغيلها.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9-%D9%88%D8%AA%D8%AA%D8%A8%D8%B9-%D8%A7%D9%84%D9%85%D8%B1%D8%A7%D8%AC%D8%B9%D8%A7%D8%AA)  الحالة الدائمة وتتبع المراجعات

يحتفظ كل تدفق بحالته الخاصة ويتتبع المراجعات بحيث يبقى التقدم محفوظًا بعد عمليات إعادة تشغيل Gateway. يتيح تتبع المراجعات اكتشاف التعارضات عندما تحاول مصادر متعددة تقديم التدفق نفسه في الوقت نفسه.
يستخدم سجل التدفقات SQLite مع صيانة محدودة لسجل الكتابة المسبقة، بما في ذلك
نقاط تحقق دورية وعند الإيقاف، بحيث لا تحتفظ بوابات Gateway طويلة التشغيل
بملفات جانبية `registry.sqlite-wal` غير محدودة.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%B3%D9%84%D9%88%D9%83-%D8%A7%D9%84%D8%A5%D9%84%D8%BA%D8%A7%D8%A1)  سلوك الإلغاء

يضبط `openclaw tasks flow cancel` نية إلغاء ثابتة على التدفق. تُلغى المهام النشطة داخل التدفق، ولا تبدأ أي خطوات جديدة. تستمر نية الإلغاء عبر عمليات إعادة التشغيل، لذلك يبقى التدفق الملغى ملغى حتى إذا أُعيد تشغيل Gateway قبل انتهاء جميع المهام الفرعية.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%A3%D9%88%D8%A7%D9%85%D8%B1-cli)  أوامر CLI

```
# List active and recent flows
openclaw tasks flow list

# Show details for a specific flow
openclaw tasks flow show <lookup>

# Cancel a running flow and its active tasks
openclaw tasks flow cancel <lookup>
```

| الأمر | الوصف |
| --- | --- |
| `openclaw tasks flow list` | يعرض التدفقات المتتبعة مع الحالة ووضع المزامنة |
| `openclaw tasks flow show <id>` | افحص تدفقًا واحدًا حسب معرف التدفق أو مفتاح البحث |
| `openclaw tasks flow cancel <id>` | ألغِ تدفقًا قيد التشغيل ومهامه النشطة |

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D9%83%D9%8A%D9%81-%D8%AA%D8%B1%D8%AA%D8%A8%D8%B7-%D8%A7%D9%84%D8%AA%D8%AF%D9%81%D9%82%D8%A7%D8%AA-%D8%A8%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  كيف ترتبط التدفقات بالمهام

تنسق التدفقات المهام ولا تستبدلها. قد يدير تدفق واحد عدة مهام خلفية خلال عمره. استخدم `openclaw tasks` لفحص سجلات المهام الفردية و`openclaw tasks flow` لفحص التدفق المنسق.

## [​](https://docs.openclaw.ai/ar/automation/taskflow\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [المهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks) — سجل العمل المنفصل الذي تنسقه التدفقات
- [CLI: المهام](https://docs.openclaw.ai/ar/cli/tasks) — مرجع أوامر CLI لـ `openclaw tasks flow`
- [نظرة عامة على الأتمتة](https://docs.openclaw.ai/ar/automation) — جميع آليات الأتمتة في لمحة
- [وظائف Cron](https://docs.openclaw.ai/ar/automation/cron-jobs) — وظائف مجدولة قد تغذي التدفقات

[Background tasks](https://docs.openclaw.ai/ar/automation/tasks) [التعليمات الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders)

Ctrl+I