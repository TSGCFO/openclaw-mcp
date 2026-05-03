---
source_url: https://docs.openclaw.ai/ar/automation/hooks
title: "\u0627\u0644\u062e\u0637\u0627\u0641\u0627\u062a - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation/hooks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Automation and tasks

الخطافات

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [البدء السريع](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)
- [أنواع الأحداث](https://docs.openclaw.ai/ar/automation/hooks#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%A3%D8%AD%D8%AF%D8%A7%D8%AB)
- [كتابة الخطّافات](https://docs.openclaw.ai/ar/automation/hooks#%D9%83%D8%AA%D8%A7%D8%A8%D8%A9-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)
- [بنية الخطّاف](https://docs.openclaw.ai/ar/automation/hooks#%D8%A8%D9%86%D9%8A%D8%A9-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81)
- [تنسيق HOOK.md](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%86%D8%B3%D9%8A%D9%82-hook-md)
- [تنفيذ المعالج](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%86%D9%81%D9%8A%D8%B0-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC)
- [أبرز سياقات الأحداث](https://docs.openclaw.ai/ar/automation/hooks#%D8%A3%D8%A8%D8%B1%D8%B2-%D8%B3%D9%8A%D8%A7%D9%82%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%AD%D8%AF%D8%A7%D8%AB)
- [اكتشاف الخطّافات](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%83%D8%AA%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)
- [حزم الخطّافات](https://docs.openclaw.ai/ar/automation/hooks#%D8%AD%D8%B2%D9%85-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)
- [الخطّافات المضمّنة](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B6%D9%85%D9%91%D9%86%D8%A9)
- [تفاصيل session-memory](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-session-memory)
- [تكوين bootstrap-extra-files](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%83%D9%88%D9%8A%D9%86-bootstrap-extra-files)
- [تفاصيل command-logger](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-command-logger)
- [تفاصيل boot-md](https://docs.openclaw.ai/ar/automation/hooks#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-boot-md)
- [خطّافات Plugin](https://docs.openclaw.ai/ar/automation/hooks#%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA-plugin)
- [التكوين](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86)
- [مرجع CLI](https://docs.openclaw.ai/ar/automation/hooks#%D9%85%D8%B1%D8%AC%D8%B9-cli)
- [أفضل الممارسات](https://docs.openclaw.ai/ar/automation/hooks#%D8%A3%D9%81%D8%B6%D9%84-%D8%A7%D9%84%D9%85%D9%85%D8%A7%D8%B1%D8%B3%D8%A7%D8%AA)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [لم يُكتشف الخطّاف](https://docs.openclaw.ai/ar/automation/hooks#%D9%84%D9%85-%D9%8A%D9%8F%D9%83%D8%AA%D8%B4%D9%81-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81)
- [الخطّاف غير مؤهل](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81-%D8%BA%D9%8A%D8%B1-%D9%85%D8%A4%D9%87%D9%84)
- [الخطّاف لا يعمل](https://docs.openclaw.ai/ar/automation/hooks#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81-%D9%84%D8%A7-%D9%8A%D8%B9%D9%85%D9%84)
- [ذات صلة](https://docs.openclaw.ai/ar/automation/hooks#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Hooks هي نصوص برمجية صغيرة تعمل عند حدوث شيء داخل Gateway. يمكن اكتشافها من الأدلة وفحصها باستخدام `openclaw hooks`. يحمّل Gateway الخطّافات الداخلية فقط بعد تمكين الخطّافات أو تكوين إدخال خطّاف واحد على الأقل، أو حزمة خطّافات، أو معالج قديم، أو دليل خطّافات إضافي.يوجد نوعان من الخطّافات في OpenClaw:

- **الخطّافات الداخلية** (هذه الصفحة): تعمل داخل Gateway عند إطلاق أحداث الوكيل، مثل `/new` أو `/reset` أو `/stop` أو أحداث دورة الحياة.
- **Webhooks**: نقاط نهاية HTTP خارجية تتيح للأنظمة الأخرى تشغيل عمل في OpenClaw. راجع [Webhooks](https://docs.openclaw.ai/ar/automation/cron-jobs#webhooks).

يمكن أيضًا تجميع الخطّافات داخل plugins. يعرض `openclaw hooks list` كلًا من الخطّافات المستقلة والخطّافات المُدارة بواسطة plugin.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%84%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%B3%D8%B1%D9%8A%D8%B9)  البدء السريع

```
# List available hooks
openclaw hooks list

# Enable a hook
openclaw hooks enable session-memory

# Check hook status
openclaw hooks check

# Get detailed information
openclaw hooks info session-memory
```

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A3%D9%86%D9%88%D8%A7%D8%B9-%D8%A7%D9%84%D8%A3%D8%AD%D8%AF%D8%A7%D8%AB)  أنواع الأحداث

| الحدث | وقت إطلاقه |
| --- | --- |
| `command:new` | إصدار الأمر `/new` |
| `command:reset` | إصدار الأمر `/reset` |
| `command:stop` | إصدار الأمر `/stop` |
| `command` | أي حدث أمر (مستمع عام) |
| `session:compact:before` | قبل أن يلخّص Compaction السجل |
| `session:compact:after` | بعد اكتمال Compaction |
| `session:patch` | عند تعديل خصائص الجلسة |
| `agent:bootstrap` | قبل حقن ملفات تمهيد مساحة العمل |
| `gateway:startup` | بعد بدء القنوات وتحميل الخطّافات |
| `gateway:shutdown` | عند بدء إيقاف Gateway |
| `gateway:pre-restart` | قبل إعادة تشغيل Gateway متوقعة |
| `message:received` | رسالة واردة من أي قناة |
| `message:transcribed` | بعد اكتمال تفريغ الصوت |
| `message:preprocessed` | بعد اكتمال المعالجة المسبقة للوسائط والروابط أو تخطيها |
| `message:sent` | تسليم رسالة صادرة |

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D9%83%D8%AA%D8%A7%D8%A8%D8%A9-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)  كتابة الخطّافات

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A8%D9%86%D9%8A%D8%A9-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81)  بنية الخطّاف

كل خطّاف هو دليل يحتوي على ملفين:

```
my-hook/
├── HOOK.md          # Metadata + documentation
└── handler.ts       # Handler implementation
```

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%86%D8%B3%D9%8A%D9%82-hook-md)  تنسيق HOOK.md

```
---
name: my-hook
description: "Short description of what this hook does"
metadata:
  { "openclaw": { "emoji": "🔗", "events": ["command:new"], "requires": { "bins": ["node"] } } }
---

# My Hook

Detailed documentation goes here.
```

**حقول البيانات الوصفية** (`metadata.openclaw`):

| الحقل | الوصف |
| --- | --- |
| `emoji` | رمز تعبيري للعرض في CLI |
| `events` | مصفوفة أحداث للاستماع إليها |
| `export` | التصدير المسمّى المراد استخدامه (الافتراضي `"default"`) |
| `os` | المنصات المطلوبة (مثل `["darwin", "linux"]`) |
| `requires` | مسارات `bins` أو `anyBins` أو `env` أو `config` المطلوبة |
| `always` | تجاوز فحوص الأهلية (قيمة منطقية) |
| `install` | طرق التثبيت |

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%86%D9%81%D9%8A%D8%B0-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC)  تنفيذ المعالج

```
const handler = async (event) => {
  if (event.type !== "command" || event.action !== "new") {
    return;
  }

  console.log(`[my-hook] New command triggered`);
  // Your logic here

  // Optionally send message to user
  event.messages.push("Hook executed!");
};

export default handler;
```

يتضمن كل حدث: `type` و`action` و`sessionKey` و`timestamp` و`messages` (ادفع إليها للإرسال إلى المستخدم) و`context` (بيانات خاصة بالحدث). يمكن أن تتضمن سياقات خطّافات وكيل وplugin الأدوات أيضًا `trace`، وهو سياق تتبع تشخيصي متوافق مع W3C وللقراءة فقط يمكن أن تمرره plugins إلى السجلات المنظمة لربط OTEL.

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A3%D8%A8%D8%B1%D8%B2-%D8%B3%D9%8A%D8%A7%D9%82%D8%A7%D8%AA-%D8%A7%D9%84%D8%A3%D8%AD%D8%AF%D8%A7%D8%AB)  أبرز سياقات الأحداث

**أحداث الأوامر** (`command:new`، `command:reset`): `context.sessionEntry`، `context.previousSessionEntry`، `context.commandSource`، `context.workspaceDir`، `context.cfg`.**أحداث الرسائل** (`message:received`): `context.from`، `context.content`، `context.channelId`، `context.metadata` (بيانات خاصة بالمزوّد تشمل `senderId` و`senderName` و`guildId`).**أحداث الرسائل** (`message:sent`): `context.to`، `context.content`، `context.success`، `context.channelId`.**أحداث الرسائل** (`message:transcribed`): `context.transcript`، `context.from`، `context.channelId`، `context.mediaPath`.**أحداث الرسائل** (`message:preprocessed`): `context.bodyForAgent` (النص النهائي المُثرى)، `context.from`، `context.channelId`.**أحداث التمهيد** (`agent:bootstrap`): `context.bootstrapFiles` (مصفوفة قابلة للتعديل)، `context.agentId`.**أحداث تصحيح الجلسة** (`session:patch`): `context.sessionEntry`، `context.patch` (الحقول التي تغيّرت فقط)، `context.cfg`. يمكن للعملاء ذوي الامتيازات فقط إطلاق أحداث التصحيح.**أحداث Compaction**: يتضمن `session:compact:before` كلًا من `messageCount` و`tokenCount`. يضيف `session:compact:after` كلًا من `compactedCount` و`summaryLength` و`tokensBefore` و`tokensAfter`.يراقب `command:stop` إصدار المستخدم للأمر `/stop`؛ إنه إلغاء/دورة حياة
أمر، وليس بوابة إنهاء للوكيل. يجب على plugins التي تحتاج إلى فحص
إجابة نهائية طبيعية وطلب مرور إضافي واحد من الوكيل استخدام خطّاف
plugin المكتوب `before_agent_finalize` بدلًا من ذلك. راجع [خطّافات Plugin](https://docs.openclaw.ai/ar/plugins/hooks).**أحداث دورة حياة Gateway**: يتضمن `gateway:shutdown` كلًا من `reason` و`restartExpectedMs` ويُطلق عند بدء إيقاف Gateway. يتضمن `gateway:pre-restart` السياق نفسه لكنه لا يُطلق إلا عندما يكون الإيقاف جزءًا من إعادة تشغيل متوقعة وتُوفَّر قيمة `restartExpectedMs` محدودة. أثناء الإيقاف، يكون انتظار كل خطّاف دورة حياة وفق أفضل جهد ومحدودًا حتى يستمر الإيقاف إذا تعطل معالج.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%83%D8%AA%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)  اكتشاف الخطّافات

تُكتشف الخطّافات من هذه الأدلة، بترتيب أسبقية التجاوز التصاعدية:

1. **الخطّافات المضمّنة**: المشحونة مع OpenClaw
2. **خطّافات Plugin**: الخطّافات المجمّعة داخل plugins المثبتة
3. **الخطّافات المُدارة**: `~/.openclaw/hooks/` (مثبتة من المستخدم، مشتركة عبر مساحات العمل). تشارك الأدلة الإضافية من `hooks.internal.load.extraDirs` هذه الأسبقية.
4. **خطّافات مساحة العمل**: `<workspace>/hooks/` (لكل وكيل، معطلة افتراضيًا حتى يتم تمكينها صراحة)

يمكن لخطّافات مساحة العمل إضافة أسماء خطّافات جديدة لكنها لا تستطيع تجاوز الخطّافات المضمّنة أو المُدارة أو المقدمة من plugin التي تحمل الاسم نفسه.يتخطى Gateway اكتشاف الخطّافات الداخلية عند بدء التشغيل حتى تُكوَّن الخطّافات الداخلية. مكّن خطّافًا مضمّنًا أو مُدارًا باستخدام `openclaw hooks enable <name>`، أو ثبّت حزمة خطّافات، أو عيّن `hooks.internal.enabled=true` للاشتراك. عند تمكين خطّاف مسمّى واحد، يحمّل Gateway معالج ذلك الخطّاف فقط؛ بينما يشترك `hooks.internal.enabled=true` وأدلة الخطّافات الإضافية والمعالجات القديمة في الاكتشاف الواسع.

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AD%D8%B2%D9%85-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA)  حزم الخطّافات

حزم الخطّافات هي حزم npm تصدّر الخطّافات عبر `openclaw.hooks` في `package.json`. ثبّتها باستخدام:

```
openclaw plugins install <path-or-spec>
```

مواصفات npm من السجل فقط (اسم الحزمة + إصدار دقيق اختياري أو dist-tag). تُرفض مواصفات Git/URL/file ونطاقات semver.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B6%D9%85%D9%91%D9%86%D8%A9)  الخطّافات المضمّنة

| الخطّاف | الأحداث | ما يفعله |
| --- | --- | --- |
| session-memory | `command:new`, `command:reset` | يحفظ سياق الجلسة في `<workspace>/memory/` |
| bootstrap-extra-files | `agent:bootstrap` | يحقن ملفات تمهيد إضافية من أنماط glob |
| command-logger | `command` | يسجل كل الأوامر في `~/.openclaw/logs/commands.log` |
| boot-md | `gateway:startup` | يشغل `BOOT.md` عند بدء Gateway |

مكّن أي خطّاف مضمّن:

```
openclaw hooks enable <hook-name>
```

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-session-memory)  تفاصيل session-memory

يستخرج آخر 15 رسالة من المستخدم/المساعد، وينشئ slug وصفيًا لاسم الملف عبر LLM، ويحفظه في `<workspace>/memory/YYYY-MM-DD-slug.md` باستخدام التاريخ المحلي للمضيف. يتطلب تكوين `workspace.dir`.

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%83%D9%88%D9%8A%D9%86-bootstrap-extra-files)  تكوين bootstrap-extra-files

```
{
  "hooks": {
    "internal": {
      "entries": {
        "bootstrap-extra-files": {
          "enabled": true,
          "paths": ["packages/*/AGENTS.md", "packages/*/TOOLS.md"]
        }
      }
    }
  }
}
```

تُحل المسارات بالنسبة إلى مساحة العمل. لا تُحمّل إلا أسماء ملفات التمهيد الأساسية المعروفة (`AGENTS.md`، `SOUL.md`، `TOOLS.md`، `IDENTITY.md`، `USER.md`، `HEARTBEAT.md`، `BOOTSTRAP.md`، `MEMORY.md`).

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-command-logger)  تفاصيل command-logger

يسجل كل أمر مائل في `~/.openclaw/logs/commands.log`.

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AA%D9%81%D8%A7%D8%B5%D9%8A%D9%84-boot-md)  تفاصيل boot-md

يشغل `BOOT.md` من مساحة العمل النشطة عند بدء Gateway.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%AE%D8%B7%D9%91%D8%A7%D9%81%D8%A7%D8%AA-plugin)  خطّافات Plugin

يمكن أن تسجل plugins خطّافات مكتوبة عبر Plugin SDK لتكامل أعمق:
اعتراض استدعاءات الأدوات، وتعديل المطالبات، والتحكم في تدفق الرسائل، والمزيد.
استخدم خطّافات plugin عندما تحتاج إلى `before_tool_call` أو `before_agent_reply`
أو `before_install` أو خطّافات دورة حياة أخرى داخل العملية.للمرجع الكامل لخطّافات plugin، راجع [خطّافات Plugin](https://docs.openclaw.ai/ar/plugins/hooks).

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%84%D8%AA%D9%83%D9%88%D9%8A%D9%86)  التكوين

```
{
  "hooks": {
    "internal": {
      "enabled": true,
      "entries": {
        "session-memory": { "enabled": true },
        "command-logger": { "enabled": false }
      }
    }
  }
}
```

متغيرات البيئة لكل خطّاف:

```
{
  "hooks": {
    "internal": {
      "entries": {
        "my-hook": {
          "enabled": true,
          "env": { "MY_CUSTOM_VAR": "value" }
        }
      }
    }
  }
}
```

أدلة الخطّافات الإضافية:

```
{
  "hooks": {
    "internal": {
      "load": {
        "extraDirs": ["/path/to/more/hooks"]
      }
    }
  }
}
```

لا يزال تنسيق تكوين المصفوفة القديمة `hooks.internal.handlers` مدعومًا للتوافق العكسي، لكن ينبغي للخطّافات الجديدة استخدام النظام القائم على الاكتشاف.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D9%85%D8%B1%D8%AC%D8%B9-cli)  مرجع CLI

```
# List all hooks (add --eligible, --verbose, or --json)
openclaw hooks list

# Show detailed info about a hook
openclaw hooks info <hook-name>

# Show eligibility summary
openclaw hooks check

# Enable/disable
openclaw hooks enable <hook-name>
openclaw hooks disable <hook-name>
```

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A3%D9%81%D8%B6%D9%84-%D8%A7%D9%84%D9%85%D9%85%D8%A7%D8%B1%D8%B3%D8%A7%D8%AA)  أفضل الممارسات

- **أبقِ المعالجات سريعة.** تعمل الخطّافات أثناء معالجة الأوامر. شغّل الأعمال الثقيلة بأسلوب الإطلاق والنسيان باستخدام `void processInBackground(event)`.
- **تعامل مع الأخطاء برفق.** لفّ العمليات الخطرة في try/catch؛ لا ترمِ استثناءً حتى تتمكن المعالجات الأخرى من العمل.
- **رشّح الأحداث مبكرًا.** عُد فورًا إذا لم يكن نوع/إجراء الحدث ذا صلة.
- **استخدم مفاتيح أحداث محددة.** فضّل `"events": ["command:new"]` على `"events": ["command"]` لتقليل الحمل.

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D9%84%D9%85-%D9%8A%D9%8F%D9%83%D8%AA%D8%B4%D9%81-%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81)  لم يُكتشف الخطّاف

```
# Verify directory structure
ls -la ~/.openclaw/hooks/my-hook/
# Should show: HOOK.md, handler.ts

# List all discovered hooks
openclaw hooks list
```

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81-%D8%BA%D9%8A%D8%B1-%D9%85%D8%A4%D9%87%D9%84)  الخطّاف غير مؤهل

```
openclaw hooks info my-hook
```

تحقق من الثنائيات المفقودة (PATH)، أو متغيرات البيئة، أو قيم التكوين، أو توافق نظام التشغيل.

### [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%A7%D9%84%D8%AE%D8%B7%D9%91%D8%A7%D9%81-%D9%84%D8%A7-%D9%8A%D8%B9%D9%85%D9%84)  الخطّاف لا يعمل

1. تحقق من تمكين الخطّاف: `openclaw hooks list`
2. أعد تشغيل عملية Gateway حتى تُعاد تحميل الخطّافات.
3. تحقق من سجلات Gateway: `./scripts/clawlog.sh | grep hook`

## [​](https://docs.openclaw.ai/ar/automation/hooks\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [مرجع CLI: الخطافات](https://docs.openclaw.ai/ar/cli/hooks)
- [Webhookات](https://docs.openclaw.ai/ar/automation/cron-jobs#webhooks)
- [خطافات Plugin](https://docs.openclaw.ai/ar/plugins/hooks) — خطافات دورة حياة Plugin داخل العملية
- [التكوين](https://docs.openclaw.ai/ar/gateway/configuration-reference#hooks)

[التعليمات الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders) [أداة \`apply\_patch\`](https://docs.openclaw.ai/ar/tools/apply-patch)

Ctrl+I