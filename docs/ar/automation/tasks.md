---
source_url: https://docs.openclaw.ai/ar/automation/tasks
title: "\u0645\u0647\u0627\u0645 \u0627\u0644\u062e\u0644\u0641\u064a\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation/tasks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Automation and tasks

مهام الخلفية

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [باختصار](https://docs.openclaw.ai/ar/automation/tasks#%D8%A8%D8%A7%D8%AE%D8%AA%D8%B5%D8%A7%D8%B1)
- [بداية سريعة](https://docs.openclaw.ai/ar/automation/tasks#%D8%A8%D8%AF%D8%A7%D9%8A%D8%A9-%D8%B3%D8%B1%D9%8A%D8%B9%D8%A9)
- [ما الذي ينشئ مهمة](https://docs.openclaw.ai/ar/automation/tasks#%D9%85%D8%A7-%D8%A7%D9%84%D8%B0%D9%8A-%D9%8A%D9%86%D8%B4%D8%A6-%D9%85%D9%87%D9%85%D8%A9)
- [دورة حياة المهمة](https://docs.openclaw.ai/ar/automation/tasks#%D8%AF%D9%88%D8%B1%D8%A9-%D8%AD%D9%8A%D8%A7%D8%A9-%D8%A7%D9%84%D9%85%D9%87%D9%85%D8%A9)
- [التسليم والإشعارات](https://docs.openclaw.ai/ar/automation/tasks#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D9%88%D8%A7%D9%84%D8%A5%D8%B4%D8%B9%D8%A7%D8%B1%D8%A7%D8%AA)
- [سياسات الإشعار](https://docs.openclaw.ai/ar/automation/tasks#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A7%D8%AA-%D8%A7%D9%84%D8%A5%D8%B4%D8%B9%D8%A7%D8%B1)
- [مرجع CLI](https://docs.openclaw.ai/ar/automation/tasks#%D9%85%D8%B1%D8%AC%D8%B9-cli)
- [لوحة مهام المحادثة (/tasks)](https://docs.openclaw.ai/ar/automation/tasks#%D9%84%D9%88%D8%AD%D8%A9-%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AD%D8%A7%D8%AF%D8%AB%D8%A9-%2Ftasks)
- [تكامل الحالة (ضغط المهام)](https://docs.openclaw.ai/ar/automation/tasks#%D8%AA%D9%83%D8%A7%D9%85%D9%84-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9-%D8%B6%D8%BA%D8%B7-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [التخزين والصيانة](https://docs.openclaw.ai/ar/automation/tasks#%D8%A7%D9%84%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D9%88%D8%A7%D9%84%D8%B5%D9%8A%D8%A7%D9%86%D8%A9)
- [أين تعيش المهام](https://docs.openclaw.ai/ar/automation/tasks#%D8%A3%D9%8A%D9%86-%D8%AA%D8%B9%D9%8A%D8%B4-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)
- [الصيانة التلقائية](https://docs.openclaw.ai/ar/automation/tasks#%D8%A7%D9%84%D8%B5%D9%8A%D8%A7%D9%86%D8%A9-%D8%A7%D9%84%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D8%A9)
- [كيف ترتبط المهام بالأنظمة الأخرى](https://docs.openclaw.ai/ar/automation/tasks#%D9%83%D9%8A%D9%81-%D8%AA%D8%B1%D8%AA%D8%A8%D8%B7-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A8%D8%A7%D9%84%D8%A3%D9%86%D8%B8%D9%85%D8%A9-%D8%A7%D9%84%D8%A3%D8%AE%D8%B1%D9%89)
- [ذات صلة](https://docs.openclaw.ai/ar/automation/tasks#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

هل تبحث عن الجدولة؟ راجع [الأتمتة والمهام](https://docs.openclaw.ai/ar/automation) لاختيار الآلية المناسبة. هذه الصفحة هي سجل النشاط لأعمال الخلفية، وليست المجدول.

تتتبع مهام الخلفية الأعمال التي تعمل **خارج جلسة محادثتك الرئيسية**: تشغيلات ACP، وإنشاءات الوكلاء الفرعيين، وتنفيذات مهام Cron المعزولة، والعمليات التي يبدأها CLI.لا تستبدل المهام الجلسات أو مهام Cron أو Heartbeat — بل هي **سجل النشاط** الذي يسجل العمل المنفصل الذي حدث، ومتى حدث، وما إذا كان قد نجح.

لا ينشئ كل تشغيل للوكيل مهمة. دورات Heartbeat والدردشة التفاعلية العادية لا تفعل ذلك. كل تنفيذات Cron، وإنشاءات ACP، وإنشاءات الوكلاء الفرعيين، وأوامر وكيل CLI تفعل ذلك.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A8%D8%A7%D8%AE%D8%AA%D8%B5%D8%A7%D8%B1)  باختصار

- المهام هي **سجلات**، وليست مجدولات — يقرر Cron وHeartbeat _متى_ يعمل العمل، وتتتبع المهام _ما حدث_.
- ينشئ ACP والوكلاء الفرعيون وكل مهام Cron وعمليات CLI مهام. دورات Heartbeat لا تفعل ذلك.
- تنتقل كل مهمة عبر `queued → running → terminal` (succeeded أو failed أو timed\_out أو cancelled أو lost).
- تبقى مهام Cron نشطة ما دام وقت تشغيل Cron لا يزال يملك المهمة؛ إذا اختفت
حالة وقت التشغيل في الذاكرة، تتحقق صيانة المهام أولا من سجل تشغيلات Cron
الدائم قبل وسم المهمة بأنها مفقودة.
- الإكمال مدفوع بالدفع: يمكن للعمل المنفصل أن يرسل إشعارا مباشرة أو يوقظ
جلسة الطالب/Heartbeat عند الانتهاء، لذلك تكون حلقات استطلاع الحالة
عادة بالشكل غير المناسب.
- تحاول تشغيلات Cron المعزولة وإكمالات الوكلاء الفرعيين، قدر الإمكان، تنظيف علامات تبويب/عمليات المتصفح المتتبعة لجلساتها الفرعية قبل محاسبة التنظيف النهائية.
- يمنع تسليم Cron المعزول الردود المرحلية القديمة من الأصل بينما لا يزال عمل الوكلاء الفرعيين اللاحقين قيد التصريف، ويفضل خرج اللاحق النهائي عندما يصل قبل التسليم.
- يتم تسليم إشعارات الإكمال مباشرة إلى قناة أو وضعها في قائمة الانتظار حتى Heartbeat التالية.
- يعرض `openclaw tasks list` كل المهام؛ ويكشف `openclaw tasks audit` المشكلات.
- يتم الاحتفاظ بالسجلات النهائية لمدة 7 أيام، ثم تزال تلقائيا.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A8%D8%AF%D8%A7%D9%8A%D8%A9-%D8%B3%D8%B1%D9%8A%D8%B9%D8%A9)  بداية سريعة

- List and filter

- Inspect

- Cancel and notify

- Audit and maintenance

- Task flow


```
# List all tasks (newest first)
openclaw tasks list

# Filter by runtime or status
openclaw tasks list --runtime acp
openclaw tasks list --status running
```

```
# Show details for a specific task (by ID, run ID, or session key)
openclaw tasks show <lookup>
```

```
# Cancel a running task (kills the child session)
openclaw tasks cancel <lookup>

# Change notification policy for a task
openclaw tasks notify <lookup> state_changes
```

```
# Run a health audit
openclaw tasks audit

# Preview or apply maintenance
openclaw tasks maintenance
openclaw tasks maintenance --apply
```

```
# Inspect TaskFlow state
openclaw tasks flow list
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D9%85%D8%A7-%D8%A7%D9%84%D8%B0%D9%8A-%D9%8A%D9%86%D8%B4%D8%A6-%D9%85%D9%87%D9%85%D8%A9)  ما الذي ينشئ مهمة

| المصدر | نوع وقت التشغيل | متى يتم إنشاء سجل مهمة | سياسة الإشعار الافتراضية |
| --- | --- | --- | --- |
| تشغيلات ACP في الخلفية | `acp` | إنشاء جلسة ACP فرعية | `done_only` |
| تنسيق الوكلاء الفرعيين | `subagent` | إنشاء وكيل فرعي عبر `sessions_spawn` | `done_only` |
| مهام Cron (كل الأنواع) | `cron` | كل تنفيذ Cron (الجلسة الرئيسية والمعزول) | `silent` |
| عمليات CLI | `cli` | أوامر `openclaw agent` التي تعمل عبر Gateway | `silent` |
| مهام وسائط الوكيل | `cli` | تشغيلات `music_generate`/`video_generate` المدعومة بجلسة | `silent` |

Notify defaults for cron and media

تستخدم مهام Cron في الجلسة الرئيسية سياسة إشعار `silent` افتراضيا — فهي تنشئ سجلات للتتبع لكنها لا تولد إشعارات. تستخدم مهام Cron المعزولة أيضا `silent` افتراضيا لكنها أوضح لأنها تعمل في جلستها الخاصة.تستخدم تشغيلات `music_generate` و`video_generate` المدعومة بجلسة أيضا سياسة إشعار `silent`. لا تزال تنشئ سجلات مهام، لكن الإكمال يعاد إلى جلسة الوكيل الأصلية كإيقاظ داخلي كي يتمكن الوكيل من كتابة رسالة المتابعة وإرفاق الوسائط المكتملة بنفسه. إذا اخترت `tools.media.asyncCompletion.directSend`، يمكن لإكمالات `video_generate` غير المتزامنة أن تحاول التسليم المباشر إلى القناة أولا؛ تبقى إكمالات `music_generate` غير المتزامنة على مسار إيقاظ جلسة الطالب.

Concurrent video\_generate guardrail

بينما لا تزال مهمة `video_generate` المدعومة بجلسة نشطة، تعمل الأداة أيضا كحاجز حماية: ترجع استدعاءات `video_generate` المتكررة في الجلسة نفسها حالة المهمة النشطة بدلا من بدء توليد ثان متزامن. استخدم `action: "status"` عندما تريد بحث تقدم/حالة صريحا من جهة الوكيل.

What does not create tasks

- دورات Heartbeat — الجلسة الرئيسية؛ راجع [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat)
- دورات الدردشة التفاعلية العادية
- ردود `/command` المباشرة

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%AF%D9%88%D8%B1%D8%A9-%D8%AD%D9%8A%D8%A7%D8%A9-%D8%A7%D9%84%D9%85%D9%87%D9%85%D8%A9)  دورة حياة المهمة

| الحالة | ما تعنيه |
| --- | --- |
| `queued` | تم إنشاؤها، وتنتظر بدء الوكيل |
| `running` | دورة الوكيل قيد التنفيذ النشط |
| `succeeded` | اكتملت بنجاح |
| `failed` | اكتملت مع خطأ |
| `timed_out` | تجاوزت المهلة المضبوطة |
| `cancelled` | أوقفها المشغل عبر `openclaw tasks cancel` |
| `lost` | فقد وقت التشغيل حالة الإسناد الموثوقة بعد فترة سماح قدرها 5 دقائق |

تحدث الانتقالات تلقائيا — عندما ينتهي تشغيل الوكيل المرتبط، تحدث حالة المهمة لتطابقه.إكمال تشغيل الوكيل هو المرجع الموثوق لسجلات المهام النشطة. ينهي التشغيل المنفصل الناجح بالحالة `succeeded`، وتنهي أخطاء التشغيل العادية بالحالة `failed`، وتنهي نتائج انتهاء المهلة أو الإجهاض بالحالة `timed_out`. إذا كان المشغل قد ألغى المهمة بالفعل، أو كان وقت التشغيل قد سجل بالفعل حالة نهائية أقوى مثل `failed` أو `timed_out` أو `lost`، فلن تخفض إشارة نجاح لاحقة تلك الحالة النهائية.`lost` واعية بوقت التشغيل:

- مهام ACP: اختفت بيانات وصف جلسة ACP الفرعية الداعمة.
- مهام الوكلاء الفرعيين: اختفت الجلسة الفرعية الداعمة من مخزن الوكيل الهدف.
- مهام Cron: لم يعد وقت تشغيل Cron يتتبع المهمة على أنها نشطة ولا يظهر
سجل تشغيلات Cron الدائم نتيجة نهائية لذلك التشغيل. لا يتعامل تدقيق CLI
غير المتصل مع حالة وقت تشغيل Cron الفارغة داخل عمليته على أنها مرجعية.
- مهام CLI: تستخدم مهام الجلسات الفرعية المعزولة الجلسة الفرعية؛ أما مهام CLI
المدعومة بالدردشة فتستخدم سياق التشغيل الحي بدلا من ذلك، لذلك لا تبقي
صفوف جلسات القناة/المجموعة/المباشر العالقة هذه المهام حية. تنهي أيضا
تشغيلات `openclaw agent` المدعومة بـ Gateway من نتيجة تشغيلها، لذلك لا تبقى
التشغيلات المكتملة نشطة حتى يوسمها المنظف بأنها `lost`.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A7%D9%84%D8%AA%D8%B3%D9%84%D9%8A%D9%85-%D9%88%D8%A7%D9%84%D8%A5%D8%B4%D8%B9%D8%A7%D8%B1%D8%A7%D8%AA)  التسليم والإشعارات

عندما تصل مهمة إلى حالة نهائية، يخطرك OpenClaw. هناك مسارا تسليم:**التسليم المباشر** — إذا كان للمهمة هدف قناة (`requesterOrigin`)، تنتقل رسالة الإكمال مباشرة إلى تلك القناة (Telegram وDiscord وSlack وما إلى ذلك). بالنسبة لإكمالات الوكلاء الفرعيين، يحافظ OpenClaw أيضا على توجيه الخيط/الموضوع المرتبط عند توفره ويمكنه ملء `to` / الحساب المفقود من المسار المخزن لجلسة الطالب (`lastChannel` / `lastTo` / `lastAccountId`) قبل التخلي عن التسليم المباشر.**التسليم في قائمة انتظار الجلسة** — إذا فشل التسليم المباشر أو لم يتم تعيين أصل، يوضع التحديث في قائمة الانتظار كحدث نظام في جلسة الطالب ويظهر في Heartbeat التالية.

يحفز إكمال المهمة إيقاظ Heartbeat فوريا حتى ترى النتيجة بسرعة — لا يلزمك انتظار نبضة Heartbeat المجدولة التالية.

يعني ذلك أن سير العمل المعتاد قائم على الدفع: ابدأ العمل المنفصل مرة واحدة، ثم دع وقت التشغيل يوقظك أو يخطرك عند الإكمال. استطلع حالة المهمة فقط عندما تحتاج إلى التصحيح أو التدخل أو تدقيق صريح.

### [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%B3%D9%8A%D8%A7%D8%B3%D8%A7%D8%AA-%D8%A7%D9%84%D8%A5%D8%B4%D8%B9%D8%A7%D8%B1)  سياسات الإشعار

تحكم في مقدار ما تسمعه عن كل مهمة:

| السياسة | ما يتم تسليمه |
| --- | --- |
| `done_only` (افتراضي) | الحالة النهائية فقط (succeeded وfailed وما إلى ذلك) — **هذا هو الافتراضي** |
| `state_changes` | كل انتقال حالة وتحديث تقدم |
| `silent` | لا شيء إطلاقا |

غير السياسة أثناء تشغيل مهمة:

```
openclaw tasks notify <lookup> state_changes
```

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D9%85%D8%B1%D8%AC%D8%B9-cli)  مرجع CLI

tasks list

```
openclaw tasks list [--runtime <acp|subagent|cron|cli>] [--status <status>] [--json]
```

أعمدة الخرج: معرف المهمة، النوع، الحالة، التسليم، معرف التشغيل، الجلسة الفرعية، الملخص.

tasks show

```
openclaw tasks show <lookup>
```

يقبل رمز البحث معرف مهمة أو معرف تشغيل أو مفتاح جلسة. يعرض السجل الكامل بما في ذلك التوقيت وحالة التسليم والخطأ والملخص النهائي.

tasks cancel

```
openclaw tasks cancel <lookup>
```

بالنسبة لمهام ACP والوكلاء الفرعيين، يقتل هذا الجلسة الفرعية. بالنسبة للمهام التي يتتبعها CLI، يسجل الإلغاء في سجل المهام (لا يوجد مقبض وقت تشغيل فرعي منفصل). تنتقل الحالة إلى `cancelled` ويتم إرسال إشعار تسليم عند الاقتضاء.

tasks notify

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

tasks audit

```
openclaw tasks audit [--json]
```

يكشف المشكلات التشغيلية. تظهر النتائج أيضا في `openclaw status` عند اكتشاف مشكلات.

| النتيجة | الخطورة | المحفز |
| --- | --- | --- |
| `stale_queued` | warn | في قائمة الانتظار لأكثر من 10 دقائق |
| `stale_running` | error | قيد التشغيل لأكثر من 30 دقيقة |
| `lost` | warn/error | اختفت ملكية المهمة المدعومة بوقت التشغيل؛ تبقى المهام المفقودة المحتفَظ بها كتحذيرات حتى `cleanupAfter`، ثم تصبح أخطاء |
| `delivery_failed` | warn | فشل التسليم وسياسة الإشعار ليست `silent` |
| `missing_cleanup` | warn | مهمة نهائية بلا طابع زمني للتنظيف |
| `inconsistent_timestamps` | warn | مخالفة في المخطط الزمني (مثلًا انتهت قبل أن تبدأ) |

صيانة المهام

```
openclaw tasks maintenance [--json]
openclaw tasks maintenance --apply [--json]
```

استخدم هذا لمعاينة أو تطبيق التسوية، وختم التنظيف، والتقليم للمهام وحالة تدفق المهام.التسوية واعية بوقت التشغيل:

- تتحقق مهام ACP/الوكيل الفرعي من جلسة الطفل الداعمة لها.
- تُوسم مهام الوكيل الفرعي التي تحتوي جلسة الطفل الخاصة بها على شاهد قبر لاسترداد إعادة التشغيل كمفقودة بدلًا من معاملتها كجلسات داعمة قابلة للاسترداد.
- تتحقق مهام Cron مما إذا كان وقت تشغيل cron ما زال يملك المهمة، ثم تستعيد الحالة النهائية من سجلات تشغيل cron المستمرة/حالة المهمة قبل الرجوع إلى `lost`. تكون عملية Gateway فقط هي المرجع الموثوق لمجموعة مهام cron النشطة داخل الذاكرة؛ يستخدم تدقيق CLI دون اتصال السجل الدائم لكنه لا يوسم مهمة cron كمفقودة لمجرد أن تلك المجموعة المحلية Set فارغة.
- تتحقق مهام CLI المدعومة بالمحادثة من سياق التشغيل الحي المالك، وليس فقط من صف جلسة المحادثة.

تنظيف الإكمال واعٍ أيضًا بوقت التشغيل:

- يحاول إكمال الوكيل الفرعي، بأفضل جهد، إغلاق تبويبات/عمليات المتصفح المتتبعة لجلسة الطفل قبل متابعة تنظيف الإعلان.
- يحاول إكمال cron المعزول، بأفضل جهد، إغلاق تبويبات/عمليات المتصفح المتتبعة لجلسة cron قبل تفكيك التشغيل بالكامل.
- ينتظر تسليم cron المعزول متابعة الوكيل الفرعي المنحدر عند الحاجة ويمنع نص إقرار الأصل المتقادم بدلًا من إعلانه.
- يفضل تسليم إكمال الوكيل الفرعي أحدث نص مساعد ظاهر؛ إذا كان ذلك فارغًا، فإنه يرجع إلى أحدث نص أداة/نتيجة أداة مُنقّى، ويمكن لتشغيلات استدعاء الأدوات التي انتهت بالمهلة فقط أن تختصر إلى ملخص تقدم جزئي قصير. تعلن التشغيلات النهائية الفاشلة حالة الفشل دون إعادة عرض نص الرد الملتقط.
- لا تحجب إخفاقات التنظيف النتيجة الحقيقية للمهمة.

قائمة \| عرض \| إلغاء تدفق المهام

```
openclaw tasks flow list [--status <status>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

استخدم هذه عندما يكون تدفق المهام المنسق هو ما يهمك بدلًا من سجل مهمة خلفية فردي.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D9%84%D9%88%D8%AD%D8%A9-%D9%85%D9%87%D8%A7%D9%85-%D8%A7%D9%84%D9%85%D8%AD%D8%A7%D8%AF%D8%AB%D8%A9-/tasks)  لوحة مهام المحادثة (`/tasks`)

استخدم `/tasks` في أي جلسة محادثة لرؤية المهام الخلفية المرتبطة بتلك الجلسة. تعرض اللوحة المهام النشطة والمكتملة حديثًا مع وقت التشغيل، والحالة، والتوقيت، وتفاصيل التقدم أو الخطأ.عندما لا تكون للجلسة الحالية مهام مرتبطة مرئية، يرجع `/tasks` إلى أعداد المهام المحلية للوكيل حتى تحصل على نظرة عامة دون كشف تفاصيل جلسات أخرى.للسجل الكامل للمشغل، استخدم CLI: `openclaw tasks list`.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%AA%D9%83%D8%A7%D9%85%D9%84-%D8%A7%D9%84%D8%AD%D8%A7%D9%84%D8%A9-%D8%B6%D8%BA%D8%B7-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  تكامل الحالة (ضغط المهام)

يتضمن `openclaw status` ملخصًا سريعًا للمهام:

```
Tasks: 3 queued · 2 running · 1 issues
```

يعرض الملخص:

- **نشطة** — عدد `queued` \+ `running`
- **إخفاقات** — عدد `failed` \+ `timed_out` \+ `lost`
- **حسب وقت التشغيل** — تفصيل حسب `acp`، و`subagent`، و`cron`، و`cli`

يستخدم كل من `/status` وأداة `session_status` لقطة مهام واعية بالتنظيف: تُفضّل المهام النشطة، وتُخفى الصفوف المكتملة المتقادمة، ولا تظهر الإخفاقات الحديثة إلا عندما لا يبقى عمل نشط. هذا يبقي بطاقة الحالة مركزة على ما يهم الآن.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A7%D9%84%D8%AA%D8%AE%D8%B2%D9%8A%D9%86-%D9%88%D8%A7%D9%84%D8%B5%D9%8A%D8%A7%D9%86%D8%A9)  التخزين والصيانة

### [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A3%D9%8A%D9%86-%D8%AA%D8%B9%D9%8A%D8%B4-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85)  أين تعيش المهام

تستمر سجلات المهام في SQLite عند:

```
$OPENCLAW_STATE_DIR/tasks/runs.sqlite
```

يُحمّل السجل في الذاكرة عند بدء Gateway ويزامن الكتابات إلى SQLite لضمان الاستمرارية عبر عمليات إعادة التشغيل.
يبقي Gateway سجل الكتابة المسبقة في SQLite محدودًا باستخدام عتبة
autocheckpoint الافتراضية في SQLite بالإضافة إلى نقاط تحقق `TRUNCATE` الدورية وعند إيقاف التشغيل.

### [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%A7%D9%84%D8%B5%D9%8A%D8%A7%D9%86%D8%A9-%D8%A7%D9%84%D8%AA%D9%84%D9%82%D8%A7%D8%A6%D9%8A%D8%A9)  الصيانة التلقائية

يعمل ماسح كل **60 ثانية** ويتولى أربعة أمور:

1

[Navigate to header](https://docs.openclaw.ai/ar/automation/tasks#)

التسوية

يتحقق مما إذا كانت المهام النشطة لا تزال تملك دعمًا موثوقًا من وقت التشغيل. تستخدم مهام ACP/الوكيل الفرعي حالة جلسة الطفل، وتستخدم مهام cron ملكية المهمة النشطة، وتستخدم مهام CLI المدعومة بالمحادثة سياق التشغيل المالك. إذا اختفت حالة الدعم تلك لأكثر من 5 دقائق، تُوسم المهمة كـ `lost`.

2

[Navigate to header](https://docs.openclaw.ai/ar/automation/tasks#)

إصلاح جلسة ACP

يغلق جلسات ACP النهائية أو اليتيمة ذات اللقطة الواحدة المملوكة للأصل، ويغلق جلسات ACP المستمرة النهائية المتقادمة أو اليتيمة فقط عندما لا يبقى أي ربط محادثة نشط.

3

[Navigate to header](https://docs.openclaw.ai/ar/automation/tasks#)

ختم التنظيف

يعيّن طابعًا زمنيًا `cleanupAfter` على المهام النهائية (endedAt + 7 أيام). أثناء الاحتفاظ، تستمر المهام المفقودة في الظهور في التدقيق كتحذيرات؛ وبعد انتهاء `cleanupAfter` أو عند غياب بيانات التنظيف الوصفية، تصبح أخطاء.

4

[Navigate to header](https://docs.openclaw.ai/ar/automation/tasks#)

التقليم

يحذف السجلات التي تجاوزت تاريخ `cleanupAfter` الخاص بها.

**الاحتفاظ:** تُحفظ سجلات المهام النهائية لمدة **7 أيام**، ثم تُقلم تلقائيًا. لا حاجة إلى أي إعداد.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D9%83%D9%8A%D9%81-%D8%AA%D8%B1%D8%AA%D8%A8%D8%B7-%D8%A7%D9%84%D9%85%D9%87%D8%A7%D9%85-%D8%A8%D8%A7%D9%84%D8%A3%D9%86%D8%B8%D9%85%D8%A9-%D8%A7%D9%84%D8%A3%D8%AE%D8%B1%D9%89)  كيف ترتبط المهام بالأنظمة الأخرى

المهام وتدفق المهام

[تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow) هو طبقة تنسيق التدفق فوق المهام الخلفية. يمكن لتدفق واحد أن ينسق عدة مهام طوال عمره باستخدام أوضاع مزامنة مُدارة أو معكوسة. استخدم `openclaw tasks` لفحص سجلات المهام الفردية و`openclaw tasks flow` لفحص التدفق المنسق.راجع [تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow) للتفاصيل.

المهام وcron

يعيش **تعريف** مهمة cron في `~/.openclaw/cron/jobs.json`؛ وتعيش حالة تنفيذ وقت التشغيل بجانبه في `~/.openclaw/cron/jobs-state.json`. ينشئ **كل** تنفيذ cron سجل مهمة، سواء كان من الجلسة الرئيسية أو معزولًا. تضبط مهام cron في الجلسة الرئيسية افتراضيًا سياسة الإشعار إلى `silent` حتى تتتبع دون إنشاء إشعارات.راجع [مهام Cron](https://docs.openclaw.ai/ar/automation/cron-jobs).

المهام وHeartbeat

تشغيلات Heartbeat هي أدوار في الجلسة الرئيسية، ولا تنشئ سجلات مهام. عندما تكتمل مهمة، يمكنها تشغيل إيقاظ Heartbeat حتى ترى النتيجة فورًا.راجع [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat).

المهام والجلسات

قد تشير المهمة إلى `childSessionKey` (حيث يجري العمل) و`requesterSessionKey` (من بدأها). الجلسات هي سياق المحادثة؛ والمهام هي تتبع النشاط فوق ذلك.

المهام وتشغيلات الوكيل

يربط `runId` الخاص بالمهمة بتشغيل الوكيل الذي ينجز العمل. تُحدّث أحداث دورة حياة الوكيل (البدء، والانتهاء، والخطأ) حالة المهمة تلقائيًا، ولا تحتاج إلى إدارة دورة الحياة يدويًا.

## [​](https://docs.openclaw.ai/ar/automation/tasks\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [الأتمتة والمهام](https://docs.openclaw.ai/ar/automation) — كل آليات الأتمتة في لمحة
- [CLI: المهام](https://docs.openclaw.ai/ar/cli/tasks) — مرجع أوامر CLI
- [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat) — أدوار دورية في الجلسة الرئيسية
- [المهام المجدولة](https://docs.openclaw.ai/ar/automation/cron-jobs) — جدولة العمل الخلفي
- [تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow) — تنسيق التدفق فوق المهام

[Scheduled tasks](https://docs.openclaw.ai/ar/automation/cron-jobs) [تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow)

Ctrl+I