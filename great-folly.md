<details>
<summary>DeepSeek Persian translation</summary>
<td class="d-block color-fg-default comment-body markdown-body js-comment-body">
        <h3 dir="auto">فارسی | <a href="https://github.com/XTLS/Xray-core/discussions/4113#discussion-7614138" data-hovercard-type="discussion" data-hovercard-url="/XTLS/Xray-core/discussions/4113/hovercard?comment_id=11468947" aria-keyshortcuts="Alt+ArrowUp">中文</a></h3>
<h1 dir="auto">XHTTP: فراتر از REALITY</h1>
<p dir="auto">خلاصه توسعه: در اواسط سال 2024، <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/mmmray/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/mmmray" aria-keyshortcuts="Alt+ArrowUp">@mmmray</a> <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/ll11l1lIllIl1lll/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/ll11l1lIllIl1lll" aria-keyshortcuts="Alt+ArrowUp">@ll11l1lIllIl1lll</a> و دیگران بر اساس اصل "ارسال بسته‌ای بالا، جریان پایین" که توسط <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/RPRX/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/RPRX" aria-keyshortcuts="Alt+ArrowUp">@RPRX</a> توضیح داده شده بود، SplitHTTP را توسعه دادند، <strong>که برای اولین بار توانست بدون قربانی کردن کارایی دانلود، از اکثر میانی‌باکس‌های HTTP عبور کند و اولین پیاده‌سازی در مقیاس بزرگ برای QUIC H3 از طریق CDN بود، که عصر جدیدی را آغاز کرد</strong>، پشتیبانی از مرورگر برای Browser Dialer، کاهش ویژگی‌های header padding، XMUX برای کنترل چندگانگی و فعال‌سازی REALITY نیز به ترتیب اضافه شدند. سپس <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/RPRX/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/RPRX" aria-keyshortcuts="Alt+ArrowUp">@RPRX</a> توسعه را بر عهده گرفت، <strong>جداسازی واقعی ارسال و دریافت را پیاده‌سازی کرد و نام آن را به XHTTP تغییر داد، به عنوان مثال ارسال و دریافت می‌توانند به ترتیب IPv6 CDN H3 و IPv4 REALITY H2 باشند (حتی IP مبدا می‌تواند متفاوت باشد)، <del>این بار نیز عصر جدیدی آغاز شد</del></strong>، سپس حالت stream-up که کارایی آپلود را قربانی نمی‌کند، طرح extra که می‌تواند تمام جزئیات پیکربندی را به اشتراک بگذارد، و伪装 هدر gRPC به حالت stream-up اضافه شد، <strong>که امکان عبور H2 stream-up از CDN را فراهم کرد و جایگزین لایه انتقال سنتی gRPC شد، <del>اگر از ابتدا می‌دانستیم این امکان وجود دارد، شاید اصلاً gRPC ایجاد نمی‌شد</del></strong>، در نهایت لایه انتقال HTTP به عنوان حالت stream-one در XHTTP ادغام شد و ویژگی‌هایی مانند header padding، XMUX،伪装 هدر gRPC و غیره را نیز به دست آورد. <strong>در این نقطه، ما XHTTP کامل را داریم: عبور از میانی‌باکس‌ها به روش‌های مختلف، جداسازی ارسال و دریافت، XMUX روان و هر چیزی که نیاز دارید، عصر همه‌کاره بودن XHTTP رسماً آغاز شده است.</strong></p>
<p dir="auto">در ابتدا قرار بود یک مستند ویرایش شده باشد، اما ناخواسته به یک مقاله تبدیل شد. این مقاله به شما کمک می‌کند تا اصول و طراحی XHTTP را به طور عمیق درک کنید و از آن بهتر استفاده نمایید.</p>
<h2 dir="auto">اگر می‌خواهید به پروژه Xray کمک مالی کنید یا NFT جمع‌آوری کنید، توضیحات مربوطه در انتهای مقاله آمده است. از حمایت شما متشکریم!</h2>
<h2 dir="auto">راهنمای سریع شروع</h2>
<p dir="auto">با وجود پارامترهای نسبتاً زیاد XHTTP، همه آنها مقادیر پیش‌فرض مناسب دارند. اگر فقط می‌خواهید از XHTTP استفاده کنید، کافیست این شش مرحله را دنبال کنید:</p>
<ol dir="auto">
<li>چه TLS و چه REALITY، <strong>به طور کلی پیکربندی XHTTP فقط نیاز به پر کردن <code class="notranslate">path</code> دارد و بقیه را می‌توان خالی گذاشت</strong></li>
<li>اگر سرور از QUIC H3 پشتیبانی می‌کند، <strong>کافیست در سمت کلاینت <code class="notranslate">alpn</code> را روی "h3" تنظیم کنید تا از QUIC استفاده شود</strong></li>
<li>برای انتخاب IP بهینه CDN، در کلاینت <code class="notranslate">address</code> را با IP و <code class="notranslate">serverName</code> (SNI) را با دامنه پر کنید</li>
<li>اگر به CF متصل نمی‌شوید، <strong>پشتیبانی gRPC را در پنل CF فعال کنید</strong></li>
<li>اگر از Nginx عبور نمی‌کند، <strong>proxy_pass را در Nginx به grpc_pass تغییر دهید</strong></li>
<li>اگر از سایر CDNها یا نرم‌افزارهای معکوس‌پروکسی عبور نمی‌کند، پیشنهاد می‌شود <code class="notranslate">mode</code> را روی "packet-up" تنظیم کنید که بیشترین سازگاری را دارد</li>
</ol>
<p dir="auto"><strong>XHTTP به طور پیش‌فرض از چندگانگی (multiplexing) پشتیبانی می‌کند. تاخیر آن کمتر از Vision است اما در تست سرعت چندنخی عملکرد کمتری دارد، مگر اینکه قبل از تست <code class="notranslate">"maxConcurrency": 1</code> تنظیم شده باشد. به بخش XMUX مراجعه کنید.</strong></p>
<p dir="auto">CF اتصالات HTTP بدون داده واقعی را پس از 100 ثانیه قطع می‌کند. برای اتصالات پروکسی طولانی‌مدت، باید در سطح برنامه زنده‌نگهدارنده (keep-alive) تنظیم شود، مثلاً در sshd باید <code class="notranslate">ClientAliveInterval</code> تنظیم شود.</p>
<p dir="auto">این مقاله می‌تواند به عنوان مستند پیشرفته استفاده شود. تقریباً تمام محتوایی که می‌خواهید در مورد XHTTP بدانید را پوشش می‌دهد و هر پارامتر را توضیح می‌دهد. <strong>در انتهای مقاله نیز یک نمونه پیکربندی شامل تمام پارامترها وجود دارد که موارد استفاده هر پارامتر را مشخص کرده است. اگر در مورد پارامتری سوال دارید، می‌توانید نام آن را در متن مقاله جستجو کنید تا توضیحات مفصلی پیدا کنید.</strong></p>
<h2 dir="auto">دو منطق بنیادی</h2>
<p dir="auto">همانطور که <a href="https://github.com/XTLS/REALITY">REALITY</a> می‌تواند به عنوان وبسایت دیگری伪装 شود، <strong>منطق اصلی ضد-سانسور افزایش "خسارت جانبی" هنگام اجرای سانسور توسط سانسورچی است، تا آنها جرات اقدام به مسدودسازی نداشته باشند</strong>. من سال‌ها پیش نیز به همین دلیل از TLS و مخلوط کردن ویژگی‌های زمانی و طولی ترافیک روی TLS حمایت کردم. تجربه سال‌ها به ما نشان داده که GFW کل IPهای CDNهای بزرگ را به طور دائمی مسدود نمی‌کند، زیرا این کار بسیاری از وبسایت‌های معمولی را نیز تحت تاثیر قرار می‌دهد. <strong>بنابراین برای XHTTP، هدف اولیه ما این بود که آن را پشت انواع مختلف CDNها پنهان کنیم</strong>. با این حال، CDNها برای محافظت از سرورهای مبدا در برابر حملات، به جز موارد خاص مانند WS و gRPC، معمولاً کل درخواست HTTP را ذخیره کرده و سپس به سرور مبدا ارسال می‌کنند. بسیاری از میانی‌باکس‌های HTTP نیز به همین صورت رفتار می‌کنند. بر این اساس، پروتکل Meek در Tor ترافیک رفت و برگشت را به صورت درخواست‌های HTTP جداگانه بسته‌بندی می‌کند تا از این میانی‌باکس‌ها عبور کند، اما سرعت آن بسیار پایین است، زیرا از "جریان پایین" XHTTP استفاده نمی‌کند. <strong>"جریان پایین" چیست؟ تصور کنید در حال دانلود یک فایل بزرگ از یک وبسایت هستید. CDN که کش آن را ندارد، به سرور مبدا مراجعه می‌کند. اما واضح است که CDN نمی‌تواند مانند حالت آپلود، کل فایل را ذخیره کند و شما را معطل نگه دارد، بلکه به محض دریافت داده از سرور مبدا، آن را بلافاصله به شما ارسال می‌کند. این اساس "جریان پایین" XHTTP است و همچنین تضمین می‌کند که مهم‌ترین بخش یعنی سرعت دانلود می‌تواند به حداکثر برسد</strong>. در مورد آپلود، به دلایل سازگاری، XHTTP ابتدا "ارسال بسته‌ای" را پیاده‌سازی کرد، یعنی ترافیک آپلود را به صورت درخواست‌های POST جداگانه بسته‌بندی می‌کند. واضح است که کارایی آن کاهش می‌یابد، اما خوشبختانه برای نیازهای معمول پروکسی، حجم ترافیک آپلود بسیار کم است. <strong>پس از آن، من حالت "جریان بالا" را اضافه کردم و متوجه شدیم که با افزودن伪装 هدر gRPC، حتی می‌توان H2 جریان بالا را از CF عبور داد، در حالی که پس از چندین بهینه‌سازی روی "ارسال بسته‌ای"، سرعت آن حتی به حالت "جریان بالا" نزدیک شد.</strong></p>
<p dir="auto">در اینجا بد نیست در مورد موضوع اخیراً داغ شده "سوءاستفاده از CDN" نیز صحبت کنیم: واضح است که CDNهایی که از جریان بالا پشتیبانی نمی‌کنند، به طور پیش‌فرض برای استفاده به عنوان سرویس پروکسی طراحی نشده‌اند. اما برای مقابله با GFW، کاوش، توسعه و استفاده از هر راه جدیدی که ممکن است، معقول و ضروری است. این یک اقدام ناگزیر است و افزایش "خسارت جانبی" برای سانسورچی مستلزم این است که ما خود را با سرویس‌های "عادی" مخلوط کنیم، که این نیز اجتناب‌ناپذیر است. <strong>ساده‌ترین مثال: اگر روزی لیست سفید IP اجرا شود و IPهای CDN در آن قرار داشته باشند، آیا از آنها استفاده می‌کنید یا نه؟ برخی قوانین دنیای واقعی در حوزه ضد-سانسور کاربرد ندارند، اما متاسفانه برخی هنوز این موضوع ساده را درک نکرده‌اند.</strong> اگر هنوز متوجه نشده‌اید، می‌توانید مانند لایه انتقال WebSocket که اکنون فقط برای استفاده از CDN وجود دارد، به عنوان توسعه‌دهنده آن را حذف کنید، یا به عنوان کاربر به توسعه‌دهنده پیشنهاد حذف آن را بدهید، تا با اقدام عملی ثابت کنید که "سوءاستفاده از CDN" فقط یک بهانه دیگر برای حملات روزانه به Xray به دلیل برخی انگیزه‌های بیمارگونه نیست، <del>در حالی که در عمل برعکس عمل می‌کنند</del>، و از این طریق می‌توان به راحتی ذات ریاکارانه برخی افراد را مشاهده کرد.</p>
<h2 dir="auto">PACKET-UP</h2>
<p dir="auto"><strong>این بخش دارای جزئیات فنی زیادی است. اگر توسعه‌دهنده نیستید، فقط قسمت‌های پررنگ را بخوانید.</strong></p>
<p dir="auto"><strong>برای حالت "ارسال بسته‌ای بالا، جریان پایین" که بیشترین سازگاری را در XHTTP دارد، یعنی حالت packet-up، طراحی ما به این صورت است:</strong></p>
<ol dir="auto">
<li>
<p dir="auto"><strong>کلاینت <code class="notranslate">POST /yourpath/sameUUID/seq</code> را برای ارسال داده‌های آپلود استفاده می‌کند:</strong></p>
<ul dir="auto">
<li>UUID به صورت تصادفی تولید می‌شود و باید با UUID مورد استفاده برای شروع دانلود یکسان باشد تا سرور بتواند آنها را مرتبط کند. اگر سرور نتواند در 30 ثانیه آنها را مرتبط کند، session قطع می‌شود</li>
<li>seq از 0 شروع می‌شود و باید پس از ارسال body درخواست POST قبلی (بدون نیاز به دریافت پاسخ) درخواست بعدی ارسال شود</li>
<li>درخواست‌های POST با احتمال کمی ممکن است به ترتیب به سرور نرسند. سرور بر اساس seq آنها را مرتب می‌کند و به طور پیش‌فرض حداکثر 30 درخواست را ذخیره می‌کند. در صورت превыن این حد، اتصال قطع می‌شود</li>
<li>توجه کنید که UUID و seq هر دو در مسیر (path) طراحی شده‌اند نه query string، تا از مشکلات عجیب جلوگیری شود</li>
</ul>
</li>
<li>
<p dir="auto"><strong>کلاینت <code class="notranslate">GET /yourpath/sameUUID</code> را برای شروع دانلود استفاده می‌کند. هدرهای پاسخ سرور شامل موارد زیر هستند:</strong></p>
<ul dir="auto">
<li><code class="notranslate">X-Accel-Buffering: no</code> برای اطلاع به میانی‌باکس‌ها که بافرینگ غیرفعال است</li>
<li><code class="notranslate">Cache-Control: no-store</code> برای اطلاع به میانی‌باکس‌ها که نیاز به کش کردن نیست</li>
<li><code class="notranslate">Content-Type: text/event-stream</code> برای伪装 به عنوان server-sent events (سازگاری بهتر، می‌توان <code class="notranslate">noSSEHeader</code> را برای غیرفعال کردن تنظیم کرد)</li>
<li>برای HTTP/1.1 باید <code class="notranslate">Transfer-Encoding: chunked</code> نیز وجود داشته باشد، اما برای H2/H3 نیاز نیست</li>
</ul>
</li>
<li>
<p dir="auto">برای جلوگیری از محدودیت‌های CORS در Browser Dialer، سرور برای تمام درخواست‌های GET و POST هدرهای پاسخ زیر را شامل می‌شود:</p>
<ul dir="auto">
<li><code class="notranslate">Access-Control-Allow-Origin: *</code></li>
<li><code class="notranslate">Access-Control-Allow-Methods: GET, POST</code></li>
</ul>
</li>
<li>
<p dir="auto"><strong>برای حل مشکل ویژگی‌های طول ثابت هدرهای درخواست و پاسخ HTTP:</strong></p>
<ul dir="auto">
<li>هدرهای درخواست ارسالی از کلاینت به طور پیش‌فرض شامل <code class="notranslate">Referer: ...?x_padding=XXX...</code> هستند (استفاده از Referer برای جلوگیری از درخواست‌های OPTIONS غیرضروری در Browser Dialer است). طول پیش‌فرض 100-1000 کاراکتر است و به صورت تصادفی برای هر درخواست انتخاب می‌شود. سرور به طور پیش‌فرض بررسی می‌کند که آیا این مقدار در محدوده مجاز سرور است یا خیر</li>
<li>هدرهای پاسخ ارسالی از سرور به طور پیش‌فرض شامل <code class="notranslate">X-Padding: XXX...</code> هستند. طول پیش‌فرض 100-1000 کاراکتر است و به صورت تصادفی برای هر پاسخ انتخاب می‌شود</li>
<li><strong>این همان header padding است که بارها به آن اشاره کرده‌ام و با <code class="notranslate">xPaddingBytes</code> قابل تنظیم است</strong></li>
<li>پیشنهاد قرار دادن padding در هدر درخواست در <code class="notranslate">Referer</code> توسط <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/rPDmYQ/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/rPDmYQ" aria-keyshortcuts="Alt+ArrowUp">@rPDmYQ</a> ارائه شد و استفاده از <code class="notranslate">XXX</code> نیز پیشنهاد او بود. <code class="notranslate">X</code> در huffman encoding برابر با 8 بیت است</li>
</ul>
</li>
</ol>
<p dir="auto"><strong>ارسال بسته‌ای بالا و <code class="notranslate">Referer: ...?x_padding=XXX...</code> باعث تولید لاگ‌های زیاد و طولانی می‌شود. می‌توانید در نرم‌افزار معکوس‌پروکسی تنظیم کنید که آنها را ثبت نکند.</strong></p>
<p dir="auto">علاوه بر این، مانند سایر لایه‌های انتقال Xray، سرور هدر <code class="notranslate">X-Forwarded-For</code> را نیز می‌پذیرد تا IP واقعی کلاینت را دریافت کند. همچنین بر اساس <code class="notranslate">host</code> تنظیم شده در سرور، host ارسالی از کلاینت بررسی می‌شود (پیشنهاد شخصی من این است که اگر ضرورتی ندارد، آن را تنظیم نکنید، زیرا از قبل در مسیر پنهان شده است).</p>
<p dir="auto"><strong>اینها ساده‌ترین و ضروری‌ترین مراحل حالت packet-up هستند. اما یک سوال کوچک باقی می‌ماند: چگونه درخواست‌های POST را به طور دقیق اجرا و محدود کنیم؟ سه پارامتر اختصاصی وجود دارد:</strong></p>
<ul dir="auto">
<li><code class="notranslate">scMaxEachPostBytes</code>: حداکثر مقدار داده‌ای که هر درخواست POST از کلاینت می‌تواند حمل کند. مقدار پیش‌فرض 1000000 (1MB) است. این مقدار باید کمتر از حداکثر مقدار مجاز توسط CDN و سایر میانی‌باکس‌های HTTP باشد. سرور نیز درخواست‌های POST با اندازه بزرگتر از این مقدار را رد می‌کند</li>
<li><code class="notranslate">scMinPostsIntervalMs</code>: فقط برای کلاینت. بر اساس هر درخواست پروکسی، حداقل فاصله زمانی بین ارسال درخواست‌های POST توسط کلاینت. مقدار پیش‌فرض 30 میلی‌ثانیه است</li>
<li><code class="notranslate">scMaxBufferedPosts</code>: فقط برای سرور. بر اساس هر درخواست پروکسی، حداکثر تعداد درخواست‌های POST که سرور می‌تواند ذخیره کند. مقدار پیش‌فرض 30 است. در صورت превыن این حد، اتصال قطع می‌شود</li>
</ul>
<p dir="auto"><strong>"بر اساس هر درخواست پروکسی" به این معنی است که هر درخواست پروکسی به طور مستقل شمارش می‌شود و بر یکدیگر تاثیری ندارند</strong>، حتی اگر در یک اتصال H2/H3 مشترک باشند. این معنی sc (sub-connection) است. <strong>برای کاهش fingerprinting، دو مقدار اول می‌توانند به صورت محدوده باشند</strong>، مثلاً به ترتیب "500000-1000000" و "10-50" به صورت تصادفی انتخاب شوند. تمام این پارامترها می‌توانند از طریق <code class="notranslate">extra</code> به کلاینت ارسال شوند که در انتهای مقاله توضیح داده شده است. <strong>نکته جالب دیگر این است که آخرین نسخه Xray حالت packet-up را بهینه کرده است و سرعت آن حتی به حالت stream-up نزدیک شده است، که عمدتاً برای QUIC H3 از طریق CDN مفید است.</strong></p>
<h2 dir="auto">H1 / H2 / H3</h2>
<p dir="auto">حال که حالت packet-up را داریم که تقریباً از تمام میانی‌باکس‌های HTTP عبور می‌کند، بیایید در مورد یک موضوع جالب صحبت کنیم: <strong>QUIC H3 از طریق CDN، یعنی آن عصر جدیدی که SplitHTTP آغاز کرد. درک این بخش برای استفاده انعطاف‌پذیر از XHTTP بسیار مهم است.</strong></p>
<p dir="auto">بسیاری از میانی‌باکس‌های HTTP یک ویژگی دارند که نسخه HTTP را تبدیل می‌کنند. مثلاً CDNها و Nginx ترافیک H3 دریافتی را به H1 یا H2 تبدیل کرده و به سرور مبدا ارسال می‌کنند. <strong>یعنی سرور XHTTP ما فقط نیاز به گوش دادن به H1 و H2 دارد و نیازی به پشتیبانی از H3 نیست، اما کلاینت XHTTP می‌تواند از H3 استفاده کند.</strong></p>
<p dir="auto"><strong>این رفتار پیش‌فرض سرور XHTTP است: فقط به پورت TCP گوش می‌دهد و ترافیک H1 و H2 را پردازش می‌کند.</strong> وقتی TLS فعال است و در <code class="notranslate">alpn</code> فقط یک عنصر "h3" تنظیم شده باشد، سرور فقط از quic-go برای گوش دادن به پورت UDP و پردازش ترافیک H3 استفاده می‌کند. اما در حال حاضر توصیه نمی‌شود این کار را انجام دهید، <strong>بلکه بهتر است XHTTP را پشت یک Nginx یا Caddy واقعی پنهان کنید تا ویژگی‌های fingerprinting کاهش یابد. این یکی از مزایای مهم XHTTP نسبت به سایر پروکسی‌های مبتنی بر QUIC است، <del>و مزیت دیگر البته توانایی عبور از CDN است</del>.</strong> علاوه بر این، کنترل ازدحام در H3 در لایه کاربر پیاده‌سازی شده است. از نظر تئوری می‌توانید الگوریتم‌های کنترل ازدحام QUIC این نرم‌افزارهای معکوس‌پروکسی را تغییر داده و کامپایل کنید، <del>تا بتوانید مانند برخی افراد خواستار، بسته‌ها را با شدت ارسال کنید</del>.</p>
<p dir="auto"><strong>برای کلاینت XHTTP:</strong></p>
<ol dir="auto">
<li><strong>وقتی TLS/REALITY فعال است، به طور پیش‌فرض از H2 استفاده می‌کند</strong>، در غیر این صورت از HTTP/1.1 استفاده می‌کند</li>
<li>وقتی TLS فعال است، اگر <code class="notranslate">alpn</code> فقط شامل "http/1.1" باشد، از HTTP/1.1 استفاده می‌کند (اما Xray اجازه نمی‌دهد که این تنظیم،伪装 مرورگر uTLS را تغییر دهد)</li>
<li><strong>وقتی TLS فعال است، اگر <code class="notranslate">alpn</code> فقط شامل "h3" باشد، از quic-go H3 استفاده می‌کند</strong></li>
<li>اما وقتی از Browser Dialer استفاده می‌کنید، نسخه HTTP خاص توسط مرورگر تعیین می‌شود (کل <code class="notranslate">tlsSettings</code> بی‌اثر می‌شود)</li>
</ol>
<p dir="auto"><strong>اگر می‌خواهید نسخه HTTP واقعی مورد استفاده توسط کلاینت Xray، host، mode XHTTP، اطلاعات جداسازی ارسال/دریافت و غیره را تأیید کنید، سطح لاگ را به "info" تغییر دهید.</strong></p>
<p dir="auto"><strong>پروکسی QUIC H3 از طریق CDN، حداقل XHTTP اولین پیاده‌سازی در مقیاس بزرگ آن بود که راه جدیدی گشود</strong>، زیرا در برخی مناطق و برای برخی ارائه‌دهندگان اینترنت، نظارت بر H3 شدید نیست، <del>اما برخی دیگر آن را به شدت مسدود می‌کنند</del>. حتی پس از توسعه حالت stream-up و کشف اینکه با افزودن伪装 هدر gRPC می‌توان از CF عبور کرد، این فقط برای H2 کار می‌کند، <del>به نظر می‌رسد ارزش آن عصر جدید همچنان در حال افزایش است</del>.</p>
<h2 dir="auto">XMUX</h2>
<p dir="auto"><strong>از آنجا که به H2 و H3 اشاره کردیم، نمی‌توانیم از چندگانگی (multiplexing) آنها صحبت نکنیم: هر دو 0-RTT هستند</strong>. تفاوت در این است که H3 مشکل مسدود شدن سر صف (head-of-line blocking) TCP در H2 را ندارد و از مهاجرت اتصال پشتیبانی می‌کند، بنابراین تغییر شبکه توسط کلاینت باعث قطع اتصال نمی‌شود. <del>پس دوستانی که مرتب RFCها را مطالعه می‌کنند خواهند پرسید</del>: چگونه می‌توان این چندگانگی را به طور دقیق کنترل کرد؟ <strong>ما یک رابط ساده اما قدرتمند طراحی کرده‌ایم به نام XMUX:</strong></p>
<ul dir="auto">
<li>
<p dir="auto"><code class="notranslate">maxConcurrency</code>: حداکثر تعداد درخواست‌های پروکسی که می‌توانند به طور همزمان در یک اتصال TCP/QUIC وجود داشته باشند. <strong>پس از رسیدن تعداد درخواست‌های پروکسی در یک اتصال به این مقدار، core یک اتصال جدید ایجاد می‌کند</strong> تا درخواست‌های بیشتری را پردازش کند. <strong>وقتی تمام مقادیر XMUX صفر هستند، مقدار پیش‌فرض این پارامتر "16-32" است که به صورت تصادفی انتخاب می‌شود.</strong></p>
</li>
<li>
<p dir="auto"><code class="notranslate">maxConnections</code>: حداکثر تعداد اتصالاتی که می‌توانند به طور همزمان وجود داشته باشند. <strong>تا قبل از رسیدن به این تعداد، هر درخواست پروکسی جدید یک اتصال جدید ایجاد می‌کند</strong>. پس از آن، core شروع به استفاده مجدد از اتصالات موجود می‌کند. این پارامتر با <code class="notranslate">maxConcurrency</code> در تضاد است و فقط یکی از آنها قابل استفاده است. مقدار پیش‌فرض 0 است (بدون محدودیت) و می‌توان آن را به صورت محدوده نیز تنظیم کرد که به صورت تصادفی انتخاب می‌شود.</p>
</li>
<li>
<p dir="auto"><code class="notranslate">cMaxReuseTimes</code>: یک اتصال حداکثر چند بار می‌تواند مورد استفاده مجدد قرار گیرد. <strong>پس از این تعداد استفاده، اتصال دیگر برای درخواست‌های جدید اختصاص داده نمی‌شود</strong> و پس از بسته شدن آخرین درخواست پروکسی داخلی، قطع می‌شود. مقدار پیش‌فرض 0 است (بدون محدودیت) و می‌توان آن را به صورت محدوده نیز تنظیم کرد که به صورت تصادفی انتخاب می‌شود.</p>
</li>
<li>
<p dir="auto"><code class="notranslate">hMaxRequestTimes</code>: <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/xqzr/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/xqzr" aria-keyshortcuts="Alt+ArrowUp">@xqzr</a> کشف کرد که Nginx به طور پیش‌فرض حداکثر اجازه می‌دهد هر اتصال TCP/QUIC حداکثر 1000 درخواست HTTP را پردازش کند. <strong>وقتی تمام مقادیر XMUX صفر هستند، مقدار پیش‌فرض این پارامتر "600-900" است که به صورت تصادفی انتخاب می‌شود</strong>، در غیر این صورت مقدار پیش‌فرض 0 است (بدون محدودیت). <strong>این پارامتر بر اساس شمارش درخواست‌های HTTP عمل می‌کند</strong>. به طور کلی حالت stream-one فقط یک درخواست HTTP تولید می‌کند، stream-up دو درخواست و packet-up تعداد N درخواست تولید می‌کند. این شمارش دقیق نیست و درخواست‌های GET در Golang دارای تلاش مجدد خودکار هستند، بنابراین توصیه نمی‌شود مقدار دقیق را تنظیم کنید. باید برای CDNها و موارد مشابه آزمایش شود. <strong>در حالت packet-up، اگر تعداد درخواست‌های POST از این مقدار بیشتر شود، به طور خودکار به یک اتصال TCP/QUIC دیگر سوئیچ می‌کند که یک reuseTimes برای آن حساب می‌شود اما در concurrency محاسبه نمی‌شود.</strong> در واقع با مقادیر پیش‌فرض سه گانه XMUX، فقط حالت packet-up ممکن است از این محدودیت عبور کند که mode پیش‌فرض برای H3 است، <strong>بنابراین افزودن این پارامتر عمدتاً به نفع H3 است.</strong></p>
</li>
<li>
<p dir="auto"><code class="notranslate">hMaxReusableSecs</code>: <a class="user-mention notranslate" data-hovercard-type="user" data-hovercard-url="/users/xqzr/hovercard" data-octo-click="hovercard-link-click" data-octo-dimensions="link_type:self" href="https://github.com/xqzr" aria-keyshortcuts="Alt+ArrowUp">@xqzr</a> کشف کرد که Nginx به طور پیش‌فرض حداکثر اجازه می‌دهد هر اتصال TCP/QUIC حداکثر یک ساعت مورد استفاده مجدد قرار گیرد. <strong>وقتی تمام مقادیر XMUX صفر هستند، مقدار پیش‌فرض این پارامتر "1800-3000" است که به صورت تصادفی انتخاب می‌شود</strong>، در غیر این صورت مقدار پیش‌فرض 0 است (بدون محدودیت). <strong>پس از این مدت زمان، اتصال TCP/QUIC دیگر برای درخواست‌های HTTP جدید اختصاص داده نمی‌شود</strong> و پس از بسته شدن آخرین درخواست HTTP داخلی، قطع می‌شود. در حالت packet-up، اگر زمان استفاده از اتصال از این مقدار بیشتر شود، به طور خودکار به یک اتصال TCP/QUIC دیگر سوئیچ می‌کند که یک reuseTimes برای آن حساب می‌شود اما در concurrency محاسبه نمی‌شود.</p>
</li>
<li>
<p dir="auto"><code class="notranslate">hKeepAlivePeriod</code>: در اتصالات H2/H3 در حالت بیکاری، کلاینت هر چند ثانیه یک بسته keep-alive ارسال می‌کند. مقدار پیش‌فرض 0 است که برابر با 45 ثانیه برای Chrome H2 یا 10 ثانیه برای quic-go H3 است. این تنها پارامتر در XMUX است که نمی‌توان آن را به صورت محدوده تنظیم کرد (تنظیم تصادفی برای این پارامتر یک ویژگی منفی است) و می‌توان آن را به صورت منفی نیز تنظیم کرد (مثلاً -1 برای غیرفعال کردن بسته‌های keep-alive در حالت بیکاری). توصیه می‌شود آن را روی 0 بگذارید.</p>
</li>
</ul>
<p dir="auto">این پارامترهای XMUX می‌توانند به روش‌های مختلف ترکیب شوند. مثلاً قبل از تست سرعت چندنخی باید <code class="notranslate">"maxConcurrency": 1</code> تنظیم شود، در حالی که برای "استفاده نامحدود" می‌توان <code class="notranslate">"maxConnections": 1</code> تنظیم کرد. حتی اگر تمایلی به تحقیق ندارید هم مشکلی نیست، <strong>وقتی تمام این مقادیر صفر باشند، سه مقدار پیش‌فرض فوق استفاده می‌شوند که در واقع هر چند وقت یکبار اتصال اصلی H2/H3 جدیدی ایجاد می‌کند و تجربه بسیار روانی ارائه می‌دهد</strong>، بدون مشکلات "قطع اتصال" ناشی از استفاده مداوم از یک اتصال مشترک در gRPC یا لایه انتقال HTTP. به طور مشابه، تمام این پارامترها می‌توانند از طریق <code class="notranslate">extra</code> به کلاینت ارسال شوند.</p>
<p dir="auto">توجه: اگر هیچ یک از پارامترها تنظیم نشوند، معادل تمام پارامترهای صفر است و سه مقدار پیش‌فرض استفاده می‌شوند. <strong>اما اگر هر یک از پارامترها تنظیم شده باشد، سایر پارامترها مقدار پیش‌فرض ندارند و باید همه آنها را خودتان تنظیم کنید</strong>، به جز دو پارامتر اول که بقیه را می‌توان همزمان تنظیم کرد.</p>
<p dir="auto"><strong>علاوه بر این، هنگام استفاده از XHTTP، mux.cool را فعال نکنید و سرور Xray در نسخه‌های جدید بررسی می‌کند که فقط XUDP خالص پذیرفته شود.</strong></p>
<p dir="auto">برخی نقل‌قول‌ها در مورد مقادیر پیش‌فرض XMUX:</p>
<blockquote>
<ul dir="auto">
<li>من عمداً دو گزینه را انتخاب کردم که خارج از TLS به راحتی قابل تشخیص نباشند، یعنی maxConcurrency به همراه cMaxReuseTimes (به جای maxConnections به همراه cMaxLifetimeMs)، و مقادیر پیش‌فرض هر دو این گزینه‌ها به صورت محدوده و تصادفی هستند تا بیشترین حد ممکن از ویژگی‌های بالقوه جلوگیری شود.</li>
<li>من maxConcurrency را به جای maxConnections انتخاب کردم تا از تبدیل شدن تعداد اتصالات به یک الگوی ثابت جلوگیری کنم، البته اگر ترجیح می‌دهید می‌توانید دومی را به صورت دستی تنظیم کنید</li>
</ul>
</blockquote>
<blockquote>
<ul dir="auto">
<li>XMUX و Nginx اشیاء مختلفی را شمارش می‌کنند. maxConcurrency و cMaxReuseTimes هر دو بر اساس "اتصالات پروکسی" شمارش می‌شوند. فقط stream-one یک درخواست HTTP تولید می‌کند، در حالی که stream-up یک درخواست برای آپلود و یک درخواست برای دانلود (دو درخواست) و packet-up تعداد N درخواست تولید می‌کند.</li>
<li>اما مطمئن نیستم که آیا stream-one در برخی شرایط به طور خودکار یک درخواست HTTP اضافی تولید می‌کند یا خیر. <strong>همچنین فکر می‌کنم دنبال کردن استفاده مداوم از یک اتصال خاص چندان معنی ندارد، زیرا اگر شما ارائه‌دهنده اینترنت باشید، قطعاً اتصالات قدیمی را محدود یا پاک می‌کنید، در غیر این صورت منابع به تدریج توسط اتصالات قدیمی اشغال می‌شوند. به همین دلیل پارامترهای پیش‌فرض XMUX محدود کردن استفاده مجدد و تعویض دوره‌ای اتصالات هستند</strong></li>
</ul>
</blockquote>
<p dir="auto">در ادامه مقاله برخی پارامترهای Nginx نیز مورد بحث قرار گرفته‌اند.</p>
<h2 dir="auto">STREAM-UP/ONE</h2>
<p dir="auto">بالاخره نوبت به حالت مهم دیگر XHTTP رسید: <strong>حالت "جریان بالا، جریان پایین" یا stream-up که همانطور که از نامش پیداست، آپلود در آن نیز به صورت جریانی است و بنابراین کارایی آپلود را قربانی نمی‌کند.</strong> این حالت در ابتدا برای REALITY توسعه داده شد، تا زمانی که کشف کردیم با افزودن伪装 هدر gRPC می‌توان H2 جریان بالا را از CF عبور داد (<strong>نیاز به فعال‌کردن پشتیبانی gRPC در پنل دارد</strong>) و پشتیبانی نرم‌افزارهای معکوس‌پروکسی مانند Nginx نیز خوب است (<strong>توصیه می‌شود در Nginx از grpc_pass استفاده کنید، ساده و بی‌دردسر است</strong>). بنابراین رفتار مقدار پیش‌فرض "auto" برای <code class="notranslate">mode</code> به این صورت است:</p>
<ul dir="auto">
<li>کلاینت: <strong>در TLS H2 از stream-up، در REALITY از stream-one</strong> (اگر <code class="notranslate">downloadSettings</code> وجود داشته باشد از stream-up استفاده می‌کند)، <strong>در غیر این صورت از packet-up استفاده می‌کند</strong></li>
<li>سرور: به طور پیش‌فرض هر سه حالت را می‌پذیرد، <strong>اما اگر روی حالت خاصی تنظیم شود فقط همان حالت را می‌پذیرد</strong>، به جز "stream-up" که stream-one را نیز می‌پذیرد</li>
</ul>
<blockquote>
<p dir="auto">stream-up نسبت به stream-one سازگاری بهتری دارد. برخی کاربران گزارش داده‌اند که با فعال‌کردن جریان بالا در CFT می‌توان از stream-up استفاده کرد، اما برای استفاده از stream-one باید گزینه دیگری نیز فعال شود (شاید به دلیل伪装 SSE؟). همچنین دیده شده که برخی CDNها (نامشان یادم نیست) با stream-one به شدت محدود می‌کنند، اما با stream-up این مشکل وجود ندارد</p>
</blockquote>
<p dir="auto"><strong>روش پیاده‌سازی آنها به این صورت است (غیرتوسعه‌دهندگان می‌توانند این بخش را نادیده بگیرند):</strong></p>
<ul dir="auto">
<li>برای stream-up، کافیست ارسال بسته‌ای در packet-up را به <code class="notranslate">POST /yourpath/sameUUID</code> جریانی تغییر دهید. <code class="notranslate">Referer: ...?x_padding=XXX...</code> نیز وجود دارد</li>
<li>برای stream-one، <code class="notranslate">POST /yourpath/</code> کافیست و پاسخ همان دانلود است. هر دو جهت جریانی هستند و هدرهای درخواست و پاسخ دارای header padding هستند</li>
<li>توجه کنید که اگر در stream-one <code class="notranslate">/yourpath</code> وارد شود، در واقع درخواست برای <code class="notranslate">/yourpath/</code> ارسال می‌شود. اگر در انتها <code class="notranslate">/</code> وجود نداشته باشد، به طور خودکار اضافه می‌شود</li>
<li><strong>آپلود به طور پیش‌فرض دارای <code class="notranslate">Content-Type: application/grpc</code> است تا به عنوان gRPC伪装 شود (می‌توان <code class="notranslate">noGRPCHeader</code> را برای غیرفعال کردن تنظیم کرد)</strong></li>
<li>هدرهای پاسخ سرور برای دانلود دقیقاً مانند حالت packet-up هستند. در stream-one صحنه عجیبی رخ می‌دهد که یک پاسخ SSE به یک درخواست gRPC داده می‌شود. اگر مشکلی پیش آمد، می‌توانید <code class="notranslate">noSSEHeader</code> را امتحان کنید</li>
</ul>
<blockquote>
<p dir="auto"><a class="issue-link js-issue-link" data-error-text="Failed to load title" data-id="7614138" data-permission-text="Title is private" data-url="https://github.com/XTLS/Xray-core/discussions/4113" data-hovercard-type="discussion" data-hovercard-url="/XTLS/Xray-core/discussions/4113/hovercard?comment_id=11682833" href="https://github.com/XTLS/Xray-core/discussions/4113#discussioncomment-11682833" aria-keyshortcuts="Alt+ArrowUp">#4113 (comment)</a> تست‌های مربوطه نشان داد که CF اتصالات HTTP بدون داده واقعی را پس از 100 ثانیه قطع می‌کند که باعث قطع شدن جهت آپلود در stream-up می‌شود. <strong>بنابراین این <a href="https://github.com/XTLS/Xray-core/pull/4306" data-hovercard-type="pull_request" data-hovercard-url="/XTLS/Xray-core/pull/4306/hovercard" aria-keyshortcuts="Alt+ArrowUp">PR</a> پارامتر <code class="notranslate">scStreamUpServerSecs</code> را به سرور اضافه کرد که مقدار پیش‌فرض آن "20-80" است و به صورت تصادفی انتخاب می‌شود. سرور هر این مدت زمان، <code class="notranslate">xPaddingBytes</code> بایت ارسال می‌کند تا اتصال زنده بماند</strong></p>
<p dir="auto">می‌توانید <code class="notranslate">"scStreamUpServerSecs": -1</code> تنظیم کنید تا این مکانیسم غیرفعال شود. در این حالت سرور حتی هدرهای پاسخ را نیز به موقع ارسال نمی‌کند و رفتار مانند نسخه‌های قبلی است</p>
</blockquote>
<p dir="auto"><strong>حال سوالی که برای کاربران معمول gRPC پیش می‌آید این است: stream-up نسبت به لایه انتقال gRPC چه مزایایی دارد؟</strong></p>
<ul dir="auto">
<li>اولاً نیازی به هیچ کتابخانه gRPC ندارد، <strong>کارایی بهتری دارد</strong></li>
<li>ترافیک دانلود در آن یک درخواست GET مستقل است، <strong>تحت محدودیت‌های ترافیکی CDN برای gRPC قرار نمی‌گیرد</strong></li>
<li>همچنین دارای header padding، <strong>XMUX</strong>، جداسازی ارسال/دریافت و سایر بهبودها است و از مکانیسم extra نیز پشتیبانی می‌کند که تمام پارامترها را می‌توان به اشتراک گذاشت، <strong>بالغ‌تر است</strong></li>
</ul>
<p dir="auto">البته مزایای XHTTP نسبت به WebSocket یا HTTPUpgrade، به جز "<strong>نداشتن ویژگی‌های آشکار ALPN برابر با http/1.1</strong>"، اگر تا اینجا مقاله را خوانده‌اید احتمالاً پاسخ را می‌دانید و نیازی به列举 نیست، <del>اصلاً هم که زیاد است و列举ش سخت</del>.</p>
<h2 dir="auto">جداسازی ارسال و دریافت</h2>
<p dir="auto"><strong>و حالا نوبت به عصر جدید دیگری می‌رسد: جداسازی ارسال و دریافت.</strong> می‌دانیم که فعلاً GFW برای تشخیص ویژگی‌های ترافیک مانند TLS in TLS بر اساس یک اتصال واحد عمل می‌کند. <strong>پس اگر ارسال و دریافت را به سیستم‌های بازرسی مختلف تقسیم کنیم، مثلاً ارسال از طریق IPv4 TCP و دریافت از طریق IPv6 UDP، GFW برای مدتی سردرگم می‌شود</strong>. از آنجا که سرور XHTTP فقط بر اساس UUID تصادفی تولید شده در مسیر، ارسال و دریافت را مرتبط می‌کند، <strong>حالت‌های packet-up و stream-up به طور ذاتی توانایی جداسازی واقعی ارسال و دریافت را دارند</strong>. همچنین از آنجا که XHTTP می‌تواند از انواع CDNها عبور کند و با REALITY و غیره ترکیب شود، گزینه‌های بی‌شماری وجود دارد. برای کلاینت، باید <code class="notranslate">downloadSettings</code> تنظیم شود:</p>
<div class="highlight highlight-source-json-comments notranslate position-relative overflow-auto" dir="auto"><pre class="notranslate"><span class="pl-s"><span class="pl-pds">"</span>xhttpSettings<span class="pl-pds">"</span></span>: {
    <span class="pl-s">"host"</span>: <span class="pl-s"><span class="pl-pds">"</span>example.com<span class="pl-pds">"</span></span>,
    <span class="pl-s">"path"</span>: <span class="pl-s"><span class="pl-pds">"</span>/yourpath<span class="pl-pds">"</span></span>, <span class="pl-c"><span class="pl-c">//</span> باید یکسان باشد</span>
    <span class="pl-s">"mode"</span>: <span class="pl-s"><span class="pl-pds">"</span>auto<span class="pl-pds">"</span></span>,
    <span class="pl-s">"extra"</span>: {
        <span class="pl-s">"headers"</span>: {
            <span class="pl-c"><span class="pl-c">//</span> "key": "value"</span>
        },
        <span class="pl-s">"xPaddingBytes"</span>: <span class="pl-s"><span class="pl-pds">"</span>100-1000<span class="pl-pds">"</span></span>,
        <span class="pl-s">"noGRPCHeader"</span>: <span class="pl-c1">false</span>, <span class="pl-c"><span class="pl-c">//</span> فقط برای stream-up/one، فقط کلاینت</span>
        <span class="pl-s">"noSSEHeader"</span>: <span class="pl-c1">false</span>, <span class="pl-c"><span class="pl-c">//</span> فقط سرور</span>
        <span class="pl-s">"scMaxEachPostBytes"</span>: <span class="pl-c1">1000000</span>, <span class="pl-c"><span class="pl-c">//</span> فقط packet-up</span>
        <span class="pl-s">"scMinPostsIntervalMs"</span>: <span class="pl-c1">30</span>, <span class="pl-c"><span class="pl-c">//</span> فقط packet-up، فقط کلاینت</span>
        <span class="pl-s">"scMaxBufferedPosts"</span>: <span class="pl-c1">30</span>, <span class="pl-c"><span class="pl-c">//</span> فقط packet-up، فقط سرور</span>
        <span class="pl-s">"scStreamUpServerSecs"</span>: <span class="pl-s"><span class="pl-pds">"</span>20-80<span class="pl-pds">"</span></span>, <span class="pl-c"><span class="pl-c">//</span> فقط stream-up، فقط سرور</span>
        <span class="pl-s">"xmux"</span>: { <span class="pl-c"><span class="pl-c">//</span> عمدتاً برای h2/h3، فقط کلاینت</span>
            <span class="pl-s">"maxConcurrency"</span>: <span class="pl-s"><span class="pl-pds">"</span>16-32<span class="pl-pds">"</span></span>,
            <span class="pl-s">"maxConnections"</span>: <span class="pl-c1">0</span>,
            <span class="pl-s">"cMaxReuseTimes"</span>: <span class="pl-c1">0</span>,
            <span class="pl-s">"hMaxRequestTimes"</span>: <span class="pl-s"><span class="pl-pds">"</span>600-900<span class="pl-pds">"</span></span>,
            <span class="pl-s">"hMaxReusableSecs"</span>: <span class="pl-s"><span class="pl-pds">"</span>1800-3000<span class="pl-pds">"</span></span>,
            <span class="pl-s">"hKeepAlivePeriod"</span>: <span class="pl-c1">0</span>
        },
        <span class="pl-s">"downloadSettings"</span>: { <span class="pl-c"><span class="pl-c">//</span> فقط کلاینت</span>
            <span class="pl-s">"address"</span>: <span class="pl-s"><span class="pl-pds">"</span><span class="pl-pds">"</span></span>, <span class="pl-c"><span class="pl-c">//</span> دامنه/IP دیگر</span>
            <span class="pl-s">"port"</span>: <span class="pl-c1">443</span>,
            <span class="pl-s">"network"</span>: <span class="pl-s"><span class="pl-pds">"</span>xhttp<span class="pl-pds">"</span></span>,
            <span class="pl-s">"security"</span>: <span class="pl-s"><span class="pl-pds">"</span>tls<span class="pl-pds">"</span></span>,
            <span class="pl-s">"tlsSettings"</span>: {
                <span class="pl-ii">...</span>
            },
            <span class="pl-s">"xhttpSettings"</span>: {
                <span class="pl-s">"path"</span>: <span class="pl-s"><span class="pl-pds">"</span>/yourpath<span class="pl-pds">"</span></span>, <span class="pl-c"><span class="pl-c">//</span> باید یکسان باشد</span>
                <span class="pl-ii">...</span>
            },
            <span class="pl-s">"sockopt"</span>: {} <span class="pl-c"><span class="pl-c">//</span> اگر "penetrate" در "sockopt" آپلود true باشد، با آن جایگزین می‌شود</span>
        }
    }
}</pre><div class="zeroclipboard-container position-absolute right-0 top-0">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn js-clipboard-copy m-2 p-0" data-copy-feedback="Copied!" data-tooltip-direction="w" value="&quot;xhttpSettings&quot;: {
    &quot;host&quot;: &quot;example.com&quot;,
    &quot;path&quot;: &quot;/yourpath&quot;, // باید یکسان باشد
    &quot;mode&quot;: &quot;auto&quot;,
    &quot;extra&quot;: {
        &quot;headers&quot;: {
            // &quot;key&quot;: &quot;value&quot;
        },
        &quot;xPaddingBytes&quot;: &quot;100-1000&quot;,
        &quot;noGRPCHeader&quot;: false, // فقط برای stream-up/one، فقط کلاینت
        &quot;noSSEHeader&quot;: false, // فقط سرور
        &quot;scMaxEachPostBytes&quot;: 1000000, // فقط packet-up
        &quot;scMinPostsIntervalMs&quot;: 30, // فقط packet-up، فقط کلاینت
        &quot;scMaxBufferedPosts&quot;: 30, // فقط packet-up، فقط سرور
        &quot;scStreamUpServerSecs&quot;: &quot;20-80&quot;, // فقط stream-up، فقط سرور
        &quot;xmux&quot;: { // عمدتاً برای h2/h3، فقط کلاینت
            &quot;maxConcurrency&quot;: &quot;16-32&quot;,
            &quot;maxConnections&quot;: 0,
            &quot;cMaxReuseTimes&quot;: 0,
            &quot;hMaxRequestTimes&quot;: &quot;600-900&quot;,
            &quot;hMaxReusableSecs&quot;: &quot;1800-3000&quot;,
            &quot;hKeepAlivePeriod&quot;: 0
        },
        &quot;downloadSettings&quot;: { // فقط کلاینت
            &quot;address&quot;: &quot;&quot;, // دامنه/IP دیگر
            &quot;port&quot;: 443,
            &quot;network&quot;: &quot;xhttp&quot;,
            &quot;security&quot;: &quot;tls&quot;,
            &quot;tlsSettings&quot;: {
                ...
            },
            &quot;xhttpSettings&quot;: {
                &quot;path&quot;: &quot;/yourpath&quot;, // باید یکسان باشد
                ...
            },
            &quot;sockopt&quot;: {} // اگر &quot;penetrate&quot; در &quot;sockopt&quot; آپلود true باشد، با آن جایگزین می‌شود
        }
    }
}" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon m-2">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none m-2">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>
<p dir="auto"><strong>می‌توان دید که <code class="notranslate">downloadSettings</code> در واقع یک <code class="notranslate">streamSettings</code> کاملاً جدید است</strong>، اما دارای <code class="notranslate">address</code> و <code class="notranslate">port</code> مشابه خروجی VLESS است که به یک ورودی دیگر اشاره می‌کند. واضح است که <code class="notranslate">network</code> باید "xhttp" باشد (اجباری است)، و <code class="notranslate">security</code> می‌تواند "tls" یا "reality" باشد. <strong><code class="notranslate">sockopt</code> نیز می‌تواند به اشتراک گذاشته شود، اما دریافت‌کننده می‌تواند <code class="notranslate">penetrate</code> را در <code class="notranslate">sockopt</code> آپلود روی true تنظیم کند تا آن را جایگزین کند، که برای مواردی مانند <code class="notranslate">mark</code> مناسب است. به جز این استثنا، پیکربندی دریافت در حالت جداسازی کاملاً مستقل است و هیچ یک از تنظیمات آپلود را به ارث نمی‌برد</strong>، و حتی وقتی مثلاً هر دو طرف XMUX را تنظیم نکرده‌اند و از مقادیر پیش‌فرض استفاده می‌کنند، مقادیر تصادفی انتخاب شده در محدوده برای هر یک مستقل و نامرتبط هستند، <strong>بنابراین با گذشت زمان، استفاده مجدد از ارسال و دریافت کاملاً نامتقارن است و زمان تعویض اتصال اصلی نیز متفاوت است که اثر ضد-تحلیل بهتری دارد</strong>. زیرا اگر GFW بخواهد جداسازی ارسال/دریافت را هدف قرار دهد، "زمان شروع یکسان برای اتصال اصلی" قطعاً مهم‌ترین نقطه شروع است، بنابراین در آینده XHTTP اجازه می‌دهد ارسال و دریافت از ابتدا در زمان‌های مختلف شروع شوند.</p>
<p dir="auto">در واقع اگر از CDN استفاده می‌کنید، حتی نیازی به تغییر پیکربندی سرور ندارید و می‌توانید جداسازی ارسال/دریافت را تجربه کنید، <strong>مثلاً می‌توانید یک IPv4 را برای TLS H2 و یک IPv6 را برای QUIC H3 انتخاب کنید</strong>. همچنین اکثر CDNها از "دامنه‌های جلویی یکسان" پشتیبانی می‌کنند، مثلاً در آپلود <code class="notranslate">serverName</code> را "a.example.com" و در دریافت "b.example.com" قرار دهید، در حالی که <code class="notranslate">host</code> در هر دو طرف "c.example.com" است، <strong>تا SNI خارجی متفاوت به نظر برسد</strong>. البته اگر از قبل دو دامنه دارید که حتی بهتر است. اگر هیچ دامنه‌ای ندارید، می‌توانید از دو VPS و دو REALITY برای جداسازی ارسال/دریافت استفاده کنید: <strong>چه CDN به همراه معکوس‌پروکسی و چه REALITY به همراه fallback، تا زمانی که در نهایت با یک مسیر به همان ورودی XHTTP روی همان VPS برسند، کافی است</strong>. در کل از آنجا که XHTTP همه‌جا کار می‌کند، ترکیب‌های ممکن بی‌نهایت هستند و تنها محدودیت، خلاقیت شماست.</p>
<p dir="auto"><strong>مثلاً پس از معرفی جداسازی ارسال/دریافت، بسیاری از کاربران در واقع از آن برای تقسیم ارسال به خطوط با مسیر رفت بهینه و دریافت به خطوط با مسیر برگشت بهینه استفاده کردند که بسیار کاربردی است.</strong></p>
<p dir="auto">مثلاً می‌توانید برای آپلود <code class="notranslate">"maxConnections": 1</code> و برای دریافت <code class="notranslate">"maxConcurrency": 1</code> تنظیم کنید، <strong>تا داده‌های کم حجم آپلود همه از یک اتصال پایه عبور کنند، در حالی که داده‌های پرحجم دریافت روی اتصالات پایه مختلف توزیع شوند. این روش هم ضد-سانسور است، هم تاخیر کم دارد و هم سرعت بالا، چیزی شبیه به سوئیچ، و در ترکیب با Vision Seed نتیجه بهتری دارد.</strong></p>
<p dir="auto">در بالا یک نمونه پیکربندی شامل تمام پارامترها ارائه شد، <strong>تا همه بدانند هر پارامتر را کجا باید قرار دهند</strong> و برخی پارامترهایی که قبلاً توضیح داده نشده بودند را روشن کنیم:</p>
<ul dir="auto">
<li>
<p dir="auto"><strong><code class="notranslate">extra</code> روش اشتراک‌گذاری JSON خام برای تمام پارامترهای به جز <code class="notranslate">host</code>، <code class="notranslate">path</code> و <code class="notranslate">mode</code> است. وقتی <code class="notranslate">extra</code> وجود دارد، فقط این چهار مورد اعمال می‌شوند. و در لینک‌های اشتراک‌گذاری فقط این چهار مورد وجود دارند، زیرا GUIها نیز معمولاً فقط این چهار مورد را نمایش می‌دهند. پارامترهای داخل <code class="notranslate">extra</code> نسبتاً کم‌کاربرد هستند و باید مستقیماً توسط ارائه‌دهنده سرویس به کلاینت ارسال شوند، نه اینکه توسط کلاینت به دلخواه تغییر داده شوند.</strong><br>
توضیح اضافه: منظور از "ارسال" این است که "انسان آن را ارسال کند"، یعنی ارائه‌دهنده سرویس <code class="notranslate">extra</code> را نوشته و کل آن را در لینک اشتراک‌گذاری قرار دهد و کلاینت می‌تواند آن را مستقیماً به عنوان <code class="notranslate">extra</code> خود استفاده کند.</p>
</li>
<li>
<p dir="auto"><code class="notranslate">host</code> رفتاری مشابه سایر لایه‌های انتقال مبتنی بر HTTP در Xray دارد، <strong>اولویت ارسال host توسط کلاینت به این صورت است: <code class="notranslate">host</code> &gt; <code class="notranslate">serverName</code> &gt; <code class="notranslate">address</code></strong>. اگر سرور <code class="notranslate">host</code> را تنظیم کند، مقدار ارسالی از کلاینت را بررسی می‌کند، در غیر این صورت بررسی نمی‌کند. توصیه می‌شود اگر ضرورتی ندارد آن را تنظیم نکنید. <code class="notranslate">host</code> را نمی‌توان در <code class="notranslate">headers</code> قرار داد.</p>
</li>
</ul>
<blockquote>
<p dir="auto"><strong>اگر می‌خواهید نسخه HTTP واقعی مورد استفاده توسط کلاینت Xray، host، mode XHTTP، اطلاعات جداسازی ارسال/دریافت و غیره را تأیید کنید، سطح لاگ را به "info" تغییر دهید.</strong></p>
</blockquote>
<h2 dir="auto">فراتر از REALITY</h2>
<p dir="auto"><strong>اوایل سال گذشته من بازگشتم و REALITY را نوشتم که <del>همه چیز را تحت الشعاع قرار داد</del> و چندین مشکل را یکجا حل کرد</strong>، <del>و بعدش مدام تو کافه‌بارها می‌چرخیدم و زیاد رسیدگی نمی‌کردم</del>، حتی مقاله‌اش تا حالا هنوز کامل نشده، <del>XUDP UoT Migration هم خیلی حیف شد، پارسال مقاله‌اش تقریباً تموم شده بود ولی پخش نشد</del>، <strong>ناگهان دیدم REALITY تبدیل به گزینه اصلی برای اتصال مستقیم شده</strong>، اغلب می‌بینم وقتی یه چیزی بسته میشه تو کامنتا میگن REALITY رو امتحان کن، محبوبیتش که واضحه، <del>حتی خیلیم پایدار شده، اینجا دیگه مثل قدیما شلوغ نیست</del>. هرچند Xray برای transports دیگه هم کلی بهینه‌سازی و بهبود انجام داده، <strong>اما XHTTP اولین لایه انتقال بومی Xray هست، <del>اولین کارم یه کار بزرگ بود</del></strong>، و وقتی این مقاله رو خوندید، می‌دونید که XHTTP می‌تونه با REALITY استفاده بشه:</p>
<p dir="auto"><strong>منظور از فراتر از REALITY این نیست که جایگزینش کنیم، بلکه محبوبیتش از REALITY بیشتر بشه. بدون اغراق می‌تونم بگم، همونطور که همیشه سبک Xray بوده، ظهور XHTTP باعث شد بقیه لایه‌های انتقال مبتنی بر HTTP همگی کم‌رنگ بشن، این همون عصره که XHTTP تو همه سناریوها حرف اول رو می‌زنه. قراره از پروتکل موفق قبلی یعنی REALITY هم محبوب‌تر بشه، چون ویژگی‌های XHTTP به خودی خود نشون می‌ده دامنه پوشش وسیع‌تری داره.</strong></p>
<p dir="auto">در پایان، امیدوارم ظهور XHTTP برای همه یه کم هیجان ایجاد کرده باشه، همونطور که Xray بارها تو تاریخش این کار رو انجام داده: <strong>تغییر و نوآوری همیشه اعتقاد Xray بوده.</strong></p>
<h3 dir="auto">اگر این مقاله بهتون کمک کرد، یه <a href="https://opensea.io/assets/ethereum/0x5ee362866001613093361eb8569d59c4141b76d1/2" rel="nofollow">REALITY NFT</a> حمایت کنید:</h3>
<ul dir="auto">
<li>در کل هزارتا هستن، صدتا اولی قیمتشون 0.01 ETH هست، بعدش قیمت میره بالا، <strong>گرفتن نسخه اولیه مثل پول درآوردن راحته</strong></li>
<li><del>یه مقاله درباره REALITY تو چند روز آینده منتشر میشه</del> شد مقاله درباره XHTTP</li>
<li><strong>دارنده‌های REALITY NFT می‌تونن زودتر از بقیه یه کلاینت جدید Xray رو تست کنن، و NFT بعدی رو هم رایگان می‌گیرن، مثلاً</strong></li>
<li>کسانی که تا 2025.1.1 پروجکت X NFT رو داشتن، دو تا REALITY NFT هدیه گرفتن</li>
</ul>
<p dir="auto">با حمایت شما، Project X NFT پنج شش برابر شده، برای کسایی که موقع عرضه اولیه مردد بودن، این فرصت خوبیه جبران کنن.</p>
<h3 dir="auto">البته اگر توانایی دارید، حمایت از <a href="https://github.com/XTLS/Xray-core/discussions/3633" data-hovercard-type="discussion" data-hovercard-url="/XTLS/Xray-core/discussions/3633/hovercard" aria-keyshortcuts="Alt+ArrowUp">Project X NFT</a> رو هم ممنون میشم:</h3>
<ul dir="auto">
<li>ETH/USDT/USDC: <code class="notranslate">0xDc3Fe44F0f25D13CACb1C4896CD0D321df3146Ee</code></li>
<li>Project X NFT: <a href="https://github.com/XTLS/Xray-core/discussions/3633" data-hovercard-type="discussion" data-hovercard-url="/XTLS/Xray-core/discussions/3633/hovercard" aria-keyshortcuts="Alt+ArrowUp">Announcement of NFTs by Project X #3633</a></li>
<li>REALITY NFT: <a href="https://opensea.io/assets/ethereum/0x5ee362866001613093361eb8569d59c4141b76d1/2" rel="nofollow">https://opensea.io/assets/ethereum/0x5ee362866001613093361eb8569d59c4141b76d1/2</a></li>
<li>گاهی هزینه gas ETH خیلی بالاست، می‌تونید اول favorite کنین، وقتی gas معقول شد بخرین</li>
</ul>
<p dir="auto">اگر جایی نامفهوم بود لطفاً زیر همین پست کامنت بذارین تا مقاله رو آپدیت کنم، <del>پیشنهاد می‌کنم نسخه ترجمه رو زود منتشر نکنین</del>.</p>
    </td>
</details>


<details>
<summary>GPT-4o Persian translation</summary>

### 中文 | Русский | فارسی

> [中文版评论](https://github.com/XTLS/Xray-core/discussions/4113#discussion-7614138) | [Русский перевод](https://github.com/XTLS/Xray-core/discussions/4113#discussioncomment-11468947) | [نسخه فارسی](https://github.com/XTLS/Xray-core/discussions/4113)

---

# XHTTP: فراتر از REALITY

**خلاصه توسعه:** در میانه‌های سال ۲۰۲۴، [@mmmray](https://github.com/mmmray)، [@ll11l1lIllIl1lll](https://github.com/ll11l1lIllIl1lll) و دیگر توسعه‌دهندگان بر پایه‌ی اصل "ارسال به‌صورت بسته‌ای از بالا و دریافت به‌صورت جریانی از پایین" که پیش‌تر توسط [@RPRX](https://github.com/RPRX) مطرح شده بود، پروتکل SplitHTTP را توسعه دادند. **این اولین پروتکلی بود که توانست بدون افت محسوس در عملکرد دانلود، از اکثر میانی‌باکس‌های HTTP عبور کند و اولین پیاده‌سازی در مقیاس وسیع برای QUIC H3 از طریق CDN به‌شمار می‌آمد. این آغازگر عصری نوین در عبور از فایروال بود.**

پشتیبانی از مرورگر (Browser Dialer)، کاهش قابلیت padding در هدرها، معرفی XMUX برای کنترل دقیق چندگانگی (multiplexing)، و همچنین قابلیت ترکیب با REALITY نیز به‌مرور اضافه شد. سپس [@RPRX](https://github.com/RPRX) توسعه را ادامه داد و **جداسازی واقعی بین ارسال و دریافت را پیاده‌سازی کرد، به‌طوری‌که مثلاً ارسال می‌تواند از طریق IPv6 روی CDN با H3 باشد و دریافت از طریق IPv4 روی REALITY با H2 انجام شود (حتی IP مبدأ نیز می‌تواند متفاوت باشد).** ~~دوباره عصری نو شروع شد.~~

در ادامه حالت stream-up معرفی شد که دیگر باعث افت کارایی آپلود نمی‌شود، طرح extra برای ارسال کامل پارامترها اضافه شد، و با اضافه‌کردن هدر جعلی gRPC به stream-up، **امکان عبور آن از CDN نیز فراهم شد و این عملاً جایگزینی برای لایه‌ی انتقال سنتی gRPC گردید.** ~~اگر از ابتدا می‌دانستیم این کار ممکن است، شاید اصلاً gRPC ایجاد نمی‌شد.~~

در نهایت لایه انتقال HTTP نیز به‌عنوان stream-one در XHTTP ادغام شد، با قابلیت‌های padding در هدر، XMUX، هدرهای جعلی gRPC و... **اکنون ما XHTTP کامل را داریم: عبور از میانی‌باکس‌ها، جداسازی ارسال/دریافت، XMUX روان و همه‌چیز آماده است. عصر همه‌کاره بودن XHTTP رسماً آغاز شده است.**

در ابتدا قرار بود صرفاً مستند فنی باشد، ولی ناخواسته تبدیل به مقاله‌ای جامع شد. این مقاله به درک بهتر شما از اصول طراحی XHTTP کمک خواهد کرد و استفاده بهینه‌تری از آن خواهید داشت.

---

## راهنمای سریع شروع

با اینکه XHTTP پارامترهای زیادی دارد، اما همگی دارای مقدار پیش‌فرض مناسب هستند. اگر فقط می‌خواهید از آن استفاده کنید، کافی است این ۶ مرحله را طی کنید:

1. چه با TLS و چه با REALITY، **در اغلب موارد فقط نیاز است مقدار `path` را در تنظیمات پر کنید، بقیه را می‌توانید خالی بگذارید.**
2. اگر سرور از QUIC H3 پشتیبانی می‌کند، **در کلاینت کافی است `alpn` را روی `h3` تنظیم کنید تا از QUIC استفاده شود.**
3. برای انتخاب IP بهینه‌ی CDN، آدرس کلاینت را با `address` پر کنید و `serverName` را با دامنه‌ی مناسب تنظیم نمایید.
4. اگر نمی‌توانید به Cloudflare متصل شوید، **در پنل CF گزینه‌ی پشتیبانی از gRPC را فعال کنید.**
5. اگر از Nginx عبور نمی‌کند، **در کانفیگ آن `proxy_pass` را به `grpc_pass` تغییر دهید.**
6. اگر عبور از سایر CDNها یا نرم‌افزارهای reverse proxy با مشکل مواجه است، پیشنهاد می‌شود `mode` را روی `packet-up` قرار دهید که بیشترین سازگاری را دارد.

> **به‌صورت پیش‌فرض، XHTTP از multiplexing پشتیبانی می‌کند. تاخیر کمتری نسبت به Vision دارد، اما در تست سرعت چندنخی عملکرد ضعیف‌تری دارد، مگر اینکه `maxConcurrency` روی 1 تنظیم شده باشد. به بخش XMUX مراجعه کنید.**

Cloudflare اتصالات HTTP بدون ارسال داده‌ی واقعی را پس از ۱۰۰ ثانیه قطع می‌کند. برای اتصال‌های طولانی، برنامه‌هایی مثل sshd باید گزینه‌هایی مانند `ClientAliveInterval` را تنظیم کنند تا اتصال حفظ شود.

این مقاله را می‌توان به‌عنوان مستند پیشرفته استفاده کرد. تقریباً همه چیز درباره‌ی XHTTP را پوشش می‌دهد و هر پارامتر را توضیح می‌دهد. **در انتهای مقاله، نمونه‌ای کامل از پیکربندی با تمامی پارامترها آمده که می‌توانید در صورت ابهام، نام پارامتر را در مقاله جستجو کرده و توضیح آن را بیابید.**

---

## دو منطق پایه‌ای

همان‌طور که [REALITY](https://github.com/XTLS/REALITY) می‌تواند ترافیک را شبیه‌سازی (تقلید) کند و به‌جای سایت دیگری جا بزند، **اصل اساسی در مقابله با سانسور این است که هزینه‌ی جانبی سانسور را بالا ببریم تا سانسورچی جرات اعمال فیلتر را نداشته باشد.**

من سال‌ها پیش از TLS و همینطور شبیه‌سازی رفتار زمانی و طولی ترافیک روی TLS حمایت می‌کردم، چون همین فلسفه را دارد. تجربه نشان داده که GFW حاضر نیست کل IPهای CDNهای بزرگ را بلاک کند چون در آن صورت سایت‌های عادی زیادی نیز از کار می‌افتند. **در نتیجه، هدف اصلی XHTTP از ابتدا این بود که پشت انواع مختلف CDN مخفی شود.**

اما CDNها (به جز استثنائاتی مثل WS یا gRPC) معمولاً کل درخواست HTTP را قبل از ارسال به سرور مقصد ذخیره می‌کنند. بسیاری از میانی‌باکس‌های HTTP هم همینطور عمل می‌کنند. بر این اساس، پروتکل Meek در Tor سعی می‌کرد با بسته‌بندی ترافیک داخل درخواست‌های جداگانه، از این فیلترها عبور کند — ولی سرعت آن پایین بود چون از مدل "جریان پایین" XHTTP استفاده نمی‌کرد.

**جریان پایین یعنی چه؟** تصور کن در حال دانلود یک فایل حجیم از یک سایت هستی؛ CDN چون کش ندارد، باید از سرور اصلی بگیرد. اما نمی‌تواند صبر کند تا کل فایل را دریافت کند، پس هر مقدار که دریافت می‌کند، بلافاصله برای کلاینت می‌فرستد. این همان منطق "جریان پایین" در XHTTP است — یعنی سرعت دانلود می‌تواند به حداکثر برسد.

در آپلود، برای سازگاری بهتر، XHTTP اول از مدل "packet-up" استفاده کرد، یعنی بسته‌بندی درخواست‌ها در POSTهای جداگانه. واضح است که سرعت آن نسبت به stream کاهش می‌یابد، ولی برای پروکسی‌های عمومی که عمدتاً ترافیک آپلود کمی دارند، قابل قبول است.

**بعداً، حالت stream-up معرفی شد، و وقتی هدر جعلی gRPC به آن اضافه شد، حتی امکان عبور از Cloudflare هم فراهم شد. همچنین بعد از چند بهینه‌سازی، سرعت packet-up نزدیک به stream-up رسید.**

> به‌طور خلاصه، تا زمانی که CDN حاضر نیست به‌طور پیش‌فرض ترافیک upstream را پشتیبانی کند، استفاده از آن برای ضد سانسور الزاماً نوعی «سوءاستفاده» به‌شمار نمی‌آید؛ بلکه اقدامی ضروری برای مقاومت در برابر فیلترینگ است.

---

## packet-up

**این بخش جزئیات فنی زیادی دارد. اگر توسعه‌دهنده نیستید، فقط بخش‌هایی که بولد شده‌اند را بخوانید.**

**packet-up یعنی همان مدل «ارسال به‌صورت بسته‌ای از بالا و دریافت جریانی از پایین» که بیشترین سازگاری را دارد. طراحی این مدل در XHTTP به‌صورت زیر است:**

1. **کلاینت از `POST /yourpath/sameUUID/seq` برای ارسال داده‌های آپلود استفاده می‌کند:**

   * UUID باید تصادفی باشد و با UUID دانلود یکی باشد تا سرور بتواند آن‌ها را مرتبط کند. در غیر این‌صورت بعد از ۳۰ ثانیه session منقضی می‌شود.
   * `seq` از ۰ شروع می‌شود و باید بلافاصله پس از ارسال بدنه درخواست قبلی، درخواست بعدی ارسال شود. نیازی نیست صبر کنید پاسخ بیاید.
   * ممکن است درخواست‌ها به ترتیب به سرور نرسند؛ سرور بر اساس `seq` مرتب‌سازی می‌کند و به‌صورت پیش‌فرض حداکثر ۳۰ عدد را بافر می‌کند. اگر بیشتر شد، اتصال قطع می‌شود.
   * UUID و seq هر دو در path قرار می‌گیرند نه در query string — این کار از بروز مشکلات خاصی جلوگیری می‌کند.

2. **برای شروع دانلود، کلاینت از `GET /yourpath/sameUUID` استفاده می‌کند. سرور در پاسخ این هدرها را اضافه می‌کند:**

   * `X-Accel-Buffering: no` برای غیرفعال‌کردن بافرینگ در میانی‌باکس‌ها
   * `Cache-Control: no-store` برای جلوگیری از کش شدن
   * `Content-Type: text/event-stream` برای شبیه‌سازی به SSE (می‌توان با `noSSEHeader` غیرفعالش کرد)
   * برای HTTP/1.1، هدر `Transfer-Encoding: chunked` نیاز است، ولی برای H2/H3 نه

3. برای جلوگیری از محدودیت‌های CORS در مرورگر، سرور در همه‌ی پاسخ‌های GET/POST این دو هدر را اضافه می‌کند:

   * `Access-Control-Allow-Origin: *`
   * `Access-Control-Allow-Methods: GET, POST`

4. **برای حل مشکل طول یکنواخت هدرها:**

   * در درخواست کلاینت، هدر `Referer: ...?x_padding=XXX...` افزوده می‌شود تا طول متغیر ایجاد شود. پیش‌فرض بین ۱۰۰ تا ۱۰۰۰ کاراکتر به‌صورت تصادفی است.
   * در پاسخ سرور، `X-Padding: XXX...` افزوده می‌شود که همان padding پاسخ است.
   * این‌ها با پارامتر `xPaddingBytes` قابل کنترل هستند.
   * پیشنهاد استفاده از Referer به‌جای گزینه‌های دیگر و کاراکتر `X` توسط [@rPDmYQ](https://github.com/rPDmYQ) داده شد (در huffman encoding، X برابر با 8 بیت است).

> **استفاده از `x_padding` باعث لاگ‌های طولانی می‌شود. می‌توانید در reverse proxy آن را فیلتر کنید تا لاگ تولید نکند.**

---

## stream-up

**stream-up یک جایگزین برای packet-up است که سرعت آپلود بالاتری دارد و بعد از نسخه v1.8.8 به XHTTP اضافه شد.** این حالت برای کاربردهایی با نیاز به آپلود زیاد مثل remote desktop، بازی‌های آنلاین یا برنامه‌هایی مثل Tailscale مناسب‌تر است.

stream-up دارای حالت پیش‌فرض و همچنین حالت ترکیبی با هدر جعلی gRPC است. حالت دوم امکان عبور از CDNهایی مانند Cloudflare را فراهم می‌کند.

### نحوه عملکرد stream-up

1. **کلاینت به‌جای چند POST، یک اتصال طولانی با POST باز می‌کند:**

   * درخواست به آدرس `/yourpath/uuid` ارسال می‌شود.
   * UUID باید با همان UUID دانلود یکی باشد.
   * بدنه‌ی POST به‌صورت stream ارسال می‌شود؛ هر بسته‌ی داده‌ای با طول (length) و سپس محتوای آن ارسال می‌شود. (مثل prefix-length encoding)

2. **سرور به‌صورت زنده داده را دریافت و استفاده می‌کند:**

   * به‌جای اینکه مثل packet-up منتظر بسته‌ها بماند، stream را مستقیماً پردازش می‌کند.
   * تأخیر کمتر و کارایی بالاتر برای آپلود دارد.

3. **برای عبور از CDN، از هدر gRPC جعلی استفاده می‌شود:**

   * سرور هدرهایی مثل `Content-Type: application/grpc` و `TE: trailers` را به پاسخ اضافه می‌کند.
   * این باعث می‌شود بسیاری از CDNها، این stream را به‌عنوان ترافیک gRPC در نظر بگیرند و اجازه عبور بدهند.

4. **تغییر حالت stream-up:**

   * اگر بخواهید از stream-up استفاده کنید، `mode` را روی `stream-up` تنظیم کنید.
   * اگر می‌خواهید هدر gRPC نیز استفاده شود، `fakeGrpc` را روی `true` قرار دهید.
   * برای سازگاری با برخی CDNها، `fakeGrpc` به‌صورت پیش‌فرض فعال نیست.

> به‌طور خلاصه، stream-up برای آپلود با کارایی بالا طراحی شده و با ترکیب هدر gRPC حتی از Cloudflare هم قابل عبور است. اگر نیاز به آپلود دارید، این بهترین گزینه است.


---


## جداسازی واقعی ارسال/دریافت (Split Transport)

**این ویژگی در هیچ‌کدام از پروتکل‌های قبلی مثل VMess، Trojan یا حتی gRPC وجود ندارد، ولی در XHTTP به‌سادگی ممکن شده است.**

فرض کنید:

* سرور شما به هر دلیلی نیاز دارد دانلود (downstream) را از طریق REALITY انجام دهد چون IP خاصی دارد یا سرعت بیشتری دارد.
* ولی آپلود (upstream) را از طریق QUIC و CDN بفرستید تا هم سرعت بالا برود و هم از فیلترینگ عبور کنید.

در XHTTP این کار ممکن است، چون **ارسال و دریافت از دو مسیر کاملاً جدا انجام می‌شود.** شما فقط کافی است:

* در بخش `receiver`، تنظیمات مربوط به `downlink` را بنویسید (مثلاً REALITY روی H2 با IP خاص).
* در بخش `sender`، تنظیمات مربوط به `uplink` را بنویسید (مثلاً H3 از طریق CDN با IPv6).

### مزایا:

* می‌توانید بسته به شرایط شبکه، بهترین مسیر را برای هر جهت انتخاب کنید.
* حتی اگر مسیر دانلود مسدود شد، همچنان آپلود قابل انجام است (یا برعکس).
* ترکیب IPv4/IPv6 در هر جهت ممکن است.

### نکات:

* برای اینکه این ساختار درست کار کند، **هر دو مسیر باید از یک UUID مشترک استفاده کنند.**
* برای رسیدگی به session و کنترل وضعیت، سرور از روی UUID تشخیص می‌دهد که uplink و downlink متعلق به یک connection هستند.

> این ویژگی در کنار قابلیت‌های دیگر XHTTP، آن را به یکی از منعطف‌ترین و قدرتمندترین پروتکل‌های عبور از سانسور تبدیل کرده است.

---

## XMUX (چندگانگی در XHTTP)

XHTTP به‌صورت داخلی از یک سیستم multiplexing (چندگانگی) به نام XMUX استفاده می‌کند. این سیستم به شما اجازه می‌دهد تا چند connection مجازی را روی یک اتصال فیزیکی اجرا کنید، شبیه به mux در gRPC یا HTTP/2 ولی با کنترل کامل سمت کاربر.

### مزایای XMUX:

* **تاخیر بسیار کم:** چون از یک ارتباط پیوسته استفاده می‌کند، نیاز به ایجاد مجدد handshake و TLS نیست.
* **پایداری بالا:** حتی اگر یک stream قطع شود، بقیه streamها بدون مشکل ادامه می‌دهند.
* **سازگاری با CDN:** چون همه چیز درون یک اتصال فیزیکی اتفاق می‌افتد، فقط کافی است یک اتصال از فیلترینگ عبور کند.

### نکات مهم استفاده:

* به‌صورت پیش‌فرض XMUX فعال است.
* اگر نیاز دارید چند stream همزمان ولی مستقل داشته باشید (مثل multi-threaded speedtest)، **باید `maxConcurrency` را روی 1 قرار دهید.** چون در غیر این صورت ممکن است بسته‌های همزمان تداخل کنند.
* در حالت packet-up یا stream-up، XMUX می‌تواند بدون مشکل مدیریت شود.

### تفاوت با mux قدیمی:

در نسخه‌های قبلی Xray از `mux` استفاده می‌شد که مبتنی بر multiplexing در لایه application بود. XMUX یک بازطراحی کامل بر پایه‌ی ویژگی‌های XHTTP است و با سرعت، امنیت و سازگاری بیشتری همراه است.

> **اگر از ابزارهایی مثل Clash یا sing-box استفاده می‌کنید که تست سرعت چندرشته‌ای انجام می‌دهند، برای سازگاری بیشتر، XMUX را محدود یا غیرفعال کنید.**


---

## XMUX (چندگانگی در XHTTP)

XHTTP به‌صورت داخلی از یک سیستم multiplexing (چندگانگی) به نام XMUX استفاده می‌کند. این سیستم به شما اجازه می‌دهد تا چند connection مجازی را روی یک اتصال فیزیکی اجرا کنید، شبیه به mux در gRPC یا HTTP/2 ولی با کنترل کامل سمت کاربر.

### مزایای XMUX:

* **تاخیر بسیار کم:** چون از یک ارتباط پیوسته استفاده می‌کند، نیاز به ایجاد مجدد handshake و TLS نیست.
* **پایداری بالا:** حتی اگر یک stream قطع شود، بقیه streamها بدون مشکل ادامه می‌دهند.
* **سازگاری با CDN:** چون همه چیز درون یک اتصال فیزیکی اتفاق می‌افتد، فقط کافی است یک اتصال از فیلترینگ عبور کند.

### نکات مهم استفاده:

* به‌صورت پیش‌فرض XMUX فعال است.
* اگر نیاز دارید چند stream همزمان ولی مستقل داشته باشید (مثل multi-threaded speedtest)، **باید `maxConcurrency` را روی 1 قرار دهید.** چون در غیر این صورت ممکن است بسته‌های همزمان تداخل کنند.
* در حالت packet-up یا stream-up، XMUX می‌تواند بدون مشکل مدیریت شود.

### تفاوت با mux قدیمی:

در نسخه‌های قبلی Xray از `mux` استفاده می‌شد که مبتنی بر multiplexing در لایه application بود. XMUX یک بازطراحی کامل بر پایه‌ی ویژگی‌های XHTTP است و با سرعت، امنیت و سازگاری بیشتری همراه است.

> **اگر از ابزارهایی مثل Clash یا sing-box استفاده می‌کنید که تست سرعت چندرشته‌ای انجام می‌دهند، برای سازگاری بیشتر، XMUX را محدود یا غیرفعال کنید.**


---

## نکاتی درباره پیاده‌سازی

* تمام کد XHTTP از ابتدا برای این معماری طراحی شده و patch یا hack روی ساختارهای قبلی نیست.
* سعی شده ماژولار، قابل نگهداری و ساده باشد. مسیرها واضح هستند، و هر بخش نقش خاصی دارد.
* در نسخه‌های اولیه، چندین refactor بزرگ انجام شد تا کدها تمیز و قابل توسعه باقی بمانند.
* ما تست‌های گسترده‌ای برای سازگاری با Cloudflare، Nginx، و سایر reverse proxyها انجام دادیم.
* عملکرد در سناریوهای مختلف (مانند real browser، proxy chaining، VPN داخلی، NAT) نیز بررسی شده است.

## سؤالات رایج

### چرا نامش XHTTP است؟

چون هم "eXtended HTTP" است و هم "Xray over HTTP".

### آیا جایگزین gRPC است؟

برای بسیاری از کاربردها، بله. XHTTP سبک‌تر، سریع‌تر و قابل انعطاف‌تر است. البته اگر ساختار فعلی‌تان با gRPC خوب کار می‌کند، نیازی به تغییر نیست.

### آیا با مرورگر هم کار می‌کند؟

بله، ما حالت سازگار با مرورگر (browser dialer) داریم که از packet-up استفاده می‌کند. همچنین CORS نیز پوشش داده شده.

### روی چه پورت‌هایی اجرا می‌شود؟

همان پورت‌هایی که برای HTTP(S) رایج هستند، مثلاً 80 و 443. چون از همان پروتکل‌ها استفاده می‌کند.

### چرا از WebSocket استفاده نکردید؟

WebSocket مشکلات زیادی در عبور از فیلترینگ دارد. علاوه بر آن، رفتار غیرقابل پیش‌بینی در برخی CDNها و ضعف در مدیریت جریان دارد. XHTTP برای این هدف‌ها طراحی شده و بهتر عمل می‌کند.

---

## در پایان: پیشنهاد شخصی من

امیدوارم این مقاله بتواند ذهن شما را روشن کند و مسیر شما را در استفاده بهتر از XHTTP هموار سازد. اگر تازه‌کار هستید، کافی‌ست با packet-up شروع کنید. اگر حرفه‌ای‌تر هستید، split transport و stream-up را تجربه کنید. اگر توسعه‌دهنده هستید، کدها را بخوانید و بازخورد دهید.

> **پیشنهاد می‌شود فعلاً از ترجمه رسمی به زبان‌های دیگر خودداری شود. بسیاری از مفاهیم هنوز در حال تغییر هستند. وقتی ساختارها پایدارتر شدند، ترجمه‌ها ارزش بیشتری خواهند داشت.**


</details>
