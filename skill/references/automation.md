# Automation

_11 pages from docs.openclaw.ai — full content preserved._

## Contents

- [التعليمات الدائمة - OpenClaw](#--openclaw)
- [تدفق المهام - OpenClaw](#--openclaw)
- [Automation & tasks - OpenClaw](#automation-tasks---openclaw)
- [Scheduled tasks - OpenClaw](#scheduled-tasks---openclaw)
- [https://docs.openclaw.ai/automation/cron-jobs.md](#httpsdocsopenclawaiautomationcron-jobsmd)
- [Hooks - OpenClaw](#hooks---openclaw)
- [Automation & tasks - OpenClaw](#automation-tasks---openclaw)
- [Background tasks - OpenClaw](#background-tasks---openclaw)
- [Ordens permanentes - OpenClaw](#ordens-permanentes---openclaw)
- [Постійні вказівки - OpenClaw](#--openclaw)
- [Потік завдань - OpenClaw](#--openclaw)

---

## التعليمات الدائمة - OpenClaw

_Source: <https://docs.openclaw.ai/ar/automation/standing-orders>_

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/automation/standing-orders#content-area)

[OpenClaw home page](https://docs.openclaw.ai/ar)

العربية

...ابحث

...ابحث

Automation and tasks

التعليمات الدائمة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [لماذا الأوامر الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders#%D9%84%D9%85%D8%A7%D8%B0%D8%A7-%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9)
- [كيف تعمل](https://docs.openclaw.ai/ar/automation/standing-orders#%D9%83%D9%8A%D9%81-%D8%AA%D8%B9%D9%85%D9%84)
- [بنية الأمر الدائم](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A8%D9%86%D9%8A%D8%A9-%D8%A7%D9%84%D8%A3%D9%85%D8%B1-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85)
- [الأوامر الدائمة مع مهام Cron](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1-%D8%A7%D9%84%D8%AF%D8%A7%D8%A6%D9%85%D8%A9-%D9%85%D8%B9-%D9%85%D9%87%D8%A7%D9%85-cron)
- [أمثلة](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A3%D9%85%D8%AB%D9%84%D8%A9)
- [المثال 1: المحتوى ووسائل التواصل الاجتماعي (دورة أسبوعية)](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A7%D9%84%D9%85%D8%AB%D8%A7%D9%84-1-%D8%A7%D9%84%D9%85%D8%AD%D8%AA%D9%88%D9%89-%D9%88%D9%88%D8%B3%D8%A7%D8%A6%D9%84-%D8%A7%D9%84%D8%AA%D9%88%D8%A7%D8%B5%D9%84-%D8%A7%D9%84%D8%A7%D8%AC%D8%AA%D9%85%D8%A7%D8%B9%D9%8A-%D8%AF%D9%88%D8%B1%D8%A9-%D8%A3%D8%B3%D8%A8%D9%88%D8%B9%D9%8A%D8%A9)
- [المثال 2: العمليات المالية (مشغلة بحدث)](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A7%D9%84%D9%85%D8%AB%D8%A7%D9%84-2-%D8%A7%D9%84%D8%B9%D9%85%D9%84%D9%8A%D8%A7%D8%AA-%D8%A7%D9%84%D9%85%D8%A7%D9%84%D9%8A%D8%A9-%D9%85%D8%B4%D8%BA%D9%84%D8%A9-%D8%A8%D8%AD%D8%AF%D8%AB)
- [المثال 3: المراقبة والتنبيهات (مستمر)](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A7%D9%84%D9%85%D8%AB%D8%A7%D9%84-3-%D8%A7%D9%84%D9%85%D8%B1%D8%A7%D9%82%D8%A8%D8%A9-%D9%88%D8%A7%D9%84%D8%AA%D9%86%D8%A8%D9%8A%D9%87%D8%A7%D8%AA-%D9%85%D8%B3%D8%AA%D9%85%D8%B1)
- [نمط التنفيذ والتحقق والتقرير](https://docs.openclaw.ai/ar/automation/standing-orders#%D9%86%D9%85%D8%B7-%D8%A7%D9%84%D8%AA%D9%86%D9%81%D9%8A%D8%B0-%D9%88%D8%A7%D9%84%D8%AA%D8%AD%D9%82%D9%82-%D9%88%D8%A7%D9%84%D8%AA%D9%82%D8%B1%D9%8A%D8%B1)
- [بنية متعددة البرامج](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A8%D9%86%D9%8A%D8%A9-%D9%85%D8%AA%D8%B9%D8%AF%D8%AF%D8%A9-%D8%A7%D9%84%D8%A8%D8%B1%D8%A7%D9%85%D8%AC)
- [أفضل الممارسات](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A3%D9%81%D8%B6%D9%84-%D8%A7%D9%84%D9%85%D9%85%D8%A7%D8%B1%D8%B3%D8%A7%D8%AA)
- [افعل](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%A7%D9%81%D8%B9%D9%84)
- [تجنّب](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%AA%D8%AC%D9%86%D9%91%D8%A8)
- [ذو صلة](https://docs.openclaw.ai/ar/automation/standing-orders#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

الأوامر الدائمة تمنح وكيلك **صلاحية تشغيل دائمة** لبرامج محددة. بدلاً من إعطاء تعليمات مهام فردية في كل مرة، تعرّف برامج ذات نطاق واضح، ومشغلات، وقواعد تصعيد — وينفذها الوكيل ذاتياً ضمن تلك الحدود.هذا هو الفرق بين أن تقول لمساعدك “أرسل التقرير الأسبوعي” كل يوم جمعة وبين منحه صلاحية دائمة: “أنت مسؤول عن التقرير الأسبوعي. اجمعه كل يوم جمعة، وأرسله، ولا تصعّد إلا إذا بدا أن هناك خطأ.”

## لماذا الأوامر الدائمة

**من دون أوامر دائمة:**

- يجب أن تطلب من الوكيل تنفيذ كل مهمة
- يبقى الوكيل خاملاً بين الطلبات
- تُنسى الأعمال الروتينية أو تتأخر
- تصبح أنت عنق الزجاجة

**مع الأوامر الدائمة:**

- ينفذ الوكيل ذاتياً ضمن حدود محددة
- يحدث العمل الروتيني وفق الجدول من دون طلب
- لا تتدخل إلا في الاستثناءات والموافقات
- يستثمر الوكيل وقت الخمول بإنتاجية

## كيف تعمل

تُعرّف الأوامر الدائمة في ملفات [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace). النهج الموصى به هو تضمينها مباشرةً في `AGENTS.md` (الذي يُحقن تلقائياً في كل جلسة) بحيث تكون دائماً ضمن سياق الوكيل. وللإعدادات الأكبر، يمكنك أيضاً وضعها في ملف مخصص مثل `standing-orders.md` والإشارة إليه من `AGENTS.md`.يحدد كل برنامج:

1. **النطاق** — ما الذي يُخوّل الوكيل بفعله
2. **المشغلات** — متى يُنفذ (جدول، حدث، أو شرط)
3. **بوابات الموافقة** — ما الذي يتطلب توقيعاً بشرياً قبل التصرف
4. **قواعد التصعيد** — متى يجب التوقف وطلب المساعدة

يحمّل الوكيل هذه التعليمات في كل جلسة عبر ملفات تمهيد مساحة العمل (راجع [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace) للاطلاع على القائمة الكاملة للملفات المحقونة تلقائياً) وينفذ وفقها، مع دمجها مع [مهام Cron](https://docs.openclaw.ai/ar/automation/cron-jobs) للفرض المستند إلى الوقت.

ضع الأوامر الدائمة في `AGENTS.md` لضمان تحميلها في كل جلسة. تمهيد مساحة العمل يحقن تلقائياً `AGENTS.md` و`SOUL.md` و`TOOLS.md` و`IDENTITY.md` و`USER.md` و`HEARTBEAT.md` و`BOOTSTRAP.md` و`MEMORY.md` — لكنه لا يحقن ملفات عشوائية في الأدلة الفرعية.

## بنية الأمر الدائم

```
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

## الأوامر الدائمة مع مهام Cron

تحدد الأوامر الدائمة **ما** الذي يُخوّل الوكيل بفعله. وتحدد [مهام Cron](https://docs.openclaw.ai/ar/automation/cron-jobs) **متى** يحدث ذلك. يعملان معاً:

```
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

يجب أن يشير موجه مهمة Cron إلى الأمر الدائم بدلاً من نسخه:

```
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

## أمثلة

### المثال 1: المحتوى ووسائل التواصل الاجتماعي (دورة أسبوعية)

```
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

### المثال 2: العمليات المالية (مشغلة بحدث)

```
## Program: Financial Processing

**Authority:** Process transaction data, generate reports, send summaries
**Approval gate:** None for analysis. Recommendations require owner approval.
**Trigger:** New data file detected OR scheduled monthly cycle

### When new data arrives

1. Detect new file in designated input directory
2. Parse and categorize all transactions
3. Compare against budget targets
4. Flag: unusual items, threshold breaches, new recurring charges
5. Generate report in designated output directory
6. Deliver summary to owner via configured channel

### Escalation rules

- Single item > $500: immediate alert
- Category > budget by 20%: flag in report
- Unrecognizable transaction: ask owner for categorization
- Failed processing after 2 retries: report failure, do not guess
```

### المثال 3: المراقبة والتنبيهات (مستمر)

```
## Program: System Monitoring

**Authority:** Check system health, restart services, send alerts
**Approval gate:** Restart services automatically. Escalate if restart fails twice.
**Trigger:** Every heartbeat cycle

### Checks

- Service health endpoints responding
- Disk space above threshold
- Pending tasks not stale (>24 hours)
- Delivery channels operational

### Response matrix

| Condition        | Action                   | Escalate?                |
| ---------------- | ------------------------ | ------------------------ |
| Service down     | Restart automatically    | Only if restart fails 2x |
| Disk space < 10% | Alert owner              | Yes                      |
| Stale task > 24h | Remind owner             | No                       |
| Channel offline  | Log and retry next cycle | If offline > 2 hours     |
```

## نمط التنفيذ والتحقق والتقرير

تعمل الأوامر الدائمة بأفضل شكل عند دمجها مع انضباط تنفيذ صارم. يجب أن تتبع كل مهمة في أمر دائم هذه الحلقة:

1. **نفّذ** — أنجز العمل الفعلي (لا تكتفِ بالإقرار بالتعليمة)
2. **تحقق** — أكد أن النتيجة صحيحة (الملف موجود، الرسالة سُلّمت، البيانات حُللت)
3. **أبلِغ** — أخبر المالك بما أُنجز وما تم التحقق منه

```
### Execution rules

- Every task follows Execute-Verify-Report. No exceptions.
- "I'll do that" is not execution. Do it, then report.
- "Done" without verification is not acceptable. Prove it.
- If execution fails: retry once with adjusted approach.
- If still fails: report failure with diagnosis. Never silently fail.
- Never retry indefinitely — 3 attempts max, then escalate.
```

يمنع هذا النمط أكثر أوضاع فشل الوكيل شيوعاً: الإقرار بالمهمة من دون إكمالها.

## بنية متعددة البرامج

للوكلاء الذين يديرون اهتمامات متعددة، نظّم الأوامر الدائمة كبرامج منفصلة ذات حدود واضحة:

```
## Program 1: [Domain A] (Weekly)

...

## Program 2: [Domain B] (Monthly + On-Demand)

...

## Program 3: [Domain C] (As-Needed)

...

## Escalation Rules (All Programs)

- [Common escalation criteria]
- [Approval gates that apply across programs]
```

يجب أن يكون لكل برنامج:

- **إيقاع تشغيل** خاص به (أسبوعي، شهري، قائم على الأحداث، مستمر)
- **بوابات موافقة** خاصة به (بعض البرامج تحتاج إشرافاً أكثر من غيرها)
- **حدود** واضحة (يجب أن يعرف الوكيل أين ينتهي برنامج ويبدأ آخر)

## أفضل الممارسات

### افعل

- ابدأ بصلاحية ضيقة ووسعها مع بناء الثقة
- عرّف بوابات موافقة صريحة للإجراءات عالية المخاطر
- ضمّن أقسام “ما يجب عدم فعله” — فالحدود مهمة بقدر الأذونات
- ادمجها مع مهام Cron للتنفيذ الموثوق المستند إلى الوقت
- راجع سجلات الوكيل أسبوعياً للتحقق من اتباع الأوامر الدائمة
- حدّث الأوامر الدائمة مع تطور احتياجاتك — فهي مستندات حية

### تجنّب

- منح صلاحية واسعة في اليوم الأول (“افعل ما تراه الأفضل”)
- تخطي قواعد التصعيد — كل برنامج يحتاج بنداً يحدد “متى تتوقف وتسأل”
- افتراض أن الوكيل سيتذكر التعليمات الشفهية — ضع كل شيء في الملف
- خلط الاهتمامات في برنامج واحد — افصل البرامج بحسب المجالات
- نسيان فرضها بمهام Cron — الأوامر الدائمة بلا مشغلات تصبح اقتراحات

## ذو صلة

- [الأتمتة والمهام](https://docs.openclaw.ai/ar/automation): كل آليات الأتمتة في لمحة واحدة.
- [مهام Cron](https://docs.openclaw.ai/ar/automation/cron-jobs): فرض الجدولة للأوامر الدائمة.
- [الخطافات](https://docs.openclaw.ai/ar/automation/hooks): سكربتات مدفوعة بالأحداث لأحداث دورة حياة الوكيل.
- [Webhooks](https://docs.openclaw.ai/ar/automation/cron-jobs#webhooks): مشغلات أحداث HTTP واردة.
- [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace): حيث توجد الأوامر الدائمة، بما في ذلك القائمة الكاملة لملفات التمهيد المحقونة تلقائياً (`AGENTS.md` و`SOUL.md` وما إلى ذلك).

[تدفق المهام](https://docs.openclaw.ai/ar/automation/taskflow) [الخطافات](https://docs.openclaw.ai/ar/automation/hooks)

Ctrl+I

---

## تدفق المهام - OpenClaw

_Source: <https://docs.openclaw.ai/ar/automation/taskflow>_

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

## كيف ترتبط التدفقات بالمهام

تنسق التدفقات المهام ولا تستبدلها. قد يدير تدفق واحد عدة مهام خلفية خلال عمره. استخدم `openclaw tasks` لفحص سجلات المهام الفردية و`openclaw tasks flow` لفحص التدفق المنسق.

## ذات صلة

- [المهام الخلفية](https://docs.openclaw.ai/ar/automation/tasks) — سجل العمل المنفصل الذي تنسقه التدفقات
- [CLI: المهام](https://docs.openclaw.ai/ar/cli/tasks) — مرجع أوامر CLI لـ `openclaw tasks flow`
- [نظرة عامة على الأتمتة](https://docs.openclaw.ai/ar/automation) — جميع آليات الأتمتة في لمحة
- [وظائف Cron](https://docs.openclaw.ai/ar/automation/cron-jobs) — وظائف مجدولة قد تغذي التدفقات

[Background tasks](https://docs.openclaw.ai/ar/automation/tasks) [التعليمات الدائمة](https://docs.openclaw.ai/ar/automation/standing-orders)

Ctrl+I

---

## Automation & tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation>_

[OpenClaw home page](https://docs.openclaw.ai/)

Automation and tasks

Automation & tasks

OpenClaw runs work in the background through tasks, scheduled jobs, inferred
commitments, event hooks, and standing instructions. This page helps you choose
the right mechanism and understand how they fit together.

## Quick decision guide

Yes

Exact

Flexible

Yes

Yes

Yes

Yes

Yes

What do you need?

Schedule work?

Track detached work?

Orchestrate multi-step flows?

React to lifecycle events?

Give the agent persistent instructions?

Remember a natural follow-up?

Exact timing or flexible?

Scheduled Tasks (Cron)

Heartbeat

Background Tasks

Task Flow

Hooks

Standing Orders

Inferred Commitments

| Use case | Recommended | Why |
| --- | --- | --- |
| Send daily report at 9 AM sharp | Scheduled Tasks (Cron) | Exact timing, isolated execution |
| Remind me in 20 minutes | Scheduled Tasks (Cron) | One-shot with precise timing (`--at`) |
| Run weekly deep analysis | Scheduled Tasks (Cron) | Standalone task, can use different model |
| Check inbox every 30 min | Heartbeat | Batches with other checks, context-aware |
| Monitor calendar for upcoming events | Heartbeat | Natural fit for periodic awareness |
| Check in after a mentioned interview | Inferred Commitments | Memory-like follow-up, no exact reminder request |
| Gentle care check-in after user context | Inferred Commitments | Scoped to the same agent and channel |
| Inspect status of a subagent or ACP run | Background Tasks | Tasks ledger tracks all detached work |
| Audit what ran and when | Background Tasks | `openclaw tasks list` and `openclaw tasks audit` |
| Multi-step research then summarize | Task Flow | Durable orchestration with revision tracking |
| Run a script on session reset | Hooks | Event-driven, fires on lifecycle events |
| Execute code on every tool call | Plugin hooks | In-process hooks can intercept tool calls |
| Always check compliance before replying | Standing Orders | Injected into every session automatically |

### Scheduled Tasks (Cron) vs Heartbeat

| Dimension | Scheduled Tasks (Cron) | Heartbeat |
| --- | --- | --- |
| Timing | Exact (cron expressions, one-shot) | Approximate (default every 30 min) |
| Session context | Fresh (isolated) or shared | Full main-session context |
| Task records | Always created | Never created |
| Delivery | Channel, webhook, or silent | Inline in main session |
| Best for | Reports, reminders, background jobs | Inbox checks, calendar, notifications |

Use Scheduled Tasks (Cron) when you need precise timing or isolated execution. Use Heartbeat when the work benefits from full session context and approximate timing is fine.

## Core concepts

### Scheduled tasks (cron)

Cron is the Gateway’s built-in scheduler for precise timing. It persists jobs, wakes the agent at the right time, and can deliver output to a chat channel or webhook endpoint. Supports one-shot reminders, recurring expressions, and inbound webhook triggers.See [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs).

### Tasks

The background task ledger tracks all detached work: ACP runs, subagent spawns, isolated cron executions, and CLI operations. Tasks are records, not schedulers. Use `openclaw tasks list` and `openclaw tasks audit` to inspect them.See [Background Tasks](https://docs.openclaw.ai/automation/tasks).

### Inferred commitments

Commitments are opt-in, short-lived follow-up memories. OpenClaw infers them
from normal conversations, scopes them to the same agent and channel, and
delivers due check-ins through heartbeat. Exact user-requested reminders still
belong to cron.See [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments).

### Task Flow

Task Flow is the flow orchestration substrate above background tasks. It manages durable multi-step flows with managed and mirrored sync modes, revision tracking, and `openclaw tasks flow list|show|cancel` for inspection.See [Task Flow](https://docs.openclaw.ai/automation/taskflow).

### Standing orders

Standing orders grant the agent permanent operating authority for defined programs. They live in workspace files (typically `AGENTS.md`) and are injected into every session. Combine with cron for time-based enforcement.See [Standing Orders](https://docs.openclaw.ai/automation/standing-orders).

### Hooks

Internal hooks are event-driven scripts triggered by agent lifecycle events
(`/new`, `/reset`, `/stop`), session compaction, gateway startup, and message
flow. They are automatically discovered from directories and can be managed
with `openclaw hooks`. For in-process tool-call interception, use
[Plugin hooks](https://docs.openclaw.ai/plugins/hooks).See [Hooks](https://docs.openclaw.ai/automation/hooks).

### Heartbeat

Heartbeat is a periodic main-session turn (default every 30 minutes). It batches multiple checks (inbox, calendar, notifications) in one agent turn with full session context. Heartbeat turns do not create task records and do not extend daily/idle session reset freshness. Use `HEARTBEAT.md` for a small checklist, or a `tasks:` block when you want due-only periodic checks inside heartbeat itself. Empty heartbeat files skip as `empty-heartbeat-file`; due-only task mode skips as `no-tasks-due`. Heartbeats defer while cron work is active or queued, and `heartbeat.skipWhenBusy` can also defer them while subagent or nested lanes are busy.See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

## How they work together

- **Cron** handles precise schedules (daily reports, weekly reviews) and one-shot reminders. All cron executions create task records.
- **Heartbeat** handles routine monitoring (inbox, calendar, notifications) in one batched turn every 30 minutes.
- **Hooks** react to specific events (session resets, compaction, message flow) with custom scripts. Plugin hooks cover tool calls.
- **Standing orders** give the agent persistent context and authority boundaries.
- **Task Flow** coordinates multi-step flows above individual tasks.
- **Tasks** automatically track all detached work so you can inspect and audit it.

## Related

- [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) — precise scheduling and one-shot reminders
- [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments) — memory-like follow-up check-ins
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — task ledger for all detached work
- [Task Flow](https://docs.openclaw.ai/automation/taskflow) — durable multi-step flow orchestration
- [Hooks](https://docs.openclaw.ai/automation/hooks) — event-driven lifecycle scripts
- [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) — in-process tool, prompt, message, and lifecycle hooks
- [Standing Orders](https://docs.openclaw.ai/automation/standing-orders) — persistent agent instructions
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference) — all config keys

[OpenProse](https://docs.openclaw.ai/prose) [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

Ctrl+I

---

## Scheduled tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/cron-jobs>_

# Intended: "9 AM on the 15th, only if it's a Monday"
# Actual:   "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 * 1
```

This fires ~5–6 times per month instead of 0–1 times per month. OpenClaw uses Croner’s default OR behavior here. To require both conditions, use Croner’s `+` day-of-week modifier (`0 9 15 * +1`) or schedule on one field and guard the other in your job’s prompt or command.

## Execution styles

| Style | `--session` value | Runs in | Best for |
| --- | --- | --- | --- |
| Main session | `main` | Next heartbeat turn | Reminders, system events |
| Isolated | `isolated` | Dedicated `cron:<jobId>` | Reports, background chores |
| Current session | `current` | Bound at creation time | Context-aware recurring work |
| Custom session | `session:custom-id` | Persistent named session | Workflows that build on history |

Main session vs isolated vs custom

**Main session** jobs enqueue a system event and optionally wake the heartbeat (`--wake now` or `--wake next-heartbeat`). Those system events do not extend daily/idle reset freshness for the target session. **Isolated** jobs run a dedicated agent turn with a fresh session. **Custom sessions** (`session:xxx`) persist context across runs, enabling workflows like daily standups that build on previous summaries.

What 'fresh session' means for isolated jobs

For isolated jobs, “fresh session” means a new transcript/session id for each run. OpenClaw may carry safe preferences such as thinking/fast/verbose settings, labels, and explicit user-selected model/auth overrides, but it does not inherit ambient conversation context from an older cron row: channel/group routing, send or queue policy, elevation, origin, or ACP runtime binding. Use `current` or `session:<id>` when a recurring job should deliberately build on the same conversation context.

Runtime cleanup

For isolated jobs, runtime teardown now includes best-effort browser cleanup for that cron session. Cleanup failures are ignored so the actual cron result still wins.Isolated cron runs also dispose any bundled MCP runtime instances created for the job through the shared runtime-cleanup path. This matches how main-session and custom-session MCP clients are torn down, so isolated cron jobs do not leak stdio child processes or long-lived MCP connections across runs.

Subagent and Discord delivery

When isolated cron runs orchestrate subagents, delivery also prefers the final descendant output over stale parent interim text. If descendants are still running, OpenClaw suppresses that partial parent update instead of announcing it.For text-only Discord announce targets, OpenClaw sends the canonical final assistant text once instead of replaying both streamed/intermediate text payloads and the final answer. Media and structured Discord payloads are still delivered as separate payloads so attachments and components are not dropped.

### Payload options for isolated jobs

[​](https://docs.openclaw.ai/automation/cron-jobs#param-message)

--message

string

required

Prompt text (required for isolated).

[​](https://docs.openclaw.ai/automation/cron-jobs#param-model)

--model

string

Model override; uses the selected allowed model for the job.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-thinking)

--thinking

string

Thinking level override.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-light-context)

--light-context

boolean

Skip workspace bootstrap file injection.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-tools)

--tools

string

Restrict which tools the job can use, for example `--tools exec,read`.

`--model` uses the selected allowed model as that job’s primary model. It is not the same as a chat-session `/model` override: configured fallback chains still apply when the job primary fails. If the requested model is not allowed or cannot be resolved, cron fails the run with an explicit validation error instead of silently falling back to the job’s agent/default model selection.Cron jobs can also carry payload-level `fallbacks`. When present, that list replaces the configured fallback chain for the job. Use `fallbacks: []` in the job payload/API when you want a strict cron run that tries only the selected model. If a job has `--model` but neither payload nor configured fallbacks, OpenClaw passes an explicit empty fallback override so the agent primary is not appended as a hidden extra retry target.Model-selection precedence for isolated jobs is:

1. Gmail hook model override (when the run came from Gmail and that override is allowed)
2. Per-job payload `model`
3. User-selected stored cron session model override
4. Agent/default model selection

Fast mode follows the resolved live selection too. If the selected model config has `params.fastMode`, isolated cron uses that by default. A stored session `fastMode` override still wins over config in either direction.If an isolated run hits a live model-switch handoff, cron retries with the switched provider/model and persists that live selection for the active run before retrying. When the switch also carries a new auth profile, cron persists that auth profile override for the active run too. Retries are bounded: after the initial attempt plus 2 switch retries, cron aborts instead of looping forever.Before an isolated cron run enters the agent runner, OpenClaw checks reachable local provider endpoints for configured `api: "ollama"` and `api: "openai-completions"` providers whose `baseUrl` is loopback, private-network, or `.local`. If that endpoint is down, the run is recorded as `skipped` with a clear provider/model error instead of starting a model call. The endpoint result is cached for 5 minutes, so many due jobs using the same dead local Ollama, vLLM, SGLang, or LM Studio server share one small probe instead of creating a request storm. Skipped provider-preflight runs do not increment execution-error backoff; enable `failureAlert.includeSkipped` when you want repeated skip notifications.

## Delivery and output

| Mode | What happens |
| --- | --- |
| `announce` | Fallback-deliver final text to the target if the agent did not send |
| `webhook` | POST finished event payload to a URL |
| `none` | No runner fallback delivery |

Use `--announce --channel telegram --to "-1001234567890"` for channel delivery. For Telegram forum topics, use `-1001234567890:topic:123`; direct RPC/config callers may also pass `delivery.threadId` as a string or number. Slack/Discord/Mattermost targets should use explicit prefixes (`channel:<id>`, `user:<id>`). Matrix room IDs are case-sensitive; use the exact room ID or `room:!room:server` form from Matrix.When announce delivery uses `channel: "last"` or omits `channel`, a provider-prefixed target such as `telegram:123` can select the channel before cron falls back to session history or a single configured channel. Only prefixes advertised by the loaded plugin are provider selectors. If `delivery.channel` is explicit, the target prefix must name the same provider; for example, `channel: "whatsapp"` with `to: "telegram:123"` is rejected instead of letting WhatsApp interpret the Telegram ID as a phone number. Target-kind and service prefixes such as `channel:<id>`, `user:<id>`, `imessage:<handle>`, and `sms:<number>` remain channel-owned target syntax, not provider selectors.For isolated jobs, chat delivery is shared. If a chat route is available, the agent can use the `message` tool even when the job uses `--no-deliver`. If the agent sends to the configured/current target, OpenClaw skips the fallback announce. Otherwise `announce`, `webhook`, and `none` only control what the runner does with the final reply after the agent turn.When an agent creates an isolated reminder from an active chat, OpenClaw stores the preserved live delivery target for the fallback announce route. Internal session keys may be lowercase; provider delivery targets are not reconstructed from those keys when current chat context is available.Implicit announce delivery uses configured channel allowlists to validate and reroute stale targets. DM pairing-store approvals are not fallback automation recipients; set `delivery.to` or configure the channel `allowFrom` entry when a scheduled job should proactively send to a DM.Failure notifications follow a separate destination path:

- `cron.failureDestination` sets a global default for failure notifications.
- `job.delivery.failureDestination` overrides that per job.
- If neither is set and the job already delivers via `announce`, failure notifications now fall back to that primary announce target.
- `delivery.failureDestination` is only supported on `sessionTarget="isolated"` jobs unless the primary delivery mode is `webhook`.
- `failureAlert.includeSkipped: true` opts a job or global cron alert policy into repeated skipped-run alerts. Skipped runs keep a separate consecutive skip counter, so they do not affect execution-error backoff.

## CLI examples

- One-shot reminder

- Recurring isolated job

- Model and thinking override

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

## Webhooks

Gateway can expose HTTP webhook endpoints for external triggers. Enable in config:

```
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
  },
}
```

### Authentication

Every request must include the hook token via header:

- `Authorization: Bearer <token>` (recommended)
- `x-openclaw-token: <token>`

Query-string tokens are rejected.

POST /hooks/wake

Enqueue a system event for the main session:

```
curl -X POST http://127.0.0.1:18789/hooks/wake \
  -H 'Authorization: Bearer SECRET' \
  -H 'Content-Type: application/json' \
  -d '{"text":"New email received","mode":"now"}'
```

[​](https://docs.openclaw.ai/automation/cron-jobs#param-text)

text

string

required

Event description.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-mode)

mode

string

default:"now"

`now` or `next-heartbeat`.

POST /hooks/agent

Run an isolated agent turn:

```
curl -X POST http://127.0.0.1:18789/hooks/agent \
  -H 'Authorization: Bearer SECRET' \
  -H 'Content-Type: application/json' \
  -d '{"message":"Summarize inbox","name":"Email","model":"openai/gpt-5.4"}'
```

Fields: `message` (required), `name`, `agentId`, `wakeMode`, `deliver`, `channel`, `to`, `model`, `fallbacks`, `thinking`, `timeoutSeconds`.

Mapped hooks (POST /hooks/<name>)

Custom hook names are resolved via `hooks.mappings` in config. Mappings can transform arbitrary payloads into `wake` or `agent` actions with templates or code transforms.

Keep hook endpoints behind loopback, tailnet, or trusted reverse proxy.

- Use a dedicated hook token; do not reuse gateway auth tokens.
- Keep `hooks.path` on a dedicated subpath; `/` is rejected.
- Set `hooks.allowedAgentIds` to limit explicit `agentId` routing.
- Keep `hooks.allowRequestSessionKey=false` unless you require caller-selected sessions.
- If you enable `hooks.allowRequestSessionKey`, also set `hooks.allowedSessionKeyPrefixes` to constrain allowed session key shapes.
- Hook payloads are wrapped with safety boundaries by default.

## Gmail PubSub integration

Wire Gmail inbox triggers to OpenClaw via Google PubSub.

**Prerequisites:**`gcloud` CLI, `gog` (gogcli), OpenClaw hooks enabled, Tailscale for the public HTTPS endpoint.

### Wizard setup (recommended)

```
openclaw webhooks gmail setup --account openclaw@gmail.com
```

This writes `hooks.gmail` config, enables the Gmail preset, and uses Tailscale Funnel for the push endpoint.

### Gateway auto-start

When `hooks.enabled=true` and `hooks.gmail.account` is set, the Gateway starts `gog gmail watch serve` on boot and auto-renews the watch. Set `OPENCLAW_SKIP_GMAIL_WATCHER=1` to opt out.

### Manual one-time setup

1

[Navigate to header](https://docs.openclaw.ai/automation/cron-jobs#)

Select the GCP project

Select the GCP project that owns the OAuth client used by `gog`:

```
gcloud auth login
gcloud config set project <project-id>
gcloud services enable gmail.googleapis.com pubsub.googleapis.com
```

2

[Navigate to header](https://docs.openclaw.ai/automation/cron-jobs#)

Create topic and grant Gmail push access

```
gcloud pubsub topics create gog-gmail-watch
gcloud pubsub topics add-iam-policy-binding gog-gmail-watch \
  --member=serviceAccount:gmail-api-push@system.gserviceaccount.com \
  --role=roles/pubsub.publisher
```

3

[Navigate to header](https://docs.openclaw.ai/automation/cron-jobs#)

Start the watch

```
gog gmail watch start \
  --account openclaw@gmail.com \
  --label INBOX \
  --topic projects/<project-id>/topics/gog-gmail-watch
```

### Gmail model override

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

## Managing jobs

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

Model override note:

- `openclaw cron add|edit --model ...` changes the job’s selected model.
- If the model is allowed, that exact provider/model reaches the isolated agent run.
- If it is not allowed or cannot be resolved, cron fails the run with an explicit validation error.
- Configured fallback chains still apply because cron `--model` is a job primary, not a session `/model` override.
- Payload `fallbacks` replaces configured fallbacks for that job; `fallbacks: []` disables fallback and makes the run strict.
- A plain `--model` with no explicit or configured fallback list does not fall through to the agent primary as a silent extra retry target.

## Configuration

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

`maxConcurrentRuns` limits both scheduled cron dispatch and isolated agent-turn execution. Isolated cron agent turns use the queue’s dedicated `cron-nested` execution lane internally, so raising this value lets independent cron LLM runs progress in parallel instead of only starting their outer cron wrappers. The shared non-cron `nested` lane is not widened by this setting.The runtime state sidecar is derived from `cron.store`: a `.json` store such as `~/clawd/cron/jobs.json` uses `~/clawd/cron/jobs-state.json`, while a store path without a `.json` suffix appends `-state.json`.If you hand-edit `jobs.json`, leave `jobs-state.json` out of source control. OpenClaw uses that sidecar for pending slots, active markers, last-run metadata, and the schedule identity that tells the scheduler when an externally edited job needs a fresh `nextRunAtMs`.Disable cron: `cron.enabled: false` or `OPENCLAW_SKIP_CRON=1`.

Retry behavior

**One-shot retry**: transient errors (rate limit, overload, network, server error) retry up to 3 times with exponential backoff. Permanent errors disable immediately.**Recurring retry**: exponential backoff (30s to 60m) between retries. Backoff resets after the next successful run.

Maintenance

`cron.sessionRetention` (default `24h`) prunes isolated run-session entries. `cron.runLog.maxBytes` / `cron.runLog.keepLines` auto-prune run-log files.

## Troubleshooting

### Command ladder

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

Cron not firing

- Check `cron.enabled` and `OPENCLAW_SKIP_CRON` env var.
- Confirm the Gateway is running continuously.
- For `cron` schedules, verify timezone (`--tz`) vs the host timezone.
- `reason: not-due` in run output means manual run was checked with `openclaw cron run <jobId> --due` and the job was not due yet.

Cron fired but no delivery

- Delivery mode `none` means no runner fallback send is expected. The agent can still send directly with the `message` tool when a chat route is available.
- Delivery target missing/invalid (`channel`/`to`) means outbound was skipped.
- For Matrix, copied or legacy jobs with lowercased `delivery.to` room IDs can fail because Matrix room IDs are case-sensitive. Edit the job to the exact `!room:server` or `room:!room:server` value from Matrix.
- Channel auth errors (`unauthorized`, `Forbidden`) mean delivery was blocked by credentials.
- If the isolated run returns only the silent token (`NO_REPLY` / `no_reply`), OpenClaw suppresses direct outbound delivery and also suppresses the fallback queued summary path, so nothing is posted back to chat.
- If the agent should message the user itself, check that the job has a usable route (`channel: "last"` with a previous chat, or an explicit channel/target).

Cron or heartbeat appears to prevent /new-style rollover

- Daily and idle reset freshness is not based on `updatedAt`; see [Session management](https://docs.openclaw.ai/concepts/session#session-lifecycle).
- Cron wakeups, heartbeat runs, exec notifications, and gateway bookkeeping may update the session row for routing/status, but they do not extend `sessionStartedAt` or `lastInteractionAt`.
- For legacy rows created before those fields existed, OpenClaw can recover `sessionStartedAt` from the transcript JSONL session header when the file is still available. Legacy idle rows without `lastInteractionAt` use that recovered start time as their idle baseline.

Timezone gotchas

- Cron without `--tz` uses the gateway host timezone.
- `at` schedules without timezone are treated as UTC.
- Heartbeat `activeHours` uses configured timezone resolution.

## Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — all automation mechanisms at a glance
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — task ledger for cron executions
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Timezone](https://docs.openclaw.ai/concepts/timezone) — timezone configuration

[Automation & tasks](https://docs.openclaw.ai/automation) [Background tasks](https://docs.openclaw.ai/automation/tasks)

Ctrl+I

---

## https://docs.openclaw.ai/automation/cron-jobs.md

_Source: <https://docs.openclaw.ai/automation/cron-jobs.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Scheduled tasks

Cron is the Gateway's built-in scheduler. It persists jobs, wakes the agent at the right time, and can deliver output back to a chat channel or webhook endpoint.

\## Quick start

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron add \
 --name "Reminder" \
 --at "2026-02-01T16:00:00Z" \
 --session main \
 --system-event "Reminder: check the cron docs draft" \
 --wake now \
 --delete-after-run
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron list
 openclaw cron show
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron runs --id
 \`\`\`

\## How cron works

\\* Cron runs \*\*inside the Gateway\*\* process (not inside the model).
\\* Job definitions persist at \`~/.openclaw/cron/jobs.json\` so restarts do not lose schedules.
\\* Runtime execution state persists next to it in \`~/.openclaw/cron/jobs-state.json\`. If you track cron definitions in git, track \`jobs.json\` and gitignore \`jobs-state.json\`.
\\* After the split, older OpenClaw versions can read \`jobs.json\` but may treat jobs as fresh because runtime fields now live in \`jobs-state.json\`.
\\* When \`jobs.json\` is edited while the Gateway is running or stopped, OpenClaw compares the changed schedule fields with pending runtime slot metadata and clears stale \`nextRunAtMs\` values. Pure formatting or key-order-only rewrites preserve the pending slot.
\\* All cron executions create \[background task\](/automation/tasks) records.
\\* On Gateway startup, overdue isolated agent-turn jobs are rescheduled out of the channel-connect window instead of replaying immediately, so Discord/Telegram startup and native-command setup stay responsive after restarts.
\\* One-shot jobs (\`--at\`) auto-delete after success by default.
\\* Isolated cron runs best-effort close tracked browser tabs/processes for their \`cron:\` session when the run completes, so detached browser automation does not leave orphaned processes behind.
\\* Isolated cron runs also guard against stale acknowledgement replies. If the first result is just an interim status update (\`on it\`, \`pulling everything together\`, and similar hints) and no descendant subagent run is still responsible for the final answer, OpenClaw re-prompts once for the actual result before delivery.
\\* Isolated cron runs prefer structured execution-denial metadata from the embedded run, then fall back to known final summary/output markers such as \`SYSTEM\_RUN\_DENIED\` and \`INVALID\_REQUEST\`, so a blocked command is not reported as a green run.
\\* Isolated cron runs also treat run-level agent failures as job errors even when no reply payload is produced, so model/provider failures increment error counters and trigger failure notifications instead of clearing the job as successful.
\\* When an isolated agent-turn job reaches \`timeoutSeconds\`, cron aborts the underlying agent run and gives it a short cleanup window. If the run does not drain, Gateway-owned cleanup force-clears that run's session ownership before cron records the timeout, so queued chat work is not left behind a stale processing session.

 Task reconciliation for cron is runtime-owned first, durable-history-backed second: an active cron task stays live while the cron runtime still tracks that job as running, even if an old child session row still exists. Once the runtime stops owning the job and the 5-minute grace window expires, maintenance checks persisted run logs and job state for the matching \`cron::\` run. If that durable history shows a terminal result, the task ledger is finalized from it; otherwise Gateway-owned maintenance can mark the task \`lost\`. Offline CLI audit can recover from durable history, but it does not treat its own empty in-process active-job set as proof that a Gateway-owned cron run is gone.

\## Schedule types

\| Kind \| CLI flag \| Description \|
\| \-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`at\` \| \`--at\` \| One-shot timestamp (ISO 8601 or relative like \`20m\`) \|
\| \`every\` \| \`--every\` \| Fixed interval \|
\| \`cron\` \| \`--cron\` \| 5-field or 6-field cron expression with optional \`--tz\` \|

Timestamps without a timezone are treated as UTC. Add \`--tz America/New\_York\` for local wall-clock scheduling.

Recurring top-of-hour expressions are automatically staggered by up to 5 minutes to reduce load spikes. Use \`--exact\` to force precise timing or \`--stagger 30s\` for an explicit window.

\### Day-of-month and day-of-week use OR logic

Cron expressions are parsed by \[croner\](https://github.com/Hexagon/croner). When both the day-of-month and day-of-week fields are non-wildcard, croner matches when \*\*either\*\* field matches — not both. This is standard Vixie cron behavior.

\`\`\`
\# Intended: "9 AM on the 15th, only if it's a Monday"
\# Actual: "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 \* 1
\`\`\`

This fires \\~5–6 times per month instead of 0–1 times per month. OpenClaw uses Croner's default OR behavior here. To require both conditions, use Croner's \`+\` day-of-week modifier (\`0 9 15 \* +1\`) or schedule on one field and guard the other in your job's prompt or command.

\## Execution styles

\| Style \| \`--session\` value \| Runs in \| Best for \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| Main session \| \`main\` \| Next heartbeat turn \| Reminders, system events \|
\| Isolated \| \`isolated\` \| Dedicated \`cron:\` \| Reports, background chores \|
\| Current session \| \`current\` \| Bound at creation time \| Context-aware recurring work \|
\| Custom session \| \`session:custom-id\` \| Persistent named session \| Workflows that build on history \|

 \*\*Main session\*\* jobs enqueue a system event and optionally wake the heartbeat (\`--wake now\` or \`--wake next-heartbeat\`). Those system events do not extend daily/idle reset freshness for the target session. \*\*Isolated\*\* jobs run a dedicated agent turn with a fresh session. \*\*Custom sessions\*\* (\`session:xxx\`) persist context across runs, enabling workflows like daily standups that build on previous summaries.

 For isolated jobs, "fresh session" means a new transcript/session id for each run. OpenClaw may carry safe preferences such as thinking/fast/verbose settings, labels, and explicit user-selected model/auth overrides, but it does not inherit ambient conversation context from an older cron row: channel/group routing, send or queue policy, elevation, origin, or ACP runtime binding. Use \`current\` or \`session:\` when a recurring job should deliberately build on the same conversation context.

 For isolated jobs, runtime teardown now includes best-effort browser cleanup for that cron session. Cleanup failures are ignored so the actual cron result still wins.

 Isolated cron runs also dispose any bundled MCP runtime instances created for the job through the shared runtime-cleanup path. This matches how main-session and custom-session MCP clients are torn down, so isolated cron jobs do not leak stdio child processes or long-lived MCP connections across runs.

 When isolated cron runs orchestrate subagents, delivery also prefers the final descendant output over stale parent interim text. If descendants are still running, OpenClaw suppresses that partial parent update instead of announcing it.

 For text-only Discord announce targets, OpenClaw sends the canonical final assistant text once instead of replaying both streamed/intermediate text payloads and the final answer. Media and structured Discord payloads are still delivered as separate payloads so attachments and components are not dropped.

\### Payload options for isolated jobs

 Prompt text (required for isolated).

 Model override; uses the selected allowed model for the job.

 Thinking level override.

 Skip workspace bootstrap file injection.

 Restrict which tools the job can use, for example \`--tools exec,read\`.

\`--model\` uses the selected allowed model as that job's primary model. It is not the same as a chat-session \`/model\` override: configured fallback chains still apply when the job primary fails. If the requested model is not allowed or cannot be resolved, cron fails the run with an explicit validation error instead of silently falling back to the job's agent/default model selection.

Cron jobs can also carry payload-level \`fallbacks\`. When present, that list replaces the configured fallback chain for the job. Use \`fallbacks: \[\]\` in the job payload/API when you want a strict cron run that tries only the selected model. If a job has \`--model\` but neither payload nor configured fallbacks, OpenClaw passes an explicit empty fallback override so the agent primary is not appended as a hidden extra retry target.

Model-selection precedence for isolated jobs is:

1\. Gmail hook model override (when the run came from Gmail and that override is allowed)
2\. Per-job payload \`model\`
3\. User-selected stored cron session model override
4\. Agent/default model selection

Fast mode follows the resolved live selection too. If the selected model config has \`params.fastMode\`, isolated cron uses that by default. A stored session \`fastMode\` override still wins over config in either direction.

If an isolated run hits a live model-switch handoff, cron retries with the switched provider/model and persists that live selection for the active run before retrying. When the switch also carries a new auth profile, cron persists that auth profile override for the active run too. Retries are bounded: after the initial attempt plus 2 switch retries, cron aborts instead of looping forever.

Before an isolated cron run enters the agent runner, OpenClaw checks reachable local provider endpoints for configured \`api: "ollama"\` and \`api: "openai-completions"\` providers whose \`baseUrl\` is loopback, private-network, or \`.local\`. If that endpoint is down, the run is recorded as \`skipped\` with a clear provider/model error instead of starting a model call. The endpoint result is cached for 5 minutes, so many due jobs using the same dead local Ollama, vLLM, SGLang, or LM Studio server share one small probe instead of creating a request storm. Skipped provider-preflight runs do not increment execution-error backoff; enable \`failureAlert.includeSkipped\` when you want repeated skip notifications.

\## Delivery and output

\| Mode \| What happens \|
\| \-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`announce\` \| Fallback-deliver final text to the target if the agent did not send \|
\| \`webhook\` \| POST finished event payload to a URL \|
\| \`none\` \| No runner fallback delivery \|

Use \`--announce --channel telegram --to "-1001234567890"\` for channel delivery. For Telegram forum topics, use \`-1001234567890:topic:123\`; direct RPC/config callers may also pass \`delivery.threadId\` as a string or number. Slack/Discord/Mattermost targets should use explicit prefixes (\`channel:\`, \`user:\`). Matrix room IDs are case-sensitive; use the exact room ID or \`room:!room:server\` form from Matrix.

When announce delivery uses \`channel: "last"\` or omits \`channel\`, a provider-prefixed target such as \`telegram:123\` can select the channel before cron falls back to session history or a single configured channel. Only prefixes advertised by the loaded plugin are provider selectors. If \`delivery.channel\` is explicit, the target prefix must name the same provider; for example, \`channel: "whatsapp"\` with \`to: "telegram:123"\` is rejected instead of letting WhatsApp interpret the Telegram ID as a phone number. Target-kind and service prefixes such as \`channel:\`, \`user:\`, \`imessage:\`, and \`sms:\` remain channel-owned target syntax, not provider selectors.

For isolated jobs, chat delivery is shared. If a chat route is available, the agent can use the \`message\` tool even when the job uses \`--no-deliver\`. If the agent sends to the configured/current target, OpenClaw skips the fallback announce. Otherwise \`announce\`, \`webhook\`, and \`none\` only control what the runner does with the final reply after the agent turn.

When an agent creates an isolated reminder from an active chat, OpenClaw stores the preserved live delivery target for the fallback announce route. Internal session keys may be lowercase; provider delivery targets are not reconstructed from those keys when current chat context is available.

Implicit announce delivery uses configured channel allowlists to validate and reroute stale targets. DM pairing-store approvals are not fallback automation recipients; set \`delivery.to\` or configure the channel \`allowFrom\` entry when a scheduled job should proactively send to a DM.

Failure notifications follow a separate destination path:

\\* \`cron.failureDestination\` sets a global default for failure notifications.
\\* \`job.delivery.failureDestination\` overrides that per job.
\\* If neither is set and the job already delivers via \`announce\`, failure notifications now fall back to that primary announce target.
\\* \`delivery.failureDestination\` is only supported on \`sessionTarget="isolated"\` jobs unless the primary delivery mode is \`webhook\`.
\\* \`failureAlert.includeSkipped: true\` opts a job or global cron alert policy into repeated skipped-run alerts. Skipped runs keep a separate consecutive skip counter, so they do not affect execution-error backoff.

\## CLI examples

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron add \
 --name "Calendar check" \
 --at "20m" \
 --session main \
 --system-event "Next heartbeat: check calendar." \
 --wake now
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron add \
 --name "Morning brief" \
 --cron "0 7 \* \* \*" \
 --tz "America/Los\_Angeles" \
 --session isolated \
 --message "Summarize overnight updates." \
 --announce \
 --channel slack \
 --to "channel:C1234567890"
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron add \
 --name "Deep analysis" \
 --cron "0 6 \* \* 1" \
 --tz "America/Los\_Angeles" \
 --session isolated \
 --message "Weekly deep analysis of project progress." \
 --model "opus" \
 --thinking high \
 --announce
 \`\`\`

\## Webhooks

Gateway can expose HTTP webhook endpoints for external triggers. Enable in config:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 hooks: {
 enabled: true,
 token: "shared-secret",
 path: "/hooks",
 },
}
\`\`\`

\### Authentication

Every request must include the hook token via header:

\\* \`Authorization: Bearer \` (recommended)
\\* \`x-openclaw-token: \`

Query-string tokens are rejected.

 Enqueue a system event for the main session:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -X POST http://127.0.0.1:18789/hooks/wake \
 -H 'Authorization: Bearer SECRET' \
 -H 'Content-Type: application/json' \
 -d '{"text":"New email received","mode":"now"}'
 \`\`\`

 Event description.

 \`now\` or \`next-heartbeat\`.

 Run an isolated agent turn:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -X POST http://127.0.0.1:18789/hooks/agent \
 -H 'Authorization: Bearer SECRET' \
 -H 'Content-Type: application/json' \
 -d '{"message":"Summarize inbox","name":"Email","model":"openai/gpt-5.4"}'
 \`\`\`

 Fields: \`message\` (required), \`name\`, \`agentId\`, \`wakeMode\`, \`deliver\`, \`channel\`, \`to\`, \`model\`, \`fallbacks\`, \`thinking\`, \`timeoutSeconds\`.

 Custom hook names are resolved via \`hooks.mappings\` in config. Mappings can transform arbitrary payloads into \`wake\` or \`agent\` actions with templates or code transforms.

 Keep hook endpoints behind loopback, tailnet, or trusted reverse proxy.

 \\* Use a dedicated hook token; do not reuse gateway auth tokens.
 \\* Keep \`hooks.path\` on a dedicated subpath; \`/\` is rejected.
 \\* Set \`hooks.allowedAgentIds\` to limit explicit \`agentId\` routing.
 \\* Keep \`hooks.allowRequestSessionKey=false\` unless you require caller-selected sessions.
 \\* If you enable \`hooks.allowRequestSessionKey\`, also set \`hooks.allowedSessionKeyPrefixes\` to constrain allowed session key shapes.
 \\* Hook payloads are wrapped with safety boundaries by default.

\## Gmail PubSub integration

Wire Gmail inbox triggers to OpenClaw via Google PubSub.

 \*\*Prerequisites:\*\* \`gcloud\` CLI, \`gog\` (gogcli), OpenClaw hooks enabled, Tailscale for the public HTTPS endpoint.

\### Wizard setup (recommended)

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw webhooks gmail setup --account openclaw@gmail.com
\`\`\`

This writes \`hooks.gmail\` config, enables the Gmail preset, and uses Tailscale Funnel for the push endpoint.

\### Gateway auto-start

When \`hooks.enabled=true\` and \`hooks.gmail.account\` is set, the Gateway starts \`gog gmail watch serve\` on boot and auto-renews the watch. Set \`OPENCLAW\_SKIP\_GMAIL\_WATCHER=1\` to opt out.

\### Manual one-time setup

 Select the GCP project that owns the OAuth client used by \`gog\`:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 gcloud auth login
 gcloud config set project
 gcloud services enable gmail.googleapis.com pubsub.googleapis.com
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 gcloud pubsub topics create gog-gmail-watch
 gcloud pubsub topics add-iam-policy-binding gog-gmail-watch \
 --member=serviceAccount:gmail-api-push@system.gserviceaccount.com \
 --role=roles/pubsub.publisher
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 gog gmail watch start \
 --account openclaw@gmail.com \
 --label INBOX \
 --topic projects//topics/gog-gmail-watch
 \`\`\`

\### Gmail model override

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 hooks: {
 gmail: {
 model: "openrouter/meta-llama/llama-3.3-70b-instruct:free",
 thinking: "off",
 },
 },
}
\`\`\`

\## Managing jobs

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
\# List all jobs
openclaw cron list

\# Show one job, including resolved delivery route
openclaw cron show

\# Edit a job
openclaw cron edit  --message "Updated prompt" --model "opus"

\# Force run a job now
openclaw cron run

\# Run only if due
openclaw cron run  --due

\# View run history
openclaw cron runs --id  --limit 50

\# Delete a job
openclaw cron remove

\# Agent selection (multi-agent setups)
openclaw cron add --name "Ops sweep" --cron "0 6 \* \* \*" --session isolated --message "Check ops queue" --agent ops
openclaw cron edit  --clear-agent
\`\`\`

 Model override note:

 \\* \`openclaw cron add\|edit --model ...\` changes the job's selected model.
 \\* If the model is allowed, that exact provider/model reaches the isolated agent run.
 \\* If it is not allowed or cannot be resolved, cron fails the run with an explicit validation error.
 \\* Configured fallback chains still apply because cron \`--model\` is a job primary, not a session \`/model\` override.
 \\* Payload \`fallbacks\` replaces configured fallbacks for that job; \`fallbacks: \[\]\` disables fallback and makes the run strict.
 \\* A plain \`--model\` with no explicit or configured fallback list does not fall through to the agent primary as a silent extra retry target.

\## Configuration

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cron: {
 enabled: true,
 store: "~/.openclaw/cron/jobs.json",
 maxConcurrentRuns: 1,
 retry: {
 maxAttempts: 3,
 backoffMs: \[60000, 120000, 300000\],
 retryOn: \["rate\_limit", "overloaded", "network", "server\_error"\],
 },
 webhookToken: "replace-with-dedicated-webhook-token",
 sessionRetention: "24h",
 runLog: { maxBytes: "2mb", keepLines: 2000 },
 },
}
\`\`\`

\`maxConcurrentRuns\` limits both scheduled cron dispatch and isolated agent-turn execution. Isolated cron agent turns use the queue's dedicated \`cron-nested\` execution lane internally, so raising this value lets independent cron LLM runs progress in parallel instead of only starting their outer cron wrappers. The shared non-cron \`nested\` lane is not widened by this setting.

The runtime state sidecar is derived from \`cron.store\`: a \`.json\` store such as \`~/clawd/cron/jobs.json\` uses \`~/clawd/cron/jobs-state.json\`, while a store path without a \`.json\` suffix appends \`-state.json\`.

If you hand-edit \`jobs.json\`, leave \`jobs-state.json\` out of source control. OpenClaw uses that sidecar for pending slots, active markers, last-run metadata, and the schedule identity that tells the scheduler when an externally edited job needs a fresh \`nextRunAtMs\`.

Disable cron: \`cron.enabled: false\` or \`OPENCLAW\_SKIP\_CRON=1\`.

 \*\*One-shot retry\*\*: transient errors (rate limit, overload, network, server error) retry up to 3 times with exponential backoff. Permanent errors disable immediately.

 \*\*Recurring retry\*\*: exponential backoff (30s to 60m) between retries. Backoff resets after the next successful run.

 \`cron.sessionRetention\` (default \`24h\`) prunes isolated run-session entries. \`cron.runLog.maxBytes\` / \`cron.runLog.keepLines\` auto-prune run-log files.

\## Troubleshooting

\### Command ladder

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw status
openclaw gateway status
openclaw cron status
openclaw cron list
openclaw cron runs --id  --limit 20
openclaw system heartbeat last
openclaw logs --follow
openclaw doctor
\`\`\`

 \\* Check \`cron.enabled\` and \`OPENCLAW\_SKIP\_CRON\` env var.
 \\* Confirm the Gateway is running continuously.
 \\* For \`cron\` schedules, verify timezone (\`--tz\`) vs the host timezone.
 \\* \`reason: not-due\` in run output means manual run was checked with \`openclaw cron run  --due\` and the job was not due yet.

 \\* Delivery mode \`none\` means no runner fallback send is expected. The agent can still send directly with the \`message\` tool when a chat route is available.
 \\* Delivery target missing/invalid (\`channel\`/\`to\`) means outbound was skipped.
 \\* For Matrix, copied or legacy jobs with lowercased \`delivery.to\` room IDs can fail because Matrix room IDs are case-sensitive. Edit the job to the exact \`!room:server\` or \`room:!room:server\` value from Matrix.
 \\* Channel auth errors (\`unauthorized\`, \`Forbidden\`) mean delivery was blocked by credentials.
 \\* If the isolated run returns only the silent token (\`NO\_REPLY\` / \`no\_reply\`), OpenClaw suppresses direct outbound delivery and also suppresses the fallback queued summary path, so nothing is posted back to chat.
 \\* If the agent should message the user itself, check that the job has a usable route (\`channel: "last"\` with a previous chat, or an explicit channel/target).

 \\* Daily and idle reset freshness is not based on \`updatedAt\`; see \[Session management\](/concepts/session#session-lifecycle).
 \\* Cron wakeups, heartbeat runs, exec notifications, and gateway bookkeeping may update the session row for routing/status, but they do not extend \`sessionStartedAt\` or \`lastInteractionAt\`.
 \\* For legacy rows created before those fields existed, OpenClaw can recover \`sessionStartedAt\` from the transcript JSONL session header when the file is still available. Legacy idle rows without \`lastInteractionAt\` use that recovered start time as their idle baseline.

 \\* Cron without \`--tz\` uses the gateway host timezone.
 \\* \`at\` schedules without timezone are treated as UTC.
 \\* Heartbeat \`activeHours\` uses configured timezone resolution.

\## Related

\\* \[Automation & Tasks\](/automation) — all automation mechanisms at a glance
\\* \[Background Tasks\](/automation/tasks) — task ledger for cron executions
\\* \[Heartbeat\](/gateway/heartbeat) — periodic main-session turns
\\* \[Timezone\](/concepts/timezone) — timezone configuration

---

## Hooks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/hooks>_

# List available hooks
openclaw hooks list

# Enable a hook
openclaw hooks enable session-memory

# Check hook status
openclaw hooks check

# Get detailed information
openclaw hooks info session-memory
```

## Event types

| Event | When it fires |
| --- | --- |
| `command:new` | `/new` command issued |
| `command:reset` | `/reset` command issued |
| `command:stop` | `/stop` command issued |
| `command` | Any command event (general listener) |
| `session:compact:before` | Before compaction summarizes history |
| `session:compact:after` | After compaction completes |
| `session:patch` | When session properties are modified |
| `agent:bootstrap` | Before workspace bootstrap files are injected |
| `gateway:startup` | After channels start and hooks are loaded |
| `gateway:shutdown` | When gateway shutdown begins |
| `gateway:pre-restart` | Before an expected gateway restart |
| `message:received` | Inbound message from any channel |
| `message:transcribed` | After audio transcription completes |
| `message:preprocessed` | After media and link preprocessing completes or is skipped |
| `message:sent` | Outbound message delivered |

## Writing hooks

### Hook structure

Each hook is a directory containing two files:

```
my-hook/
├── HOOK.md          # Metadata + documentation
└── handler.ts       # Handler implementation
```

### HOOK.md format

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

**Metadata fields** (`metadata.openclaw`):

| Field | Description |
| --- | --- |
| `emoji` | Display emoji for CLI |
| `events` | Array of events to listen for |
| `export` | Named export to use (defaults to `"default"`) |
| `os` | Required platforms (e.g., `["darwin", "linux"]`) |
| `requires` | Required `bins`, `anyBins`, `env`, or `config` paths |
| `always` | Bypass eligibility checks (boolean) |
| `install` | Installation methods |

### Handler implementation

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

Each event includes: `type`, `action`, `sessionKey`, `timestamp`, `messages` (push to send to user), and `context` (event-specific data). Agent and tool plugin hook contexts can also include `trace`, a read-only W3C-compatible diagnostic trace context that plugins may pass into structured logs for OTEL correlation.

### Event context highlights

**Command events** (`command:new`, `command:reset`): `context.sessionEntry`, `context.previousSessionEntry`, `context.commandSource`, `context.workspaceDir`, `context.cfg`.**Message events** (`message:received`): `context.from`, `context.content`, `context.channelId`, `context.metadata` (provider-specific data including `senderId`, `senderName`, `guildId`).**Message events** (`message:sent`): `context.to`, `context.content`, `context.success`, `context.channelId`.**Message events** (`message:transcribed`): `context.transcript`, `context.from`, `context.channelId`, `context.mediaPath`.**Message events** (`message:preprocessed`): `context.bodyForAgent` (final enriched body), `context.from`, `context.channelId`.**Bootstrap events** (`agent:bootstrap`): `context.bootstrapFiles` (mutable array), `context.agentId`.**Session patch events** (`session:patch`): `context.sessionEntry`, `context.patch` (only changed fields), `context.cfg`. Only privileged clients can trigger patch events.**Compaction events**: `session:compact:before` includes `messageCount`, `tokenCount`. `session:compact:after` adds `compactedCount`, `summaryLength`, `tokensBefore`, `tokensAfter`.`command:stop` observes the user issuing `/stop`; it is cancellation/command
lifecycle, not an agent-finalization gate. Plugins that need to inspect a
natural final answer and ask the agent for one more pass should use the typed
plugin hook `before_agent_finalize` instead. See [Plugin hooks](https://docs.openclaw.ai/plugins/hooks).**Gateway lifecycle events**: `gateway:shutdown` includes `reason` and `restartExpectedMs` and fires when gateway shutdown begins. `gateway:pre-restart` includes the same context but only fires when shutdown is part of an expected restart and a finite `restartExpectedMs` value is supplied. During shutdown, each lifecycle hook wait is best-effort and bounded so shutdown continues if a handler stalls.

## Hook discovery

Hooks are discovered from these directories, in order of increasing override precedence:

1. **Bundled hooks**: shipped with OpenClaw
2. **Plugin hooks**: hooks bundled inside installed plugins
3. **Managed hooks**: `~/.openclaw/hooks/` (user-installed, shared across workspaces). Extra directories from `hooks.internal.load.extraDirs` share this precedence.
4. **Workspace hooks**: `<workspace>/hooks/` (per-agent, disabled by default until explicitly enabled)

Workspace hooks can add new hook names but cannot override bundled, managed, or plugin-provided hooks with the same name.The Gateway skips internal hook discovery on startup until internal hooks are configured. Enable a bundled or managed hook with `openclaw hooks enable <name>`, install a hook pack, or set `hooks.internal.enabled=true` to opt in. When you enable one named hook, the Gateway loads only that hook’s handler; `hooks.internal.enabled=true`, extra hook directories, and legacy handlers opt into broad discovery.

### Hook packs

Hook packs are npm packages that export hooks via `openclaw.hooks` in `package.json`. Install with:

```
openclaw plugins install <path-or-spec>
```

Npm specs are registry-only (package name + optional exact version or dist-tag). Git/URL/file specs and semver ranges are rejected.

## Bundled hooks

| Hook | Events | What it does |
| --- | --- | --- |
| session-memory | `command:new`, `command:reset` | Saves session context to `<workspace>/memory/` |
| bootstrap-extra-files | `agent:bootstrap` | Injects additional bootstrap files from glob patterns |
| command-logger | `command` | Logs all commands to `~/.openclaw/logs/commands.log` |
| boot-md | `gateway:startup` | Runs `BOOT.md` when the gateway starts |

Enable any bundled hook:

```
openclaw hooks enable <hook-name>
```

### session-memory details

Extracts the last 15 user/assistant messages, generates a descriptive filename slug via LLM, and saves to `<workspace>/memory/YYYY-MM-DD-slug.md` using the host local date. Requires `workspace.dir` to be configured.

### bootstrap-extra-files config

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

Paths resolve relative to workspace. Only recognized bootstrap basenames are loaded (`AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md`, `MEMORY.md`).

### command-logger details

Logs every slash command to `~/.openclaw/logs/commands.log`.

### boot-md details

Runs `BOOT.md` from the active workspace when the gateway starts.

## Plugin hooks

Plugins can register typed hooks through the Plugin SDK for deeper integration:
intercepting tool calls, modifying prompts, controlling message flow, and more.
Use plugin hooks when you need `before_tool_call`, `before_agent_reply`,
`before_install`, or other in-process lifecycle hooks.For the complete plugin hook reference, see [Plugin hooks](https://docs.openclaw.ai/plugins/hooks).

## Configuration

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

Per-hook environment variables:

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

Extra hook directories:

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

The legacy `hooks.internal.handlers` array config format is still supported for backwards compatibility, but new hooks should use the discovery-based system.

## CLI reference

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

## Best practices

- **Keep handlers fast.** Hooks run during command processing. Fire-and-forget heavy work with `void processInBackground(event)`.
- **Handle errors gracefully.** Wrap risky operations in try/catch; do not throw so other handlers can run.
- **Filter events early.** Return immediately if the event type/action is not relevant.
- **Use specific event keys.** Prefer `"events": ["command:new"]` over `"events": ["command"]` to reduce overhead.

## Troubleshooting

### Hook not discovered

```
# Verify directory structure
ls -la ~/.openclaw/hooks/my-hook/
# Should show: HOOK.md, handler.ts

# List all discovered hooks
openclaw hooks list
```

### Hook not eligible

```
openclaw hooks info my-hook
```

Check for missing binaries (PATH), environment variables, config values, or OS compatibility.

### Hook not executing

1. Verify the hook is enabled: `openclaw hooks list`
2. Restart your gateway process so hooks reload.
3. Check gateway logs: `./scripts/clawlog.sh | grep hook`

## Related

- [CLI Reference: hooks](https://docs.openclaw.ai/cli/hooks)
- [Webhooks](https://docs.openclaw.ai/automation/cron-jobs#webhooks)
- [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) — in-process plugin lifecycle hooks
- [Configuration](https://docs.openclaw.ai/gateway/configuration-reference#hooks)

[Standing orders](https://docs.openclaw.ai/automation/standing-orders) [apply\_patch tool](https://docs.openclaw.ai/tools/apply-patch)

Ctrl+I

---

## Automation & tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/index>_

[OpenClaw home page](https://docs.openclaw.ai/)

Automation and tasks

Automation & tasks

OpenClaw runs work in the background through tasks, scheduled jobs, inferred
commitments, event hooks, and standing instructions. This page helps you choose
the right mechanism and understand how they fit together.

## Quick decision guide

Yes

Exact

Flexible

Yes

Yes

Yes

Yes

Yes

What do you need?

Schedule work?

Track detached work?

Orchestrate multi-step flows?

React to lifecycle events?

Give the agent persistent instructions?

Remember a natural follow-up?

Exact timing or flexible?

Scheduled Tasks (Cron)

Heartbeat

Background Tasks

Task Flow

Hooks

Standing Orders

Inferred Commitments

| Use case | Recommended | Why |
| --- | --- | --- |
| Send daily report at 9 AM sharp | Scheduled Tasks (Cron) | Exact timing, isolated execution |
| Remind me in 20 minutes | Scheduled Tasks (Cron) | One-shot with precise timing (`--at`) |
| Run weekly deep analysis | Scheduled Tasks (Cron) | Standalone task, can use different model |
| Check inbox every 30 min | Heartbeat | Batches with other checks, context-aware |
| Monitor calendar for upcoming events | Heartbeat | Natural fit for periodic awareness |
| Check in after a mentioned interview | Inferred Commitments | Memory-like follow-up, no exact reminder request |
| Gentle care check-in after user context | Inferred Commitments | Scoped to the same agent and channel |
| Inspect status of a subagent or ACP run | Background Tasks | Tasks ledger tracks all detached work |
| Audit what ran and when | Background Tasks | `openclaw tasks list` and `openclaw tasks audit` |
| Multi-step research then summarize | Task Flow | Durable orchestration with revision tracking |
| Run a script on session reset | Hooks | Event-driven, fires on lifecycle events |
| Execute code on every tool call | Plugin hooks | In-process hooks can intercept tool calls |
| Always check compliance before replying | Standing Orders | Injected into every session automatically |

### Scheduled Tasks (Cron) vs Heartbeat

| Dimension | Scheduled Tasks (Cron) | Heartbeat |
| --- | --- | --- |
| Timing | Exact (cron expressions, one-shot) | Approximate (default every 30 min) |
| Session context | Fresh (isolated) or shared | Full main-session context |
| Task records | Always created | Never created |
| Delivery | Channel, webhook, or silent | Inline in main session |
| Best for | Reports, reminders, background jobs | Inbox checks, calendar, notifications |

Use Scheduled Tasks (Cron) when you need precise timing or isolated execution. Use Heartbeat when the work benefits from full session context and approximate timing is fine.

## Core concepts

### Scheduled tasks (cron)

Cron is the Gateway’s built-in scheduler for precise timing. It persists jobs, wakes the agent at the right time, and can deliver output to a chat channel or webhook endpoint. Supports one-shot reminders, recurring expressions, and inbound webhook triggers.See [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs).

### Tasks

The background task ledger tracks all detached work: ACP runs, subagent spawns, isolated cron executions, and CLI operations. Tasks are records, not schedulers. Use `openclaw tasks list` and `openclaw tasks audit` to inspect them.See [Background Tasks](https://docs.openclaw.ai/automation/tasks).

### Inferred commitments

Commitments are opt-in, short-lived follow-up memories. OpenClaw infers them
from normal conversations, scopes them to the same agent and channel, and
delivers due check-ins through heartbeat. Exact user-requested reminders still
belong to cron.See [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments).

### Task Flow

Task Flow is the flow orchestration substrate above background tasks. It manages durable multi-step flows with managed and mirrored sync modes, revision tracking, and `openclaw tasks flow list|show|cancel` for inspection.See [Task Flow](https://docs.openclaw.ai/automation/taskflow).

### Standing orders

Standing orders grant the agent permanent operating authority for defined programs. They live in workspace files (typically `AGENTS.md`) and are injected into every session. Combine with cron for time-based enforcement.See [Standing Orders](https://docs.openclaw.ai/automation/standing-orders).

### Hooks

Internal hooks are event-driven scripts triggered by agent lifecycle events
(`/new`, `/reset`, `/stop`), session compaction, gateway startup, and message
flow. They are automatically discovered from directories and can be managed
with `openclaw hooks`. For in-process tool-call interception, use
[Plugin hooks](https://docs.openclaw.ai/plugins/hooks).See [Hooks](https://docs.openclaw.ai/automation/hooks).

### Heartbeat

Heartbeat is a periodic main-session turn (default every 30 minutes). It batches multiple checks (inbox, calendar, notifications) in one agent turn with full session context. Heartbeat turns do not create task records and do not extend daily/idle session reset freshness. Use `HEARTBEAT.md` for a small checklist, or a `tasks:` block when you want due-only periodic checks inside heartbeat itself. Empty heartbeat files skip as `empty-heartbeat-file`; due-only task mode skips as `no-tasks-due`. Heartbeats defer while cron work is active or queued, and `heartbeat.skipWhenBusy` can also defer them while subagent or nested lanes are busy.See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

## How they work together

- **Cron** handles precise schedules (daily reports, weekly reviews) and one-shot reminders. All cron executions create task records.
- **Heartbeat** handles routine monitoring (inbox, calendar, notifications) in one batched turn every 30 minutes.
- **Hooks** react to specific events (session resets, compaction, message flow) with custom scripts. Plugin hooks cover tool calls.
- **Standing orders** give the agent persistent context and authority boundaries.
- **Task Flow** coordinates multi-step flows above individual tasks.
- **Tasks** automatically track all detached work so you can inspect and audit it.

## Related

- [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) — precise scheduling and one-shot reminders
- [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments) — memory-like follow-up check-ins
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — task ledger for all detached work
- [Task Flow](https://docs.openclaw.ai/automation/taskflow) — durable multi-step flow orchestration
- [Hooks](https://docs.openclaw.ai/automation/hooks) — event-driven lifecycle scripts
- [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) — in-process tool, prompt, message, and lifecycle hooks
- [Standing Orders](https://docs.openclaw.ai/automation/standing-orders) — persistent agent instructions
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference) — all config keys

[OpenProse](https://docs.openclaw.ai/prose) [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

Ctrl+I

---

## Background tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/tasks>_

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

## What creates a task

| Source | Runtime type | When a task record is created | Default notify policy |
| --- | --- | --- | --- |
| ACP background runs | `acp` | Spawning a child ACP session | `done_only` |
| Subagent orchestration | `subagent` | Spawning a subagent via `sessions_spawn` | `done_only` |
| Cron jobs (all types) | `cron` | Every cron execution (main-session and isolated) | `silent` |
| CLI operations | `cli` | `openclaw agent` commands that run through the gateway | `silent` |
| Agent media jobs | `cli` | Session-backed `music_generate`/`video_generate` runs | `silent` |

Notify defaults for cron and media

Main-session cron tasks use `silent` notify policy by default — they create records for tracking but do not generate notifications. Isolated cron tasks also default to `silent` but are more visible because they run in their own session.Session-backed `music_generate` and `video_generate` runs also use `silent` notify policy. They still create task records, but completion is handed back to the original agent session as an internal wake so the agent can write the follow-up message and attach the finished media itself. If you opt into `tools.media.asyncCompletion.directSend`, async `video_generate` completions can try direct channel delivery first; async `music_generate` completions stay on the requester-session wake path.

Concurrent video\_generate guardrail

While a session-backed `video_generate` task is still active, the tool also acts as a guardrail: repeated `video_generate` calls in that same session return the active task status instead of starting a second concurrent generation. Use `action: "status"` when you want an explicit progress/status lookup from the agent side.

What does not create tasks

- Heartbeat turns — main-session; see [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- Normal interactive chat turns
- Direct `/command` responses

## Task lifecycle

agent starts

completes ok

error

timeout exceeded

operator cancels

session gone > 5 min

session gone > 5 min

queued

running

succeeded

failed

timed\_out

cancelled

lost

| Status | What it means |
| --- | --- |
| `queued` | Created, waiting for the agent to start |
| `running` | Agent turn is actively executing |
| `succeeded` | Completed successfully |
| `failed` | Completed with an error |
| `timed_out` | Exceeded the configured timeout |
| `cancelled` | Stopped by the operator via `openclaw tasks cancel` |
| `lost` | The runtime lost authoritative backing state after a 5-minute grace period |

Transitions happen automatically — when the associated agent run ends, the task status updates to match.Agent run completion is authoritative for active task records. A successful detached run finalizes as `succeeded`, ordinary run errors finalize as `failed`, and timeout or abort outcomes finalize as `timed_out`. If an operator already cancelled the task, or the runtime already recorded a stronger terminal state such as `failed`, `timed_out`, or `lost`, a later success signal does not downgrade that terminal status.`lost` is runtime-aware:

- ACP tasks: backing ACP child session metadata disappeared.
- Subagent tasks: backing child session disappeared from the target agent store.
- Cron tasks: the cron runtime no longer tracks the job as active and durable
cron run history does not show a terminal result for that run. Offline CLI
audit does not treat its own empty in-process cron runtime state as authority.
- CLI tasks: isolated child-session tasks use the child session; chat-backed
CLI tasks use the live run context instead, so lingering
channel/group/direct session rows do not keep them alive. Gateway-backed
`openclaw agent` runs also finalize from their run result, so completed runs
do not sit active until the sweeper marks them `lost`.

## Delivery and notifications

When a task reaches a terminal state, OpenClaw notifies you. There are two delivery paths:**Direct delivery** — if the task has a channel target (the `requesterOrigin`), the completion message goes straight to that channel (Telegram, Discord, Slack, etc.). For subagent completions, OpenClaw also preserves bound thread/topic routing when available and can fill a missing `to` / account from the requester session’s stored route (`lastChannel` / `lastTo` / `lastAccountId`) before giving up on direct delivery.**Session-queued delivery** — if direct delivery fails or no origin is set, the update is queued as a system event in the requester’s session and surfaces on the next heartbeat.

Task completion triggers an immediate heartbeat wake so you see the result quickly — you do not have to wait for the next scheduled heartbeat tick.

That means the usual workflow is push-based: start detached work once, then let the runtime wake or notify you on completion. Poll task state only when you need debugging, intervention, or an explicit audit.

### Notification policies

Control how much you hear about each task:

| Policy | What is delivered |
| --- | --- |
| `done_only` (default) | Only terminal state (succeeded, failed, etc.) — **this is the default** |
| `state_changes` | Every state transition and progress update |
| `silent` | Nothing at all |

Change the policy while a task is running:

```
openclaw tasks notify <lookup> state_changes
```

## CLI reference

tasks list

```
openclaw tasks list [--runtime <acp|subagent|cron|cli>] [--status <status>] [--json]
```

Output columns: Task ID, Kind, Status, Delivery, Run ID, Child Session, Summary.

tasks show

```
openclaw tasks show <lookup>
```

The lookup token accepts a task ID, run ID, or session key. Shows the full record including timing, delivery state, error, and terminal summary.

tasks cancel

```
openclaw tasks cancel <lookup>
```

For ACP and subagent tasks, this kills the child session. For CLI-tracked tasks, cancellation is recorded in the task registry (there is no separate child runtime handle). Status transitions to `cancelled` and a delivery notification is sent when applicable.

tasks notify

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

tasks audit

```
openclaw tasks audit [--json]
```

Surfaces operational issues. Findings also appear in `openclaw status` when issues are detected.

| Finding | Severity | Trigger |
| --- | --- | --- |
| `stale_queued` | warn | Queued for more than 10 minutes |
| `stale_running` | error | Running for more than 30 minutes |
| `lost` | warn/error | Runtime-backed task ownership disappeared; retained lost tasks warn until `cleanupAfter`, then become errors |
| `delivery_failed` | warn | Delivery failed and notify policy is not `silent` |
| `missing_cleanup` | warn | Terminal task with no cleanup timestamp |
| `inconsistent_timestamps` | warn | Timeline violation (for example ended before started) |

tasks maintenance

```
openclaw tasks maintenance [--json]
openclaw tasks maintenance --apply [--json]
```

Use this to preview or apply reconciliation, cleanup stamping, and pruning for tasks and Task Flow state.Reconciliation is runtime-aware:

- ACP/subagent tasks check their backing child session.
- Subagent tasks whose child session has a restart-recovery tombstone are marked lost instead of being treated as recoverable backing sessions.
- Cron tasks check whether the cron runtime still owns the job, then recover terminal status from persisted cron run logs/job state before falling back to `lost`. Only the Gateway process is authoritative for the in-memory cron active-job set; offline CLI audit uses durable history but does not mark a cron task lost solely because that local Set is empty.
- Chat-backed CLI tasks check the owning live run context, not just the chat session row.

Completion cleanup is also runtime-aware:

- Subagent completion best-effort closes tracked browser tabs/processes for the child session before announce cleanup continues.
- Isolated cron completion best-effort closes tracked browser tabs/processes for the cron session before the run fully tears down.
- Isolated cron delivery waits out descendant subagent follow-up when needed and suppresses stale parent acknowledgement text instead of announcing it.
- Subagent completion delivery prefers the latest visible assistant text; if that is empty it falls back to sanitized latest tool/toolResult text, and timeout-only tool-call runs can collapse to a short partial-progress summary. Terminal failed runs announce failure status without replaying captured reply text.
- Cleanup failures do not mask the real task outcome.

tasks flow list \| show \| cancel

```
openclaw tasks flow list [--status <status>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

Use these when the orchestrating Task Flow is the thing you care about rather than one individual background task record.

## Chat task board (`/tasks`)

Use `/tasks` in any chat session to see background tasks linked to that session. The board shows active and recently completed tasks with runtime, status, timing, and progress or error detail.When the current session has no visible linked tasks, `/tasks` falls back to agent-local task counts so you still get an overview without leaking other-session details.For the full operator ledger, use the CLI: `openclaw tasks list`.

## Status integration (task pressure)

`openclaw status` includes an at-a-glance task summary:

```
Tasks: 3 queued · 2 running · 1 issues
```

The summary reports:

- **active** — count of `queued` \+ `running`
- **failures** — count of `failed` \+ `timed_out` \+ `lost`
- **byRuntime** — breakdown by `acp`, `subagent`, `cron`, `cli`

Both `/status` and the `session_status` tool use a cleanup-aware task snapshot: active tasks are preferred, stale completed rows are hidden, and recent failures only surface when no active work remains. This keeps the status card focused on what matters right now.

## Storage and maintenance

### Where tasks live

Task records persist in SQLite at:

```
$OPENCLAW_STATE_DIR/tasks/runs.sqlite
```

The registry loads into memory at gateway start and syncs writes to SQLite for durability across restarts.
The Gateway keeps the SQLite write-ahead log bounded by using SQLite’s default
autocheckpoint threshold plus periodic and shutdown `TRUNCATE` checkpoints.

### Automatic maintenance

A sweeper runs every **60 seconds** and handles four things:

1

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Reconciliation

Checks whether active tasks still have authoritative runtime backing. ACP/subagent tasks use child-session state, cron tasks use active-job ownership, and chat-backed CLI tasks use the owning run context. If that backing state is gone for more than 5 minutes, the task is marked `lost`.

2

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

ACP session repair

Closes terminal or orphaned parent-owned one-shot ACP sessions, and closes stale terminal or orphaned persistent ACP sessions only when no active conversation binding remains.

3

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Cleanup stamping

Sets a `cleanupAfter` timestamp on terminal tasks (endedAt + 7 days). During retention, lost tasks still appear in audit as warnings; after `cleanupAfter` expires or when cleanup metadata is missing, they are errors.

4

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Pruning

Deletes records past their `cleanupAfter` date.

**Retention:** terminal task records are kept for **7 days**, then automatically pruned. No configuration needed.

## How tasks relate to other systems

Tasks and Task Flow

[Task Flow](https://docs.openclaw.ai/automation/taskflow) is the flow orchestration layer above background tasks. A single flow may coordinate multiple tasks over its lifetime using managed or mirrored sync modes. Use `openclaw tasks` to inspect individual task records and `openclaw tasks flow` to inspect the orchestrating flow.See [Task Flow](https://docs.openclaw.ai/automation/taskflow) for details.

Tasks and cron

A cron job **definition** lives in `~/.openclaw/cron/jobs.json`; runtime execution state lives beside it in `~/.openclaw/cron/jobs-state.json`. **Every** cron execution creates a task record — both main-session and isolated. Main-session cron tasks default to `silent` notify policy so they track without generating notifications.See [Cron Jobs](https://docs.openclaw.ai/automation/cron-jobs).

Tasks and heartbeat

Heartbeat runs are main-session turns — they do not create task records. When a task completes, it can trigger a heartbeat wake so you see the result promptly.See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

Tasks and sessions

A task may reference a `childSessionKey` (where work runs) and a `requesterSessionKey` (who started it). Sessions are conversation context; tasks are activity tracking on top of that.

Tasks and agent runs

A task’s `runId` links to the agent run doing the work. Agent lifecycle events (start, end, error) automatically update the task status — you do not need to manage the lifecycle manually.

## Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — all automation mechanisms at a glance
- [CLI: Tasks](https://docs.openclaw.ai/cli/tasks) — CLI command reference
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) — scheduling background work
- [Task Flow](https://docs.openclaw.ai/automation/taskflow) — flow orchestration above tasks

[Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) [Task flow](https://docs.openclaw.ai/automation/taskflow)

Ctrl+I

---

## Ordens permanentes - OpenClaw

_Source: <https://docs.openclaw.ai/pt-BR/automation/standing-orders>_

[Pular para o conteúdo principal](https://docs.openclaw.ai/pt-BR/automation/standing-orders#content-area)

[OpenClaw home page](https://docs.openclaw.ai/pt-BR)

Português (BR)

Pesquisar...

Pesquisar...

Automation and tasks

Ordens permanentes

[Get started](https://docs.openclaw.ai/pt-BR) [Install](https://docs.openclaw.ai/pt-BR/install) [Channels](https://docs.openclaw.ai/pt-BR/channels) [Agents](https://docs.openclaw.ai/pt-BR/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pt-BR/tools) [Models](https://docs.openclaw.ai/pt-BR/providers) [Platforms](https://docs.openclaw.ai/pt-BR/platforms) [Gateway & Ops](https://docs.openclaw.ai/pt-BR/gateway) [Reference](https://docs.openclaw.ai/pt-BR/cli) [Help](https://docs.openclaw.ai/pt-BR/help)

Na página

- [Por que usar ordens permanentes](https://docs.openclaw.ai/pt-BR/automation/standing-orders#por-que-usar-ordens-permanentes)
- [Como elas funcionam](https://docs.openclaw.ai/pt-BR/automation/standing-orders#como-elas-funcionam)
- [Anatomia de uma ordem permanente](https://docs.openclaw.ai/pt-BR/automation/standing-orders#anatomia-de-uma-ordem-permanente)
- [Ordens permanentes mais tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/standing-orders#ordens-permanentes-mais-tarefas-cron)
- [Exemplos](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplos)
- [Exemplo 1: conteúdo e redes sociais (ciclo semanal)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-1-conte%C3%BAdo-e-redes-sociais-ciclo-semanal)
- [Exemplo 2: operações financeiras (acionadas por evento)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-2-opera%C3%A7%C3%B5es-financeiras-acionadas-por-evento)
- [Exemplo 3: monitoramento e alertas (contínuo)](https://docs.openclaw.ai/pt-BR/automation/standing-orders#exemplo-3-monitoramento-e-alertas-cont%C3%ADnuo)
- [Padrão executar-verificar-relatar](https://docs.openclaw.ai/pt-BR/automation/standing-orders#padr%C3%A3o-executar-verificar-relatar)
- [Arquitetura multiprograma](https://docs.openclaw.ai/pt-BR/automation/standing-orders#arquitetura-multiprograma)
- [Melhores práticas](https://docs.openclaw.ai/pt-BR/automation/standing-orders#melhores-pr%C3%A1ticas)
- [Faça](https://docs.openclaw.ai/pt-BR/automation/standing-orders#fa%C3%A7a)
- [Evite](https://docs.openclaw.ai/pt-BR/automation/standing-orders#evite)
- [Relacionados](https://docs.openclaw.ai/pt-BR/automation/standing-orders#relacionados)

Ordens permanentes concedem ao seu agente **autoridade operacional permanente** para programas definidos. Em vez de fornecer instruções de tarefa individuais a cada vez, você define programas com escopo, acionadores e regras de escalonamento claros — e o agente executa autonomamente dentro desses limites.Esta é a diferença entre dizer ao seu assistente “envie o relatório semanal” toda sexta-feira e conceder autoridade permanente: “Você é responsável pelo relatório semanal. Compile-o toda sexta-feira, envie-o e só escale se algo parecer errado.”

## Por que usar ordens permanentes

**Sem ordens permanentes:**

- Você precisa solicitar ao agente cada tarefa
- O agente fica ocioso entre solicitações
- Trabalho rotineiro é esquecido ou atrasado
- Você se torna o gargalo

**Com ordens permanentes:**

- O agente executa autonomamente dentro de limites definidos
- Trabalho rotineiro acontece no cronograma sem solicitação
- Você só se envolve em exceções e aprovações
- O agente preenche o tempo ocioso de forma produtiva

## Como elas funcionam

Ordens permanentes são definidas nos arquivos do seu [espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace). A abordagem recomendada é incluí-las diretamente em `AGENTS.md` (que é injetado automaticamente em todas as sessões), para que o agente sempre as tenha em contexto. Para configurações maiores, você também pode colocá-las em um arquivo dedicado, como `standing-orders.md`, e referenciá-lo a partir de `AGENTS.md`.Cada programa especifica:

1. **Escopo** — o que o agente está autorizado a fazer
2. **Acionadores** — quando executar (cronograma, evento ou condição)
3. **Portões de aprovação** — o que exige aprovação humana antes da ação
4. **Regras de escalonamento** — quando parar e pedir ajuda

O agente carrega essas instruções em todas as sessões por meio dos arquivos de inicialização do espaço de trabalho (consulte [Espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace) para ver a lista completa de arquivos injetados automaticamente) e as executa, combinadas com [tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs) para imposição baseada em tempo.

Coloque ordens permanentes em `AGENTS.md` para garantir que elas sejam carregadas em todas as sessões. A inicialização do espaço de trabalho injeta automaticamente `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md` e `MEMORY.md` — mas não arquivos arbitrários em subdiretórios.

## Anatomia de uma ordem permanente

```
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

## Ordens permanentes mais tarefas Cron

Ordens permanentes definem **o que** o agente está autorizado a fazer. [Tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs) definem **quando** isso acontece. Elas funcionam juntas:

```
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

O prompt da tarefa Cron deve referenciar a ordem permanente em vez de duplicá-la:

```
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

## Exemplos

### Exemplo 1: conteúdo e redes sociais (ciclo semanal)

```
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

### Exemplo 2: operações financeiras (acionadas por evento)

```
## Program: Financial Processing

**Authority:** Process transaction data, generate reports, send summaries
**Approval gate:** None for analysis. Recommendations require owner approval.
**Trigger:** New data file detected OR scheduled monthly cycle

### When new data arrives

1. Detect new file in designated input directory
2. Parse and categorize all transactions
3. Compare against budget targets
4. Flag: unusual items, threshold breaches, new recurring charges
5. Generate report in designated output directory
6. Deliver summary to owner via configured channel

### Escalation rules

- Single item > $500: immediate alert
- Category > budget by 20%: flag in report
- Unrecognizable transaction: ask owner for categorization
- Failed processing after 2 retries: report failure, do not guess
```

### Exemplo 3: monitoramento e alertas (contínuo)

```
## Program: System Monitoring

**Authority:** Check system health, restart services, send alerts
**Approval gate:** Restart services automatically. Escalate if restart fails twice.
**Trigger:** Every heartbeat cycle

### Checks

- Service health endpoints responding
- Disk space above threshold
- Pending tasks not stale (>24 hours)
- Delivery channels operational

### Response matrix

| Condition        | Action                   | Escalate?                |
| ---------------- | ------------------------ | ------------------------ |
| Service down     | Restart automatically    | Only if restart fails 2x |
| Disk space < 10% | Alert owner              | Yes                      |
| Stale task > 24h | Remind owner             | No                       |
| Channel offline  | Log and retry next cycle | If offline > 2 hours     |
```

## Padrão executar-verificar-relatar

Ordens permanentes funcionam melhor quando combinadas com uma disciplina rigorosa de execução. Cada tarefa em uma ordem permanente deve seguir este ciclo:

1. **Executar** — Faça o trabalho real (não apenas reconheça a instrução)
2. **Verificar** — Confirme que o resultado está correto (o arquivo existe, a mensagem foi entregue, os dados foram analisados)
3. **Relatar** — Informe ao proprietário o que foi feito e o que foi verificado

```
### Execution rules

- Every task follows Execute-Verify-Report. No exceptions.
- "I'll do that" is not execution. Do it, then report.
- "Done" without verification is not acceptable. Prove it.
- If execution fails: retry once with adjusted approach.
- If still fails: report failure with diagnosis. Never silently fail.
- Never retry indefinitely — 3 attempts max, then escalate.
```

Esse padrão evita o modo de falha mais comum do agente: reconhecer uma tarefa sem concluí-la.

## Arquitetura multiprograma

Para agentes que gerenciam várias áreas, organize ordens permanentes como programas separados com limites claros:

```
## Program 1: [Domain A] (Weekly)

...

## Program 2: [Domain B] (Monthly + On-Demand)

...

## Program 3: [Domain C] (As-Needed)

...

## Escalation Rules (All Programs)

- [Common escalation criteria]
- [Approval gates that apply across programs]
```

Cada programa deve ter:

- Sua própria **cadência de acionamento** (semanal, mensal, orientada por eventos, contínua)
- Seus próprios **portões de aprovação** (alguns programas precisam de mais supervisão do que outros)
- **Limites** claros (o agente deve saber onde um programa termina e outro começa)

## Melhores práticas

### Faça

- Comece com autoridade restrita e expanda conforme a confiança aumenta
- Defina portões de aprovação explícitos para ações de alto risco
- Inclua seções “O que NÃO fazer” — limites importam tanto quanto permissões
- Combine com tarefas Cron para execução confiável baseada em tempo
- Revise os logs do agente semanalmente para verificar se as ordens permanentes estão sendo seguidas
- Atualize as ordens permanentes conforme suas necessidades evoluem — elas são documentos vivos

### Evite

- Conceder autoridade ampla no primeiro dia (“faça o que você achar melhor”)
- Ignorar regras de escalonamento — todo programa precisa de uma cláusula de “quando parar e perguntar”
- Presumir que o agente lembrará instruções verbais — coloque tudo no arquivo
- Misturar assuntos em um único programa — programas separados para domínios separados
- Esquecer de impor com tarefas Cron — ordens permanentes sem acionadores viram sugestões

## Relacionados

- [Automação e tarefas](https://docs.openclaw.ai/pt-BR/automation): todos os mecanismos de automação em resumo.
- [Tarefas Cron](https://docs.openclaw.ai/pt-BR/automation/cron-jobs): imposição de cronograma para ordens permanentes.
- [Hooks](https://docs.openclaw.ai/pt-BR/automation/hooks): scripts orientados por eventos para eventos do ciclo de vida do agente.
- [Webhooks](https://docs.openclaw.ai/pt-BR/automation/cron-jobs#webhooks): acionadores de eventos HTTP de entrada.
- [Espaço de trabalho do agente](https://docs.openclaw.ai/pt-BR/concepts/agent-workspace): onde ficam as ordens permanentes, incluindo a lista completa de arquivos de inicialização injetados automaticamente (`AGENTS.md`, `SOUL.md` etc.).

[Fluxo de tarefas](https://docs.openclaw.ai/pt-BR/automation/taskflow) [Ganchos](https://docs.openclaw.ai/pt-BR/automation/hooks)

Ctrl+I

---

## Постійні вказівки - OpenClaw

_Source: <https://docs.openclaw.ai/uk/automation/standing-orders>_

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation/standing-orders#content-area)

[OpenClaw home page](https://docs.openclaw.ai/uk)

Українська

Пошук...

Пошук...

Automation and tasks

Постійні вказівки

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Навіщо потрібні постійні вказівки](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BD%D0%B0%D0%B2%D1%96%D1%89%D0%BE-%D0%BF%D0%BE%D1%82%D1%80%D1%96%D0%B1%D0%BD%D1%96-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)
- [Як це працює](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%8F%D0%BA-%D1%86%D0%B5-%D0%BF%D1%80%D0%B0%D1%86%D1%8E%D1%94)
- [Анатомія постійної вказівки](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%B0%D0%BD%D0%B0%D1%82%D0%BE%D0%BC%D1%96%D1%8F-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D0%BE%D1%97-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)
- [Постійні вказівки та Cron jobs](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8-%D1%82%D0%B0-cron-jobs)
- [Приклади](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4%D0%B8)
- [Приклад 1: контент і соціальні мережі (щотижневий цикл)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-1-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BD%D1%82-%D1%96-%D1%81%D0%BE%D1%86%D1%96%D0%B0%D0%BB%D1%8C%D0%BD%D1%96-%D0%BC%D0%B5%D1%80%D0%B5%D0%B6%D1%96-%D1%89%D0%BE%D1%82%D0%B8%D0%B6%D0%BD%D0%B5%D0%B2%D0%B8%D0%B9-%D1%86%D0%B8%D0%BA%D0%BB)
- [Приклад 2: фінансові операції (запуск за подією)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-2-%D1%84%D1%96%D0%BD%D0%B0%D0%BD%D1%81%D0%BE%D0%B2%D1%96-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%86%D1%96%D1%97-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%B7%D0%B0-%D0%BF%D0%BE%D0%B4%D1%96%D1%94%D1%8E)
- [Приклад 3: моніторинг і сповіщення (безперервно)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-3-%D0%BC%D0%BE%D0%BD%D1%96%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3-%D1%96-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D0%BD%D1%8F-%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D1%80%D0%B5%D1%80%D0%B2%D0%BD%D0%BE)
- [Шаблон виконати-перевірити-звітувати](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD-%D0%B2%D0%B8%D0%BA%D0%BE%D0%BD%D0%B0%D1%82%D0%B8-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%B8%D1%82%D0%B8-%D0%B7%D0%B2%D1%96%D1%82%D1%83%D0%B2%D0%B0%D1%82%D0%B8)
- [Архітектура з кількома програмами](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%B0%D1%80%D1%85%D1%96%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D0%B7-%D0%BA%D1%96%D0%BB%D1%8C%D0%BA%D0%BE%D0%BC%D0%B0-%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D0%BC%D0%B8)
- [Найкращі практики](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BD%D0%B0%D0%B9%D0%BA%D1%80%D0%B0%D1%89%D1%96-%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B8)
- [Робіть](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%80%D0%BE%D0%B1%D1%96%D1%82%D1%8C)
- [Уникайте](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%83%D0%BD%D0%B8%D0%BA%D0%B0%D0%B9%D1%82%D0%B5)
- [Пов’язане](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

Постійні вказівки надають вашому агенту **постійні операційні повноваження** для визначених програм. Замість того щоб щоразу давати окремі інструкції для завдань, ви визначаєте програми з чіткою сферою дії, тригерами та правилами ескалації — і агент виконує їх автономно в межах цих кордонів.У цьому й полягає різниця між тим, щоб щоп’ятниці казати вашому помічнику «надішли щотижневий звіт», і наданням постійних повноважень: «Щотижневий звіт — у твоїй відповідальності. Готуй його щоп’ятниці, надсилай і ескалюй лише якщо щось виглядає не так».

## Навіщо потрібні постійні вказівки

**Без постійних вказівок:**

- Ви маєте надсилати агенту запит для кожного завдання
- Агент простоює між запитами
- Рутинна робота забувається або відкладається
- Ви стаєте вузьким місцем

**Із постійними вказівками:**

- Агент виконує роботу автономно в межах визначених кордонів
- Рутинна робота відбувається за розкладом без додаткових запитів
- Ви долучаєтесь лише для винятків і погоджень
- Агент продуктивно використовує час простою

## Як це працює

Постійні вказівки визначаються у файлах вашого [робочого простору агента](https://docs.openclaw.ai/uk/concepts/agent-workspace). Рекомендований підхід — включати їх безпосередньо в `AGENTS.md` (який автоматично додається в кожну сесію), щоб агент завжди мав їх у контексті. Для більших конфігурацій ви також можете розмістити їх в окремому файлі, наприклад `standing-orders.md`, і посилатися на нього з `AGENTS.md`.Кожна програма визначає:

1. **Сферу дії** — що агент уповноважений робити
2. **Тригери** — коли виконувати (за розкладом, подією або умовою)
3. **Точки погодження** — що потребує людського схвалення перед виконанням
4. **Правила ескалації** — коли слід зупинитися й попросити допомоги

Агент завантажує ці інструкції в кожній сесії через bootstrap-файли робочого простору (див. [Робочий простір агента](https://docs.openclaw.ai/uk/concepts/agent-workspace) для повного списку файлів, що додаються автоматично) і виконує їх у поєднанні з [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) для примусового запуску за часом.

Розміщуйте постійні вказівки в `AGENTS.md`, щоб гарантувати їх завантаження в кожній сесії. Bootstrap робочого простору автоматично додає `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md` і `MEMORY.md` — але не довільні файли в підкаталогах.

## Анатомія постійної вказівки

```
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

## Постійні вказівки та Cron jobs

Постійні вказівки визначають, **що** агент уповноважений робити. [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) визначають, **коли** це відбувається. Вони працюють разом:

```
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

Запит для Cron job має посилатися на постійну вказівку, а не дублювати її:

```
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

## Приклади

### Приклад 1: контент і соціальні мережі (щотижневий цикл)

```
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

### Приклад 2: фінансові операції (запуск за подією)

```
## Program: Financial Processing

**Authority:** Process transaction data, generate reports, send summaries
**Approval gate:** None for analysis. Recommendations require owner approval.
**Trigger:** New data file detected OR scheduled monthly cycle

### When new data arrives

1. Detect new file in designated input directory
2. Parse and categorize all transactions
3. Compare against budget targets
4. Flag: unusual items, threshold breaches, new recurring charges
5. Generate report in designated output directory
6. Deliver summary to owner via configured channel

### Escalation rules

- Single item > $500: immediate alert
- Category > budget by 20%: flag in report
- Unrecognizable transaction: ask owner for categorization
- Failed processing after 2 retries: report failure, do not guess
```

### Приклад 3: моніторинг і сповіщення (безперервно)

```
## Program: System Monitoring

**Authority:** Check system health, restart services, send alerts
**Approval gate:** Restart services automatically. Escalate if restart fails twice.
**Trigger:** Every heartbeat cycle

### Checks

- Service health endpoints responding
- Disk space above threshold
- Pending tasks not stale (>24 hours)
- Delivery channels operational

### Response matrix

| Condition        | Action                   | Escalate?                |
| ---------------- | ------------------------ | ------------------------ |
| Service down     | Restart automatically    | Only if restart fails 2x |
| Disk space < 10% | Alert owner              | Yes                      |
| Stale task > 24h | Remind owner             | No                       |
| Channel offline  | Log and retry next cycle | If offline > 2 hours     |
```

## Шаблон виконати-перевірити-звітувати

Постійні вказівки працюють найкраще в поєднанні зі строгою дисципліною виконання. Кожне завдання в постійній вказівці має дотримуватися такого циклу:

1. **Виконати** — Зробити фактичну роботу (а не просто підтвердити інструкцію)
2. **Перевірити** — Підтвердити, що результат правильний (файл існує, повідомлення доставлено, дані розібрано)
3. **Звітувати** — Повідомити власнику, що було зроблено і що було перевірено

```
### Execution rules

- Every task follows Execute-Verify-Report. No exceptions.
- "I'll do that" is not execution. Do it, then report.
- "Done" without verification is not acceptable. Prove it.
- If execution fails: retry once with adjusted approach.
- If still fails: report failure with diagnosis. Never silently fail.
- Never retry indefinitely — 3 attempts max, then escalate.
```

Цей шаблон запобігає найпоширенішому збою агентів: підтвердженню завдання без його завершення.

## Архітектура з кількома програмами

Для агентів, які керують кількома напрямами, організовуйте постійні вказівки як окремі програми з чіткими межами:

```
## Program 1: [Domain A] (Weekly)

...

## Program 2: [Domain B] (Monthly + On-Demand)

...

## Program 3: [Domain C] (As-Needed)

...

## Escalation Rules (All Programs)

- [Common escalation criteria]
- [Approval gates that apply across programs]
```

Кожна програма має мати:

- власну **частоту тригерів** (щотижня, щомісяця, за подією, безперервно)
- власні **точки погодження** (деякі програми потребують більшого нагляду, ніж інші)
- чіткі **межі** (агент має знати, де закінчується одна програма і починається інша)

## Найкращі практики

### Робіть

- Починайте з вузьких повноважень і розширюйте їх у міру зростання довіри
- Визначайте явні точки погодження для дій із високим ризиком
- Додавайте розділи «Що НЕ робити» — межі так само важливі, як і дозволи
- Поєднуйте з Cron jobs для надійного виконання за розкладом
- Щотижня переглядайте журнали агента, щоб переконатися, що постійні вказівки виконуються
- Оновлюйте постійні вказівки відповідно до зміни ваших потреб — це живі документи

### Уникайте

- Надавати широкі повноваження в перший день («роби все, що вважаєш за потрібне»)
- Пропускати правила ескалації — кожна програма має містити умову «коли зупинитися і запитати»
- Припускати, що агент запам’ятає усні інструкції — заносьте все у файл
- Змішувати різні напрями в одній програмі — окремі програми для окремих сфер
- Забувати про примусове виконання через Cron jobs — постійні вказівки без тригерів стають лише рекомендаціями

## Пов’язане

- [Автоматизація і завдання](https://docs.openclaw.ai/uk/automation): усі механізми автоматизації з одного погляду.
- [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs): забезпечення виконання постійних вказівок за розкладом.
- [Hooks](https://docs.openclaw.ai/uk/automation/hooks): скрипти, що запускаються за подіями життєвого циклу агента.
- [Webhooks](https://docs.openclaw.ai/uk/automation/cron-jobs#webhooks): вхідні HTTP-тригери подій.
- [Робочий простір агента](https://docs.openclaw.ai/uk/concepts/agent-workspace): де зберігаються постійні вказівки, включно з повним списком bootstrap-файлів, що додаються автоматично (`AGENTS.md`, `SOUL.md` тощо).

[Потік завдань](https://docs.openclaw.ai/uk/automation/taskflow) [Хуки](https://docs.openclaw.ai/uk/automation/hooks)

Ctrl+I

---

## Потік завдань - OpenClaw

_Source: <https://docs.openclaw.ai/uk/automation/taskflow>_

# List active and recent flows
openclaw tasks flow list

# Show details for a specific flow
openclaw tasks flow show <lookup>

# Cancel a running flow and its active tasks
openclaw tasks flow cancel <lookup>
```

| Команда | Опис |
| --- | --- |
| `openclaw tasks flow list` | Показує відстежувані потоки зі статусом і режимом синхронізації |
| `openclaw tasks flow show <id>` | Переглянути один потік за ідентифікатором потоку або ключем пошуку |
| `openclaw tasks flow cancel <id>` | Скасувати запущений потік і його активні завдання |

## Як потоки пов’язані із завданнями

Потоки координують завдання, а не замінюють їх. Один потік за час свого існування може керувати кількома фоновими завданнями. Використовуйте `openclaw tasks`, щоб переглядати окремі записи завдань, і `openclaw tasks flow`, щоб переглядати потік-оркестратор.

## Пов’язане

- [Background Tasks](https://docs.openclaw.ai/uk/automation/tasks) — реєстр відокремленої роботи, яку координують потоки
- [CLI: tasks](https://docs.openclaw.ai/uk/cli/tasks) — довідник команд CLI для `openclaw tasks flow`
- [Automation Overview](https://docs.openclaw.ai/uk/automation) — усі механізми автоматизації в одному огляді
- [Cron Jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) — заплановані завдання, які можуть передавати дані в потоки

[Background tasks](https://docs.openclaw.ai/uk/automation/tasks) [Постійні вказівки](https://docs.openclaw.ai/uk/automation/standing-orders)

Ctrl+I

---
