# Semaphore
Semaphore is simply a **variable** or **abstract data type** used in multiproccessing. This variable is used to solve critical section problems and to achieve process synchronization in the multi processing environment. A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions.
The semaphore concept was invented by Dutch computer scientist Edsger Dijkstra in 1962 or 1963, when Dijkstra and his team were developing an operating system for the Electrologica X8. That system eventually became known as THE multiprogramming system. 

<div dir="rtl">

# سمافور (نشان بر) چیست؟

متغیر یا مقداری است که در محیط های چند پردازشی یا همروند مورد استفاده قرار می گیرد و کار سمافور این است که به همه پردازش ها مقدار یکسانی را بدهد.
سمافورها اولین بار به‌وسیلهٔ دانشمند علوم رایانه هلندی، ادسخر دیکسترا معرفی شدند. و امروزه به‌طور گسترده‌ای در سیستم عاملها مورد استفاده قرار می‌گیرند.

## شیوهٔ عملکرد سمافور

اصل اساسی این است که دو یا چند فرایند می‌توانند به وسیلهٔ سیگنال‌های ساده با یکدیگر همکاری کنند. هر فرایند را می‌توان در نقطهٔ خاصی از اجرا متوقف نموده، و تا رسیدن سیگنال خاصی از اجرای آن جلوگیری نمود. برای ایجاد این اثر، از متغیرهای خاصی به نام سمافور استفاده می‌گردد.

هر فرایندی که بخواهد به منبع مشترک دسترسی داشته باشد، اعمال زیر را انجام خواهد داد:

مقدار سمافور را بررسی می‌کند.
در صورتی که مقدار سمافور مثبت باشد، فرایند می‌تواند از منبع مشترک استفاده کند. در این صورت، فرایند یک واحد از سمافور می‌کاهد تا نشان دهد که یک واحد از منبع مشترک را استفاده نموده‌است.
در صورتی که مقدار سمافور صفر یا کوچکتر از صفر باشد، فرایند به خواب می‌رود تا زمانی که سمافور مقداری مثبت به خود بگیرد. در این حالت فرایند از خواب بیدار شده و از مرحلهٔ یک شروع می‌کند.
هنگامی که فرایند کار خود را با منبع تمام نمود، یک واحد به سمافور اضافه می‌گردد. هر زمان که مقدار سمافور به صفر یا بیشتر برسد، یکی از فرایند(هایی) که به خواب رفته به صورت تصادفی یا به روش FIFO توسط سیستم‌عامل بیدار می‌شود. در این حالت بلافاصله فرایند بیدار شده منبع را در دست می‌گیرد و مجدداً پس از اتمام کار یک واحد از سمافور کم می‌شود. اگر مقدار سمافوری صفر باشد و چند فرایند بلوکه شده در آن وجود داشته باشد، با افزایش یک واحدی سمافور، مقدار سمافور همچنان صفر باقی می‌ماند اما یکی از فرایندهای بلوکه شده آزاد می‌شود.

## پیاده سازی
ابتدا به این موضوع می پردازیم که سمافور ها چگونه و تحت چه شرایطی کار کی کنند: 
<div dir="ltr">

```
    function V(semaphore S, integer I):
        [S ← S + I]
    function P(semaphore S, integer I):
        repeat:
            [if S>= 0:
                S ← S - I
                break]
```
</div>

- متد wait (P): 
مقدار سمافور را یک واحد کاهش داده و یک واحد از منبع اشتراکی را مصرف می‌کند. اگر در هنگام کاهش، مقدار منفی شد، پروسه‌ای که wait()‎ را اجرا کرده بلوکه می‌شود و در انتهای صف سمافور قرار می‌گیرد تا منابع توسط پروسه‌های دیگر آزاد شوند.
- متد signal (V):
مقدار سمافور را یک واحد افزایش می‌دهد. پس از افزایش دادن، اگر مقدار قبل سمافور منفی باشد (به این معنی که در حال حاضر پروسه‌هایی در صف سمافور منتظر دریافت منبع هستند)، یکی از پروسه‌ها از صف آماده وارد صف اجرا می‌شود و منبع آزاد شده را در اختیار می‌گیرد.

### پازیکس (Posix)
استاندارد پازیکس دسته‌ای تابع برای کار بر روی سمافورها تعریف می‌کند که در سیستم‌عاملهای سازگار با این استاندارد قابل استفاده هستند. برای استفاده از این توابع باید فایل سرایند semaphore.h را در کد منبع درج کرد. در این استاندارد یک نوع داده به نام sem_t تعبیه شده که برای تعریف کردن یک ساختار از نوع سمافور استفاده می‌شود.

به کمک تابع sem_init()‎ می‌توان یک سمافور را آماده‌سازی کرد. قبل از انجام هر کاری، این تابع باید بر روی سمافور اجرا شود. این تابع به شکل زیر اعلان شده است: 
<div dir="ltr">

``` c
    int sem_init(sem_t *sem, int pshared, unsigned int value);
```
</div>

پارامتر sem همان ساختار نوع sem_t است که باید به روش فراخوانی با ارجاع به تابع ارسال شود. پارامتر value مقدار اولیه سمافور را تعیین می‌کند. به عبارت دیگر، این تابع مقدار value به عنوان مقدار اولیه سمافور sem تعیین می‌کند. اگر پارامتر pshared غیر صفر باشد، مشخص‌کننده سمافور مشترکی است که می‌تواند توسط چند فرایند مورد استفاده قرار بگیرد. به عبارت دیگر، پارامتر pshared تعیین می‌کند که آیا سمافور قرار است بین ریسه‌های یک فرایند به اشتراک گذاشته شود یا بین چند فرایند مجزا. برای اشتراک گذاشتن یک سمافور بین چند فرایند، سمافور باید در یک حافظه مشترک قرار گیرد تا همه فرایندها بتوانند به آن دسترسی داشته باشند. هر پروسه‌ای که به آدرس sem دسترسی داشته باشد، می‌تواند بر روی سمافور عملیات انجام دهد. بعد از اینکه این تابع بر روی یک سمافور با موفقیت اجرا شد، می‌تواند از آن سمافور در توابع دیگر استفاده کرد. این تابع در صورت موفقیت مقدار صفر و در صورت شکست مقدار ‎-1 را برمی‌گرداند و متغیر سراسری errno را با خطای مورد نظر مقداردهی می‌کند.
<div dir="ltr">

``` c
    int sem_getvalue(sem_t * restrict sem, int * restrict sval);
```
</div>

این تابع مقدار فعلی سمافور sem را در متغیر sval قرار می‌دهد. در صورت موفقیت مقدار صفر و در صورت شکست مقدار -۱‎ برمی‌گردد.
<div dir="ltr">

``` c
    int sem_wait(sem_t *sem);
```
</div>

این تابع یک واحد از سمافور sem کم می‌کند، به این معنی که قرار است یک واحد از منبع اشتراکی استفاده شود. فرایند باید قبل از استفاده از منبع اشتراکی این تابع را بر روی سمافور اجرا کند. اگر مقدار فعلی سمافور صفر باشد (به این معنی که هم‌اکنون منبع اشتراکی در اختیار فرایند دیگری است)، فرایند فراخوان بلوکه می‌شود، تا وقتی که فرایند دیگر منبع اشتراکی را آزاد کند.
<div dir="ltr">

``` c
    int sem_trywait(sem_t *sem);
```
</div>

این تابع هم مشابه sem_wait است. اما اگر مقدار سمافور صفر باشد، پروسه بلوکه نخواهد شد و در عوض یک خطا برخواهد گشت.
<div dir="ltr">

``` c
    int sem_post(sem_t *sem);
```
</div>

این تابع سمافور sem را یک واحد افزایش می‌دهد. پس از اینکه فرایندها کار خود را با منبع اشتراکی به اتمام رساندند، باید این تابع را بر روی سمافور فراخوانی کنند تا فرایندهای دیگر بتوانند از منبع استفاده کنند. اگر در حال حاضر فرایند(هایی) بر روی سمافور بلوکه شده باشد، فرایندی که اولویت بالاتری دارد بیدار شده و منبع را در دست خواهد گرفت. در صورت موفقیت مقدار صفر را برمی‌گرداند.
<div dir="ltr">

``` c
    int sem_destroy(sem_t *sem);
```
</div>

این تابع سمافور sem را نابود می‌کند و منابع آن را به سیستم برمی‌گرداند. پس از اینکه این تابع بر روی یک سمافور با موفقیت اجرا شد، سمافور مورد نظر دیگر قابل استفاده نیست و هیچ تابعی نباید بر روی آن منتظر دستیابی به منبع اشتراکی باشد. مگر اینکه بار دیگر هم تابع sem_init روی سمافور اجرا شود.


برای درک بهتر مفاهیم بالا بهتر است به مثال زیر توجه کنید :

## مثال ۱ :
یک کتابخانه دانشگاه را فرض بگیرید که در آن اتاق هایی برای مطالعه دانشجو ها وجود دارد تعداد این اتاق ها ۳۰ عدد است و تعداد دانشجویان موجود ۱۰۰ نفر حال ما به دانشجویان این اتاق ها را می دهیم و ۳۰ نفر اول از طریق پیشخوان یا پذیرش کتابخانه اتاق هارا دریافت می کند پیشخوان یا پذیرش اتاق های درحال استفاده را کم می کند از مقداراتاق های موجود حال اگر کار ۵ دانشجو با اتاق ها تمام شود به پذیرش اعلام می کند و پذیرش ۵ نفر بعدی را به اتاق ها هدایت می کند البته لازم به ذکر است که الویت با کسی است که زود تر به اتاق های مورد نظر آمده است.

<div dir="ltr">

[Source Code Example1.][Github Example1]

</div>

برای درک بهتر سورس کد ۱ بهتر است به sleep های سورس کد توجه کنید.

[Github Example1]: https://github.com/ariakh55/Semaphore/blob/master/Example01.c

>آریا خوشنود - امید رضا کشتکار.
</div>