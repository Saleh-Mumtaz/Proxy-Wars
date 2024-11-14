# All-on-443
SSLH-EV Xray Nginx


<img width="847" alt="sslhev" src="https://github.com/user-attachments/assets/12b3d870-c980-463e-b7e8-2c9bce330d84">
<br />



**هدف:**<br /> 
میخواهیم در سرور فقط پورت ۴۴۳ و ۸۰ باز باشند. میخواهیم روی پورت ۴۴۳ سایت واقعی، پنل، اینباند ریلیتی و سی دی ان و پروکسی تلگرام و پروکسی socks5 و ssh داشته باشیم یا حداقل همیشه امکان اجراشون روی ۴۴۳ رو داشته باشیم به طوری که بقیه سرویس ها به مشکل نخورند.

به شخصه فکر میکنم ssh روی پورت ۴۴۳ نیازی نیست.<br />


پورت ۴۴۳ توسط SSLH اشغال میشه و بقیه برنامه ها باید از اون ترافیک خودشون رو تحویل بگیرن، که یعنی ایپی درخواست دهنده به دست سرویس نمیرسه مگر اینکه ویکی SSLH رو بخوانید و روش Transparent proxy رو اجرا کنید.


اینباند ریلیتی روی ۴۴۴۳ خواهد بود، انجینکس روی ۸۴۴۳، پروکسی تلگرام روی ۵۴۴۳ و ssh روی ۶۴۴۳.


مثل دفعه قبل دامنه ریلیتی باید تیک خاموش باشه. باید براش با اسکریپت gfw4fun هم انجینکس رو تنظیم کنیم هم گواهی امنیتی بگیریم.



این دفعه از سوکت دامنه استفاده خواهیم کرد، احتمال داره به خطا بر بخورید که بخاطر یا دسترسی نادرست دادن هست یا به ترتیب نادرست اجرا کردنش. نحوه کارکردنش به صورت زیر هست:<br />
اتصال ریلیتی معتبر<br />
[ client <--> sslh:443 <--> xray:4443 | check if it is authentic REALITY request ]<br />

انجینکس ترافیکی که از نظر هسته ریلیتی معتبر نیست رو میگیره و بر اساس url مسیریابی خودش رو روی اون انجام میده، یا سایت رو میاره یا پنل رو یا اینباند سی دی ان تشخیص میده رمزنگاری رو برمیداره برش میگردونه دست هسته.<br />
[ client <--> sslh:443 <--> xray:4443 <--> nginx:8443 <--> xray:inbound's_local_port ]<br />

اتصال به پنل توسط انجینکس<br />
[ client <--> sslh:443 <--> xray:4443 <--> nginx:8443 <--> x-ui:panel_port ]<br />

و در مورد MTProxy قبل فرستادن درخواست‌ها به هسته ایکس‌ری قرار میگیره.<br />
[ client <--> sslh:443 | check if sni == zula.ir <--> mtproxy:5443 ]<br />


تاکیدات قبلی، اسپایدرایکس رو خالی نگذارید، شورت ایدی رو خالی نگذارید هر کدوم رو اگر میتونید چندتا تعریف کنید و با , از هم جداشون کنید و به هرکاربر ترکیب متفاوتی بدید.<br />
در قسمت اسپایدر هر رشته ای از اعداد و حروف که میخواهید قرار بدید، من ed=2560? هم قرار دادم به مشکلی نخورد نمیدانم بدرد میخوره برای tcp یا خیر.<br />


ادرس اسکریپت https://github.com/GFW4Fun/x-ui-pro<br />


نصب کننده پنل تنظیم انجینکس و گرفتن گواهی برای سی دی ان. <br />


پنل رو پورتش رو یادداشت کنید یا با دستور x-ui و وارد کردن ۱۰ مشخصات رو بگیرید، لینکی که داده رو https هست تبدیل به htttp کنید و جلوی دامنه پورت پنل رو بگذارید، قبلش دیوار اتش رو باید خاموش کرده باشید.<br />




**حتما در هنگام ساخت پروتوکل اینباند ریلیتی رو که روی ویلس گذاشتید با پورت ۴۴۳ برید پایین امنیت بذارید روی ریلیتی بعد روی clients کلیک کنید و flow  رو روی xtls-rprx-vision تنظیم کنید.**<br />


عکس زیر دو کانفیگ پوشانده شده ریلیتی سلف استیل هستند و بقیه روی سی دی ان.



![Screenshot 2024-11-03 083003](https://github.com/user-attachments/assets/820e9639-91df-4626-b780-7cddc20d658e)


از هر سی دی انی که وبسوکت پشتیبانی کنه یا split رو قبول بکنه میتونید استفاده کنید.<br />
مشکل این روش محدود شدن شما به یک اینباند ریلیتی هست.<br />


![Screenshot 2024-11-03 084400](https://github.com/user-attachments/assets/4b8cde2e-7276-4778-bca4-c6ada979c2eb)


# راه اندازی

0: خاموش کردن دیوار اتش
```
ufw disable
```

نصب پنل 
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -panel 1 -cdn off"
```


نصب سایت فیک **به هیچ عنوان رد نشود**
```
bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -RandomTemplate yes
```


تنظیم کانفیگ انجینکس
```
sudo nano /etc/nginx/sites-available/yourdomain.com
```
<br />

برای هر دامنه مجزا باید اسکریپت اجرا بشه.برای دامنه های سی دی ان، ابتدا تیک خاموش باشه، بعد که اسکریپت گواهی دامنه رو گرفت و تنظیم کرد و تغییرات قبلی رو روی فایل اون دامنه اجرا کردید تیکش رو روشن کنید.<br />
در این جا فقط و فقط پورت 443 رو به ```unix:/etc/xray.socket``` تغییر بدید. چپ چین راست چینش شاید بهم بریزد عکس رو توجه کنید. دقت کنید دو خط دارای ۴۴۳ هستن، اونی که بین دو خط ۸۰ هست رو پاک کنید و فقط اخرین خط ۴۴۳ رو تغییر بدید.


![Screenshot 2024-11-03 091246](https://github.com/user-attachments/assets/292dc711-22a8-470f-8012-6aa05c1c55a9)


To this:


![image](https://github.com/user-attachments/assets/d052a7a4-37ab-4bcf-abe9-29a22056941e)



تست انجینکس
```
ln -fs "/etc/nginx/sites-available/yourdomain.com" "/etc/nginx/sites-enabled/"
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl restart nginx
```


ارور برخورد کردید این دستورات رو اجرا کنید سپس دستورات قبلی رو اجرا کنید.

```
sudo fuser -k 80/tcp
sudo fuser -k 443/tcp
sudo fuser -k 8443/tcp
sudo systemctl start nginx
```


# نصب پنل

ساخت اینباند ریلیتی
**تاکیدات قبلی، اسپایدرایکس رو خالی نگذارید، شورت ایدی رو خالی نگذارید هر کدوم رو اگر میتونید چندتا تعریف کنید(شورت ایدی رو خود پنل ثنایی میده و هر کلاینت هم فکر کنم رندوم یکی رو میذاره براش) و با , از هم جداشون کنید و به هرکاربر ترکیب متفاوتی از شورت ایدی و اسپایدرایکس بدید.<br />
در قسمت اسپایدر هر رشته ای از اعداد و حروف که میخواهید قرار بدید، من ed=2560? هم قرار دادم به مشکلی نخورد فکر نمیکنم بدرد این جا بخوره چون کاربرد اسپایدرایکس گفتن چیز دیگری غیر از path در ترنسپورت های دیگه هست.یعنی کاراکترهای این حالتی هم میتونید داخل هر اسپایدرایکس بگذارید.<br />**
**حتما در هنگام ساخت پروتوکل اینباند ریلیتی رو که روی ویلس گذاشتید با پورت ۴۴۳ برید پایین امنیت بذارید روی ریلیتی، بعد روی clients کلیک کنید و flow  رو روی xtls-rprx-vision تنظیم کنید. برای کلاینت های جدید پنجره ساختشون خودش فیلدش رو داره اونی که اخرش udp443 هست را انتخاب نکنید!**<br />


![image](https://github.com/user-attachments/assets/ae17859a-b43f-4507-a309-f06107c58fb1)
![image](https://github.com/user-attachments/assets/d56fe5b4-e872-4a85-b149-3cc0cdccab4e)
![image](https://github.com/user-attachments/assets/5e726e58-3d70-4f8f-affe-b19ddeda0fa2)



ساخت اینباند سی دی ان<br />


از gfw4fun میباشد:<br />


![image](https://github.com/user-attachments/assets/4db3a6c3-62a1-4abe-bda6-171cf0ad2bc7)
![image](https://github.com/user-attachments/assets/e9ab2e33-71b1-45f0-9b77-0469cd69d81f)
![image](https://github.com/user-attachments/assets/7f914763-564d-464f-a10d-c7bdde12e5cb)



# SSLH

قسمت سخت ماجرا که واقعا شاید ارزشش رو نداشته باشه.<br />


https://raw.githubusercontent.com/Saleh-Mumtaz/Proxy-Wars/refs/heads/main/sslh-ev


Create SSLH config:


```
nano /etc/sslh.cfg
```


Put the following block in that file:


Important! 2789 is my ssh port, change your ssh port which i mentioned how at the bottom of this page, and replace 2789 with your own number.


```
foreground: true;
inetd: false;
timeout: 2;
pidfile: "/var/run/sslh.pid";

# Listen on public port 443 for incoming connections
listen:
(
    { host: "0.0.0.0"; port: "443"; }
);

# Protocol definitions
protocols:
(

    # Forward SSH traffic to the local SSH server on port 2789
    { name: "ssh"; host: "127.0.0.1"; port: "2789"; },

    # Forward HTTPS (TLS) traffic to Xray on localhost:4443
    { name: "tls"; host: "127.0.0.1"; port: "4443"; }

);
```

Edit the service file

```
sudo nano /usr/lib/systemd/system/sslh.service
```

Delete all lines and paste this:<br />
(for vim, press esc, then press gg, then press d shift+g)


```
[Unit]
Description=SSL/SSH multiplexer
After=network.target
Documentation=man:sslh(8)

[Service]
User=root
RuntimeDirectory=sslh
EnvironmentFile=/etc/default/sslh
ExecStart=/usr/sbin/sslh-ev --foreground --config /etc/sslh.cfg
Backlog=100
KillMode=control-group
#Hardening
PrivateTmp=true
CapabilityBoundingSet=CAP_SETGID CAP_SETUID CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
SecureBits=noroot-locked
ProtectSystem=strict
ProtectHome=true
ProtectKernelModules=true
ProtectKernelTunables=true
ProtectControlGroups=true
MountFlags=private
NoNewPrivileges=true
PrivateDevices=true
RestrictAddressFamilies=AF_INET AF_INET6 AF_UNIX
MemoryDenyWriteExecute=true
DynamicUser=true

[Install]
WantedBy=multi-user.target
```

Then


```
sudo systemctl daemon-reload
sudo systemctl restart sslh
sudo systemctl status sslh
```

Remember that, sslh-fork is for small setups, vpn for 10 people, sslh-select is for medium setup, and ev can handle as much as your vps would have the resource to do so.



For ufw

Important! 2789 is my ssh port, change yours and replace 2789 with it.
```
ufw status
ufw reset
ufw default deny incoming
ufw default allow outgoing
ufw allow 80,443,2789/tcp
ufw allow 80,443,2789/udp
```
Change ssh port:
```
vi /etc/ssh/sshd_config
```
```
#Port 22
```
```
systemctl reload sshd
```
**Since ubuntu 24.04:**


https://ubuntuhandbook.org/index.php/2024/04/install-ssh-ubuntu-2404/
edit sshd_config but for changes to take effect:
```
sudo systemctl daemon-reload
```
```
sudo systemctl restart ssh.socket
```
To revert to previous mode:


twinsen said: Long story short - for all those who do not like this change:
```
systemctl disable --now ssh.socket
systemctl enable --now ssh.service
```


UFW at last
```
ufw enable
```


Why did you keep ssh exposed? wasn't this for having ssh on 443?


Yes but to make sure everything works fine, we will let this setup work for a couple of days.<br />You can be reckless and delete your ssh port from ufw rules, but doesn't worth it, first check for a day, then do that.


# About SSLH

Wouldn't SSLH crash? if you configure it correctly, no, but nothing is 100 percent reliable.<br />Despite its documents which are not very clear for configuration, this program is very powerful and would not crash.<br />**A lot of companies and servers** which we don't know are using this or similar softwares to allow ssh to their servers on 443 port, becuase it is the only exposed port along 80.

