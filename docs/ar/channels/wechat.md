---
source_url: https://docs.openclaw.ai/ar/channels/wechat
title: "WeChat - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/wechat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Regional platforms

WeChat

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [التسمية](https://docs.openclaw.ai/ar/channels/wechat#%D8%A7%D9%84%D8%AA%D8%B3%D9%85%D9%8A%D8%A9)
- [كيف يعمل](https://docs.openclaw.ai/ar/channels/wechat#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84)
- [التثبيت](https://docs.openclaw.ai/ar/channels/wechat#%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA)
- [تسجيل الدخول](https://docs.openclaw.ai/ar/channels/wechat#%D8%AA%D8%B3%D8%AC%D9%8A%D9%84-%D8%A7%D9%84%D8%AF%D8%AE%D9%88%D9%84)
- [التحكم في الوصول](https://docs.openclaw.ai/ar/channels/wechat#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)
- [التوافق](https://docs.openclaw.ai/ar/channels/wechat#%D8%A7%D9%84%D8%AA%D9%88%D8%A7%D9%81%D9%82)
- [عملية Sidecar](https://docs.openclaw.ai/ar/channels/wechat#%D8%B9%D9%85%D9%84%D9%8A%D8%A9-sidecar)
- [استكشاف الأخطاء وإصلاحها](https://docs.openclaw.ai/ar/channels/wechat#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)
- [مستندات ذات صلة](https://docs.openclaw.ai/ar/channels/wechat#%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

يتصل OpenClaw بـ WeChat من خلال Plugin القناة الخارجي من Tencent
`@tencent-weixin/openclaw-weixin`.الحالة: Plugin خارجي. الدردشات المباشرة والوسائط مدعومة. ولا يتم الإعلان عن دردشات المجموعات
بواسطة بيانات قدرات Plugin الحالية.

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%A7%D9%84%D8%AA%D8%B3%D9%85%D9%8A%D8%A9)  التسمية

- **WeChat** هو الاسم الظاهر للمستخدم في هذه المستندات.
- **Weixin** هو الاسم المستخدم بواسطة حزمة Tencent وبواسطة معرّف Plugin.
- `openclaw-weixin` هو معرّف قناة OpenClaw.
- `@tencent-weixin/openclaw-weixin` هي حزمة npm.

استخدم `openclaw-weixin` في أوامر CLI ومسارات الإعدادات.

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D9%83%D9%8A%D9%81-%D9%8A%D8%B9%D9%85%D9%84)  كيف يعمل

لا يوجد كود WeChat داخل مستودع OpenClaw الأساسي. يوفّر OpenClaw
العقد العام لـ Plugin القنوات، ويوفّر Plugin الخارجي
وقت التشغيل الخاص بـ WeChat:

1. يقوم `openclaw plugins install` بتثبيت `@tencent-weixin/openclaw-weixin`.
2. يكتشف Gateway بيان Plugin ويحمّل نقطة دخول Plugin.
3. يسجّل Plugin معرّف القناة `openclaw-weixin`.
4. يبدأ `openclaw channels login --channel openclaw-weixin` تسجيل الدخول عبر QR.
5. يخزّن Plugin بيانات اعتماد الحساب ضمن دليل حالة OpenClaw.
6. عند بدء تشغيل Gateway، يبدأ Plugin مراقب Weixin الخاص به لكل
حساب مُهيأ.
7. تُطبَّع رسائل WeChat الواردة عبر عقد القناة، وتُوجَّه إلى
وكيل OpenClaw المحدد، ثم تُرسَل مرة أخرى عبر مسار الإرسال الصادر الخاص بـ Plugin.

هذا الفصل مهم: يجب أن يظل OpenClaw core مستقلًا عن القنوات. فتسجيل دخول WeChat،
واستدعاءات واجهة Tencent iLink البرمجية، ورفع/تنزيل الوسائط، ورموز السياق، ومراقبة
الحسابات كلها مملوكة لـ Plugin الخارجي.

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%A7%D9%84%D8%AA%D8%AB%D8%A8%D9%8A%D8%AA)  التثبيت

تثبيت سريع:

```
npx -y @tencent-weixin/openclaw-weixin-cli install
```

تثبيت يدوي:

```
openclaw plugins install "@tencent-weixin/openclaw-weixin"
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

أعد تشغيل Gateway بعد التثبيت:

```
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%AA%D8%B3%D8%AC%D9%8A%D9%84-%D8%A7%D9%84%D8%AF%D8%AE%D9%88%D9%84)  تسجيل الدخول

شغّل تسجيل الدخول عبر QR على الجهاز نفسه الذي يشغّل Gateway:

```
openclaw channels login --channel openclaw-weixin
```

امسح رمز QR باستخدام WeChat على هاتفك وأكّد تسجيل الدخول. يحفظ Plugin
رمز الحساب محليًا بعد نجاح المسح.لإضافة حساب WeChat آخر، شغّل أمر تسجيل الدخول نفسه مرة أخرى. وبالنسبة للحسابات
المتعددة، اعزل جلسات الرسائل المباشرة حسب الحساب، والقناة، والمرسل:

```
openclaw config set session.dmScope per-account-channel-peer
```

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%A7%D9%84%D8%AA%D8%AD%D9%83%D9%85-%D9%81%D9%8A-%D8%A7%D9%84%D9%88%D8%B5%D9%88%D9%84)  التحكم في الوصول

تستخدم الرسائل المباشرة نموذج الاقتران وallowlist العادي في OpenClaw الخاص بـ Plugins
القنوات.للموافقة على مرسلين جدد:

```
openclaw pairing list openclaw-weixin
openclaw pairing approve openclaw-weixin <CODE>
```

للاطلاع على نموذج التحكم الكامل في الوصول، راجع [الاقتران](https://docs.openclaw.ai/ar/channels/pairing).

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%A7%D9%84%D8%AA%D9%88%D8%A7%D9%81%D9%82)  التوافق

يتحقق Plugin من إصدار OpenClaw المضيف عند بدء التشغيل.

| خط Plugin | إصدار OpenClaw | وسم npm |
| --- | --- | --- |
| `2.x` | `>=2026.3.22` | `latest` |
| `1.x` | `>=2026.1.0 <2026.3.22` | `legacy` |

إذا أبلغ Plugin أن إصدار OpenClaw لديك قديم جدًا، فإما أن تحدّث
OpenClaw أو تثبّت خط Plugin القديم:

```
openclaw plugins install @tencent-weixin/openclaw-weixin@legacy
```

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%B9%D9%85%D9%84%D9%8A%D8%A9-sidecar)  عملية Sidecar

يمكن لـ Plugin الخاص بـ WeChat تشغيل أعمال مساعدة إلى جانب Gateway أثناء مراقبته
لواجهة Tencent iLink البرمجية. في المشكلة #68451، كشف مسار المساعد هذا عن خطأ في
تنظيف OpenClaw العام لـ Gateway القديم: إذ كان بإمكان عملية فرعية أن تحاول تنظيف
عملية Gateway الأصلية، مما يسبب حلقات إعادة تشغيل تحت مديري العمليات مثل systemd.يستثني تنظيف بدء التشغيل الحالي في OpenClaw العملية الحالية وأسلافها،
لذلك يجب ألا تقتل أداة مساعدة القناة Gateway الذي أطلقها. وهذا الإصلاح
عام؛ وليس مسارًا خاصًا بـ WeChat داخل core.

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D8%A7%D8%B3%D8%AA%D9%83%D8%B4%D8%A7%D9%81-%D8%A7%D9%84%D8%A3%D8%AE%D8%B7%D8%A7%D8%A1-%D9%88%D8%A5%D8%B5%D9%84%D8%A7%D8%AD%D9%87%D8%A7)  استكشاف الأخطاء وإصلاحها

تحقق من التثبيت والحالة:

```
openclaw plugins list
openclaw channels status --probe
openclaw --version
```

إذا ظهرت القناة على أنها مثبتة ولكنها لا تتصل، فتأكد من أن Plugin
ممكّن وأعد التشغيل:

```
openclaw config set plugins.entries.openclaw-weixin.enabled true
openclaw gateway restart
```

إذا كان Gateway يُعاد تشغيله بشكل متكرر بعد تمكين WeChat، فحدّث كلًا من OpenClaw وPlugin:

```
npm view @tencent-weixin/openclaw-weixin version
openclaw plugins install "@tencent-weixin/openclaw-weixin" --force
openclaw gateway restart
```

تعطيل مؤقت:

```
openclaw config set plugins.entries.openclaw-weixin.enabled false
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/ar/channels/wechat\#%D9%85%D8%B3%D8%AA%D9%86%D8%AF%D8%A7%D8%AA-%D8%B0%D8%A7%D8%AA-%D8%B5%D9%84%D8%A9)  مستندات ذات صلة

- نظرة عامة على القنوات: [قنوات الدردشة](https://docs.openclaw.ai/ar/channels)
- الاقتران: [الاقتران](https://docs.openclaw.ai/ar/channels/pairing)
- توجيه القنوات: [توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing)
- بنية Plugin: [بنية Plugin](https://docs.openclaw.ai/ar/plugins/architecture)
- SDK الخاص بـ Plugin القنوات: [SDK لـ Plugin القنوات](https://docs.openclaw.ai/ar/plugins/sdk-channel-plugins)
- الحزمة الخارجية: [@tencent-weixin/openclaw-weixin](https://www.npmjs.com/package/@tencent-weixin/openclaw-weixin)

[سطر](https://docs.openclaw.ai/ar/channels/line) [بوت QQ](https://docs.openclaw.ai/ar/channels/qqbot)

Ctrl+I