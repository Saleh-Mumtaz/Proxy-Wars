# All-on-443
XRAY-REALITY-VISION-SELF-STEAL + VLESS-CDN-WS + WEBSITE + WEBPANNEL SIMULTANEOUSLY  ALL ON port 443


<img width="738" alt="443" src="https://github.com/user-attachments/assets/294fbd7e-a445-4969-83c8-6821795fb88e">
<br />



**هدف:**<br /> 
ریلیتی و کانفیگ سی دی ان و پنل و سایت html همگی همزمان روی پورت ۴۴۳، به کمک ایپی و دامنه تمیز. **توجه کنید:** ایپی که فکر کردید تمیز بوده، پینگ گرفتید مشکل نداشت و این صحبتها(برای ریلیتی)، یا دامنه‌ای که داشتید فکر کردید تمیز و کامل clean هست(برای cdn)  اما در واقع قبلا فلگ شده و شما اطلاع نداشتید، دوباره روش ترافیک بره مطمئن باشید دوباره مسدود میشه. در مورد ایپی، سابنت های هتزنر یا aeza خیلی پیش میاد.(برید اخرین بخش در این صفحه ([ترجمه چند کامنت](https://github.com/Saleh-Mumtaz/Proxy-Wars/blob/main/first-order.md#%D8%AA%D8%B1%D8%AC%D9%85%D9%87-%DA%86%D9%86%D8%AF-%DA%A9%D8%A7%D9%85%D9%86%D8%AA)) متوجه میشید)<br />شاید بگید رولز کلاودفلر و پورت دستی در رکورد اروان هست چرا سی دی ان روی ۴۴۳ بیاریم، تا زمانی این جواب میداد، بعد سی دی ان های داخلی هم ترافیکشون از دیوار اتش عبور کرد، و کانفیگ های اشتباه سی دی ان ایرانی همگی بلاک شدن.<br />


کانفیگ ریلیتی روی پورت ۴۴۳ و انجینکس روی پورت ۸۴۴۳ خواهد بود.


دامنه ای که sni ریلیتی هست تیک پروکسی سی دی انش باید خاموش باشه، اسکریپت روی سرورمون براش سرتیفیکت میگیره.<br />



فیلد sni در اینباند ریلیتی باید همین دامنه تیک خاموش باشه.


در اینباند ریلیتی dest باید روی 127.0.0.1:8443 باشه، علت: ایکس‌ری میاد هر اتصال غیر ریلیتی رو میفرسته به destو در نتیجه وقتی کسی غیر یوزر ریلیتی درخواستش بهش برسه بدون هیچ تغییری در اون میفرستش دست انجینکس.<br />
این شامل تمام درخواست های اینباندهای سی دی ان، درخواست از طرف مرورگر افراد برای باز کردن دامنه ما، و درخواست شما برای باز کردن پنل که شامل دامنه و مسیر پنل هست میشه. بدون هیچ تغییری میرسن دست انجینکس. پس این یعنی اگر مهارت کار با انجینکس داشته باشید، میتونید پروکسی تلگرام روی ۴۴۳ با فیک تی ال اس و حتی اس اس اچ روی ۴۴۳ بیارید بالا ولی من فعلا چنین کاری رو یاد ندارم و ابزار SSLH رو پیدا کردم برای اینکار.


چرا مدیریت کننده پورت ۴۴۳ ایکس‌ری باشه نه انجینکس؟ در گروه روسی زبان هسته به این نتیجه رسیده شد که اون برنامه مواجه شونده با رکوئست های غیر مجاز(چه افراد چه سیستم فیلترینگ) خود هسته باشد بهتر از انجینکس هست. وگرنه شما میتونید با بلوک استریم و pre read درخواست های ریلیتی رو بفرستید برای هسته و این انجینکس باشه که در ۴۴۳ درخواست ها رو مدیریت کند.


دوستان پرسیده بودن چرا ریلیتی رو نمیشه پشت سی دی ان انداخت، علتش استفاده از tcp(stream) هست، سی دی ان میخواد محتوا رو کش کنه لاکن ما میخواهیم در لحظه همه چیز به سرور برسه. برای همین در اینباندهای سی دی ان از ws و grpc یا split استفاده میشه.اینهاهم خودشون روی یک اتصال tcp هستن، ولی انتقال به صورت message-oriented انجام میشه.<br />
مشکل ما با سی دی ان خارجی اون تشخیص tls-in-tls و گاهی utls بود(مگر اینکه کانفیگ سی دی ان بیارید بالا که http خالی باشه)، من خودم روی یک دامنه ۷۵۰ گیگ با کلاودفلر عبور دادم بعدش طوری شد سایت بالا می‌امد ولی وی پی ان وصل نمیشد، فقط اتصال مرورگر واقعی جواب میداد.(راه حلش browser dialer هست، 2dust گفت علاقه ای به اجراش نداره سخته براش) البته که بعدا روش همین ریلیتی رو اجرا کردم و بدون مشکلی کار کرد. 


اسکریپت زیر از GFW4FUN برای ما پنل و انجینکس رو نصب میکنه و گواهی امنیتی دامنه رومیگیره. <br />
https://github.com/GFW4Fun/x-ui-pro



در عکس زیر هوهنهایم روی سی دی ان و دوتای دیگه مستقیم همگی روی یک سرور همزمان هستند.(خاموش کردن اروان بعد از ۵۰ گیگ علت دیگری داشت اما برای تست خوب بود. که بدانید همزمان از سی دی ان اروان ابردراک زس اراز و کلاودفلر جی کور فستلی و همه و همه روی یک پورت ۴۴۳ همگی tls میتونید استفاده کنید، شرط لازم و کافی پشتیبانی از وبسوکت یا splithttp هست. )<br />


![Screenshot 2024-11-03 083003](https://github.com/user-attachments/assets/820e9639-91df-4626-b780-7cddc20d658e)
![80](https://github.com/user-attachments/assets/b73829b5-b8a3-4827-855c-b4c26b7b2abc)

توضیح تصویر پورت ۸۰: اگر به کانفیگ انجینکس دقت کنید، اون به پورت ۸۰ هم گوش میده، برای همین هست وقتی پنل از کار میفته روی ۴۴۳ وقتی s رو از https حذف کنید بالا میاد. و درنتیجه میتونید کانفیگ سی دی ان با پورت ۸۰ داشته باشید بدون دخالت اینباند ریلیتی که ۴۴۳ رو اشغال کرده. یک کانفیگ که رمزنگاری نشده. به شرط اینکه در کلاودفلر always use https و hsts **غیرفعال باشن** و حالت انکریپشن روی Full(strict) و Strict **نباشه**. دقت کنید، با منطقی که در [پیش به سوی ستارگان](https://github.com/Saleh-Mumtaz/Proxy-Wars/blob/main/first-impact.md) توسط یوهان توضیح داده شده، شما از این هم که استفاده کنید، به هرحال تلگرام یا گوگل یا هر سایت دیگه ای نمیان که رمزنگاریشون رو کنار بگذارن، پس داده های دیده شده در کانال هم رمزنگاری شده هستن، اما یک اشکال **بسیار بزرگ** داره که به همین علت **نباید** از این استفاده کنید مگر برای تست یکسری چیزها یا شرایط به شدت استثنایی، و اون این هست که تمام مراحل Handshake هم دیده میشن که این خوب نیست.اون پنج بسته همه به صورت کامل دیده میشن. مشکل tls-in-tls رو **نداره** اما خب fingerprint هم نداره.(برای حل این مسئله میتونید از cdn ایرانی استفاده بکنید) **پس تبعات استفادش به عهده خودتون هست**.<br /> ساخت کانفیگش توی **پنل** مشابه همون تی ال اس دار خواهد بود(فقط اکسترنال پروکسی پورتش باید ۸۰ باشه، اونجا ۴۴۳ هست)، اما، در کانفیگ کاربر برخلاف اون بخش امنیت خالی هست. پایین تر که برید متوجه حرفم میشید.<br />
> [!CAUTION]
> تبعات استفادش به عهده خودتون هست.

![nake](https://github.com/user-attachments/assets/437972c6-e22f-4365-a122-f54127f4cf65
)




از هر سی دی انی که وبسوکت پشتیبانی کنه یا اگر نداشت از splithttp پشتیبانی کنه میتونید استفاده کنید، همزمان هم میتونید استفاده کنید .<br />
برای هر سی دی ان قاعدتا باید دامنه مجزا واردش کنید.


مشکل این روش محدود شدن شما به یک اینباند ریلیتی هست، اما با وجود داشتن شورت ایدی و uuid مشکلی نیست.<br /> اگر اتصال با اختلال کم میخواهید از سی دی ان ایرانی استفاده کنید ولی تا جای ممکن مستقیم وصل بشید.


![443](https://github.com/user-attachments/assets/812788ee-0cb7-4454-b768-4f463586eec9)



## Setup

اول دیوار اتش سرور رو غیرفعال میکنیم.
```
ufw disable
```



از اسکریپت این دوست چینی برای نصب و تنطیم پنل انجینکس و دامنه استفاده میکنیم.
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -panel 1 -cdn off"
```
در جریان باشید اسکریپتش خیلی چیزهای اضافی مثل تور سایفون وارپ وی۲ری پنل نصب میکنه، کرون تب ریستارت انجینکس هسته ایکس ری روزانه و تمدید گواهی های امنیتی به صورت ماهانه هم تنظیم میکنه.<br />


**اون لینکی که برای پنل میده رو، کپی کنید**، s رو از https پاک کنید، پورت ۸۰ دست انجینکس هست، و تا موقع راه اندازی کامل با این لینک پنل رو مدیریت کنید. بعد از پایان کار با همون لینک https بدون پورت میتونید وارد بشید، قبل از بستن این ادرس http تو یک تب دیگه چک کنید که پنل https بالا میاد یا خیر.


اسکریپتش برای نصب دامنه های بعدی میتونه بارها و بارها اجرا بشه، اما داره هربار کد رو تغییر میده و یهو دیدید زد یک چیزی رو خراب کرد سرور کلا خوابید، من خودم هرچی اضافه داره رو حذف کردم، از یک نسخه به بعد تنظیمات انجینکسش دیگه دست نخوردن، و بنظرم دیگه نیازی هم نداریم بیشتر از اون.<br />


**مهم:** **حتما حتما سایت فیک رو بالا بیارید**، در اموزش ویدئویی و اموزش متنی قبلی خود من تاکیدی نشد اما اینبار تاکید میکنم این دستور رو اجرا کنید و یک سایت اچ تی ام ال خودش بالا میاره.
```
bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -RandomTemplate yes
```

## Nginx


حالا کانفیگ انجینکس برای دامنه رو تغییر میدیم: 
```
sudo nano /etc/nginx/sites-available/yourdomain.com
```
<br />


۴۴۳ ها رو تغییر بدید به ۸۴۴۳<br />
شما میتونید از سوکت دامنه یونیکس unix domain socket برای اتصال انجینکس و ایکس‌ری استفاده کنید، راحت هست اما اینجا نیاوردمش.تعداد اتصالات تی سی پی لوکال بین انجینکس و ایکس‌ری رو بسیار کاهش میده، من ۳۰۰۰ تا کانکشن در پنل میدیدم با اجرای این روش اومد ۵۰ تا، علت پیشنهادم این هست که سازنده ایکس‌ری و ویکی لینوکس گفتن از پورت لوکال عملکرد بهتری داره و در استفاده های تعداد بالا خودش رو خیلی خوب نشون میده.


![Screenshot 2024-11-03 091246](https://github.com/user-attachments/assets/292dc711-22a8-470f-8012-6aa05c1c55a9)


To this:


![Screenshot 2024-11-03 091207](https://github.com/user-attachments/assets/e395e357-5959-4c1d-b7ce-1c21de24cbc4)


تست کانفیگ جدید انجینکس و راه اندازه دوباره
```
ln -fs "/etc/nginx/sites-available/yourdomain.com" "/etc/nginx/sites-enabled/"
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl restart nginx
```


اگر به ارور برخورد کردید:
```
sudo fuser -k 80/tcp
sudo fuser -k 443/tcp
sudo fuser -k 8443/tcp
sudo systemctl start nginx
```
و دستورات بلوک قبلیش رو دوباره اجرا کنید. یا می‌تونید که خط زیر رو در ```vim /lib/systemd/system/nginx.service``` اضافه کنید، یه خط جدید قبل ExecStartPre ایجاد کنید و اضافه اش کنید:
```
ExecStartPre=/bin/rm -f /etc/xray.socket
```
دقت کنید که بعد تغییرات شما باید دو خط با ExecStartPre شروع شوند. بعد دستورات زیر رو بزنید که تغییرات اعمال بشن:
```
systemctl daemon-reload
systemctl restart nginx
```

## Xray REALITY Inbound

## self-stealing<br />


![image](https://github.com/user-attachments/assets/e7d96b83-027c-415f-b5d0-1255f8d4a0c9)
![image](https://github.com/user-attachments/assets/b040064f-c7ec-47cb-8f0b-6397eba2f464)

پورت ۴۴۳، پروتوکل ویلس، بعد میرید پایین security رو میگذارید روی ریلیتی، بعد روی clients کلیک میکنید باز بشه، بعد براش flow رو روی **xtls-rprx-vision** تنظیم میکنید، اما حتما پس از این که کاربر جدید هم ساختید برای اون ها هم موقع ساخت این رو تنظیم کنید در همون پنجره کوچکی که باز میشه برای ساخت گزینش **(flow)** خواهد بود، xtls-rprx-vision-udp443 رو انتخاب **نکنید**!<br />
شورت ایدی ها رو **پاک نکنید!**
اسپایدر ایکس رو خالی **نگذارید**، میتونید چندین اسپایدرایکس قرار بدین داخل این فیلد، سازنده گفته به ازای هر کلاینت متفاوت بدین ولی خب امکانش رو من ندیدم چطور تنظیم کنیم که هربار کپی کردن یک رندوم رو بردارد، باید به مهندس ثنایی بگیم همونطوری که تنظیم کرده با هرکلاینت شورت ایدی متفاوتی بده باید برای اسپایدرایکس هم بگیم اگر میتونه این رو قرار بده.از اون سمت هم میگن در بهتر کردن اتصال تاثیری نداره، پس اصلا چرا هست رو خود rprx میدونه.

## self signed certificate
- ریلیتی بدون دامنه(اون هدفی که اصلا براش طراحی شده بود)<br />
انجینکس رو خودتون نصب و راه اندازی کنید، از اسکریپت اشاره شده هیچ کدوم اجرا نکنید مگر اون دستور بالا اوردن سایت فیک، که اون رو بعد از نصب و راه اندازی دستی انجینکس باید انجام بدین.
داخل یک دایرکتوری مثل /root یا /etc برید، دستورات زیر رو به ترتیب بزنید، openssl باید نصب باشه.
```
openssl ecparam -name prime256v1 -genkey -noout -out google.key
openssl req -x509 -nodes -days 365 -key google.key -out google.crt -subj "/CN=google.com"
openssl x509 -text -noout -in google.crt
```
دستور اخر وضعیت گواهی های cn رو بررسی میکنه، این گواهی ها فقط توسط سرور شما پذیرفته شده هستن، ارتباطی با خود گوگل ندارند.<br />
حالا باید انجینکس رو کانفیگ کنید.
```
vim /etc/nginx/sites-available/google.com
```
بلوک زیر رو داخلش کپی کنید و ذخیره کنید بیاید بیرون، مسیر گواهی های امنیتی من در /root ، بودن برای خودتون تغییرش بدید.
```
server {
	server_tokens off;
	server_name google.com *.google.com;
	listen 80;
	listen 8443 ssl;
 	listen [::]:80;
        listen [::]:8443 ssl;
	index index.html index.htm index.php index.nginx-debian.html;
	root /var/www/html/;
	ssl_protocols TLSv1.3;
	ssl_ciphers HIGH:!aNULL:!eNULL:!MD5:!DES:!RC4:!ADH:!SSLv3:!EXP:!PSK:!DSS;
	ssl_certificate /root/google.crt;
	ssl_certificate_key /root/google.key;
	if ($host !~* ^(.+\.)?google.com$ ){return 444;}
	if ($scheme ~* https) {set $safe 1;}
	if ($ssl_server_name !~* ^(.+\.)?google.com$ ) {set $safe "${safe}0"; }
	if ($safe = 10){return 444;}
	if ($request_uri ~ "(\"|'|`|~|,|:|--|;|%|\$|&&|\?\?|0x00|0X00|\||\|\{|\}|\[|\]|<|>|\.\.\.|\.\.\/|\/\/\/)"){set $hack 1;}
	error_page 400 402 403 500 501 502 503 504 =404 /404;
	proxy_intercept_errors on;
}
```
این دستورات بزنید تا فایل جدید رو بشناسه
```
ln -fs "/etc/nginx/sites-available/google.com" "/etc/nginx/sites-enabled/"
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl restart nginx
```
در sni باید www.google.com بگذارید، و در dest هم، 127.0.0.1:8443 رو قرار بدید. flow و شورت ایدی و اسپاید و غیره همه مثل اینباند قبلی، self-steal.<br />
این روش رو برخلاف قبلی من روش ترافیک سنگین نداشتم، نمیدونم اگر شرایط ساختی که گفته شد و داشتن ایپی واقعا تمیز رو رعایت کنید چقدر دووم میاره.
> [!CAUTION]
> ~~این روش توصیه نمیشه، من پکت های Client Hello و Server Hello در این حالت با وایرشارک ضبط کردم. OCSP stapling و Certificate chain and validity توسط دیوار اتش در اینجا میتونن چک بشن.~~<br />
> این روش توسط ایرانسل شناسایی می‌شود، لطفا از یک دامنه و ایپی کاملا تمیز برای کار کردن روی همه اپراتورها استفاده کنید. در کمال تعجب ایرانسل بعد از پنج شش روز شناسایی و فیلتر کردن ایپی سرور من با دیوار اتش بقیه اینترنت کشور سینک نکرده و روی باقی کار میکند که تا این لحظه 27.09.1403 14:41 حدود 3.6TB ترافیک عبور کرده است. انتشار کانفیگ ۱۰ روز پیش بوده است.
## Xray CDN Inbound


- **نحوه ساخت اینباند سی دی ان از صفحه دوست چینی:**<br />
توجه کنید که اینجا دامنه ای که تیک پروکسی سی دی انش روشن هست استفاده کنید. مطابق ویدئوی اموزشی.
**مهم** حتما utls رو روی chrome بگذارید، رندوم که از همین ها یکی انتخاب میکنه اما رندومایزد هربار یک مشخصه میسازه و در صحبت با دوستان چینی اینطوری گفتن همون بهتر chrome باشه برای همه.
- مشکل tls-in-tls برای سی دی ان همچنان باقی هست، چرا که xtls-rprx-vision برای وبسوکت وجود نداره. بستگی به دامنتون داره چقدر طول بکشه تا مسدود بشه. تاکید روی استفاده از مستقیم هست، اگر امکانش به هیچ عنوان نیست سی دی ان ایرانی استفاده کنید.


من در اموزش متنی خودم منظورم این بود برای سی دی ان هم باید اول تیک پروکسی سی دی ان رو خاموش کنید بعد براش اسکریپت gfw4fun رو اجرا کنید، تنظیم که شد برید پورت های داخل کانفیگ اون دامنه رو مثل دامنه ریلیتی تغییر بدید.بعد هم برگردید تیک سی دی ان رو روشن کنید، تا برای اونها هم سایت بالا بیاد. نحوه پاسخ انجینکس به درخواست ورودی که براش سایتی تنظیم نشده را نمیدونم چه هست، احتمال زیاد سایت بالا نخواهد امد یا حداقل https نخواهد بود، چون گواهی های امنیتی اون دامنه رو نداره.

From https://github.com/GFW4Fun/x-ui-pro#server-configuration-wrench <br />



![image](https://github.com/user-attachments/assets/4db3a6c3-62a1-4abe-bda6-171cf0ad2bc7)
![image](https://github.com/user-attachments/assets/e9ab2e33-71b1-45f0-9b77-0469cd69d81f)
![image](https://github.com/user-attachments/assets/7f914763-564d-464f-a10d-c7bdde12e5cb)




For ufw

فعالسازی دوباره دیواراتش سرور، پورت ۲۷۸۹ مربوط به اس اس اچ من هست با مال خودتون عوض کنید، همچنین اگر امکانش بود پورت پنل رو باز نگذارید و موقع ضرورت قوانین دیواراتشتون رو بروز کنید که پورت پنل رو اجازه بده تغییرات یا درست کردنش رو انجام بدید بعد برگردونید به حالت قبلی. تمام هدف این هست پورت های کمی از سرور باز باشن، و پاسخ سرور به درخواست های نامعتبر مثلا درخواست های سیستم فیلترینگ واکنش یک سایت واقعی رو بده.<br />
```
ufw status
ufw reset
ufw default deny incoming
ufw default allow outgoing
ufw allow 80,443,2789/tcp
ufw allow 80,443,2789/udp
```

فعال سازی در اخر
```
ufw enable
```
# تانل ریورس خود هسته ایکس‌ری
همه این کارها رو، یا حتی اون اموزش SSLH-EV رو میشه روی یک سرور ایران اجرا کرد، بعد یک کلاینت در اینباند ریلیتی بسازید، کانفیگ رو خروجی بگیرید که وارد اوتباندهای پنل خارج کنید. بعد برای ریورس خود هسته اول خارج رو تنظیم، بعد بیاید ایران رو تنظیم کنید. مشکلی پیش اومد یا نمیخواید ایپی خارجتون به ایران وصل بشه، میتونید همه این کارها رو انجام بدید روی سرور ایران اما، روی همین سرور ایران  ارگو تانل کلاودفلر اجرا کنید، یک کانفیگ بسازید با ارگو، حالا این دفعه این کانفیگ ارگو رو وارد اوتباندهای پنل خارج کنید و مثل قبل عمل کنید، که اینبار اون با واسطه کلاودفلر وصل بشه و ایپی های کلاود رو ایران ببینه. کانفیگ کاربر هم که همین ریلیتی هست.
# چرا بازگشت به ریلیتی؟
سازنده هسته یعنی rprx و یکی از دوستانش fangliding در گفت‌وگوی گیتهاب گفتند بنظرشون این تشخیص ریلیتی نیست و مشکل از اشتباه کانفیگ کردن ما هست. یا اینکه دیوار اتش داره همه port forwarding ها رو میبنده و تا زمانی که ببنده این روش جواب نمیده.<br />
https://github.com/XTLS/Xray-core/discussions/3269<br />در همون بحث خود rprx گفته من چندین روش جدید قدرتمندتر هم ساختم و برای روز مبادا دارم برای وقتی که ریلیتی رو در چین مسدود کنند، تا اون زمان ارائشون نمیکنم، fangliding هم توضیح داد که در dest و sni وقتی سایتی مثل گوگل یا زولا میزنید، یا هرسایت معروف یا غیر معروف که ایپیش روی سابنت شما نیست،  دیوار اتش با دادن رکوئست که توسط ایکس‌ری میرسه به ‌dest و پینگ کردن ‌sni متوجه میشه شما ایپیت با ایپی اون sni همخوانی نداره و دقیقا مشابه یک port forwarder تشخیصت میده. <br />همچنان دوستان ایرانی این حوزه معتقد هستن خیر فیلترینگ ما به قدرت تشخیص ریلیتی رسیده. به هرحال، دوستان چینی برای حل این مسئله از sni shunting استفاده کرده اند، ترجمش میشه از مسیر اصلی خارج کردن sni، وقتی رکوئست دیوار اتش رسید به هسته و اون دادش به انجینکس، انجینکس مثلا گوگل یا زولا رو نمایش بده اما ایپی که به دست ارسال کننده درخواست میرسه ایپی سرور ما باشه.(این چیزی بود که حدس میزدم باشه، ولی طور دیگریست) بنابراین شما میتونید شانتینگ رو هم تست کنید، چون به هرحال در SNI گوگل یا هر سایت لیست سفید دیگری خواهد بود و خب این بهتر هست، ~~مگر اینکه با یک تغییر ساده SNI سایت های مشکوک رو خود دیواراتش ایران به ایپی تبدیل کنه و با ایپی مقصد مقایسه کنه.~~ وقتی دارید هتزنر استفاده میکنید و SNI گوگل هست این واضح است که هتزنر با گوگل نه ایپی و نه ASN مربوط به هم دارند، پس شاید، شاید برای سایت های غیرمعروف جواب بده، که اون موقع به این نتیجه میرسیم چرا یک دامنه نخریم و محکم کاری نکنیم.<br />
(من فکر میکنم نحوه تشخیص اینکه سرور ما به اون sni استفاده شده ارتباطی نداره رو نمیتونه به وسیله اینکه خودش بیاد پینگ کنه و ببیند تو asn اون هستیم یا نه انجام بده، اینکار غیرممکن هست، اول از همه گوگل Google LLC  [هشت میلیون](https://ipinfo.io/AS15169) ایپی داره، چطور میخواد بفهمه کدام ایپی رو چک کند کدام اتصال به گوگل؟ خب فرض کنیم resolve کرد ایپی ها همخوانی نداشت، گوگل ایپی بسیار زیاد داره. میخواد هرچه همخوانی نداشت ببنده؟ حل این مشکل براش شاید این باشه بیاد برای این دامنه ها، دی ان اس داخل کشور رو دستکاری کند یک سری ایپی مشخص تعریف کند، خارج از این بازه را چک کند، مشکل بعدی این است که مگر گوگل فقط یک سایت هست؟ سرویس های بسیار متنوعی داره، همگی هم ایپی ها گوگل هستند) <br />
https://github.com/chika0801/Xray-examples/tree/main/VLESS-Vision-REALITY/nginx_sni_shunting


بعد خواندن این بحث من با توجه به مشخصات سیستم حاکم بر کشور مطمئن شدم چیزی که ما داشته باشیم و چین و روسیه نداشته باشند وجود ندارد، خصوصا در حوزه it، برای صحت سنجی یک کانفیگ ریلیتی در مهسا انجی پخش کردم.بعد از یک ترابایت از همه اپراتورها از سراسر کشور در نهایت ایپی خورد.(دامنه کاملا سالم هست و من روش دوباره ریلیتی بالا اوردم تا الان ۵۰۰ گیگ عبور داده، بعضی دوستان همین اخیرا در گروه ها میگفتن با ریلیتی ما تو ۳ ماه تا ۵ ترابایت هم عبور دادیم قبل زده شدن ایپی.


ستاپ اشتباه بود، ریلیتی پورت ۸۴۴۳ و سایت ۴۴۳. همچنین احتمال دخالت گزارشگران ایپی به کمک یک نرم افزاری مثل وایرشارک ولی برای اندروید هم بود. یا حتی اجرای مهساانجی روی بلواستک و استفاده از همان وایرشارک. چرا این حرف رو میزنم؟ علتش این هست که روی همون سرور بالا(سروری که بعدش حالت سایت و ریلیتی روی یک پورت رو اجرا کردم نه سرور کانفیگ مهساانجی که بعد یک ترا فیلتر شد) من ستاپ اشتباه رو پیاده کردم، ۱۵۰ گیگابایت ازش رد شد، همه روی همراه اول، بدون بسته شدن ایپی.


![image](https://github.com/user-attachments/assets/e20a2057-cb5a-4696-ac89-7ccc7260c40f)

![image](https://github.com/user-attachments/assets/39066134-7f1c-4bc0-bba0-489d389136ad)

![image](https://github.com/user-attachments/assets/e1f68c98-a7a4-435f-89a3-524f1d161786)


همراه اولی ها ۵۶۰ گیگابایت مصرف کردند، ایرانسلی ها ۲۶۰ گیگابایت.



**یک نکته** از سازنده پنل کافکا رو دیدم که بد نیست اینجا هم بگم: هدف ساخت وی‌پی‌ان مبتنی بر tls این هست که ترافیک ما کاربران و سرورمون به طور کامل شبیه به بازدید از یک سایت https(اتصال همه کاربران فقط به ۴۴۳) عادی باشه.



به کمک REALITY به عنوان security اینباند، مشخصه های یک اتصال tls به طور بی نقص شبیه سازی میشه (tls1.3 x25519)


مشخصه های یک مرورگر واقعی رو utls میسازه(chrome) (توجه: کتابخانه utls باز هست و فیلترچی میتونه براحتی بررسیش کنه و بالاخره مشخصه هایی ازش که نیاز داره رو بدست بیاره تا تشخیصش بده(هنوز نکرده یا نتوانسته))(Fingerprint)


و xtls-rprx-vision مشکل تشخیص tls-in-tls رو حل میکند.(Deep Packet Inspection DPI)


فالبک هم که در اینباند ریلیتی destination شده به درخواست های نامعتبر واکنش یک سایت واقعی را نشون میده.(ACTIVE PROBING)

# ترجمه چند کامنت
RPRX [Comment](https://github.com/net4people/bbs/issues/327#issuecomment-1935672503) توسعه دهنده اصلی هسته ایکس‌ری<br />
![image](https://github.com/user-attachments/assets/06ffe194-2382-477c-aca3-bbf93510d028)
![image](https://github.com/user-attachments/assets/8a94be93-2a36-4521-80aa-c3f9cc354728)
![image](https://github.com/user-attachments/assets/a5341a95-71cc-42d1-a6d3-b32e2485e9a0)
![image](https://github.com/user-attachments/assets/51e9d8d4-e0e9-4d3b-8628-90313168db71)
![image](https://github.com/user-attachments/assets/71654801-2b0c-40d1-826c-0e4d28222d64)








