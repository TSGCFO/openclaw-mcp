---
source_url: https://docs.openclaw.ai/ar/concepts/agent
title: "\u0628\u064a\u0626\u0629 \u062a\u0634\u063a\u064a\u0644 \u0627\u0644\u0648\u0643\u064a\u0644 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/concepts/agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Fundamentals

بيئة تشغيل الوكيل

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [مساحة العمل (مطلوبة)](https://docs.openclaw.ai/ar/concepts/agent#%D9%85%D8%B3%D8%A7%D8%AD%D8%A9-%D8%A7%D9%84%D8%B9%D9%85%D9%84-%D9%85%D8%B7%D9%84%D9%88%D8%A8%D8%A9)
- [ملفات التمهيد (تُحقن)](https://docs.openclaw.ai/ar/concepts/agent#%D9%85%D9%84%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D9%85%D9%87%D9%8A%D8%AF-%D8%AA%D9%8F%D8%AD%D9%82%D9%86)
- [الأدوات المدمجة](https://docs.openclaw.ai/ar/concepts/agent#%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AF%D9%85%D8%AC%D8%A9)
- [Skills](https://docs.openclaw.ai/ar/concepts/agent#skills)
- [حدود وقت التشغيل](https://docs.openclaw.ai/ar/concepts/agent#%D8%AD%D8%AF%D9%88%D8%AF-%D9%88%D9%82%D8%AA-%D8%A7%D9%84%D8%AA%D8%B4%D8%BA%D9%8A%D9%84)
- [الجلسات](https://docs.openclaw.ai/ar/concepts/agent#%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)
- [التوجيه أثناء البث](https://docs.openclaw.ai/ar/concepts/agent#%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D8%A3%D8%AB%D9%86%D8%A7%D8%A1-%D8%A7%D9%84%D8%A8%D8%AB)
- [مراجع النماذج](https://docs.openclaw.ai/ar/concepts/agent#%D9%85%D8%B1%D8%A7%D8%AC%D8%B9-%D8%A7%D9%84%D9%86%D9%85%D8%A7%D8%B0%D8%AC)
- [الإعدادات (الحد الأدنى)](https://docs.openclaw.ai/ar/concepts/agent#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D8%AD%D8%AF-%D8%A7%D9%84%D8%A3%D8%AF%D9%86%D9%89)
- [ذات صلة](https://docs.openclaw.ai/ar/concepts/agent#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

يشغّل OpenClaw **وقت تشغيل وكيل مضمّنًا واحدًا** — عملية وكيل واحدة لكل
Gateway، مع مساحة عمله الخاصة وملفات التمهيد ومخزن الجلسات الخاص به. تغطي هذه الصفحة
عقد وقت التشغيل هذا: ما الذي يجب أن تحتويه مساحة العمل، وأي الملفات تُحقن،
وكيف تُمهّد الجلسات بالاستناد إليها.

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D9%85%D8%B3%D8%A7%D8%AD%D8%A9-%D8%A7%D9%84%D8%B9%D9%85%D9%84-%D9%85%D8%B7%D9%84%D9%88%D8%A8%D8%A9)  مساحة العمل (مطلوبة)

يستخدم OpenClaw دليل مساحة عمل وكيل واحدًا (`agents.defaults.workspace`) بوصفه دليل العمل **الوحيد** للوكيل (`cwd`) للأدوات والسياق.موصى به: استخدم `openclaw setup` لإنشاء `~/.openclaw/openclaw.json` إذا كان مفقودًا وتهيئة ملفات مساحة العمل.تخطيط مساحة العمل الكامل \+ دليل النسخ الاحتياطي: [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace)إذا كان `agents.defaults.sandbox` مفعّلًا، يمكن للجلسات غير الرئيسية تجاوز ذلك باستخدام
مساحات عمل لكل جلسة ضمن `agents.defaults.sandbox.workspaceRoot` (راجع
[إعدادات Gateway](https://docs.openclaw.ai/ar/gateway/configuration)).

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D9%85%D9%84%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D8%AA%D9%85%D9%87%D9%8A%D8%AF-%D8%AA%D9%8F%D8%AD%D9%82%D9%86)  ملفات التمهيد (تُحقن)

داخل `agents.defaults.workspace`، يتوقع OpenClaw هذه الملفات القابلة للتعديل من المستخدم:

- `AGENTS.md` — تعليمات التشغيل \+ “الذاكرة”
- `SOUL.md` — الشخصية والحدود والنبرة
- `TOOLS.md` — ملاحظات الأدوات التي يحتفظ بها المستخدم (مثل `imsg` و`sag` والاصطلاحات)
- `BOOTSTRAP.md` — طقس تشغيل أولي لمرة واحدة (يُحذف بعد الإكمال)
- `IDENTITY.md` — اسم الوكيل/طابعه/رمزه التعبيري
- `USER.md` — ملف المستخدم \+ طريقة المخاطبة المفضلة

في أول دورة لجلسة جديدة، يحقن OpenClaw محتويات هذه الملفات مباشرة في سياق الوكيل.تُتخطى الملفات الفارغة. تُقصّ الملفات الكبيرة وتُختصر مع علامة بحيث تبقى المطالبات خفيفة (اقرأ الملف للحصول على المحتوى الكامل).إذا كان ملف مفقودًا، يحقن OpenClaw سطر علامة “ملف مفقود” واحدًا (وسيُنشئ `openclaw setup` قالبًا افتراضيًا آمنًا).يُنشأ `BOOTSTRAP.md` فقط من أجل **مساحة عمل جديدة تمامًا** (لا توجد ملفات تمهيد أخرى). إذا حذفته بعد إكمال الطقس، فيجب ألا يُعاد إنشاؤه عند عمليات إعادة التشغيل اللاحقة.لتعطيل إنشاء ملفات التمهيد بالكامل (لمساحات العمل المجهزة مسبقًا)، عيّن:

```
{ agents: { defaults: { skipBootstrap: true } } }
```

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%AF%D9%85%D8%AC%D8%A9)  الأدوات المدمجة

الأدوات الأساسية (read/exec/edit/write وأدوات النظام ذات الصلة) متاحة دائمًا،
وفقًا لسياسة الأدوات. `apply_patch` اختياري ومحكوم بواسطة
`tools.exec.applyPatch`. لا يتحكم `TOOLS.md` في الأدوات الموجودة؛ بل هو
إرشاد لكيف تريد _أنت_ استخدامها.

## [​](https://docs.openclaw.ai/ar/concepts/agent\#skills)  Skills

يحمّل OpenClaw Skills من هذه المواقع (الأولوية الأعلى أولًا):

- مساحة العمل: `<workspace>/skills`
- Skills وكيل المشروع: `<workspace>/.agents/skills`
- Skills الوكيل الشخصية: `~/.agents/skills`
- المُدارة/المحلية: `~/.openclaw/skills`
- المضمّنة (المشحونة مع التثبيت)
- مجلدات Skills إضافية: `skills.load.extraDirs`

يمكن حجب Skills بواسطة الإعداد/البيئة (راجع `skills` في [إعدادات Gateway](https://docs.openclaw.ai/ar/gateway/configuration)).

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%AD%D8%AF%D9%88%D8%AF-%D9%88%D9%82%D8%AA-%D8%A7%D9%84%D8%AA%D8%B4%D8%BA%D9%8A%D9%84)  حدود وقت التشغيل

يُبنى وقت تشغيل الوكيل المضمّن على نواة وكيل Pi (النماذج والأدوات
ومسار المطالبة). إدارة الجلسات والاكتشاف وتوصيل الأدوات وتسليم القنوات
هي طبقات يملكها OpenClaw فوق تلك النواة.

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)  الجلسات

تُخزّن نسخ الجلسات بصيغة JSONL في:

- `~/.openclaw/agents/<agentId>/sessions/<SessionId>.jsonl`

معرّف الجلسة ثابت ويختاره OpenClaw.
لا تُقرأ مجلدات الجلسات القديمة من أدوات أخرى.

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87-%D8%A3%D8%AB%D9%86%D8%A7%D8%A1-%D8%A7%D9%84%D8%A8%D8%AB)  التوجيه أثناء البث

عندما يكون وضع الطابور `steer`، تُحقن الرسائل الواردة في التشغيل الحالي.
يُسلّم التوجيه المصطف **بعد انتهاء دورة المساعد الحالية من**
**تنفيذ استدعاءات أدواتها**، وقبل استدعاء LLM التالي. يفرّغ Pi كل رسائل
التوجيه المعلّقة معًا لـ `steer`؛ أما `queue` القديم فيفرّغ رسالة واحدة لكل
حد نموذج. لم يعد التوجيه يتخطى استدعاءات الأدوات المتبقية من رسالة
المساعد الحالية.عندما يكون وضع الطابور `followup` أو `collect`، تُحتجز الرسائل الواردة حتى
تنتهي الدورة الحالية، ثم تبدأ دورة وكيل جديدة مع الحمولات المصطفة. راجع
[الطابور](https://docs.openclaw.ai/ar/concepts/queue) و [طابور التوجيه](https://docs.openclaw.ai/ar/concepts/queue-steering) لمعرفة وضع
وسلوك الحدود.يرسل بث الكتل كتل المساعد المكتملة بمجرد انتهائها؛ وهو
**معطّل افتراضيًا** (`agents.defaults.blockStreamingDefault: "off"`).
اضبط الحد عبر `agents.defaults.blockStreamingBreak` (`text_end` مقابل `message_end`؛ الافتراضي text\_end).
تحكّم في تقطيع الكتل اللين باستخدام `agents.defaults.blockStreamingChunk` (القيمة الافتراضية
800–1200 حرف؛ يفضّل فواصل الفقرات، ثم الأسطر الجديدة؛ والجمل أخيرًا).
ادمج المقاطع المبثوثة باستخدام `agents.defaults.blockStreamingCoalesce` لتقليل
الإزعاج الناتج عن الأسطر المفردة (دمج قائم على الخمول قبل الإرسال). تتطلب القنوات غير Telegram
تعيين `*.blockStreaming: true` صراحةً لتمكين ردود الكتل.
تُصدر ملخصات الأدوات المطوّلة عند بدء الأداة (بلا تأخير تجميعي)؛ ويبث Control UI
مخرجات الأدوات عبر أحداث الوكيل عند توفرها.
تفاصيل أكثر: [البث \+ التقطيع](https://docs.openclaw.ai/ar/concepts/streaming).

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D9%85%D8%B1%D8%A7%D8%AC%D8%B9-%D8%A7%D9%84%D9%86%D9%85%D8%A7%D8%B0%D8%AC)  مراجع النماذج

تُحلّل مراجع النماذج في الإعدادات (مثل `agents.defaults.model` و`agents.defaults.models`) بالتقسيم عند أول `/`.

- استخدم `provider/model` عند إعداد النماذج.
- إذا كان معرّف النموذج نفسه يحتوي على `/` (بنمط OpenRouter)، فأضف بادئة المزوّد (مثال: `openrouter/moonshotai/kimi-k2`).
- إذا حذفت المزوّد، يحاول OpenClaw استخدام اسم مستعار أولًا، ثم مطابقة مزوّد مهيأ فريدة لمعرّف النموذج الدقيق ذلك، وبعدها فقط يعود إلى مزوّد الإعداد الافتراضي. إذا لم يعد ذلك المزوّد يوفّر النموذج الافتراضي المهيأ، يعود OpenClaw إلى أول مزوّد/نموذج مهيأ بدلًا من إظهار افتراضي قديم لمزوّد تمت إزالته.

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D8%A7%D9%84%D8%AD%D8%AF-%D8%A7%D9%84%D8%A3%D8%AF%D9%86%D9%89)  الإعدادات (الحد الأدنى)

كحد أدنى، عيّن:

- `agents.defaults.workspace`
- `channels.whatsapp.allowFrom` (موصى به بشدة)

* * *

_التالي: [محادثات المجموعات](https://docs.openclaw.ai/ar/channels/group-messages)_ 🦞

## [​](https://docs.openclaw.ai/ar/concepts/agent\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace)
- [توجيه متعدد الوكلاء](https://docs.openclaw.ai/ar/concepts/multi-agent)
- [إدارة الجلسات](https://docs.openclaw.ai/ar/concepts/session)

[بنية Gateway](https://docs.openclaw.ai/ar/concepts/architecture) [حلقة الوكيل](https://docs.openclaw.ai/ar/concepts/agent-loop)

Ctrl+I