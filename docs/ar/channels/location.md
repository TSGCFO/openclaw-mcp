---
source_url: https://docs.openclaw.ai/ar/channels/location
title: "\u062a\u062d\u0644\u064a\u0644 \u0645\u0648\u0642\u0639 \u0627\u0644\u0642\u0646\u0627\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/channels/location#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Configuration

تحليل موقع القناة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [تنسيق النص](https://docs.openclaw.ai/ar/channels/location#%D8%AA%D9%86%D8%B3%D9%8A%D9%82-%D8%A7%D9%84%D9%86%D8%B5)
- [حقول السياق](https://docs.openclaw.ai/ar/channels/location#%D8%AD%D9%82%D9%88%D9%84-%D8%A7%D9%84%D8%B3%D9%8A%D8%A7%D9%82)
- [ملاحظات القناة](https://docs.openclaw.ai/ar/channels/location#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9)
- [ذو صلة](https://docs.openclaw.ai/ar/channels/location#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

يقوم OpenClaw بتوحيد المواقع المشتركة الواردة من قنوات الدردشة إلى:

- نص موجز للإحداثيات يُلحَق بنص الرسالة الواردة، و
- حقول منظَّمة في حمولة سياق الرد التلقائي. يتم عرض التسميات والعناوين والتعليقات/الأوصاف التي توفرها القناة داخل المطالبة عبر كتلة JSON مشتركة للبيانات الوصفية غير الموثوقة، وليس بشكل مضمَّن داخل نص المستخدم.

المدعوم حاليًا:

- **Telegram** (دبابيس المواقع \+ الأماكن \+ المواقع المباشرة)
- **WhatsApp** (`locationMessage` \+ `liveLocationMessage`)
- **Matrix** (`m.location` مع `geo_uri`)

## [​](https://docs.openclaw.ai/ar/channels/location\#%D8%AA%D9%86%D8%B3%D9%8A%D9%82-%D8%A7%D9%84%D9%86%D8%B5)  تنسيق النص

تُعرَض المواقع كسطور واضحة من دون أقواس:

- دبوس:
  - `📍 48.858844, 2.294351 ±12m`
- مكان مُسمّى:
  - `📍 48.858844, 2.294351 ±12m`
- مشاركة مباشرة:
  - `🛰 الموقع المباشر: 48.858844, 2.294351 ±12m`

إذا تضمنت القناة تسمية أو عنوانًا أو تعليقًا/وصفًا، فسيتم الاحتفاظ به في حمولة السياق وسيظهر في المطالبة على شكل JSON غير موثوق داخل كتلة مسوَّرة:

````
الموقع (بيانات وصفية غير موثوقة):
```json
{
"latitude": 48.858844,
"longitude": 2.294351,
"name": "Eiffel Tower",
"address": "Champ de Mars, Paris",
"caption": "Meet here"
}
```
````

## [​](https://docs.openclaw.ai/ar/channels/location\#%D8%AD%D9%82%D9%88%D9%84-%D8%A7%D9%84%D8%B3%D9%8A%D8%A7%D9%82)  حقول السياق

عند وجود موقع، تتم إضافة هذه الحقول إلى `ctx`:

- `LocationLat` (رقم)
- `LocationLon` (رقم)
- `LocationAccuracy` (رقم، بالأمتار؛ اختياري)
- `LocationName` (سلسلة نصية؛ اختياري)
- `LocationAddress` (سلسلة نصية؛ اختياري)
- `LocationSource` (`pin | place | live`)
- `LocationIsLive` (قيمة منطقية)
- `LocationCaption` (سلسلة نصية؛ اختياري)

يتعامل عارض المطالبة مع `LocationName` و`LocationAddress` و`LocationCaption` على أنها بيانات وصفية غير موثوقة ويحوّلها إلى JSON عبر نفس المسار المقيّد المستخدم لسياقات القنوات الأخرى.

## [​](https://docs.openclaw.ai/ar/channels/location\#%D9%85%D9%84%D8%A7%D8%AD%D8%B8%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D9%86%D8%A7%D8%A9)  ملاحظات القناة

- **Telegram**: تُربَط الأماكن بالقيمتين `LocationName/LocationAddress`؛ وتستخدم المواقع المباشرة `live_period`.
- **WhatsApp**: تملأ `locationMessage.comment` و`liveLocationMessage.caption` الحقل `LocationCaption`.
- **Matrix**: يتم تحليل `geo_uri` كموقع دبوس؛ ويتم تجاهل الارتفاع وتكون `LocationIsLive` دائمًا false.

## [​](https://docs.openclaw.ai/ar/channels/location\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [أمر الموقع (العُقد)](https://docs.openclaw.ai/ar/nodes/location-command)
- [التقاط الكاميرا](https://docs.openclaw.ai/ar/nodes/camera)
- [فهم الوسائط](https://docs.openclaw.ai/ar/nodes/media-understanding)

[توجيه القنوات](https://docs.openclaw.ai/ar/channels/channel-routing) [استكشاف مشكلات القنوات وإصلاحها](https://docs.openclaw.ai/ar/channels/troubleshooting)

Ctrl+I