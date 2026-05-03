---
source_url: https://docs.openclaw.ai/ar/cli/memory
title: "\u0627\u0644\u0630\u0627\u0643\u0631\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/cli/memory#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Agents and sessions

الذاكرة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [openclaw memory](https://docs.openclaw.ai/ar/cli/memory#openclaw-memory)
- [أمثلة](https://docs.openclaw.ai/ar/cli/memory#%D8%A3%D9%85%D8%AB%D9%84%D8%A9)
- [الخيارات](https://docs.openclaw.ai/ar/cli/memory#%D8%A7%D9%84%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA)
- [Dreaming](https://docs.openclaw.ai/ar/cli/memory#dreaming)
- [ذات صلة](https://docs.openclaw.ai/ar/cli/memory#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/ar/cli/memory\#openclaw-memory)  `openclaw memory`

إدارة فهرسة الذاكرة الدلالية والبحث فيها.
يوفره Plugin الذاكرة النشطة (الافتراضي: `memory-core`؛ عيّن `plugins.slots.memory = "none"` للتعطيل).ذات صلة:

- مفهوم الذاكرة: [الذاكرة](https://docs.openclaw.ai/ar/concepts/memory)
- ويكي الذاكرة: [ويكي الذاكرة](https://docs.openclaw.ai/ar/plugins/memory-wiki)
- Wiki CLI: [wiki](https://docs.openclaw.ai/ar/cli/wiki)
- Plugins: [Plugins](https://docs.openclaw.ai/ar/tools/plugin)

## [​](https://docs.openclaw.ai/ar/cli/memory\#%D8%A3%D9%85%D8%AB%D9%84%D8%A9)  أمثلة

```
openclaw memory status
openclaw memory status --deep
openclaw memory status --fix
openclaw memory index --force
openclaw memory search "meeting notes"
openclaw memory search --query "deployment" --max-results 20
openclaw memory promote --limit 10 --min-score 0.75
openclaw memory promote --apply
openclaw memory promote --json --min-recall-count 0 --min-unique-queries 0
openclaw memory promote-explain "router vlan"
openclaw memory promote-explain "router vlan" --json
openclaw memory rem-harness
openclaw memory rem-harness --json
openclaw memory status --json
openclaw memory status --deep --index
openclaw memory status --deep --index --verbose
openclaw memory status --agent main
openclaw memory index --agent main --verbose
```

## [​](https://docs.openclaw.ai/ar/cli/memory\#%D8%A7%D9%84%D8%AE%D9%8A%D8%A7%D8%B1%D8%A7%D8%AA)  الخيارات

`memory status` و`memory index`:

- `--agent <id>`: حصر النطاق في وكيل واحد. بدونه، تعمل هذه الأوامر لكل وكيل مكوّن؛ وإذا لم تكن قائمة الوكلاء مكوّنة، فإنها تعود إلى الوكيل الافتراضي.
- `--verbose`: إصدار سجلات مفصلة أثناء الفحوصات والفهرسة.

`memory status`:

- `--deep`: فحص توفر المتجه \+ التضمين. يبقى `memory status` العادي سريعًا ولا يشغّل اختبار تضمين حيًا. يتخطى البحث المعجمي QMD ‏`searchMode: "search"` فحوصات المتجه الدلالي وصيانة التضمينات حتى مع `--deep`.
- `--index`: تشغيل إعادة فهرسة إذا كان المخزن متسخًا (يتضمن `--deep`).
- `--fix`: إصلاح أقفال الاستدعاء القديمة وتطبيع بيانات الترويج الوصفية.
- `--json`: طباعة مخرجات JSON.

إذا أظهر `memory status` الحالة `Dreaming status: blocked`، فهذا يعني أن Cron المدار الخاص بـ Dreaming مفعّل لكن Heartbeat الذي يشغّله لا يعمل للوكيل الافتراضي. راجع [Dreaming لا يعمل أبدًا](https://docs.openclaw.ai/ar/concepts/dreaming#dreaming-never-runs-status-shows-blocked) لمعرفة السببين الشائعين.`memory index`:

- `--force`: فرض إعادة فهرسة كاملة.

`memory search`:

- إدخال الاستعلام: مرّر إما `[query]` موضعيًا أو `--query <text>`.
- إذا تم توفير كليهما، فسيكون لـ`--query` الأسبقية.
- إذا لم يتم توفير أي منهما، يخرج الأمر بخطأ.
- `--agent <id>`: حصر النطاق في وكيل واحد (الافتراضي: الوكيل الافتراضي).
- `--max-results <n>`: تقييد عدد النتائج المُعادة.
- `--min-score <n>`: تصفية المطابقات ذات الدرجة المنخفضة.
- `--json`: طباعة نتائج JSON.

`memory promote`:معاينة ترقيات الذاكرة قصيرة المدى وتطبيقها.

```
openclaw memory promote [--apply] [--limit <n>] [--include-promoted]
```

- `--apply` — كتابة الترقيات إلى `MEMORY.md` (الافتراضي: المعاينة فقط).
- `--limit <n>` — تحديد الحد الأقصى لعدد المرشحين المعروضين.
- `--include-promoted` — تضمين الإدخالات التي تمت ترقيتها بالفعل في الدورات السابقة.

الخيارات الكاملة:

- يرتّب المرشحين قصيري المدى من `memory/YYYY-MM-DD.md` باستخدام إشارات ترقية موزونة (`frequency`، `relevance`، `query diversity`، `recency`، `consolidation`، `conceptual richness`).
- يستخدم إشارات قصيرة المدى من كل من استدعاءات الذاكرة وعمليات الاستيعاب اليومية، إضافة إلى إشارات تعزيز مرحلة light/REM.
- عند تفعيل Dreaming، يدير `memory-core` تلقائيًا مهمة Cron واحدة تشغّل مسحًا كاملًا (`light -> REM -> deep`) في الخلفية (لا يلزم تنفيذ `openclaw cron add` يدويًا).
- `--agent <id>`: حصر النطاق في وكيل واحد (الافتراضي: الوكيل الافتراضي).
- `--limit <n>`: الحد الأقصى للمرشحين المراد إرجاعهم/تطبيقهم.
- `--min-score <n>`: الحد الأدنى لدرجة الترقية الموزونة.
- `--min-recall-count <n>`: الحد الأدنى لعدد الاستدعاءات المطلوب للمرشح.
- `--min-unique-queries <n>`: الحد الأدنى لعدد الاستعلامات المميزة المطلوب للمرشح.
- `--apply`: إلحاق المرشحين المحددين بـ`MEMORY.md` ووضع علامة عليهم بأنهم تمت ترقيتهم.
- `--include-promoted`: تضمين المرشحين الذين تمت ترقيتهم بالفعل في المخرجات.
- `--json`: طباعة مخرجات JSON.

`memory promote-explain`:شرح مرشح ترقية محدد وتفصيل درجته.

```
openclaw memory promote-explain <selector> [--agent <id>] [--include-promoted] [--json]
```

- `<selector>`: مفتاح المرشح أو جزء من المسار أو جزء من المقتطف للبحث عنه.
- `--agent <id>`: حصر النطاق في وكيل واحد (الافتراضي: الوكيل الافتراضي).
- `--include-promoted`: تضمين المرشحين الذين تمت ترقيتهم بالفعل.
- `--json`: طباعة مخرجات JSON.

`memory rem-harness`:معاينة تأملات REM والحقائق المرشحة ومخرجات الترقية العميقة دون كتابة أي شيء.

```
openclaw memory rem-harness [--agent <id>] [--include-promoted] [--json]
```

- `--agent <id>`: حصر النطاق في وكيل واحد (الافتراضي: الوكيل الافتراضي).
- `--include-promoted`: تضمين المرشحين العميقين الذين تمت ترقيتهم بالفعل.
- `--json`: طباعة مخرجات JSON.

## [​](https://docs.openclaw.ai/ar/cli/memory\#dreaming)  Dreaming

Dreaming هو نظام دمج الذاكرة في الخلفية بثلاث مراحل متعاونة:
**light** (فرز/تهيئة المواد قصيرة المدى)، و **deep** (ترقية الحقائق المتينة
إلى `MEMORY.md`)، و **REM** (التأمل وإبراز السمات).

- فعّله باستخدام `plugins.entries.memory-core.config.dreaming.enabled: true`.
- بدّله من الدردشة باستخدام `/dreaming on|off` (أو افحصه باستخدام `/dreaming status`).
- يعمل Dreaming وفق جدول مسح مدار واحد (`dreaming.frequency`) وينفّذ المراحل بالترتيب: light، REM، deep.
- تكتب مرحلة deep فقط الذاكرة المتينة إلى `MEMORY.md`.
- تُكتب مخرجات المراحل القابلة للقراءة البشرية وإدخالات اليوميات إلى `DREAMS.md` (أو `dreams.md` الموجود)، مع تقارير اختيارية لكل مرحلة في `memory/dreaming/<phase>/YYYY-MM-DD.md`.
- يستخدم الترتيب إشارات موزونة: تكرار الاستدعاء، صلة الاسترجاع، تنوع الاستعلامات، الحداثة الزمنية، الدمج عبر الأيام، والغنى المفاهيمي المشتق.
- تعيد الترقية قراءة الملاحظة اليومية الحية قبل الكتابة إلى `MEMORY.md`، لذلك لا تتم ترقية المقتطفات قصيرة المدى المعدلة أو المحذوفة من لقطات مخزن الاستدعاء القديمة.
- تشترك عمليات `memory promote` المجدولة واليدوية في الإعدادات الافتراضية نفسها لمرحلة deep ما لم تمرر تجاوزات العتبات عبر CLI.
- تتوسع عمليات التشغيل التلقائية عبر مساحات عمل الذاكرة المكوّنة.

الجدولة الافتراضية:

- **وتيرة المسح**: `dreaming.frequency = 0 3 * * *`
- **عتبات deep**: `minScore=0.8`، `minRecallCount=3`، `minUniqueQueries=3`، `recencyHalfLifeDays=14`، `maxAgeDays=30`

مثال:

```
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

ملاحظات:

- يطبع `memory index --verbose` تفاصيل لكل مرحلة (الموفر، النموذج، المصادر، نشاط الدُفعات).
- يتضمن `memory status` أي مسارات إضافية مكوّنة عبر `memorySearch.extraPaths`.
- إذا كانت حقول مفتاح API البعيد للذاكرة النشطة فعليًا مكوّنة كـSecretRefs، يحل الأمر هذه القيم من لقطة Gateway النشطة. إذا لم يكن Gateway متاحًا، يفشل الأمر بسرعة.
- ملاحظة انحراف إصدار Gateway: يتطلب مسار الأمر هذا Gateway يدعم `secrets.resolve`؛ تعيد Gateways الأقدم خطأ طريقة غير معروفة.
- اضبط وتيرة المسح المجدول باستخدام `dreaming.frequency`. سياسة ترقية deep داخلية بخلاف ذلك؛ استخدم أعلام CLI على `memory promote` عندما تحتاج إلى تجاوزات يدوية لمرة واحدة.
- يعاين `memory rem-harness --path <file-or-dir> --grounded` أقسام `What Happened` و`Reflections` و`Possible Lasting Updates` المؤسَّسة من الملاحظات اليومية التاريخية دون كتابة أي شيء.
- يكتب `memory rem-backfill --path <file-or-dir>` إدخالات يوميات مؤسَّسة قابلة للعكس في `DREAMS.md` لمراجعة الواجهة.
- يقوم `memory rem-backfill --path <file-or-dir> --stage-short-term` أيضًا بزرع مرشحين متينين مؤسَّسين في مخزن الترقية قصيرة المدى الحي حتى تتمكن مرحلة deep العادية من ترتيبهم.
- يزيل `memory rem-backfill --rollback` إدخالات اليوميات المؤسَّسة المكتوبة سابقًا، ويزيل `memory rem-backfill --rollback-short-term` المرشحين المؤسَّسين قصيري المدى الذين تمت تهيئتهم سابقًا.
- راجع [Dreaming](https://docs.openclaw.ai/ar/concepts/dreaming) للاطلاع على أوصاف المراحل الكاملة ومرجع التكوين.

## [​](https://docs.openclaw.ai/ar/cli/memory\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [مرجع CLI](https://docs.openclaw.ai/ar/cli)
- [نظرة عامة على الذاكرة](https://docs.openclaw.ai/ar/concepts/memory)

[CLI الاستدلال](https://docs.openclaw.ai/ar/cli/infer) [\`openclaw commitments\`](https://docs.openclaw.ai/ar/cli/commitments)

Ctrl+I