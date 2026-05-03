---
source_url: https://docs.openclaw.ai/ar/gateway/health
title: "\u0641\u062d\u0648\u0635\u0627\u062a \u0627\u0644\u0635\u062d\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/gateway/health#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Health and diagnostics

فحوصات الصحة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [فحوصات سريعة](https://docs.openclaw.ai/ar/gateway/health#%D9%81%D8%AD%D9%88%D8%B5%D8%A7%D8%AA-%D8%B3%D8%B1%D9%8A%D8%B9%D8%A9)
- [تشخيصات معمقة](https://docs.openclaw.ai/ar/gateway/health#%D8%AA%D8%B4%D8%AE%D9%8A%D8%B5%D8%A7%D8%AA-%D9%85%D8%B9%D9%85%D9%82%D8%A9)
- [إعدادات مراقب الصحة](https://docs.openclaw.ai/ar/gateway/health#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D9%85%D8%B1%D8%A7%D9%82%D8%A8-%D8%A7%D9%84%D8%B5%D8%AD%D8%A9)
- [عند فشل شيء ما](https://docs.openclaw.ai/ar/gateway/health#%D8%B9%D9%86%D8%AF-%D9%81%D8%B4%D9%84-%D8%B4%D9%8A%D8%A1-%D9%85%D8%A7)
- [الأمر المخصص “health”](https://docs.openclaw.ai/ar/gateway/health#%D8%A7%D9%84%D8%A3%D9%85%D8%B1-%D8%A7%D9%84%D9%85%D8%AE%D8%B5%D8%B5-%E2%80%9Chealth%E2%80%9D)
- [ذات صلة](https://docs.openclaw.ai/ar/gateway/health#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

دليل قصير للتحقق من اتصال القناة دون تخمين.

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D9%81%D8%AD%D9%88%D8%B5%D8%A7%D8%AA-%D8%B3%D8%B1%D9%8A%D8%B9%D8%A9)  فحوصات سريعة

- `openclaw status` — ملخص محلي: قابلية الوصول إلى Gateway/الوضع، تلميح التحديث، عمر مصادقة القناة المرتبطة، الجلسات + النشاط الأخير.
- `openclaw status --all` — تشخيص محلي كامل (للقراءة فقط، ملوّن، آمن للصقه عند التصحيح).
- `openclaw status --deep` — يطلب من Gateway قيد التشغيل إجراء فحص صحة مباشر (`health` مع `probe:true`)، بما في ذلك فحوصات القنوات لكل حساب عند دعمها.
- `openclaw health` — يطلب من Gateway قيد التشغيل لقطة حالته الصحية (WS فقط؛ لا توجد مقابس قنوات مباشرة من CLI).
- `openclaw health --verbose` — يفرض فحص صحة مباشرًا ويطبع تفاصيل اتصال Gateway.
- `openclaw health --json` — إخراج لقطة الصحة بصيغة قابلة للقراءة آليًا.
- أرسل `/status` كرسالة مستقلة في WhatsApp/WebChat للحصول على رد حالة دون استدعاء الوكيل.
- السجلات: تابع `/tmp/openclaw/openclaw-*.log` وصفِّ حسب `web-heartbeat`، و`web-reconnect`، و`web-auto-reply`، و`web-inbound`.

بالنسبة إلى Discord وموفري الدردشة الآخرين، لا تمثل صفوف الجلسات حيوية المقبس.
تقرأ `openclaw sessions` و`sessions.list` في Gateway وأداة الوكيل `sessions_list`
حالة المحادثة المخزنة. يمكن للمزوّد إعادة الاتصال وإظهار حالة قناة سليمة
قبل تجسيد أي صف جلسة جديد. استخدم حالة القناة وأوامر الصحة أعلاه لفحوصات الاتصال المباشر.

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D8%AA%D8%B4%D8%AE%D9%8A%D8%B5%D8%A7%D8%AA-%D9%85%D8%B9%D9%85%D9%82%D8%A9)  تشخيصات معمقة

- بيانات الاعتماد على القرص: `ls -l ~/.openclaw/credentials/whatsapp/<accountId>/creds.json` (ينبغي أن يكون mtime حديثًا).
- مخزن الجلسات: `ls -l ~/.openclaw/agents/<agentId>/sessions/sessions.json` (يمكن تجاوز المسار في الإعدادات). يعرض `status` العدد والمستلمين الأحدث.
- تدفق إعادة الربط: `openclaw channels logout && openclaw channels login --verbose` عند ظهور رموز الحالة 409–515 أو `loggedOut` في السجلات. (ملاحظة: تدفق تسجيل الدخول عبر QR يعيد التشغيل تلقائيًا مرة واحدة للحالة 515 بعد الاقتران.)
- التشخيصات مفعّلة افتراضيًا. يسجل Gateway الحقائق التشغيلية ما لم يتم ضبط `diagnostics.enabled: false`. تسجل أحداث الذاكرة أعداد بايت RSS/heap، وضغط العتبة، وضغط النمو. تسجل تحذيرات الحيوية تأخير حلقة الأحداث، واستخدام حلقة الأحداث، ونسبة نوى CPU، وأعداد الجلسات النشطة/المنتظرة/المصفوفة عندما تكون العملية قيد التشغيل لكنها مشبعة. تسجل أحداث الحمولة الزائدة ما رُفض أو اقتُطع أو قُسّم، بالإضافة إلى الأحجام والحدود عند توفرها. لا تسجل نص الرسالة، أو محتويات المرفقات، أو جسم Webhook، أو جسم الطلب أو الاستجابة الخام، أو الرموز، أو ملفات تعريف الارتباط، أو القيم السرية. يبدأ Heartbeat نفسه مسجل الاستقرار المحدود، المتاح عبر `openclaw gateway stability` أو `diagnostics.stability` في Gateway RPC. تستمر عمليات خروج Gateway الفادحة، ومهل إيقاف التشغيل، وفشل بدء إعادة التشغيل في حفظ أحدث لقطة للمسجل ضمن `~/.openclaw/logs/stability/` عند وجود أحداث؛ افحص أحدث حزمة محفوظة باستخدام `openclaw gateway stability --bundle latest`.
- لتقارير الأخطاء، شغّل `openclaw gateway diagnostics export` وأرفق ملف zip المُنشأ. يجمع التصدير ملخص Markdown، وأحدث حزمة استقرار، وبيانات وصفية منقّحة للسجلات، ولقطات حالة/صحة Gateway منقّحة، وشكل الإعدادات. وهو مصمم للمشاركة: تُحذف أو تُنقّح نصوص الدردشة، وأجسام Webhook، ومخرجات الأدوات، وبيانات الاعتماد، وملفات تعريف الارتباط، ومعرّفات الحسابات/الرسائل، والقيم السرية. راجع [تصدير التشخيصات](https://docs.openclaw.ai/ar/gateway/diagnostics).

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF%D8%A7%D8%AA-%D9%85%D8%B1%D8%A7%D9%82%D8%A8-%D8%A7%D9%84%D8%B5%D8%AD%D8%A9)  إعدادات مراقب الصحة

- `gateway.channelHealthCheckMinutes`: عدد مرات فحص Gateway لصحة القناة. الافتراضي: `5`. اضبطه على `0` لتعطيل عمليات إعادة التشغيل الخاصة بمراقب الصحة عالميًا.
- `gateway.channelStaleEventThresholdMinutes`: المدة التي يمكن أن تبقى فيها قناة متصلة خاملة قبل أن يعتبرها مراقب الصحة قديمة ويعيد تشغيلها. الافتراضي: `30`. أبقِ هذا أكبر من أو يساوي `gateway.channelHealthCheckMinutes`.
- `gateway.channelMaxRestartsPerHour`: حد متحرك لمدة ساعة واحدة لعمليات إعادة تشغيل مراقب الصحة لكل قناة/حساب. الافتراضي: `10`.
- `channels.<provider>.healthMonitor.enabled`: تعطيل عمليات إعادة تشغيل مراقب الصحة لقناة محددة مع إبقاء المراقبة العالمية مفعّلة.
- `channels.<provider>.accounts.<accountId>.healthMonitor.enabled`: تجاوز متعدد الحسابات تكون له الأولوية على الإعداد على مستوى القناة.
- تنطبق هذه التجاوزات لكل قناة على مراقبات القنوات المدمجة التي تعرضها اليوم: Discord، وGoogle Chat، وiMessage، وMicrosoft Teams، وSignal، وSlack، وTelegram، وWhatsApp.

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D8%B9%D9%86%D8%AF-%D9%81%D8%B4%D9%84-%D8%B4%D9%8A%D8%A1-%D9%85%D8%A7)  عند فشل شيء ما

- `logged out` أو الحالة 409–515 ← أعد الربط باستخدام `openclaw channels logout` ثم `openclaw channels login`.
- تعذر الوصول إلى Gateway ← ابدأ تشغيله: `openclaw gateway --port 18789` (استخدم `--force` إذا كان المنفذ مشغولًا).
- لا توجد رسائل واردة ← تأكد من أن الهاتف المرتبط متصل بالإنترنت وأن المرسل مسموح به (`channels.whatsapp.allowFrom`)؛ في دردشات المجموعات، تأكد من تطابق قائمة السماح \+ قواعد الإشارة (`channels.whatsapp.groups`، و`agents.list[].groupChat.mentionPatterns`).

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D8%A7%D9%84%D8%A3%D9%85%D8%B1-%D8%A7%D9%84%D9%85%D8%AE%D8%B5%D8%B5-%E2%80%9Chealth%E2%80%9D)  الأمر المخصص “health”

يطلب `openclaw health` من Gateway قيد التشغيل لقطة حالته الصحية (لا توجد مقابس قنوات
مباشرة من CLI). افتراضيًا، يمكنه إرجاع لقطة Gateway مخزنة مؤقتًا حديثة؛ ثم
يحدّث Gateway ذلك التخزين المؤقت في الخلفية. يفرض `openclaw health --verbose`
فحصًا مباشرًا بدلًا من ذلك. يبلّغ الأمر عن بيانات الاعتماد المرتبطة/عمر المصادقة عند توفرها،
وملخصات الفحص لكل قناة، وملخص مخزن الجلسات، ومدة الفحص. يخرج
برمز غير صفري إذا تعذر الوصول إلى Gateway أو فشل الفحص/انتهت مهلته.الخيارات:

- `--json`: إخراج JSON قابل للقراءة آليًا
- `--timeout <ms>`: تجاوز مهلة الفحص الافتراضية البالغة 10 ثوانٍ
- `--verbose`: فرض فحص مباشر وطباعة تفاصيل اتصال Gateway
- `--debug`: اسم مستعار لـ `--verbose`

تتضمن لقطة الصحة: `ok` (قيمة منطقية)، و`ts` (طابع زمني)، و`durationMs` (وقت الفحص)، وحالة كل قناة، وتوافر الوكيل، وملخص مخزن الجلسات.

## [​](https://docs.openclaw.ai/ar/gateway/health\#%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  ذات صلة

- [دليل تشغيل Gateway](https://docs.openclaw.ai/ar/gateway)
- [تصدير التشخيصات](https://docs.openclaw.ai/ar/gateway/diagnostics)
- [استكشاف أخطاء Gateway وإصلاحها](https://docs.openclaw.ai/ar/gateway/troubleshooting)

[Trusted proxy auth](https://docs.openclaw.ai/ar/gateway/trusted-proxy-auth) [Heartbeat](https://docs.openclaw.ai/ar/gateway/heartbeat)

Ctrl+I