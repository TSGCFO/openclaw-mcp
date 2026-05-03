---
source_url: https://docs.openclaw.ai/ar/providers/deepseek
title: "DeepSeek - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/providers/deepseek#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Providers

DeepSeek

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [بدء الاستخدام](https://docs.openclaw.ai/ar/providers/deepseek#%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)
- [الكتالوج المضمّن](https://docs.openclaw.ai/ar/providers/deepseek#%D8%A7%D9%84%D9%83%D8%AA%D8%A7%D9%84%D9%88%D8%AC-%D8%A7%D9%84%D9%85%D8%B6%D9%85%D9%91%D9%86)
- [التفكير والأدوات](https://docs.openclaw.ai/ar/providers/deepseek#%D8%A7%D9%84%D8%AA%D9%81%D9%83%D9%8A%D8%B1-%D9%88%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA)
- [الاختبار المباشر](https://docs.openclaw.ai/ar/providers/deepseek#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1)
- [مثال إعداد](https://docs.openclaw.ai/ar/providers/deepseek#%D9%85%D8%AB%D8%A7%D9%84-%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)
- [ذو صلة](https://docs.openclaw.ai/ar/providers/deepseek#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[DeepSeek](https://www.deepseek.com/) يوفّر نماذج ذكاء اصطناعي قوية بواجهة API متوافقة مع OpenAI.

| الخاصية | القيمة |
| --- | --- |
| المزوّد | `deepseek` |
| المصادقة | `DEEPSEEK_API_KEY` |
| API | متوافق مع OpenAI |
| عنوان URL الأساسي | `https://api.deepseek.com` |

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D8%A8%D8%AF%D8%A1-%D8%A7%D9%84%D8%A7%D8%B3%D8%AA%D8%AE%D8%AF%D8%A7%D9%85)  بدء الاستخدام

1

[Navigate to header](https://docs.openclaw.ai/ar/providers/deepseek#)

احصل على مفتاح API الخاص بك

أنشئ مفتاح API على [platform.deepseek.com](https://platform.deepseek.com/api_keys).

2

[Navigate to header](https://docs.openclaw.ai/ar/providers/deepseek#)

شغّل الإعداد الأولي

```
openclaw onboard --auth-choice deepseek-api-key
```

سيطلب هذا مفتاح API الخاص بك ويعيّن `deepseek/deepseek-v4-flash` كنموذج افتراضي.

3

[Navigate to header](https://docs.openclaw.ai/ar/providers/deepseek#)

تحقّق من توفر النماذج

```
openclaw models list --provider deepseek
```

لفحص الكتالوج الثابت المضمّن بدون الحاجة إلى Gateway قيد التشغيل،
استخدم:

```
openclaw models list --all --provider deepseek
```

إعداد غير تفاعلي

للتثبيتات النصية أو التي تعمل بلا واجهة، مرّر كل العلامات مباشرة:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice deepseek-api-key \
  --deepseek-api-key "$DEEPSEEK_API_KEY" \
  --skip-health \
  --accept-risk
```

إذا كان Gateway يعمل كخدمة خلفية (launchd/systemd)، فتأكد من أن `DEEPSEEK_API_KEY`
متاح لتلك العملية (على سبيل المثال، في `~/.openclaw/.env` أو عبر
`env.shellEnv`).

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D8%A7%D9%84%D9%83%D8%AA%D8%A7%D9%84%D9%88%D8%AC-%D8%A7%D9%84%D9%85%D8%B6%D9%85%D9%91%D9%86)  الكتالوج المضمّن

| مرجع النموذج | الاسم | الإدخال | السياق | الحد الأقصى للإخراج | ملاحظات |
| --- | --- | --- | --- | --- | --- |
| `deepseek/deepseek-v4-flash` | DeepSeek V4 Flash | text | 1,000,000 | 384,000 | النموذج الافتراضي؛ سطح V4 يدعم التفكير |
| `deepseek/deepseek-v4-pro` | DeepSeek V4 Pro | text | 1,000,000 | 384,000 | سطح V4 يدعم التفكير |
| `deepseek/deepseek-chat` | DeepSeek Chat | text | 131,072 | 8,192 | سطح DeepSeek V3.2 غير مخصص للتفكير |
| `deepseek/deepseek-reasoner` | DeepSeek Reasoner | text | 131,072 | 65,536 | سطح V3.2 مفعّل للاستدلال |

تدعم نماذج V4 عنصر التحكم `thinking` في DeepSeek. يعيد OpenClaw أيضًا تشغيل
`reasoning_content` من DeepSeek في الجولات اللاحقة حتى تتمكن جلسات التفكير التي تتضمن
استدعاءات أدوات من المتابعة.
استخدم `/think xhigh` أو `/think max` مع نماذج DeepSeek V4 لطلب
الحد الأقصى من `reasoning_effort` لدى DeepSeek.

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D8%A7%D9%84%D8%AA%D9%81%D9%83%D9%8A%D8%B1-%D9%88%D8%A7%D9%84%D8%A3%D8%AF%D9%88%D8%A7%D8%AA)  التفكير والأدوات

لجلسات التفكير في DeepSeek V4 عقد إعادة تشغيل أكثر صرامة من معظم
المزوّدين المتوافقين مع OpenAI: بعد أن تستخدم جولة مفعّلة للتفكير الأدوات، يتوقع DeepSeek
أن تتضمن رسائل المساعد المعاد تشغيلها من تلك الجولة
`reasoning_content` في الطلبات اللاحقة. يتعامل OpenClaw مع هذا داخل
Plugin الخاص بـ DeepSeek، لذلك يعمل استخدام الأدوات الطبيعي متعدد الجولات مع
`deepseek/deepseek-v4-flash` و`deepseek/deepseek-v4-pro`.إذا بدّلت جلسة حالية من مزوّد آخر متوافق مع OpenAI إلى
نموذج DeepSeek V4، فقد لا تحتوي جولات استدعاء أدوات المساعد الأقدم على
`reasoning_content` أصلي من DeepSeek. يملأ OpenClaw ذلك الحقل المفقود عند إعادة تشغيل
رسائل المساعد لطلبات التفكير في DeepSeek V4 حتى يتمكن المزوّد من قبول
السجل بدون الحاجة إلى `/new`.عند تعطيل التفكير في OpenClaw (بما في ذلك اختيار **None** في الواجهة)،
يرسل OpenClaw إلى DeepSeek `thinking: { type: "disabled" }` ويزيل
`reasoning_content` المعاد تشغيله من السجل الصادر. هذا يُبقي جلسات التفكير المعطّل
على مسار DeepSeek غير المخصص للتفكير.استخدم `deepseek/deepseek-v4-flash` للمسار السريع الافتراضي. استخدم
`deepseek/deepseek-v4-pro` عندما تريد نموذج V4 الأقوى ويمكنك قبول
تكلفة أو زمن استجابة أعلى.

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D8%A7%D9%84%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1-%D8%A7%D9%84%D9%85%D8%A8%D8%A7%D8%B4%D8%B1)  الاختبار المباشر

تتضمن مجموعة النماذج المباشرة الحية نموذج DeepSeek V4 ضمن مجموعة النماذج الحديثة. لتشغيل
فحوصات النماذج المباشرة الخاصة بـ DeepSeek V4 فقط:

```
OPENCLAW_LIVE_PROVIDERS=deepseek \
OPENCLAW_LIVE_MODELS="deepseek/deepseek-v4-flash,deepseek/deepseek-v4-pro" \
pnpm test:live src/agents/models.profiles.live.test.ts
```

يتحقق ذلك الفحص المباشر من أن كلا نموذجي V4 يمكنهما الإكمال، وأن جولات المتابعة الخاصة بالتفكير/الأدوات
تحافظ على حمولة إعادة التشغيل التي يتطلبها DeepSeek.

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D9%85%D8%AB%D8%A7%D9%84-%D8%A5%D8%B9%D8%AF%D8%A7%D8%AF)  مثال إعداد

```
{
  env: { DEEPSEEK_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "deepseek/deepseek-v4-flash" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/ar/providers/deepseek\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

[**اختيار النموذج** \\
\\
اختيار المزوّدين، ومراجع النماذج، وسلوك تجاوز الفشل.](https://docs.openclaw.ai/ar/concepts/model-providers)

[**مرجع الإعدادات** \\
\\
مرجع الإعدادات الكامل للوكلاء والنماذج والمزوّدين.](https://docs.openclaw.ai/ar/gateway/configuration-reference)

[Deepinfra](https://docs.openclaw.ai/ar/providers/deepinfra) [ElevenLabs](https://docs.openclaw.ai/ar/providers/elevenlabs)

Ctrl+I