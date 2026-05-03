---
source_url: https://docs.openclaw.ai/ar/channels/troubleshooting
title: "\u0627\u0633\u062a\u0643\u0634\u0627\u0641 \u0645\u0634\u0643\u0644\u0627\u062a \u0627\u0644\u0642\u0646\u0648\u0627\u062a \u0648\u0625\u0635\u0644\u0627\u062d\u0647\u0627 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/troubleshooting#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

استكشاف مشكلات القنوات وإصلاحها

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [سلّم الأوامر](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%B3%D9%84%D9%91%D9%85-%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)
- [WhatsApp](https://docs.openclaw.ai/ar/channels/troubleshooting#whatsapp)
- [توقيعات فشل WhatsApp](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-whatsapp)
- [Telegram](https://docs.openclaw.ai/ar/channels/troubleshooting#telegram)
- [توقيعات فشل Telegram](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-telegram)
- [Discord](https://docs.openclaw.ai/ar/channels/troubleshooting#discord)
- [توقيعات فشل Discord](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-discord)
- [Slack](https://docs.openclaw.ai/ar/channels/troubleshooting#slack)
- [توقيعات فشل Slack](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-slack)
- [iMessage و BlueBubbles](https://docs.openclaw.ai/ar/channels/troubleshooting#imessage-%D9%88-bluebubbles)
- [توقيعات فشل iMessage و BlueBubbles](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-imessage-%D9%88-bluebubbles)
- [Signal](https://docs.openclaw.ai/ar/channels/troubleshooting#signal)
- [توقيعات فشل Signal](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-signal)
- [QQ Bot](https://docs.openclaw.ai/ar/channels/troubleshooting#qq-bot)
- [توقيعات فشل QQ Bot](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-qq-bot)
- [Matrix](https://docs.openclaw.ai/ar/channels/troubleshooting#matrix)
- [توقيعات فشل Matrix](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-matrix)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/troubleshooting#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

استخدم هذه الصفحة عندما تتصل قناة لكن يكون السلوك غير صحيح.

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%B3%D9%84%D9%91%D9%85-%D8%A7%D9%84%D8%A3%D9%88%D8%A7%D9%85%D8%B1)  سلّم الأوامر

شغّل هذه بالترتيب أولاً:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

خط الأساس السليم:

- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`، أو `write-capable`، أو `admin-capable`
- يعرض فحص القناة أن النقل متصل، وحيثما كان مدعوماً، `works` أو `audit ok`

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#whatsapp)  WhatsApp

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-whatsapp)  توقيعات فشل WhatsApp

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| متصل لكن لا توجد ردود DM | `openclaw pairing list whatsapp` | وافق على المرسل أو بدّل سياسة/قائمة السماح لـ DM. |
| يتم تجاهل رسائل المجموعة | تحقق من `requireMention` \+ أنماط الإشارة في الإعداد | اذكر الروبوت أو خفف سياسة الإشارة لتلك المجموعة. |
| تنتهي مهلة تسجيل الدخول عبر QR مع 408 | تحقق من متغيرات بيئة Gateway `HTTPS_PROXY` / `HTTP_PROXY` | عيّن وكيلاً قابلاً للوصول؛ استخدم `NO_PROXY` للتجاوزات فقط. |
| حلقات قطع اتصال/إعادة تسجيل دخول عشوائية | `openclaw channels status --probe` \+ السجلات | تُعلَّم عمليات إعادة الاتصال الأخيرة حتى عند الاتصال حالياً؛ راقب السجلات، أعد تشغيل Gateway، ثم أعد الربط إذا استمر التذبذب. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء WhatsApp](https://docs.openclaw.ai/ar/channels/whatsapp#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#telegram)  Telegram

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-telegram)  توقيعات فشل Telegram

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| `/start` لكن لا يوجد تدفق رد قابل للاستخدام | `openclaw pairing list telegram` | وافق على الاقتران أو غيّر سياسة DM. |
| الروبوت متصل لكن المجموعة تبقى صامتة | تحقق من متطلب الإشارة ووضع خصوصية الروبوت | عطّل وضع الخصوصية لإتاحة الظهور في المجموعة أو اذكر الروبوت. |
| فشل الإرسال مع أخطاء شبكة | افحص السجلات بحثاً عن فشل استدعاءات Telegram API | أصلح توجيه DNS/IPv6/الوكيل إلى `api.telegram.org`. |
| يبلّغ بدء التشغيل أن `getMe returned 401` | تحقق من مصدر الرمز المميز المكوَّن | انسخ رمز BotFather المميز مجدداً أو أعد توليده وحدّث `botToken` أو `tokenFile` أو `TELEGRAM_BOT_TOKEN` للحساب الافتراضي. |
| يتوقف الاستقصاء أو يعيد الاتصال ببطء | `openclaw logs --follow` لتشخيصات الاستقصاء | رقِّ؛ إذا كانت عمليات إعادة التشغيل نتائج إيجابية كاذبة، فاضبط `pollingStallThresholdMs`. ما زالت التوقفات المستمرة تشير إلى الوكيل/DNS/IPv6. |
| يتم رفض `setMyCommands` عند بدء التشغيل | افحص السجلات بحثاً عن `BOT_COMMANDS_TOO_MUCH` | قلّل أوامر Telegram الخاصة بـ Plugin/المهارات/المخصصة أو عطّل القوائم الأصلية. |
| بعد الترقية، قائمة السماح تمنعك | `openclaw security audit` وقوائم السماح في الإعداد | شغّل `openclaw doctor --fix` أو استبدل `@username` بمعرّفات مرسلين رقمية. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء Telegram](https://docs.openclaw.ai/ar/channels/telegram#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#discord)  Discord

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-discord)  توقيعات فشل Discord

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| الروبوت متصل لكن لا توجد ردود في الخادم | `openclaw channels status --probe` | اسمح للخادم/القناة وتحقق من نية محتوى الرسائل. |
| يتم تجاهل رسائل المجموعة | تحقق من السجلات بحثاً عن إسقاطات بوابة الإشارات | اذكر الروبوت أو عيّن `requireMention: false` للخادم/القناة. |
| ردود DM مفقودة | `openclaw pairing list discord` | وافق على اقتران DM أو اضبط سياسة DM. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء Discord](https://docs.openclaw.ai/ar/channels/discord#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#slack)  Slack

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-slack)  توقيعات فشل Slack

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| وضع Socket متصل لكن لا توجد استجابات | `openclaw channels status --probe` | تحقق من رمز التطبيق \+ رمز الروبوت والنطاقات المطلوبة؛ راقب `botTokenStatus` / `appTokenStatus = configured_unavailable` في الإعدادات المدعومة بـ SecretRef. |
| DMs محظورة | `openclaw pairing list slack` | وافق على الاقتران أو خفف سياسة DM. |
| يتم تجاهل رسالة القناة | تحقق من `groupPolicy` وقائمة السماح للقنوات | اسمح للقناة أو بدّل السياسة إلى `open`. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء Slack](https://docs.openclaw.ai/ar/channels/slack#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#imessage-%D9%88-bluebubbles)  iMessage و BlueBubbles

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-imessage-%D9%88-bluebubbles)  توقيعات فشل iMessage و BlueBubbles

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| لا توجد أحداث واردة | تحقق من قابلية الوصول إلى Webhook/الخادم وأذونات التطبيق | أصلح عنوان URL الخاص بـ Webhook أو حالة خادم BlueBubbles. |
| يمكن الإرسال لكن لا يوجد استقبال على macOS | تحقق من أذونات خصوصية macOS لأتمتة Messages | امنح أذونات TCC مجدداً وأعد تشغيل عملية القناة. |
| مرسل DM محظور | `openclaw pairing list imessage` أو `openclaw pairing list bluebubbles` | وافق على الاقتران أو حدّث قائمة السماح. |

استكشاف الأخطاء الكامل:

- [استكشاف أخطاء iMessage](https://docs.openclaw.ai/ar/channels/imessage#troubleshooting)
- [استكشاف أخطاء BlueBubbles](https://docs.openclaw.ai/ar/channels/bluebubbles#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#signal)  Signal

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-signal)  توقيعات فشل Signal

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| البرنامج الخفي قابل للوصول لكن الروبوت صامت | `openclaw channels status --probe` | تحقق من عنوان URL/حساب البرنامج الخفي `signal-cli` ووضع الاستقبال. |
| DM محظور | `openclaw pairing list signal` | وافق على المرسل أو اضبط سياسة DM. |
| ردود المجموعة لا تُشغَّل | تحقق من قائمة سماح المجموعة وأنماط الإشارة | أضف المرسل/المجموعة أو خفف البوابة. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء Signal](https://docs.openclaw.ai/ar/channels/signal#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#qq-bot)  QQ Bot

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-qq-bot)  توقيعات فشل QQ Bot

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| يرد الروبوت “gone to Mars” | تحقق من `appId` و `clientSecret` في الإعداد | عيّن بيانات الاعتماد أو أعد تشغيل Gateway. |
| لا توجد رسائل واردة | `openclaw channels status --probe` | تحقق من بيانات الاعتماد على QQ Open Platform. |
| لا يتم نسخ الصوت نصياً | تحقق من إعداد موفر STT | كوّن `channels.qqbot.stt` أو `tools.media.audio`. |
| الرسائل الاستباقية لا تصل | تحقق من متطلبات تفاعل منصة QQ | قد يحظر QQ الرسائل التي يبدأها الروبوت دون تفاعل حديث. |

استكشاف الأخطاء الكامل: [استكشاف أخطاء QQ Bot](https://docs.openclaw.ai/ar/channels/qqbot#troubleshooting)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#matrix)  Matrix

### [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%AA%D9%88%D9%82%D9%8A%D8%B9%D8%A7%D8%AA-%D9%81%D8%B4%D9%84-matrix)  توقيعات فشل Matrix

| العَرَض | أسرع فحص | الإصلاح |
| --- | --- | --- |
| تم تسجيل الدخول لكن رسائل الغرفة تُتجاهل | `openclaw channels status --probe` | تحقق من `groupPolicy`، وقائمة سماح الغرف، وبوابة الإشارات. |
| DMs لا تُعالَج | `openclaw pairing list matrix` | وافق على المرسل أو اضبط سياسة DM. |
| تفشل الغرف المشفرة | `openclaw matrix verify status` | أعد التحقق من الجهاز، ثم تحقق من `openclaw matrix verify backup status`. |
| استعادة النسخ الاحتياطي معلقة/معطلة | `openclaw matrix verify backup status` | شغّل `openclaw matrix verify backup restore` أو أعد التشغيل باستخدام مفتاح استرداد. |
| يبدو التوقيع المتبادل/التمهيد غير صحيح | `openclaw matrix verify bootstrap` | أصلح التخزين السري، والتوقيع المتبادل، وحالة النسخ الاحتياطي دفعة واحدة. |

الإعداد والتهيئة بالكامل: [Matrix](https://docs.openclaw.ai/ar/channels/matrix)

## [​](https://docs.openclaw.ai/ar/channels/troubleshooting\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [الاقتران](https://docs.openclaw.ai/ar/channels/pairing)
- [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing)
- [استكشاف أخطاء Gateway](https://docs.openclaw.ai/ar/gateway/troubleshooting)

[تحليل موقع القناة](https://docs.openclaw.ai/ar/channels/location) [قناة ضمان الجودة](https://docs.openclaw.ai/ar/channels/qa-channel)

Ctrl+I