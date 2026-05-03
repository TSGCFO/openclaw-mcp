---
source_url: https://docs.openclaw.ai/ar/channels/broadcast-groups
title: "\u0645\u062c\u0645\u0648\u0639\u0627\u062a \u0627\u0644\u0628\u062b - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/broadcast-groups#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

مجموعات البث

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [نظرة عامة](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9)
- [حالات الاستخدام](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%AD%D8%A7%D9%84%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)
- [الإعداد](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [الإعداد الأساسي](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A)
- [استراتيجية المعالجة](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D8%B3%D8%AA%D8%B1%D8%A7%D8%AA%D9%8A%D8%AC%D9%8A%D8%A9-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC%D8%A9)
- [مثال كامل](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%85%D8%AB%D8%A7%D9%84-%D9%83%D8%A7%D9%85%D9%84)
- [كيف يعمل](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84)
- [تدفق الرسائل](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%AA%D8%AF%D9%81%D9%82-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)
- [عزل الجلسات](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%B9%D8%B2%D9%84-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)
- [مثال: جلسات معزولة](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%85%D8%AB%D8%A7%D9%84-%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-%D9%85%D8%B9%D8%B2%D9%88%D9%84%D8%A9)
- [أفضل الممارسات](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A3%D9%81%D8%B6%D9%84-%D8%A7%D9%84%D9%85%D9%85%D8%A7%D8%B1%D8%B3%D8%A7%D8%AA)
- [التوافق](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%AA%D9%88%D8%A7%D9%81%D9%82)
- [المزوّدون](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D9%85%D8%B2%D9%88%D9%91%D8%AF%D9%88%D9%86)
- [التوجيه](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [أمثلة](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A3%D9%85%D8%AB%D9%84%D8%A9)
- [مرجع API](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%85%D8%B1%D8%AC%D8%B9-api)
- [مخطط الإعداد](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D9%85%D8%AE%D8%B7%D8%B7-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [الحقول](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%AD%D9%82%D9%88%D9%84)
- [القيود](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D9%82%D9%8A%D9%88%D8%AF)
- [التحسينات المستقبلية](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%A7%D9%84%D8%AA%D8%AD%D8%B3%D9%8A%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%82%D8%A8%D9%84%D9%8A%D8%A9)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/broadcast-groups#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**الحالة:** تجريبي. أُضيف في 2026.1.9.

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9)  نظرة عامة

تتيح مجموعات البث لعدة وكلاء معالجة الرسالة نفسها والرد عليها في الوقت نفسه. يتيح لك ذلك إنشاء فرق وكلاء متخصصة تعمل معا داخل مجموعة WhatsApp واحدة أو رسالة مباشرة واحدة، وكل ذلك باستخدام رقم هاتف واحد.النطاق الحالي: **WhatsApp فقط** (قناة الويب).تُقيَّم مجموعات البث بعد قوائم السماح للقنوات وقواعد تفعيل المجموعات. في مجموعات WhatsApp، يعني ذلك أن البث يحدث عندما كان OpenClaw سيرد عادة (مثلا: عند الإشارة، بحسب إعدادات مجموعتك).

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%AD%D8%A7%D9%84%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)  حالات الاستخدام

1\. فرق وكلاء متخصصة

انشر عدة وكلاء بمسؤوليات محددة ومركزة:

```
Group: "Development Team"
Agents:
  - CodeReviewer (reviews code snippets)
  - DocumentationBot (generates docs)
  - SecurityAuditor (checks for vulnerabilities)
  - TestGenerator (suggests test cases)
```

يعالج كل وكيل الرسالة نفسها ويقدم منظوره المتخصص.

2\. دعم متعدد اللغات

```
Group: "International Support"
Agents:
  - Agent_EN (responds in English)
  - Agent_DE (responds in German)
  - Agent_ES (responds in Spanish)
```

3\. سير عمل ضمان الجودة

```
Group: "Customer Support"
Agents:
  - SupportAgent (provides answer)
  - QAAgent (reviews quality, only responds if issues found)
```

4\. أتمتة المهام

```
Group: "Project Management"
Agents:
  - TaskTracker (updates task database)
  - TimeLogger (logs time spent)
  - ReportGenerator (creates summaries)
```

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  الإعداد

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF-%D8%A7%D9%84%D8%A3%D8%B3%D8%A7%D8%B3%D9%8A)  الإعداد الأساسي

أضف قسم `broadcast` على المستوى الأعلى (بجوار `bindings`). المفاتيح هي معرفات أقران WhatsApp:

- محادثات المجموعات: JID المجموعة (مثلا `120363403215116621@g.us`)
- الرسائل المباشرة: رقم هاتف بصيغة E.164 (مثلا `+15551234567`)

```
{
  "broadcast": {
    "120363403215116621@g.us": ["alfred", "baerbel", "assistant3"]
  }
}
```

**النتيجة:** عندما كان OpenClaw سيرد في هذه المحادثة، سيشغل الوكلاء الثلاثة جميعا.

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D8%B3%D8%AA%D8%B1%D8%A7%D8%AA%D9%8A%D8%AC%D9%8A%D8%A9-%D8%A7%D9%84%D9%85%D8%B9%D8%A7%D9%84%D8%AC%D8%A9)  استراتيجية المعالجة

تحكم في كيفية معالجة الوكلاء للرسائل:

- parallel (default)

- sequential


يعالج جميع الوكلاء في الوقت نفسه:

```
{
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": ["alfred", "baerbel"]
  }
}
```

يعالج الوكلاء بالترتيب (ينتظر كل واحد انتهاء السابق):

```
{
  "broadcast": {
    "strategy": "sequential",
    "120363403215116621@g.us": ["alfred", "baerbel"]
  }
}
```

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%85%D8%AB%D8%A7%D9%84-%D9%83%D8%A7%D9%85%D9%84)  مثال كامل

```
{
  "agents": {
    "list": [\
      {\
        "id": "code-reviewer",\
        "name": "Code Reviewer",\
        "workspace": "/path/to/code-reviewer",\
        "sandbox": { "mode": "all" }\
      },\
      {\
        "id": "security-auditor",\
        "name": "Security Auditor",\
        "workspace": "/path/to/security-auditor",\
        "sandbox": { "mode": "all" }\
      },\
      {\
        "id": "docs-generator",\
        "name": "Documentation Generator",\
        "workspace": "/path/to/docs-generator",\
        "sandbox": { "mode": "all" }\
      }\
    ]
  },
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": ["code-reviewer", "security-auditor", "docs-generator"],
    "120363424282127706@g.us": ["support-en", "support-de"],
    "+15555550123": ["assistant", "logger"]
  }
}
```

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84)  كيف يعمل

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%AA%D8%AF%D9%81%D9%82-%D8%A7%D9%84%D8%B1%D8%B3%D8%A7%D8%A6%D9%84)  تدفق الرسائل

1

[Navigate to header](https://docs.openclaw.ai/ar/channels/broadcast-groups#)

وصول رسالة واردة

تصل رسالة مجموعة WhatsApp أو رسالة مباشرة.

2

[Navigate to header](https://docs.openclaw.ai/ar/channels/broadcast-groups#)

فحص البث

يتحقق النظام مما إذا كان معرف النظير موجودا في `broadcast`.

3

[Navigate to header](https://docs.openclaw.ai/ar/channels/broadcast-groups#)

إذا كان ضمن قائمة البث

- يعالج جميع الوكلاء المدرجين الرسالة.
- لكل وكيل مفتاح جلسة خاص به وسياق معزول.
- يعالج الوكلاء بالتوازي (افتراضيا) أو بالتتابع.

4

[Navigate to header](https://docs.openclaw.ai/ar/channels/broadcast-groups#)

إذا لم يكن ضمن قائمة البث

ينطبق التوجيه العادي (أول ربط مطابق).

لا تتجاوز مجموعات البث قوائم السماح للقنوات أو قواعد تفعيل المجموعات (الإشارات/الأوامر/إلخ). إنها تغير فقط _أي الوكلاء يعملون_ عندما تكون الرسالة مؤهلة للمعالجة.

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%B9%D8%B2%D9%84-%D8%A7%D9%84%D8%AC%D9%84%D8%B3%D8%A7%D8%AA)  عزل الجلسات

يحتفظ كل وكيل في مجموعة بث بما يلي منفصلا تماما:

- **مفاتيح الجلسة** (`agent:alfred:whatsapp:group:120363...` مقابل `agent:baerbel:whatsapp:group:120363...`)
- **سجل المحادثة** (لا يرى الوكيل رسائل الوكلاء الآخرين)
- **مساحة العمل** (بيئات sandbox منفصلة إذا تم إعدادها)
- **الوصول إلى الأدوات** (قوائم سماح/رفض مختلفة)
- **الذاكرة/السياق** (ملفات IDENTITY.md وSOUL.md منفصلة، إلخ)
- **مخزن سياق المجموعة** (رسائل المجموعة الحديثة المستخدمة للسياق) مشترك لكل نظير، لذلك يرى جميع وكلاء البث السياق نفسه عند تشغيلهم

يتيح هذا لكل وكيل أن يمتلك:

- شخصيات مختلفة
- وصولا مختلفا إلى الأدوات (مثلا، قراءة فقط مقابل قراءة وكتابة)
- نماذج مختلفة (مثلا، opus مقابل sonnet)
- Skills مختلفة مثبتة

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%85%D8%AB%D8%A7%D9%84-%D8%AC%D9%84%D8%B3%D8%A7%D8%AA-%D9%85%D8%B9%D8%B2%D9%88%D9%84%D8%A9)  مثال: جلسات معزولة

في المجموعة `120363403215116621@g.us` مع الوكلاء `["alfred", "baerbel"]`:

- سياق Alfred

- سياق Bärbel


```
Session: agent:alfred:whatsapp:group:120363403215116621@g.us
History: [user message, alfred's previous responses]
Workspace: /Users/user/openclaw-alfred/
Tools: read, write, exec
```

```
Session: agent:baerbel:whatsapp:group:120363403215116621@g.us
History: [user message, baerbel's previous responses]
Workspace: /Users/user/openclaw-baerbel/
Tools: read only
```

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A3%D9%81%D8%B6%D9%84-%D8%A7%D9%84%D9%85%D9%85%D8%A7%D8%B1%D8%B3%D8%A7%D8%AA)  أفضل الممارسات

1\. أبق الوكلاء مركزين

صمم كل وكيل بمسؤولية واحدة واضحة:

```
{
  "broadcast": {
    "DEV_GROUP": ["formatter", "linter", "tester"]
  }
}
```

✅ **جيد:** لكل وكيل مهمة واحدة. ❌ **سيئ:** وكيل عام واحد باسم “dev-helper”.

2\. استخدم أسماء وصفية

اجعل وظيفة كل وكيل واضحة:

```
{
  "agents": {
    "security-scanner": { "name": "Security Scanner" },
    "code-formatter": { "name": "Code Formatter" },
    "test-generator": { "name": "Test Generator" }
  }
}
```

3\. اضبط وصولا مختلفا إلى الأدوات

امنح الوكلاء الأدوات التي يحتاجونها فقط:

```
{
  "agents": {
    "reviewer": {
      "tools": { "allow": ["read", "exec"] } // Read-only
    },
    "fixer": {
      "tools": { "allow": ["read", "write", "edit", "exec"] } // Read-write
    }
  }
}
```

4\. راقب الأداء

مع كثرة الوكلاء، ضع في الحسبان:

- استخدام `"strategy": "parallel"` (الافتراضي) للسرعة
- قصر مجموعات البث على 5-10 وكلاء
- استخدام نماذج أسرع للوكلاء الأبسط

5\. تعامل مع الإخفاقات بسلاسة

يفشل الوكلاء بشكل مستقل. خطأ وكيل واحد لا يحظر الآخرين:

```
Message → [Agent A ✓, Agent B ✗ error, Agent C ✓]
Result: Agent A and C respond, Agent B logs error
```

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%AA%D9%88%D8%A7%D9%81%D9%82)  التوافق

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D9%85%D8%B2%D9%88%D9%91%D8%AF%D9%88%D9%86)  المزوّدون

تعمل مجموعات البث حاليا مع:

- ✅ WhatsApp (منفذ)
- 🚧 Telegram (مخطط له)
- 🚧 Discord (مخطط له)
- 🚧 Slack (مخطط له)

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%AA%D9%88%D8%AC%D9%8A%D9%87)  التوجيه

تعمل مجموعات البث إلى جانب التوجيه الحالي:

```
{
  "bindings": [\
    {\
      "match": { "channel": "whatsapp", "peer": { "kind": "group", "id": "GROUP_A" } },\
      "agentId": "alfred"\
    }\
  ],
  "broadcast": {
    "GROUP_B": ["agent1", "agent2"]
  }
}
```

- `GROUP_A`: يرد alfred فقط (التوجيه العادي).
- `GROUP_B`: يرد agent1 وagent2 (البث).

**الأسبقية:** تأخذ `broadcast` الأولوية على `bindings`.

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

الوكلاء لا يردون

**تحقق مما يلي:**

1. معرفات الوكلاء موجودة في `agents.list`.
2. تنسيق معرف النظير صحيح (مثلا، `120363403215116621@g.us`).
3. الوكلاء ليسوا في قوائم الرفض.

**التصحيح:**

```
tail -f ~/.openclaw/logs/gateway.log | grep broadcast
```

وكيل واحد فقط يرد

**السبب:** قد يكون معرف النظير موجودا في `bindings` لكنه غير موجود في `broadcast`.**الإصلاح:** أضفه إلى إعداد البث أو أزله من bindings.

مشكلات الأداء

إذا كان الأداء بطيئا مع كثرة الوكلاء:

- قلل عدد الوكلاء لكل مجموعة.
- استخدم نماذج أخف (sonnet بدلا من opus).
- تحقق من وقت بدء تشغيل sandbox.

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A3%D9%85%D8%AB%D9%84%D8%A9)  أمثلة

المثال 1: فريق مراجعة الكود

```
{
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": [\
      "code-formatter",\
      "security-scanner",\
      "test-coverage",\
      "docs-checker"\
    ]
  },
  "agents": {
    "list": [\
      {\
        "id": "code-formatter",\
        "workspace": "~/agents/formatter",\
        "tools": { "allow": ["read", "write"] }\
      },\
      {\
        "id": "security-scanner",\
        "workspace": "~/agents/security",\
        "tools": { "allow": ["read", "exec"] }\
      },\
      {\
        "id": "test-coverage",\
        "workspace": "~/agents/testing",\
        "tools": { "allow": ["read", "exec"] }\
      },\
      { "id": "docs-checker", "workspace": "~/agents/docs", "tools": { "allow": ["read"] } }\
    ]
  }
}
```

**يرسل المستخدم:** مقتطف كود.**الردود:**

- code-formatter: “تم إصلاح المسافات البادئة وإضافة تلميحات الأنواع”
- security-scanner: “⚠️ ثغرة حقن SQL في السطر 12”
- test-coverage: “التغطية 45%، والاختبارات لحالات الخطأ مفقودة”
- docs-checker: “سلسلة التوثيق مفقودة للدالة `process_data`”

المثال 2: دعم متعدد اللغات

```
{
  "broadcast": {
    "strategy": "sequential",
    "+15555550123": ["detect-language", "translator-en", "translator-de"]
  },
  "agents": {
    "list": [\
      { "id": "detect-language", "workspace": "~/agents/lang-detect" },\
      { "id": "translator-en", "workspace": "~/agents/translate-en" },\
      { "id": "translator-de", "workspace": "~/agents/translate-de" }\
    ]
  }
}
```

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%85%D8%B1%D8%AC%D8%B9-api)  مرجع API

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D9%85%D8%AE%D8%B7%D8%B7-%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  مخطط الإعداد

```
interface OpenClawConfig {
  broadcast?: {
    strategy?: "parallel" | "sequential";
    [peerId: string]: string[];
  };
}
```

### [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%AD%D9%82%D9%88%D9%84)  الحقول

[​](https://docs.openclaw.ai/ar/channels/broadcast-groups#param-strategy)

strategy

"parallel" \| "sequential"

افتراضي:"\\"parallel\\""

كيفية معالجة الوكلاء. يشغل `parallel` جميع الوكلاء في الوقت نفسه؛ ويشغلهم `sequential` بترتيب المصفوفة.

[​](https://docs.openclaw.ai/ar/channels/broadcast-groups#param-peer-id)

\[peerId\]

string\[\]

JID مجموعة WhatsApp أو رقم E.164 أو معرف نظير آخر. القيمة هي مصفوفة معرفات الوكلاء الذين ينبغي أن يعالجوا الرسائل.

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D9%82%D9%8A%D9%88%D8%AF)  القيود

1. **الحد الأقصى للوكلاء:** لا يوجد حد صارم، لكن 10 وكلاء أو أكثر قد يكون بطيئا.
2. **السياق المشترك:** لا يرى الوكلاء ردود بعضهم البعض (حسب التصميم).
3. **ترتيب الرسائل:** قد تصل الردود المتوازية بأي ترتيب.
4. **حدود المعدل:** يحتسب جميع الوكلاء ضمن حدود معدل WhatsApp.

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%A7%D9%84%D8%AA%D8%AD%D8%B3%D9%8A%D9%86%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%82%D8%A8%D9%84%D9%8A%D8%A9)  التحسينات المستقبلية

الميزات المخطط لها:

- [ ]  وضع السياق المشترك (يرى الوكلاء ردود بعضهم البعض)
- [ ]  تنسيق الوكلاء (يمكن للوكلاء إرسال إشارات لبعضهم البعض)
- [ ]  اختيار الوكلاء ديناميكيا (اختيار الوكلاء بناء على محتوى الرسالة)
- [ ]  أولويات الوكلاء (يرد بعض الوكلاء قبل غيرهم)

## [​](https://docs.openclaw.ai/ar/channels/broadcast-groups\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing)
- [المجموعات](https://docs.openclaw.ai/ar/channels/groups)
- [أدوات صندوق الرمل متعددة الوكلاء](https://docs.openclaw.ai/ar/tools/multi-agent-sandbox-tools)
- [الإقران](https://docs.openclaw.ai/ar/channels/pairing)
- [إدارة الجلسات](https://docs.openclaw.ai/ar/concepts/session)

[Groups](https://docs.openclaw.ai/ar/channels/groups) [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing)

Ctrl+I