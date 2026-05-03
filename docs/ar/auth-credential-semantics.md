---
source_url: https://docs.openclaw.ai/ar/auth-credential-semantics
title: "\u062f\u0644\u0627\u0644\u0627\u062a \u0628\u064a\u0627\u0646\u0627\u062a \u0627\u0639\u062a\u0645\u0627\u062f \u0627\u0644\u0645\u0635\u0627\u062f\u0642\u0629 - OpenClaw"
---

[الانتقال إلى المحتوى الرئيسي](https://docs.openclaw.ai/ar/auth-credential-semantics#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/ar)

![SA](https://d3gk2c5xim1je2.cloudfront.net/flags/SA.svg)

العربية

...ابحث

Ctrl K

...ابحث

Navigation

Authentication and secrets

دلالات بيانات اعتماد المصادقة

[Get started](https://docs.openclaw.ai/ar) [Install](https://docs.openclaw.ai/ar/install) [Channels](https://docs.openclaw.ai/ar/channels) [Agents](https://docs.openclaw.ai/ar/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/ar/tools) [Models](https://docs.openclaw.ai/ar/providers) [Platforms](https://docs.openclaw.ai/ar/platforms) [Gateway & Ops](https://docs.openclaw.ai/ar/gateway) [Reference](https://docs.openclaw.ai/ar/cli) [Help](https://docs.openclaw.ai/ar/help)

في هذه الصفحة

- [رموز أسباب الفحص المستقرة](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%B1%D9%85%D9%88%D8%B2-%D8%A3%D8%B3%D8%A8%D8%A7%D8%A8-%D8%A7%D9%84%D9%81%D8%AD%D8%B5-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%82%D8%B1%D8%A9)
- [بيانات اعتماد الرمز المميز](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF-%D8%A7%D9%84%D8%B1%D9%85%D8%B2-%D8%A7%D9%84%D9%85%D9%85%D9%8A%D8%B2)
- [قواعد الأهلية](https://docs.openclaw.ai/ar/auth-credential-semantics#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%A3%D9%87%D9%84%D9%8A%D8%A9)
- [قواعد الحلّ](https://docs.openclaw.ai/ar/auth-credential-semantics#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%AD%D9%84%D9%91)
- [قابلية نقل نسخ الوكلاء](https://docs.openclaw.ai/ar/auth-credential-semantics#%D9%82%D8%A7%D8%A8%D9%84%D9%8A%D8%A9-%D9%86%D9%82%D9%84-%D9%86%D8%B3%D8%AE-%D8%A7%D9%84%D9%88%D9%83%D9%84%D8%A7%D8%A1)
- [تصفية ترتيب المصادقة الصريح](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%AA%D8%B5%D9%81%D9%8A%D8%A9-%D8%AA%D8%B1%D8%AA%D9%8A%D8%A8-%D8%A7%D9%84%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9-%D8%A7%D9%84%D8%B5%D8%B1%D9%8A%D8%AD)
- [حل هدف الفحص](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%AD%D9%84-%D9%87%D8%AF%D9%81-%D8%A7%D9%84%D9%81%D8%AD%D8%B5)
- [اكتشاف بيانات اعتماد CLI الخارجية](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%A7%D9%83%D8%AA%D8%B4%D8%A7%D9%81-%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF-cli-%D8%A7%D9%84%D8%AE%D8%A7%D8%B1%D8%AC%D9%8A%D8%A9)
- [حارس سياسة SecretRef في OAuth](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%AD%D8%A7%D8%B1%D8%B3-%D8%B3%D9%8A%D8%A7%D8%B3%D8%A9-secretref-%D9%81%D9%8A-oauth)
- [رسائل متوافقة مع الإصدارات القديمة](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D9%85%D8%AA%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D9%85%D8%B9-%D8%A7%D9%84%D8%A5%D8%B5%D8%AF%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D8%AF%D9%8A%D9%85%D8%A9)
- [ذو صلة](https://docs.openclaw.ai/ar/auth-credential-semantics#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

تحدد هذه الوثيقة دلالات أهلية بيانات الاعتماد وحلّها المعتمدة المستخدمة عبر:

- `resolveAuthProfileOrder`
- `resolveApiKeyForProfile`
- `models status --probe`
- `doctor-auth`

الهدف هو إبقاء سلوك وقت الاختيار وسلوك وقت التشغيل متوافقين.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%B1%D9%85%D9%88%D8%B2-%D8%A3%D8%B3%D8%A8%D8%A7%D8%A8-%D8%A7%D9%84%D9%81%D8%AD%D8%B5-%D8%A7%D9%84%D9%85%D8%B3%D8%AA%D9%82%D8%B1%D8%A9)  رموز أسباب الفحص المستقرة

- `ok`
- `excluded_by_auth_order`
- `missing_credential`
- `invalid_expires`
- `expired`
- `unresolved_ref`
- `no_model`

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF-%D8%A7%D9%84%D8%B1%D9%85%D8%B2-%D8%A7%D9%84%D9%85%D9%85%D9%8A%D8%B2)  بيانات اعتماد الرمز المميز

تدعم بيانات اعتماد الرمز المميز (`type: "token"`) القيمة المضمنة `token` و/أو `tokenRef`.

### [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%A3%D9%87%D9%84%D9%8A%D8%A9)  قواعد الأهلية

1. يكون ملف تعريف الرمز المميز غير مؤهل عندما يكون كل من `token` و`tokenRef` غائبين.
2. `expires` اختياري.
3. إذا كان `expires` موجودًا، فيجب أن يكون رقمًا منتهيًا أكبر من `0`.
4. إذا كان `expires` غير صالح (`NaN`، أو `0`، أو قيمة سالبة، أو غير منتهية، أو من نوع خاطئ)، يكون ملف التعريف غير مؤهل مع `invalid_expires`.
5. إذا كان `expires` في الماضي، يكون ملف التعريف غير مؤهل مع `expired`.
6. لا يتجاوز `tokenRef` التحقق من صحة `expires`.

### [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D9%82%D9%88%D8%A7%D8%B9%D8%AF-%D8%A7%D9%84%D8%AD%D9%84%D9%91)  قواعد الحلّ

1. تطابق دلالات أداة الحل دلالات الأهلية لـ `expires`.
2. بالنسبة إلى ملفات التعريف المؤهلة، يمكن حل مادة الرمز المميز من القيمة المضمنة أو من `tokenRef`.
3. تنتج المراجع التي يتعذر حلها `unresolved_ref` في مخرجات `models status --probe`.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D9%82%D8%A7%D8%A8%D9%84%D9%8A%D8%A9-%D9%86%D9%82%D9%84-%D9%86%D8%B3%D8%AE-%D8%A7%D9%84%D9%88%D9%83%D9%84%D8%A7%D8%A1)  قابلية نقل نسخ الوكلاء

وراثة مصادقة الوكيل تتم بالقراءة عبر المصدر. عندما لا يملك الوكيل ملف تعريف محليًا، يمكنه حل ملفات التعريف من مخزن الوكيل الافتراضي/الرئيسي في وقت التشغيل دون نسخ المادة السرية إلى ملف `auth-profiles.json` الخاص به.تستخدم تدفقات النسخ الصريحة، مثل `openclaw agents add`، سياسة قابلية النقل هذه:

- ملفات تعريف `api_key` قابلة للنقل ما لم يكن `copyToAgents: false`.
- ملفات تعريف `token` قابلة للنقل ما لم يكن `copyToAgents: false`.
- ملفات تعريف `oauth` غير قابلة للنقل افتراضيًا لأن رموز التحديث قد تكون أحادية الاستخدام أو حساسة للدوران.
- يمكن لتدفقات OAuth المملوكة للمزوّد الاشتراك باستخدام `copyToAgents: true` فقط عندما تكون سلامة نسخ مادة التحديث بين الوكلاء معروفة.

تبقى ملفات التعريف غير القابلة للنقل متاحة عبر وراثة القراءة عبر المصدر ما لم يسجل الوكيل الهدف الدخول بشكل منفصل وينشئ ملف تعريف محليًا خاصًا به.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%AA%D8%B5%D9%81%D9%8A%D8%A9-%D8%AA%D8%B1%D8%AA%D9%8A%D8%A8-%D8%A7%D9%84%D9%85%D8%B5%D8%A7%D8%AF%D9%82%D8%A9-%D8%A7%D9%84%D8%B5%D8%B1%D9%8A%D8%AD)  تصفية ترتيب المصادقة الصريح

- عند تعيين `auth.order.<provider>` أو تجاوز ترتيب مخزن المصادقة لمزوّد، لا يفحص `models status --probe` إلا معرّفات ملفات التعريف التي تبقى ضمن ترتيب المصادقة المحلول لذلك المزوّد.
- لا تتم تجربة ملف تعريف مخزن لذلك المزوّد محذوف من الترتيب الصريح بصمت لاحقًا. تعرض مخرجات الفحص ذلك باستخدام `reasonCode: excluded_by_auth_order` والتفصيل `Excluded by auth.order for this provider.`

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%AD%D9%84-%D9%87%D8%AF%D9%81-%D8%A7%D9%84%D9%81%D8%AD%D8%B5)  حل هدف الفحص

- يمكن أن تأتي أهداف الفحص من ملفات تعريف المصادقة أو بيانات اعتماد البيئة أو `models.json`.
- إذا كان لدى مزوّد بيانات اعتماد لكن OpenClaw لا يستطيع حل مرشح نموذج قابل للفحص له، فإن `models status --probe` يعرض `status: no_model` مع `reasonCode: no_model`.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%A7%D9%83%D8%AA%D8%B4%D8%A7%D9%81-%D8%A8%D9%8A%D8%A7%D9%86%D8%A7%D8%AA-%D8%A7%D8%B9%D8%AA%D9%85%D8%A7%D8%AF-cli-%D8%A7%D9%84%D8%AE%D8%A7%D8%B1%D8%AC%D9%8A%D8%A9)  اكتشاف بيانات اعتماد CLI الخارجية

- لا تُكتشف بيانات الاعتماد الخاصة بوقت التشغيل فقط والمملوكة لواجهات CLI الخارجية إلا عندما يكون المزوّد أو وقت التشغيل أو ملف تعريف المصادقة ضمن نطاق العملية الحالية، أو عندما يكون ملف تعريف محلي مخزن لذلك المصدر الخارجي موجودًا بالفعل.
- ينبغي لمستدعي مخزن المصادقة اختيار وضع اكتشاف صريح لـ CLI الخارجية: `none` للمصادقة المستمرة/مصادقة Plugin فقط، أو `existing` لتحديث ملفات تعريف CLI الخارجية المخزنة مسبقًا، أو `scoped` لمجموعة محددة من المزوّدين/ملفات التعريف.
- تمرر مسارات القراءة فقط/الحالة `allowKeychainPrompt: false`؛ فهي تستخدم بيانات اعتماد CLI الخارجية المدعومة بالملفات فقط ولا تقرأ نتائج macOS Keychain أو تعيد استخدامها.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%AD%D8%A7%D8%B1%D8%B3-%D8%B3%D9%8A%D8%A7%D8%B3%D8%A9-secretref-%D9%81%D9%8A-oauth)  حارس سياسة SecretRef في OAuth

- مُدخل SecretRef مخصص لبيانات الاعتماد الثابتة فقط.
- إذا كانت بيانات اعتماد ملف التعريف هي `type: "oauth"`، فلا تكون كائنات SecretRef مدعومة لمادة بيانات اعتماد ملف التعريف تلك.
- إذا كان `auth.profiles.<id>.mode` هو `"oauth"`، فيُرفض مُدخل `keyRef`/`tokenRef` المدعوم بـ SecretRef لذلك الملف التعريفي.
- تُعد الانتهاكات حالات فشل صارمة في مسارات حل مصادقة بدء التشغيل/إعادة التحميل.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%B1%D8%B3%D8%A7%D8%A6%D9%84-%D9%85%D8%AA%D9%88%D8%A7%D9%81%D9%82%D8%A9-%D9%85%D8%B9-%D8%A7%D9%84%D8%A5%D8%B5%D8%AF%D8%A7%D8%B1%D8%A7%D8%AA-%D8%A7%D9%84%D9%82%D8%AF%D9%8A%D9%85%D8%A9)  رسائل متوافقة مع الإصدارات القديمة

للتوافق مع السكربتات، تُبقي أخطاء الفحص هذا السطر الأول دون تغيير:`Auth profile credentials are missing or expired.`يمكن إضافة تفاصيل ملائمة للبشر ورموز أسباب مستقرة في الأسطر اللاحقة.

## [​](https://docs.openclaw.ai/ar/auth-credential-semantics\#%D8%B0%D9%88-%D8%B5%D9%84%D8%A9)  ذو صلة

- [إدارة الأسرار](https://docs.openclaw.ai/ar/gateway/secrets)
- [تخزين المصادقة](https://docs.openclaw.ai/ar/concepts/oauth)

[المصادقة](https://docs.openclaw.ai/ar/gateway/authentication) [Secrets management](https://docs.openclaw.ai/ar/gateway/secrets)

Ctrl+I