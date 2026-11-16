# NutriSnap

مشروع جامعي عربي للتعرّف على الطعام من الصورة وعرض معلومات غذائية مبسطة. الواجهة React وTypeScript، والخادم المحلي Node.js وExpress. يستخدم الخادم OpenAI للتعرّف البصري وUSDA FoodData Central للقيم الغذائية.

## المتطلبات

- Node.js 18 أو أحدث.
- مفتاح OpenAI API شخصي للتحليل الفعلي. لا ترسل المفتاح في المحادثة ولا تضعه في ملفات الواجهة.
- مفتاح USDA FoodData Central اختياري؛ يعمل المثال افتراضيًا باستخدام `DEMO_KEY` ذي حدود الاستخدام الأقل.

## التشغيل على Windows PowerShell

1. فك ضغط مجلد المشروع كاملًا، ثم افتح PowerShell وانتقل إلى المجلد الذي يحتوي مباشرة على `package.json`، مثل:

   ```powershell
   cd D:\NutriSnap
   Get-ChildItem package.json
   ```

   إذا لم يظهر `package.json`، انتقل إلى المجلد الداخلي الذي يحتوي ملفات المشروع أو أعد فك الضغط؛ يجب تنفيذ الأوامر من هذا المجلد.

2. ثبّت المكتبات وأنشئ ملف إعداد محلي:

   ```powershell
   npm install
   Copy-Item .env.example .env
   notepad .env
   ```

3. في `.env`، ضع مفتاحك الشخصي بعد `OPENAI_API_KEY=`. اترك `FDC_API_KEY=` فارغًا لاستخدام مفتاح USDA التجريبي، أو ضع مفتاح FoodData Central الخاص بك. احفظ الملف وأغلق المفكرة. مثال شكلي فقط:

   ```dotenv
   OPENAI_API_KEY=ضع_مفتاحك_هنا
   OPENAI_MODEL=gpt-6-luna
   FDC_API_KEY=
   PORT=4000
   ```

   لا تستخدم النص الشكلي كما هو؛ استبدله بمفتاحك محليًا. ملف `.env` مستثنى من Git ومن حزمة ZIP.

4. قبل التشغيل، أوقف أي تشغيل قديم لـ `npm run dev` أو `npm run dev:all` في نوافذ الطرفية الأخرى بالضغط على `Ctrl+C`؛ لا تشغّل نسختين على المنافذ نفسها.

5. شغّل الواجهة والخادم معًا بأمر واحد:

   ```powershell
   npm run dev:all
   ```

6. افتح **http://localhost:3000**. الواجهة على المنفذ 3000، وخادم Express على المنفذ 4000؛ يمر طلب الواجهة إلى `/api/analyze` عبر Vite تلقائيًا. اترك نافذة PowerShell مفتوحة أثناء الاستخدام، وأوقف الطرفين بـ `Ctrl+C`.

## ما الذي يحدث عند التحليل؟

ترسل الواجهة الصورة كـ `multipart/form-data` في الحقل `image` إلى `POST /api/analyze`. يتحقق الخادم من صيغة JPG أو PNG أو WEBP ومن حدّ 10 ميغابايت، ثم يرسلها إلى OpenAI للتعرّف على الطعام، ويبحث عن أقرب سجل غذائي في USDA. لا تُحفظ الصورة على الخادم. تُعرض القيم المتاحة لكل 100 غرام مع مصدرها، ولا تُختلق قيم عند تعذر العثور على سجل.

لا يمكن تأكيد وجود مسببات الحساسية أو غيابها من الصورة؛ راجع دائمًا قائمة المكونات والملصق الغذائي. النتائج للتثقيف وليست نصيحة طبية، وقد تختلف عن الطعام الفعلي وطريقة تحضيره.

## التحقق من إعداد الخادم

بعد تشغيل الطرفين، افتح `http://localhost:3000/api/health`. يعرض المسار حالة إعداد مفاتيح الخدمات دون كشف قيمها. إذا كان `openaiConfigured` يساوي `false`، فتحقق من أن المفتاح أُضيف إلى `.env` ثم أوقف التشغيل وأعد `npm run dev:all`.

## أوامر التطوير

```powershell
npm run typecheck
npm run build
npm run preview
```

## مصادر الخدمات

- [OpenAI — إدخال الصور والرؤية](https://platform.openai.com/docs/guides/images-vision)
- [OpenAI — Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
- [GPT-6 Luna](https://developers.openai.com/api/docs/models/gpt-6-luna)
- [USDA FoodData Central API](https://fdc.nal.usda.gov/api-guide)
