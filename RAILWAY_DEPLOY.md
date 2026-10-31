# نشر Music_Bexo على Railway

## إعداد الخدمة
1. ارفع محتويات هذا المجلد إلى مستودع GitHub خاص.
2. في Railway اختر **New Project → Deploy from GitHub Repo**.
3. اختر المستودع؛ سيكتشف Railway ملف `Dockerfile` ويبني الحاوية.
4. اجعل الخدمة Worker تعمل بأمر `python main.py` (وهو أمر Dockerfile الافتراضي). لا تضف Healthcheck لأن الخدمة تستخدم Telegram polling ولا تفتح HTTP port.

## المتغيرات (Service → Variables)
- `BOT_TOKEN`: توكن البوت من BotFather.
- `OWNER_ID`: رقم حساب المطور الرقمي.
- `CHANNEL_USERNAME`: أسماء القنوات مفصولة بفواصل، مثل `BEXO50,xppx56`.
- `MAX_FILE_SIZE_MB`: اتركه `20` مع Telegram API الرسمي، أو احذف المتغير ليُستخدم الافتراضي الحالي في الكود.
- `LOG_LEVEL`: `INFO`.
- `DATA_DIR`: `/data` (اختياري؛ هذا هو الافتراضي).

## إعداد التخزين الدائم
1. افتح الخدمة في Railway ثم **Volumes → Add Volume**.
2. اجعل Mount Path هو `/data`.
3. أعد النشر بعد إضافة الـ Volume.

قاعدة البيانات `bot_stats.db` وإعدادات الاشتراك الإجباري وذاكرة غلاف القناة تحفظ في `/data`. إذا كانت قاعدة البيانات القديمة موجودة في المشروع، تُنسخ تلقائياً إلى الـ Volume في أول تشغيل فقط.

## تنبيهات مهمة
- لا ترفع `.env` إلى GitHub ولا ترسل `BOT_TOKEN` لأحد. أضف التوكن من Variables فقط.
- البوت يعمل بـ long polling؛ لا تشغّل أكثر من نسخة Worker بالتوكن نفسه حتى لا يحدث تعارض في استقبال التحديثات.
- Telegram Bot API الرسمي يفرض حدوداً على تنزيل الملفات (عادةً 20MB عبر `getFile`)؛ لذلك لا تتوقع دعم تنزيل الملفات الكبيرة كما في Local Bot API Server.
- الملفات المؤقتة داخل `/app` ليست دائمة، وهذا مقصود؛ أما البيانات المهمة فداخل Volume.
