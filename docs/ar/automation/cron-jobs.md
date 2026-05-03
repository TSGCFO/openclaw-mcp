---
source_url: https://docs.openclaw.ai/ar/automation/cron-jobs
title: "\u0627\u0644\u0645\u0647\u0627\u0645 \u0627\u0644\u0645\u062c\u062f\u0648\u0644\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation/cron-jobs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Automation and tasks

المهام المجدولة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [بدء سريع](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A8%D8%AF%D8%A1-%D8%B3%D8%B1%D9%8A%D8%B9)
- [كيف يعمل cron](https://docs.openclaw.ai/ar/automation/cron-jobs#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84-cron)
- [أنواع الجداول](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%AC%D8%AF%D8%A7%D9%88%D9%84)
- [يستخدم يوم الشهر ويوم الأسبوع منطق OR](https://docs.openclaw.ai/ar/automation/cron-jobs#%D9%8A%D8%B3%D8%AA%D8%AE%D8%AF%D9%85-%D9%8A%D9%88%D9%85-%D8%A7%D9%84%D8%B4%D9%87%D8%B1-%D9%88%D9%8A%D9%88%D9%85-%D8%A7%D9%84%D8%A3%D8%B3%D8%A8%D9%88%D8%B9-%D9%85%D9%86%D8%B7%D9%82-or)
- [أساليب التنفيذ](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A3%D8%B3%D8%A7%D9%84%D9%8A%D8%A8-%D8%A7%D9%84%D8%AA%D9%86%D9%81%D9%8A%D8%B0)
- [خيارات الحمولة للمهام المعزولة](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%AD%D9%85%D9%88%D9%84%D8%A9-%D9%84%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%B9%D8%B2%D9%88%D9%84%D8%A9)
- [التسليم والمخرجات](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D9%88%D8%A7%D9%84%D9%85%D8%AE%D8%B1%D8%AC%D8%A7%D8%AA)
- [أمثلة CLI](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A3%D9%85%D8%AB%D9%84%D8%A9-cli)
- [Webhooks](https://docs.openclaw.ai/ar/automation/cron-jobs#webhooks)
- [المصادقة](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A7%D9%84%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9)
- [تكامل Gmail PubSub](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%AA%D9%83%D8%A7%D9%85%D9%84-gmail-pubsub)
- [إعداد المعالج (موصى به)](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)
- [بدء Gateway تلقائيًا](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A8%D8%AF%D8%A1-gateway-%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D9%8B%D8%A7)
- [إعداد يدوي لمرة واحدة](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D9%8A%D8%AF%D9%88%D9%8A-%D9%84%D9%85%D8%B1%D8%A9-%D9%88%D8%A7%D8%AD%D8%AF%D8%A9)
- [تجاوز نموذج Gmail](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%AA%D8%AC%D8%A7%D9%88%D8%B2-%D9%86%D9%85%D9%88%D8%B0%D8%AC-gmail)
- [إدارة المهام](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A5%D8%AF%D8%A7%D8%B1%D8%A9-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [الإعدادات](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [سلم الأوامر](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%B3%D9%84%D9%85-%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)
- [ذات صلة](https://docs.openclaw.ai/ar/automation/cron-jobs#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Cron هو المجدول المدمج في Gateway. يحفظ المهام، ويوقظ الوكيل في الوقت المناسب، ويمكنه تسليم المخرجات مرة أخرى إلى قناة دردشة أو نقطة نهاية Webhook.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A8%D8%AF%D8%A1-%D8%B3%D8%B1%D9%8A%D8%B9)  بدء سريع

1

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

إضافة تذكير لمرة واحدة

```
openclaw cron add \
  --name "Reminder" \
  --at "2026-02-01T16:00:00Z" \
  --session main \
  --system-event "Reminder: check the cron docs draft" \
  --wake now \
  --delete-after-run
```

2

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

تحقق من مهامك

```
openclaw cron list
openclaw cron show <job-id>
```

3

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

عرض سجل التشغيل

```
openclaw cron runs --id <job-id>
```

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84-cron)  كيف يعمل cron

- يعمل Cron **داخل عملية Gateway** (وليس داخل النموذج).
- تستمر تعريفات المهام في `~/.openclaw/cron/jobs.json` حتى لا تفقد عمليات إعادة التشغيل الجداول.
- تستمر حالة التنفيذ وقت التشغيل بجانبه في `~/.openclaw/cron/jobs-state.json`. إذا كنت تتتبع تعريفات cron في git، فتتبع `jobs.json` وأضف `jobs-state.json` إلى gitignore.
- بعد الفصل، يمكن لإصدارات OpenClaw الأقدم قراءة `jobs.json` لكنها قد تتعامل مع المهام كأنها جديدة لأن حقول وقت التشغيل أصبحت الآن في `jobs-state.json`.
- عند تحرير `jobs.json` أثناء تشغيل Gateway أو توقفه، يقارن OpenClaw حقول الجدولة المتغيرة مع بيانات تعريف خانة وقت التشغيل المعلقة ويمسح قيم `nextRunAtMs` القديمة. تحافظ عمليات إعادة الكتابة التي تقتصر على التنسيق أو ترتيب المفاتيح فقط على الخانة المعلقة.
- تنشئ كل عمليات تنفيذ cron سجلات [مهمة خلفية](https://docs.openclaw.ai/ar/automation/tasks).
- عند بدء Gateway، يعاد جدولة مهام أدوار الوكيل المعزولة المتأخرة إلى خارج نافذة اتصال القناة بدلا من إعادة تشغيلها فورا، بحيث يبقى بدء تشغيل Discord/Telegram وإعداد الأوامر الأصلية سريع الاستجابة بعد عمليات إعادة التشغيل.
- تحذف مهام المرة الواحدة (`--at`) نفسها تلقائيا بعد النجاح افتراضيا.
- تحاول عمليات cron المعزولة بأفضل جهد إغلاق علامات تبويب المتصفح/العمليات المتتبعة لجلسة `cron:<jobId>` الخاصة بها عند اكتمال التشغيل، حتى لا تترك أتمتة المتصفح المنفصلة عمليات يتيمة خلفها.
- تحمي عمليات cron المعزولة أيضا من ردود الإقرار القديمة. إذا كانت النتيجة الأولى مجرد تحديث حالة مؤقت (`on it`، و`pulling everything together`، وتلميحات مشابهة) ولم يعد أي تشغيل وكيل فرعي تابع مسؤولا عن الإجابة النهائية، يعيد OpenClaw المطالبة مرة واحدة للحصول على النتيجة الفعلية قبل التسليم.
- تفضل عمليات cron المعزولة بيانات تعريف رفض التنفيذ المنظمة من التشغيل المضمن، ثم تعود إلى علامات الملخص/المخرجات النهائية المعروفة مثل `SYSTEM_RUN_DENIED` و`INVALID_REQUEST`، حتى لا يبلغ عن أمر محظور كتشغيل ناجح.
- تتعامل عمليات cron المعزولة أيضا مع إخفاقات الوكيل على مستوى التشغيل كأخطاء مهمة حتى عندما لا تنتج حمولة رد، بحيث تزيد إخفاقات النموذج/الموفر عدادات الأخطاء وتطلق إشعارات الفشل بدلا من مسح المهمة كناجحة.
- عندما تصل مهمة دور وكيل معزولة إلى `timeoutSeconds`، يجهض cron تشغيل الوكيل الأساسي ويمنحه نافذة تنظيف قصيرة. إذا لم يفرغ التشغيل، يفرض تنظيف مملوك لـ Gateway مسح ملكية جلسة ذلك التشغيل قبل أن يسجل cron انتهاء المهلة، حتى لا يبقى عمل الدردشة في الطابور خلف جلسة معالجة قديمة.

تسوية المهام الخاصة بـ cron مملوكة لوقت التشغيل أولا، ومدعومة بالسجل الدائم ثانيا: تبقى مهمة cron النشطة مباشرة ما دام وقت تشغيل cron لا يزال يتتبع تلك المهمة كقيد التشغيل، حتى إذا كان صف جلسة فرعية قديم لا يزال موجودا. بمجرد أن يتوقف وقت التشغيل عن امتلاك المهمة وتنتهي نافذة السماح البالغة 5 دقائق، تتحقق الصيانة من سجلات التشغيل المحفوظة وحالة المهمة للتشغيل المطابق `cron:<jobId>:<startedAt>`. إذا أظهر ذلك السجل الدائم نتيجة نهائية، ينهى دفتر المهام منها؛ وإلا يمكن للصيانة المملوكة لـ Gateway تعليم المهمة كـ `lost`. يمكن لتدقيق CLI دون اتصال الاسترداد من السجل الدائم، لكنه لا يتعامل مع مجموعة المهام النشطة داخل العملية الفارغة الخاصة به كدليل على اختفاء تشغيل cron مملوك لـ Gateway.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%AC%D8%AF%D8%A7%D9%88%D9%84)  أنواع الجداول

| النوع | علم CLI | الوصف |
| --- | --- | --- |
| `at` | `--at` | طابع زمني لمرة واحدة (ISO 8601 أو نسبي مثل `20m`) |
| `every` | `--every` | فاصل زمني ثابت |
| `cron` | `--cron` | تعبير cron من 5 حقول أو 6 حقول مع `--tz` اختياري |

تعامل الطوابع الزمنية دون منطقة زمنية كـ UTC. أضف `--tz America/New_York` للجدولة حسب ساعة الحائط المحلية.توزع تعبيرات التكرار عند بداية الساعة تلقائيا بفارق يصل إلى 5 دقائق لتقليل قمم الحمل. استخدم `--exact` لفرض توقيت دقيق أو `--stagger 30s` لنافذة صريحة.

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D9%8A%D8%B3%D8%AA%D8%AE%D8%AF%D9%85-%D9%8A%D9%88%D9%85-%D8%A7%D9%84%D8%B4%D9%87%D8%B1-%D9%88%D9%8A%D9%88%D9%85-%D8%A7%D9%84%D8%A3%D8%B3%D8%A8%D9%88%D8%B9-%D9%85%D9%86%D8%B7%D9%82-or)  يستخدم يوم الشهر ويوم الأسبوع منطق OR

تحلل تعبيرات cron بواسطة [croner](https://github.com/Hexagon/croner). عندما يكون حقلا يوم الشهر ويوم الأسبوع كلاهما غير شاملين، يطابق croner عندما يطابق **أي** حقل منهما، وليس كلاهما. هذا هو سلوك Vixie cron القياسي.

```
# Intended: "9 AM on the 15th, only if it's a Monday"
# Actual:   "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 * 1
```

يشغل هذا نحو 5-6 مرات شهريا بدلا من 0-1 مرة شهريا. يستخدم OpenClaw هنا سلوك OR الافتراضي في Croner. لاشتراط تحقق الشرطين معا، استخدم معدّل يوم الأسبوع `+` في Croner (`0 9 15 * +1`) أو جدوله على حقل واحد وتحقق من الآخر في مطالبة المهمة أو أمرها.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A3%D8%B3%D8%A7%D9%84%D9%8A%D8%A8-%D8%A7%D9%84%D8%AA%D9%86%D9%81%D9%8A%D8%B0)  أساليب التنفيذ

| الأسلوب | قيمة `--session` | يعمل في | الأنسب لـ |
| --- | --- | --- | --- |
| الجلسة الرئيسية | `main` | دور Heartbeat التالي | التذكيرات، أحداث النظام |
| معزول | `isolated` | `cron:<jobId>` مخصص | التقارير، الأعمال الخلفية |
| الجلسة الحالية | `current` | مرتبط في وقت الإنشاء | العمل المتكرر الواعي بالسياق |
| جلسة مخصصة | `session:custom-id` | جلسة مسماة مستمرة | تدفقات العمل التي تبني على السجل |

الجلسة الرئيسية مقابل المعزولة مقابل المخصصة

تدرج مهام **الجلسة الرئيسية** حدث نظام في الطابور وتوقظ Heartbeat اختياريا (`--wake now` أو `--wake next-heartbeat`). لا تمد أحداث النظام هذه حداثة إعادة الضبط اليومية/الخاملة للجلسة المستهدفة. تعمل المهام **المعزولة** بدور وكيل مخصص مع جلسة جديدة. تستمر **الجلسات المخصصة** (`session:xxx`) بالسياق عبر عمليات التشغيل، مما يتيح تدفقات عمل مثل الاجتماعات اليومية التي تبني على الملخصات السابقة.

ما معنى 'جلسة جديدة' للمهام المعزولة

بالنسبة للمهام المعزولة، تعني “جلسة جديدة” معرف نص/جلسة جديدا لكل تشغيل. قد يحمل OpenClaw تفضيلات آمنة مثل إعدادات التفكير/السريع/المفصل، والتسميات، وتجاوزات النموذج/المصادقة الصريحة التي اختارها المستخدم، لكنه لا يرث سياق المحادثة المحيط من صف cron أقدم: توجيه القناة/المجموعة، سياسة الإرسال أو الطابور، الرفع، الأصل، أو ربط وقت تشغيل ACP. استخدم `current` أو `session:<id>` عندما يجب أن تبني مهمة متكررة عمدا على سياق المحادثة نفسه.

تنظيف وقت التشغيل

بالنسبة للمهام المعزولة، يتضمن تفكيك وقت التشغيل الآن تنظيف المتصفح بأفضل جهد لجلسة cron تلك. تتجاهل إخفاقات التنظيف حتى تبقى نتيجة cron الفعلية هي الحاكمة.تتخلص عمليات cron المعزولة أيضا من أي مثيلات وقت تشغيل MCP مضمّنة أنشئت للمهمة عبر مسار تنظيف وقت التشغيل المشترك. يطابق هذا طريقة تفكيك عملاء MCP للجلسة الرئيسية والجلسة المخصصة، لذلك لا تسرب مهام cron المعزولة عمليات stdio فرعية أو اتصالات MCP طويلة العمر عبر عمليات التشغيل.

الوكيل الفرعي وتسليم Discord

عندما تنسق عمليات cron المعزولة وكلاء فرعيين، يفضل التسليم أيضا مخرجات التابع النهائية على نص الوالد المؤقت القديم. إذا كان التابعون لا يزالون قيد التشغيل، يكبت OpenClaw تحديث الوالد الجزئي ذلك بدلا من إعلانه.بالنسبة لأهداف إعلان Discord النصية فقط، يرسل OpenClaw نص المساعد النهائي المعتمد مرة واحدة بدلا من إعادة تشغيل كل من حمولات النص المتدفقة/الوسيطة والإجابة النهائية. لا تزال حمولات Discord الوسائطية والمنظمة تسلم كحمولات منفصلة حتى لا تسقط المرفقات والمكونات.

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D8%AD%D9%85%D9%88%D9%84%D8%A9-%D9%84%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%B9%D8%B2%D9%88%D9%84%D8%A9)  خيارات الحمولة للمهام المعزولة

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-message)

--message

string

مطلوب

نص المطالبة (مطلوب للمعزول).

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-model)

--model

string

تجاوز النموذج؛ يستخدم النموذج المسموح المحدد للمهمة.

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-thinking)

--thinking

string

تجاوز مستوى التفكير.

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-light-context)

--light-context

boolean

تخطي حقن ملف تمهيد مساحة العمل.

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-tools)

--tools

string

تقييد الأدوات التي يمكن للمهمة استخدامها، على سبيل المثال `--tools exec,read`.

يستخدم `--model` النموذج المسموح المحدد كنموذج أساسي لتلك المهمة. ليس ذلك مثل تجاوز `/model` لجلسة دردشة: لا تزال سلاسل الرجوع المكونة تنطبق عندما يفشل النموذج الأساسي للمهمة. إذا لم يكن النموذج المطلوب مسموحا أو تعذر حله، يفشل cron التشغيل بخطأ تحقق صريح بدلا من الرجوع بصمت إلى اختيار نموذج الوكيل/النموذج الافتراضي للمهمة.يمكن لمهام Cron أيضا حمل `fallbacks` على مستوى الحمولة. عند وجودها، تستبدل تلك القائمة سلسلة الرجوع المكونة للمهمة. استخدم `fallbacks: []` في حمولة/واجهة برمجة تطبيقات المهمة عندما تريد تشغيل cron صارما يجرب النموذج المحدد فقط. إذا كانت للمهمة `--model` لكن لا توجد حمولات رجوع ولا رجوعات مكونة، يمرر OpenClaw تجاوز رجوع فارغا صريحا حتى لا يضاف النموذج الأساسي للوكيل كهدف إعادة محاولة إضافي مخفي.أسبقية اختيار النموذج للمهام المعزولة هي:

1. تجاوز نموذج خطاف Gmail (عندما يأتي التشغيل من Gmail ويكون ذلك التجاوز مسموحا)
2. `model` في حمولة كل مهمة
3. تجاوز نموذج جلسة cron المخزن الذي اختاره المستخدم
4. اختيار نموذج الوكيل/الافتراضي

يتبع الوضع السريع الاختيار الحي المحلول أيضا. إذا كان تكوين النموذج المحدد يحتوي على `params.fastMode`، يستخدم cron المعزول ذلك افتراضيا. ولا يزال تجاوز `fastMode` المخزن للجلسة يتغلب على التكوين في أي من الاتجاهين.إذا صادف تشغيل معزول تسليما لتبديل نموذج حي، يعيد cron المحاولة بالموفر/النموذج الذي تم التبديل إليه ويحفظ ذلك الاختيار الحي للتشغيل النشط قبل إعادة المحاولة. عندما يحمل التبديل أيضا ملف تعريف مصادقة جديدا، يحفظ cron تجاوز ملف تعريف المصادقة ذلك للتشغيل النشط أيضا. إعادة المحاولات محدودة: بعد المحاولة الأولية بالإضافة إلى محاولتي تبديل، يجهض cron بدلا من الدوران إلى الأبد.قبل أن يدخل تشغيل cron معزول إلى مشغل الوكيل، يتحقق OpenClaw من نقاط نهاية الموفر المحلي القابلة للوصول لموفري `api: "ollama"` و`api: "openai-completions"` المكونين الذين يكون `baseUrl` لديهم loopback أو شبكة خاصة أو `.local`. إذا كانت نقطة النهاية تلك معطلة، يسجل التشغيل كـ `skipped` مع خطأ موفر/نموذج واضح بدلا من بدء استدعاء نموذج. تخزن نتيجة نقطة النهاية في الذاكرة المؤقتة لمدة 5 دقائق، بحيث تشارك العديد من المهام المستحقة التي تستخدم خادم Ollama أو vLLM أو SGLang أو LM Studio المحلي المعطل نفسه مسبارا صغيرا واحدا بدلا من إنشاء عاصفة طلبات. لا تزيد عمليات تخطي الفحص المسبق للموفر من تراجع أخطاء التنفيذ؛ فعّل `failureAlert.includeSkipped` عندما تريد إشعارات تخطي متكررة.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D9%88%D8%A7%D9%84%D9%85%D8%AE%D8%B1%D8%AC%D8%A7%D8%AA)  التسليم والمخرجات

| الوضع | ما يحدث |
| --- | --- |
| `announce` | تسليم احتياطي للنص النهائي إلى الهدف إذا لم يرسل الوكيل |
| `webhook` | POST حمولة حدث منته إلى URL |
| `none` | لا يوجد تسليم احتياطي من المشغل |

استخدم `--announce --channel telegram --to "-1001234567890"` للتسليم إلى القناة. بالنسبة إلى موضوعات منتديات Telegram، استخدم `-1001234567890:topic:123`؛ ويمكن لمستدعي RPC/الإعدادات المباشرين أيضًا تمرير `delivery.threadId` كسلسلة نصية أو رقم. ينبغي أن تستخدم أهداف Slack/Discord/Mattermost بادئات صريحة (`channel:<id>`، `user:<id>`). معرّفات غرف Matrix حساسة لحالة الأحرف؛ استخدم معرّف الغرفة الدقيق أو صيغة `room:!room:server` من Matrix.عندما يستخدم تسليم الإعلان `channel: "last"` أو يحذف `channel`، يمكن لهدف ذي بادئة مزوّد مثل `telegram:123` أن يختار القناة قبل أن يرجع Cron إلى سجل الجلسة أو قناة واحدة مضبوطة. البادئات التي يعلنها Plugin المحمّل فقط هي محددات مزوّد. إذا كان `delivery.channel` صريحًا، فيجب أن تسمي بادئة الهدف المزوّد نفسه؛ على سبيل المثال، يُرفض `channel: "whatsapp"` مع `to: "telegram:123"` بدلًا من السماح لـ WhatsApp بتفسير معرّف Telegram كرقم هاتف. تظل بادئات نوع الهدف والخدمة مثل `channel:<id>` و`user:<id>` و`imessage:<handle>` و`sms:<number>` صيغة أهداف مملوكة للقناة، وليست محددات مزوّد.بالنسبة إلى المهام المعزولة، يكون تسليم المحادثة مشتركًا. إذا كان مسار محادثة متاحًا، فيمكن للوكيل استخدام أداة `message` حتى عندما تستخدم المهمة `--no-deliver`. إذا أرسل الوكيل إلى الهدف المضبوط/الحالي، يتخطى OpenClaw إعلان الرجوع الاحتياطي. بخلاف ذلك، تتحكم `announce` و`webhook` و`none` فقط فيما يفعله المشغّل بالرد النهائي بعد دورة الوكيل.عندما ينشئ وكيل تذكيرًا معزولًا من محادثة نشطة، يخزّن OpenClaw هدف التسليم الحي المحفوظ لمسار إعلان الرجوع الاحتياطي. قد تكون مفاتيح الجلسة الداخلية بأحرف صغيرة؛ ولا تُعاد صياغة أهداف تسليم المزوّد من تلك المفاتيح عندما يكون سياق المحادثة الحالي متاحًا.يستخدم تسليم الإعلان الضمني قوائم السماح المضبوطة للقنوات للتحقق من الأهداف القديمة وإعادة توجيهها. موافقات مخزن إقران الرسائل المباشرة ليست مستلمي أتمتة احتياطيين؛ اضبط `delivery.to` أو اضبط إدخال `allowFrom` للقناة عندما ينبغي لمهمة مجدولة أن ترسل استباقيًا إلى رسالة مباشرة.تتبع إشعارات الفشل مسار وجهة منفصلًا:

- يضبط `cron.failureDestination` افتراضيًا عامًا لإشعارات الفشل.
- يتجاوزه `job.delivery.failureDestination` لكل مهمة.
- إذا لم يُضبط أي منهما وكانت المهمة تُسلّم بالفعل عبر `announce`، فإن إشعارات الفشل ترجع الآن إلى هدف الإعلان الأساسي ذلك.
- لا يُدعم `delivery.failureDestination` إلا في مهام `sessionTarget="isolated"` ما لم يكن وضع التسليم الأساسي هو `webhook`.
- يؤدي `failureAlert.includeSkipped: true` إلى إدخال مهمة أو سياسة تنبيه Cron عامة في تنبيهات التشغيلات المتخطاة المتكررة. تحتفظ التشغيلات المتخطاة بعدّاد تخطٍ متتالٍ منفصل، لذلك لا تؤثر في التراجع الخاص بأخطاء التنفيذ.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A3%D9%85%D8%AB%D9%84%D8%A9-cli)  أمثلة CLI

- تذكير لمرة واحدة

- مهمة معزولة متكررة

- تجاوز النموذج والتفكير


```
openclaw cron add \
  --name "Calendar check" \
  --at "20m" \
  --session main \
  --system-event "Next heartbeat: check calendar." \
  --wake now
```

```
openclaw cron add \
  --name "Morning brief" \
  --cron "0 7 * * *" \
  --tz "America/Los_Angeles" \
  --session isolated \
  --message "Summarize overnight updates." \
  --announce \
  --channel slack \
  --to "channel:C1234567890"
```

```
openclaw cron add \
  --name "Deep analysis" \
  --cron "0 6 * * 1" \
  --tz "America/Los_Angeles" \
  --session isolated \
  --message "Weekly deep analysis of project progress." \
  --model "opus" \
  --thinking high \
  --announce
```

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#webhooks)  Webhooks

يمكن لـ Gateway كشف نقاط نهاية Webhook عبر HTTP للمحفزات الخارجية. فعّل ذلك في الإعدادات:

```
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
  },
}
```

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A7%D9%84%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9)  المصادقة

يجب أن يتضمن كل طلب رمز الخطاف عبر ترويسة:

- `Authorization: Bearer <token>` (موصى به)
- `x-openclaw-token: <token>`

تُرفض رموز سلسلة الاستعلام.

POST /hooks/wake

أدرج حدث نظام للجلسة الرئيسية في قائمة الانتظار:

```
curl -X POST http://127.0.0.1:18789/hooks/wake \
  -H 'Authorization: Bearer SECRET' \
  -H 'Content-Type: application/json' \
  -d '{"text":"New email received","mode":"now"}'
```

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-text)

text

string

مطلوب

وصف الحدث.

[​](https://docs.openclaw.ai/ar/automation/cron-jobs#param-mode)

mode

string

افتراضي:"now"

`now` أو `next-heartbeat`.

POST /hooks/agent

شغّل دورة وكيل معزولة:

```
curl -X POST http://127.0.0.1:18789/hooks/agent \
  -H 'Authorization: Bearer SECRET' \
  -H 'Content-Type: application/json' \
  -d '{"message":"Summarize inbox","name":"Email","model":"openai/gpt-5.4"}'
```

الحقول: `message` (مطلوب)، `name`، `agentId`، `wakeMode`، `deliver`، `channel`، `to`، `model`، `fallbacks`، `thinking`، `timeoutSeconds`.

الخطافات المعيّنة (POST /hooks/<name>)

تُحل أسماء الخطافات المخصصة عبر `hooks.mappings` في الإعدادات. يمكن للتعيينات تحويل أي حمولات إلى إجراءات `wake` أو `agent` باستخدام قوالب أو تحويلات برمجية.

أبقِ نقاط نهاية الخطافات خلف local loopback أو tailnet أو وكيل عكسي موثوق.

- استخدم رمز خطاف مخصصًا؛ لا تعد استخدام رموز مصادقة Gateway.
- أبقِ `hooks.path` على مسار فرعي مخصص؛ يُرفض `/`.
- اضبط `hooks.allowedAgentIds` لتقييد توجيه `agentId` الصريح.
- أبقِ `hooks.allowRequestSessionKey=false` ما لم تكن تحتاج إلى جلسات يختارها المستدعي.
- إذا فعّلت `hooks.allowRequestSessionKey`، فاضبط أيضًا `hooks.allowedSessionKeyPrefixes` لتقييد أشكال مفاتيح الجلسات المسموح بها.
- تُغلّف حمولات الخطافات بحدود أمان افتراضيًا.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%AA%D9%83%D8%A7%D9%85%D9%84-gmail-pubsub)  تكامل Gmail PubSub

اربط محفزات صندوق وارد Gmail بـ OpenClaw عبر Google PubSub.

**المتطلبات الأساسية:**`gcloud` CLI، و`gog` (gogcli)، وخطافات OpenClaw مفعّلة، وTailscale لنقطة نهاية HTTPS العامة.

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC-%D9%85%D9%88%D8%B5%D9%89-%D8%A8%D9%87)  إعداد المعالج (موصى به)

```
openclaw webhooks gmail setup --account openclaw@gmail.com
```

يكتب هذا إعدادات `hooks.gmail`، ويفعّل الإعداد المسبق لـ Gmail، ويستخدم Tailscale Funnel لنقطة نهاية الدفع.

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A8%D8%AF%D8%A1-gateway-%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D9%8B%D8%A7)  بدء Gateway تلقائيًا

عندما يكون `hooks.enabled=true` و`hooks.gmail.account` مضبوطًا، يبدأ Gateway تشغيل `gog gmail watch serve` عند الإقلاع ويجدد المراقبة تلقائيًا. اضبط `OPENCLAW_SKIP_GMAIL_WATCHER=1` لإلغاء الاشتراك.

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D9%8A%D8%AF%D9%88%D9%8A-%D9%84%D9%85%D8%B1%D8%A9-%D9%88%D8%A7%D8%AD%D8%AF%D8%A9)  إعداد يدوي لمرة واحدة

1

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

اختر مشروع GCP

اختر مشروع GCP الذي يملك عميل OAuth المستخدم بواسطة `gog`:

```
gcloud auth login
gcloud config set project <project-id>
gcloud services enable gmail.googleapis.com pubsub.googleapis.com
```

2

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

أنشئ الموضوع وامنح Gmail وصول الدفع

```
gcloud pubsub topics create gog-gmail-watch
gcloud pubsub topics add-iam-policy-binding gog-gmail-watch \
  --member=serviceAccount:gmail-api-push@system.gserviceaccount.com \
  --role=roles/pubsub.publisher
```

3

[Navigate to header](https://docs.openclaw.ai/ar/automation/cron-jobs#)

ابدأ المراقبة

```
gog gmail watch start \
  --account openclaw@gmail.com \
  --label INBOX \
  --topic projects/<project-id>/topics/gog-gmail-watch
```

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%AA%D8%AC%D8%A7%D9%88%D8%B2-%D9%86%D9%85%D9%88%D8%B0%D8%AC-gmail)  تجاوز نموذج Gmail

```
{
  hooks: {
    gmail: {
      model: "openrouter/meta-llama/llama-3.3-70b-instruct:free",
      thinking: "off",
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A5%D8%AF%D8%A7%D8%B1%D8%A9-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  إدارة المهام

```
# List all jobs
openclaw cron list

# Show one job, including resolved delivery route
openclaw cron show <jobId>

# Edit a job
openclaw cron edit <jobId> --message "Updated prompt" --model "opus"

# Force run a job now
openclaw cron run <jobId>

# Run only if due
openclaw cron run <jobId> --due

# View run history
openclaw cron runs --id <jobId> --limit 50

# Delete a job
openclaw cron remove <jobId>

# Agent selection (multi-agent setups)
openclaw cron add --name "Ops sweep" --cron "0 6 * * *" --session isolated --message "Check ops queue" --agent ops
openclaw cron edit <jobId> --clear-agent
```

ملاحظة تجاوز النموذج:

- يغيّر `openclaw cron add|edit --model ...` النموذج المحدد للمهمة.
- إذا كان النموذج مسموحًا به، يصل ذلك المزوّد/النموذج الدقيق إلى تشغيل الوكيل المعزول.
- إذا لم يكن مسموحًا به أو تعذّر حله، يفشل Cron التشغيل بخطأ تحقق صريح.
- تظل سلاسل الرجوع الاحتياطي المضبوطة سارية لأن `--model` في Cron هو نموذج أساسي للمهمة، وليس تجاوز `/model` للجلسة.
- يستبدل `fallbacks` في الحمولة عمليات الرجوع الاحتياطي المضبوطة لتلك المهمة؛ وتعطّل `fallbacks: []` الرجوع الاحتياطي وتجعل التشغيل صارمًا.
- لا يسقط `--model` عادي دون قائمة رجوع احتياطي صريحة أو مضبوطة إلى النموذج الأساسي للوكيل كهدف إعادة محاولة إضافي صامت.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  الإعدادات

```
{
  cron: {
    enabled: true,
    store: "~/.openclaw/cron/jobs.json",
    maxConcurrentRuns: 1,
    retry: {
      maxAttempts: 3,
      backoffMs: [60000, 120000, 300000],
      retryOn: ["rate_limit", "overloaded", "network", "server_error"],
    },
    webhookToken: "replace-with-dedicated-webhook-token",
    sessionRetention: "24h",
    runLog: { maxBytes: "2mb", keepLines: 2000 },
  },
}
```

يحد `maxConcurrentRuns` من كل من إرسال Cron المجدول وتنفيذ دورة الوكيل المعزولة. تستخدم دورات وكلاء Cron المعزولة مسار التنفيذ المخصص `cron-nested` الخاص بقائمة الانتظار داخليًا، لذلك يتيح رفع هذه القيمة لتشغيلات Cron LLM المستقلة أن تتقدم بالتوازي بدلًا من بدء أغلفة Cron الخارجية فقط. لا يتم توسيع مسار `nested` المشترك غير الخاص بـ Cron عبر هذا الإعداد.تُشتق حاوية حالة وقت التشغيل الجانبية من `cron.store`: فمخزن `.json` مثل `~/clawd/cron/jobs.json` يستخدم `~/clawd/cron/jobs-state.json`، بينما يضيف مسار المخزن الذي لا ينتهي بلاحقة `.json` اللاحقة `-state.json`.إذا عدّلت `jobs.json` يدويًا، فاترك `jobs-state.json` خارج التحكم بالمصدر. يستخدم OpenClaw هذه الحاوية الجانبية للخانات المعلقة، والعلامات النشطة، وبيانات آخر تشغيل الوصفية، وهوية الجدولة التي تخبر المجدول متى تحتاج مهمة معدّلة خارجيًا إلى `nextRunAtMs` جديد.تعطيل Cron: `cron.enabled: false` أو `OPENCLAW_SKIP_CRON=1`.

سلوك إعادة المحاولة

**إعادة محاولة لمرة واحدة**: تُعاد محاولة الأخطاء العابرة (حد المعدل، التحميل الزائد، الشبكة، خطأ الخادم) حتى 3 مرات مع تراجع أسي. الأخطاء الدائمة تُعطّل فورًا.**إعادة محاولة متكررة**: تراجع أسي (من 30 ثانية إلى 60 دقيقة) بين المحاولات. يُعاد ضبط التراجع بعد التشغيل الناجح التالي.

الصيانة

يزيل `cron.sessionRetention` (الافتراضي `24h`) إدخالات جلسات التشغيل المعزولة القديمة. تنظّف `cron.runLog.maxBytes` / `cron.runLog.keepLines` ملفات سجل التشغيل تلقائيًا.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

### [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%B3%D9%84%D9%85-%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)  سلم الأوامر

```
openclaw status
openclaw gateway status
openclaw cron status
openclaw cron list
openclaw cron runs --id <jobId> --limit 20
openclaw system heartbeat last
openclaw logs --follow
openclaw doctor
```

Cron لا يعمل

- تحقق من `cron.enabled` ومتغير البيئة `OPENCLAW_SKIP_CRON`.
- تأكد من أن Gateway يعمل باستمرار.
- بالنسبة إلى جداول `cron`، تحقق من المنطقة الزمنية (`--tz`) مقارنةً بالمنطقة الزمنية للمضيف.
- يعني `reason: not-due` في مخرجات التشغيل أن التشغيل اليدوي فُحص باستخدام `openclaw cron run <jobId> --due` وأن المهمة لم يحن موعدها بعد.

تم تشغيل Cron لكن لم يتم التسليم

- يعني وضع التسليم `none` أنه لا يُتوقع إرسال احتياطي من المشغّل. ولا يزال بإمكان الوكيل الإرسال مباشرةً باستخدام أداة `message` عند توفر مسار دردشة.
- يعني هدف التسليم المفقود/غير الصالح (`channel`/`to`) أنه تم تخطي الإرسال الصادر.
- بالنسبة إلى Matrix، قد تفشل المهام المنسوخة أو القديمة التي تحتوي على معرّفات غرف `delivery.to` بأحرف صغيرة لأن معرّفات غرف Matrix حساسة لحالة الأحرف. عدّل المهمة إلى قيمة `!room:server` أو `room:!room:server` الدقيقة من Matrix.
- تعني أخطاء مصادقة القناة (`unauthorized`، `Forbidden`) أن التسليم حُظر بسبب بيانات الاعتماد.
- إذا أعاد التشغيل المعزول الرمز الصامت فقط (`NO_REPLY` / `no_reply`)، فإن OpenClaw يمنع التسليم الصادر المباشر ويمنع أيضًا مسار الملخص الاحتياطي المدرج في قائمة الانتظار، لذلك لا يُنشر أي شيء مرة أخرى في الدردشة.
- إذا كان ينبغي للوكيل أن يراسل المستخدم بنفسه، فتحقق من أن المهمة لديها مسار قابل للاستخدام (`channel: "last"` مع دردشة سابقة، أو قناة/هدف صريح).

يبدو أن Cron أو Heartbeat يمنع انتقال /new-style

- لا تستند حداثة إعادة التعيين اليومية وعند الخمول إلى `updatedAt`؛ راجع [إدارة الجلسات](https://docs.openclaw.ai/ar/concepts/session#session-lifecycle).
- قد تحدّث إيقاظات Cron، وتشغيلات Heartbeat، وإشعارات exec، ومسك سجلات Gateway صف الجلسة لأغراض التوجيه/الحالة، لكنها لا تمدد `sessionStartedAt` أو `lastInteractionAt`.
- بالنسبة إلى الصفوف القديمة التي أُنشئت قبل وجود هذه الحقول، يمكن لـ OpenClaw استرداد `sessionStartedAt` من ترويسة جلسة JSONL في النص عندما يظل الملف متاحًا. تستخدم صفوف الخمول القديمة التي لا تحتوي على `lastInteractionAt` وقت البدء المسترد هذا كخط أساس للخمول.

محاذير المنطقة الزمنية

- يستخدم Cron من دون `--tz` المنطقة الزمنية لمضيف Gateway.
- تُعامل جداول `at` التي لا تحتوي على منطقة زمنية على أنها UTC.
- يستخدم `activeHours` في Heartbeat حل المنطقة الزمنية المُكوّن.

## [​](https://docs.openclaw.ai/ar/automation/cron-jobs\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [الأتمتة والمهام](https://docs.openclaw.ai/ar/automation) — جميع آليات الأتمتة في لمحة
- [مهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks) — سجل المهام لتنفيذات Cron
- [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat) — أدوار الجلسة الرئيسية الدورية
- [المنطقة الزمنية](https://docs.openclaw.ai/ar/concepts/timezone) — تكوين المنطقة الزمنية

[الأتمتة والمهام](https://docs.openclaw.ai/ar/automation) [Background tasks](https://docs.openclaw.ai/ar/automation/tasks)

Ctrl+I