---
source_url: https://docs.openclaw.ai/ar/cli/sandbox
title: "CLI \u0628\u064a\u0626\u0629 \u0627\u0644\u0627\u062e\u062a\u0628\u0627\u0631 \u0627\u0644\u0645\u0639\u0632\u0648\u0644\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/cli/sandbox#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Tools and execution

CLI بيئة الاختبار المعزولة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [نظرة عامة](https://docs.openclaw.ai/ar/cli/sandbox#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9)
- [الأوامر](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)
- [openclaw sandbox explain](https://docs.openclaw.ai/ar/cli/sandbox#openclaw-sandbox-explain)
- [openclaw sandbox list](https://docs.openclaw.ai/ar/cli/sandbox#openclaw-sandbox-list)
- [openclaw sandbox recreate](https://docs.openclaw.ai/ar/cli/sandbox#openclaw-sandbox-recreate)
- [حالات الاستخدام](https://docs.openclaw.ai/ar/cli/sandbox#%D8%AD%D8%A7%D9%84%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)
- [بعد تحديث صورة Docker](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%AD%D8%AF%D9%8A%D8%AB-%D8%B5%D9%88%D8%B1%D8%A9-docker)
- [بعد تغيير إعدادات sandbox](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-sandbox)
- [بعد تغيير هدف SSH أو مواد مصادقة SSH](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D9%87%D8%AF%D9%81-ssh-%D8%A3%D9%88-%D9%85%D9%88%D8%A7%D8%AF-%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9-ssh)
- [بعد تغيير مصدر OpenShell أو سياسته أو وضعه](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D9%85%D8%B5%D8%AF%D8%B1-openshell-%D8%A3%D9%88-%D8%B3%D9%8A%D8%A7%D8%B3%D8%AA%D9%87-%D8%A3%D9%88-%D9%88%D8%B6%D8%B9%D9%87)
- [بعد تغيير setupCommand](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-setupcommand)
- [لوكيل محدد فقط](https://docs.openclaw.ai/ar/cli/sandbox#%D9%84%D9%88%D9%83%D9%8A%D9%84-%D9%85%D8%AD%D8%AF%D8%AF-%D9%81%D9%82%D8%B7)
- [سبب الحاجة إلى ذلك](https://docs.openclaw.ai/ar/cli/sandbox#%D8%B3%D8%A8%D8%A8-%D8%A7%D9%84%D8%AD%D8%A7%D8%AC%D8%A9-%D8%A5%D9%84%D9%89-%D8%B0%D9%84%D9%83)
- [الإعدادات](https://docs.openclaw.ai/ar/cli/sandbox#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)
- [ذات صلة](https://docs.openclaw.ai/ar/cli/sandbox#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

إدارة أوقات تشغيل sandbox لتنفيذ الوكلاء بشكل معزول.

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D9%86%D8%B8%D8%B1%D8%A9-%D8%B9%D8%A7%D9%85%D8%A9)  نظرة عامة

يمكن لـ OpenClaw تشغيل الوكلاء في أوقات تشغيل sandbox معزولة لأغراض الأمان. تساعدك أوامر `sandbox` على فحص أوقات التشغيل هذه وإعادة إنشائها بعد التحديثات أو تغييرات الإعدادات.يعني ذلك اليوم عادة:

- حاويات Docker sandbox
- أوقات تشغيل SSH sandbox عندما تكون `agents.defaults.sandbox.backend = "ssh"`
- أوقات تشغيل OpenShell sandbox عندما تكون `agents.defaults.sandbox.backend = "openshell"`

بالنسبة إلى `ssh` وOpenShell `remote`، تكون إعادة الإنشاء أكثر أهمية من Docker:

- مساحة العمل البعيدة هي المصدر المعتمد بعد البذر الأولي
- يحذف `openclaw sandbox recreate` مساحة العمل البعيدة المعتمدة هذه للنطاق المحدد
- يؤدي الاستخدام التالي إلى بذرها مرة أخرى من مساحة العمل المحلية الحالية

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)  الأوامر

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#openclaw-sandbox-explain)  `openclaw sandbox explain`

افحص وضع/نطاق/وصول مساحة عمل sandbox **الفعلي**، وسياسة أدوات sandbox، وبوابات الرفع (مع مسارات مفاتيح الإعدادات للإصلاح).

```
openclaw sandbox explain
openclaw sandbox explain --session agent:main:main
openclaw sandbox explain --agent work
openclaw sandbox explain --json
```

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#openclaw-sandbox-list)  `openclaw sandbox list`

اعرض كل أوقات تشغيل sandbox مع حالتها وإعداداتها.

```
openclaw sandbox list
openclaw sandbox list --browser  # List only browser containers
openclaw sandbox list --json     # JSON output
```

**يتضمن الإخراج:**

- اسم وقت التشغيل وحالته
- الخلفية (`docker`، `openshell`، إلخ)
- تسمية الإعدادات وما إذا كانت تطابق الإعدادات الحالية
- العمر (الوقت منذ الإنشاء)
- وقت الخمول (الوقت منذ آخر استخدام)
- الجلسة/الوكيل المرتبط

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#openclaw-sandbox-recreate)  `openclaw sandbox recreate`

أزِل أوقات تشغيل sandbox لفرض إعادة إنشائها بالإعدادات المحدثة.

```
openclaw sandbox recreate --all                # Recreate all containers
openclaw sandbox recreate --session main       # Specific session
openclaw sandbox recreate --agent mybot        # Specific agent
openclaw sandbox recreate --browser            # Only browser containers
openclaw sandbox recreate --all --force        # Skip confirmation
```

**الخيارات:**

- `--all`: إعادة إنشاء كل حاويات sandbox
- `--session <key>`: إعادة إنشاء الحاوية لجلسة محددة
- `--agent <id>`: إعادة إنشاء الحاويات لوكيل محدد
- `--browser`: إعادة إنشاء حاويات المتصفح فقط
- `--force`: تخطي مطالبة التأكيد

تُعاد إنشاء أوقات التشغيل تلقائياً عند استخدام الوكيل في المرة التالية.

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%AD%D8%A7%D9%84%D8%A7%D8%AA-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)  حالات الاستخدام

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%AD%D8%AF%D9%8A%D8%AB-%D8%B5%D9%88%D8%B1%D8%A9-docker)  بعد تحديث صورة Docker

```
# Pull new image
docker pull openclaw-sandbox:latest
docker tag openclaw-sandbox:latest openclaw-sandbox:bookworm-slim

# Update config to use new image
# Edit config: agents.defaults.sandbox.docker.image (or agents.list[].sandbox.docker.image)

# Recreate containers
openclaw sandbox recreate --all
```

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-sandbox)  بعد تغيير إعدادات sandbox

```
# Edit config: agents.defaults.sandbox.* (or agents.list[].sandbox.*)

# Recreate to apply new config
openclaw sandbox recreate --all
```

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D9%87%D8%AF%D9%81-ssh-%D8%A3%D9%88-%D9%85%D9%88%D8%A7%D8%AF-%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9-ssh)  بعد تغيير هدف SSH أو مواد مصادقة SSH

```
# Edit config:
# - agents.defaults.sandbox.backend
# - agents.defaults.sandbox.ssh.target
# - agents.defaults.sandbox.ssh.workspaceRoot
# - agents.defaults.sandbox.ssh.identityFile / certificateFile / knownHostsFile
# - agents.defaults.sandbox.ssh.identityData / certificateData / knownHostsData

openclaw sandbox recreate --all
```

بالنسبة إلى خلفية `ssh` الأساسية، تحذف إعادة الإنشاء جذر مساحة العمل البعيدة لكل نطاق
على هدف SSH. يؤدي التشغيل التالي إلى بذرها مرة أخرى من مساحة العمل المحلية.

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-%D9%85%D8%B5%D8%AF%D8%B1-openshell-%D8%A3%D9%88-%D8%B3%D9%8A%D8%A7%D8%B3%D8%AA%D9%87-%D8%A3%D9%88-%D9%88%D8%B6%D8%B9%D9%87)  بعد تغيير مصدر OpenShell أو سياسته أو وضعه

```
# Edit config:
# - agents.defaults.sandbox.backend
# - plugins.entries.openshell.config.from
# - plugins.entries.openshell.config.mode
# - plugins.entries.openshell.config.policy

openclaw sandbox recreate --all
```

بالنسبة إلى وضع OpenShell `remote`، تحذف إعادة الإنشاء مساحة العمل البعيدة المعتمدة
لذلك النطاق. يؤدي التشغيل التالي إلى بذرها مرة أخرى من مساحة العمل المحلية.

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A8%D8%B9%D8%AF-%D8%AA%D8%BA%D9%8A%D9%8A%D8%B1-setupcommand)  بعد تغيير setupCommand

```
openclaw sandbox recreate --all
# or just one agent:
openclaw sandbox recreate --agent family
```

### [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D9%84%D9%88%D9%83%D9%8A%D9%84-%D9%85%D8%AD%D8%AF%D8%AF-%D9%81%D9%82%D8%B7)  لوكيل محدد فقط

```
# Update only one agent's containers
openclaw sandbox recreate --agent alfred
```

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%B3%D8%A8%D8%A8-%D8%A7%D9%84%D8%AD%D8%A7%D8%AC%D8%A9-%D8%A5%D9%84%D9%89-%D8%B0%D9%84%D9%83)  سبب الحاجة إلى ذلك

عند تحديث إعدادات sandbox:

- تستمر أوقات التشغيل الحالية بالعمل بالإعدادات القديمة.
- لا تُزال أوقات التشغيل إلا بعد 24 ساعة من عدم النشاط.
- يحافظ الوكلاء المستخدمون بانتظام على أوقات التشغيل القديمة إلى أجل غير مسمى.

استخدم `openclaw sandbox recreate` لفرض إزالة أوقات التشغيل القديمة. تُعاد إنشاؤها تلقائياً بالإعدادات الحالية عند الحاجة إليها لاحقاً.

فضّل `openclaw sandbox recreate` على التنظيف اليدوي الخاص بالخلفية. فهو يستخدم سجل أوقات التشغيل في Gateway ويتجنب حالات عدم التطابق عندما تتغير مفاتيح النطاق أو الجلسة.

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%A7%D9%84%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA)  الإعدادات

توجد إعدادات sandbox في `~/.openclaw/openclaw.json` ضمن `agents.defaults.sandbox` (توضع التجاوزات الخاصة بكل وكيل في `agents.list[].sandbox`):

```
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "all", // off, non-main, all
        "backend": "docker", // docker, ssh, openshell
        "scope": "agent", // session, agent, shared
        "docker": {
          "image": "openclaw-sandbox:bookworm-slim",
          "containerPrefix": "openclaw-sbx-",
          // ... more Docker options
        },
        "prune": {
          "idleHours": 24, // Auto-prune after 24h idle
          "maxAgeDays": 7, // Auto-prune after 7 days
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/cli/sandbox\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [مرجع CLI](https://docs.openclaw.ai/ar/cli)
- [Sandboxing](https://docs.openclaw.ai/ar/gateway/sandboxing)
- [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace)
- [Doctor](https://docs.openclaw.ai/ar/gateway/doctor): يتحقق من إعداد sandbox.

[العُقَد](https://docs.openclaw.ai/ar/cli/nodes) [Config](https://docs.openclaw.ai/ar/cli/config)

Ctrl+I