---
source_url: https://docs.openclaw.ai/ar/automation
title: "\u0627\u0644\u0623\u062a\u0645\u062a\u0629 \u0648\u0627\u0644\u0645\u0647\u0627\u0645 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Automation and tasks

الأتمتة والمهام

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [دليل اتخاذ القرار السريع](https://docs.openclaw.ai/ar/automation#%D8%AF%D9%84%D9%8A%D9%84-%D8%A7%D8%AA%D8%AE%D8%A7%D8%B0-%D8%A7%D9%84%D9%82%D8%B1%D8%A7%D8%B1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [المهام المجدولة (Cron) مقابل Heartbeat](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AC%D8%AF%D9%88%D9%84%D8%A9-cron-%D9%85%D9%82%D8%A7%D8%A8%D9%84-heartbeat)
- [المفاهيم الأساسية](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D9%85%D9%81%D8%A7%D9%87%D9%8A%D9%85-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)
- [المهام المجدولة (cron)](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AC%D8%AF%D9%88%D9%84%D8%A9-cron)
- [المهام](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [الالتزامات المستنتجة](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D8%A7%D9%84%D8%AA%D8%B2%D8%A7%D9%85%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%86%D8%AA%D8%AC%D8%A9)
- [Task Flow](https://docs.openclaw.ai/ar/automation#task-flow)
- [الأوامر الدائمة](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9)
- [الخطافات](https://docs.openclaw.ai/ar/automation#%D8%A7%D9%84%D8%AE%D8%B7%D8%A7%D9%81%D8%A7%D8%AA)
- [Heartbeat](https://docs.openclaw.ai/ar/automation#heartbeat)
- [كيف تعمل معًا](https://docs.openclaw.ai/ar/automation#%D9%83%D9%8A%D9%81-%D8%AA%D8%B9%D9%85%D9%84-%D9%85%D8%B9%D9%8B%D8%A7)
- [ذات صلة](https://docs.openclaw.ai/ar/automation#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

يشغّل OpenClaw العمل في الخلفية عبر المهام، والوظائف المجدولة، والالتزامات المستنتجة، وخطافات الأحداث، والتعليمات الدائمة. تساعدك هذه الصفحة على اختيار الآلية المناسبة وفهم كيفية تكاملها معًا.

## [​](https://docs.openclaw.ai/ar/automation\#%D8%AF%D9%84%D9%8A%D9%84-%D8%A7%D8%AA%D8%AE%D8%A7%D8%B0-%D8%A7%D9%84%D9%82%D8%B1%D8%A7%D8%B1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  دليل اتخاذ القرار السريع

| حالة الاستخدام | الموصى به | السبب |
| --- | --- | --- |
| إرسال تقرير يومي في التاسعة صباحًا تمامًا | المهام المجدولة (Cron) | توقيت دقيق وتنفيذ معزول |
| ذكّرني بعد 20 دقيقة | المهام المجدولة (Cron) | تشغيل لمرة واحدة بتوقيت دقيق (`--at`) |
| تشغيل تحليل عميق أسبوعي | المهام المجدولة (Cron) | مهمة مستقلة، ويمكن أن تستخدم نموذجًا مختلفًا |
| فحص صندوق الوارد كل 30 دقيقة | Heartbeat | يجمعها مع فحوصات أخرى، وواعٍ بالسياق |
| مراقبة التقويم للأحداث القادمة | Heartbeat | ملاءمة طبيعية للوعي الدوري |
| المتابعة بعد مقابلة مذكورة | الالتزامات المستنتجة | متابعة شبيهة بالذاكرة، من دون طلب تذكير دقيق |
| تسجيل متابعة لطيف للرعاية بعد سياق المستخدم | الالتزامات المستنتجة | مقيّد بالوكيل والقناة نفسيهما |
| فحص حالة وكيل فرعي أو تشغيل ACP | مهام الخلفية | سجل المهام يتتبع كل الأعمال المنفصلة |
| تدقيق ما شُغّل ومتى | مهام الخلفية | `openclaw tasks list` و`openclaw tasks audit` |
| بحث متعدد الخطوات ثم تلخيص | Task Flow | تنسيق دائم مع تتبع المراجعات |
| تشغيل سكربت عند إعادة ضبط الجلسة | الخطافات | مدفوع بالأحداث، ويُطلق عند أحداث دورة الحياة |
| تنفيذ كود عند كل استدعاء أداة | خطافات Plugin | يمكن للخطافات داخل العملية اعتراض استدعاءات الأدوات |
| تحقق دائمًا من الامتثال قبل الرد | الأوامر الدائمة | تُحقن تلقائيًا في كل جلسة |

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AC%D8%AF%D9%88%D9%84%D8%A9-cron-%D9%85%D9%82%D8%A7%D8%A8%D9%84-heartbeat)  المهام المجدولة (Cron) مقابل Heartbeat

| البعد | المهام المجدولة (Cron) | Heartbeat |
| --- | --- | --- |
| التوقيت | دقيق (تعبيرات cron، لمرة واحدة) | تقريبي (افتراضيًا كل 30 دقيقة) |
| سياق الجلسة | جديد (معزول) أو مشترك | سياق الجلسة الرئيسية كاملًا |
| سجلات المهام | تُنشأ دائمًا | لا تُنشأ أبدًا |
| التسليم | قناة أو Webhook أو صامت | ضمن الجلسة الرئيسية |
| الأفضل لـ | التقارير، التذكيرات، وظائف الخلفية | فحوصات صندوق الوارد، التقويم، الإشعارات |

استخدم المهام المجدولة (Cron) عندما تحتاج إلى توقيت دقيق أو تنفيذ معزول. استخدم Heartbeat عندما يستفيد العمل من سياق الجلسة الكامل ويكون التوقيت التقريبي مقبولًا.

## [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D9%85%D9%81%D8%A7%D9%87%D9%8A%D9%85-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A%D8%A9)  المفاهيم الأساسية

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AC%D8%AF%D9%88%D9%84%D8%A9-cron)  المهام المجدولة (cron)

Cron هو المجدول المدمج في Gateway للتوقيت الدقيق. يستبقي الوظائف، ويوقظ الوكيل في الوقت المناسب، ويمكنه تسليم المخرجات إلى قناة دردشة أو نقطة نهاية Webhook. يدعم التذكيرات لمرة واحدة، والتعبيرات المتكررة، ومحفزات Webhook الواردة.راجع [المهام المجدولة](https://docs.openclaw.ai/ar/automation/cron-jobs).

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  المهام

يتتبع سجل مهام الخلفية كل الأعمال المنفصلة: تشغيلات ACP، وإنشاءات الوكلاء الفرعيين، وتنفيذات cron المعزولة، وعمليات CLI. المهام سجلات وليست مجدولات. استخدم `openclaw tasks list` و`openclaw tasks audit` لفحصها.راجع [مهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks).

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D8%A7%D9%84%D8%AA%D8%B2%D8%A7%D9%85%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%86%D8%AA%D8%AC%D8%A9)  الالتزامات المستنتجة

الالتزامات هي ذكريات متابعة قصيرة العمر ومشروطة بالاشتراك. يستنتجها OpenClaw من المحادثات العادية، ويقيّدها بالوكيل والقناة نفسيهما، ويسلّم المتابعات المستحقة عبر Heartbeat. أما التذكيرات الدقيقة التي يطلبها المستخدم صراحةً فمكانها لا يزال cron.راجع [الالتزامات المستنتجة](https://docs.openclaw.ai/ar/concepts/commitments).

### [​](https://docs.openclaw.ai/ar/automation\#task-flow)  Task Flow

Task Flow هو طبقة تنسيق التدفقات فوق مهام الخلفية. يدير تدفقات متعددة الخطوات ودائمة مع أوضاع مزامنة مُدارة ومعكوسة، وتتبع المراجعات، و`openclaw tasks flow list|show|cancel` للفحص.راجع [Task Flow](https://docs.openclaw.ai/ar/automation/taskflow).

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9)  الأوامر الدائمة

تمنح الأوامر الدائمة الوكيل سلطة تشغيلية دائمة لبرامج محددة. تعيش في ملفات مساحة العمل (عادةً `AGENTS.md`) وتُحقن في كل جلسة. اجمعها مع cron للتنفيذ القائم على الوقت.راجع [الأوامر الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders).

### [​](https://docs.openclaw.ai/ar/automation\#%D8%A7%D9%84%D8%AE%D8%B7%D8%A7%D9%81%D8%A7%D8%AA)  الخطافات

الخطافات الداخلية هي سكربتات مدفوعة بالأحداث تُشغّلها أحداث دورة حياة الوكيل (`/new`، و`/reset`، و`/stop`)، وCompaction الجلسة، وبدء تشغيل Gateway، وتدفق الرسائل. تُكتشف تلقائيًا من الأدلة ويمكن إدارتها باستخدام `openclaw hooks`. لاعتراض استدعاءات الأدوات داخل العملية، استخدم [خطافات Plugin](https://docs.openclaw.ai/ar/plugins/hooks).راجع [الخطافات](https://docs.openclaw.ai/ar/automation/hooks).

### [​](https://docs.openclaw.ai/ar/automation\#heartbeat)  Heartbeat

Heartbeat هو دور دوري في الجلسة الرئيسية (افتراضيًا كل 30 دقيقة). يجمع عدة فحوصات (صندوق الوارد، التقويم، الإشعارات) في دور وكيل واحد مع سياق الجلسة الكامل. لا تُنشئ أدوار Heartbeat سجلات مهام ولا تمدد حداثة إعادة ضبط الجلسة اليومية/الخاملة. استخدم `HEARTBEAT.md` لقائمة تحقق صغيرة، أو كتلة `tasks:` عندما تريد فحوصات دورية مستحقة فقط داخل Heartbeat نفسه. تتجاوز ملفات Heartbeat الفارغة التنفيذ باسم `empty-heartbeat-file`؛ ويتجاوز وضع المهام المستحقة فقط التنفيذ باسم `no-tasks-due`. تؤجَّل Heartbeats أثناء نشاط عمل cron أو اصطفافه، ويمكن لـ`heartbeat.skipWhenBusy` أيضًا تأجيلها عندما تكون مسارات الوكيل الفرعي أو المسارات المتداخلة مشغولة.راجع [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat).

## [​](https://docs.openclaw.ai/ar/automation\#%D9%83%D9%8A%D9%81-%D8%AA%D8%B9%D9%85%D9%84-%D9%85%D8%B9%D9%8B%D8%A7)  كيف تعمل معًا

- يتعامل **Cron** مع الجداول الدقيقة (التقارير اليومية، المراجعات الأسبوعية) والتذكيرات لمرة واحدة. كل تنفيذات cron تُنشئ سجلات مهام.
- يتعامل **Heartbeat** مع المراقبة الروتينية (صندوق الوارد، التقويم، الإشعارات) في دور مجمّع واحد كل 30 دقيقة.
- تتفاعل **الخطافات** مع أحداث محددة (إعادة ضبط الجلسة، وCompaction، وتدفق الرسائل) باستخدام سكربتات مخصصة. تغطي خطافات Plugin استدعاءات الأدوات.
- تمنح **الأوامر الدائمة** الوكيل سياقًا مستمرًا وحدودًا للسلطة.
- ينسق **Task Flow** التدفقات متعددة الخطوات فوق المهام الفردية.
- تتتبع **المهام** تلقائيًا كل الأعمال المنفصلة كي تتمكن من فحصها وتدقيقها.

## [​](https://docs.openclaw.ai/ar/automation\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [المهام المجدولة](https://docs.openclaw.ai/ar/automation/cron-jobs) — الجدولة الدقيقة والتذكيرات لمرة واحدة
- [الالتزامات المستنتجة](https://docs.openclaw.ai/ar/concepts/commitments) — متابعات شبيهة بالذاكرة
- [مهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks) — سجل المهام لكل الأعمال المنفصلة
- [Task Flow](https://docs.openclaw.ai/ar/automation/taskflow) — تنسيق تدفقات متعددة الخطوات ودائمة
- [الخطافات](https://docs.openclaw.ai/ar/automation/hooks) — سكربتات دورة حياة مدفوعة بالأحداث
- [خطافات Plugin](https://docs.openclaw.ai/ar/plugins/hooks) — خطافات الأدوات والمطالبات والرسائل ودورة الحياة داخل العملية
- [الأوامر الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders) — تعليمات وكيل مستمرة
- [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat) — أدوار دورية في الجلسة الرئيسية
- [مرجع التهيئة](https://docs.openclaw.ai/ar/gateway/configuration-reference) — كل مفاتيح التهيئة

[OpenProse](https://docs.openclaw.ai/ar/prose) [Scheduled tasks](https://docs.openclaw.ai/ar/automation/cron-jobs)

Ctrl+I