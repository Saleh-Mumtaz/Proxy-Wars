# All-on-443
XRAY-REALITY-VISION-SELF-STEAL + VLESS-CDN-WS + WEBSITE + WEBPANNEL SIMULTANEOUSLY  ALL ON port 443


<img width="738" alt="443" src="https://github.com/user-attachments/assets/294fbd7e-a445-4969-83c8-6821795fb88e">
<br />



**Goal:**<br /> 
REALITY and CDN inbounds on 443 simultaneously on public 443 port.

xray REALITY inbound on port 4443, nginx on port 8443.


REALITY SNI will be our domain which we get SSL certificates on our own VPS.<br />Important: CDN/Cloud-sign should be off! Domain should resolve into our VPS'S IP.


The first SNI in this field, should be the A record without CDN proxy. Other SNIs with different CDNs should be after that.<br />
Before receiving certs do not activate CDN proxy on CDN SNIs.


dest of REALITY wil be 127.0.0.1:8443. XRAY will forward any non-REALITY traffic without any changes to 8443, nginx.<br />
That will contain the whole URL. path of the site, panel, or CDN configs, will be deciphered by nginx and routed back to xray inbound with a random port, or to its own site or x-ui panel.


Here in the **gfw4fun** script, we have /port/anytext path for cdn configs.<br />
In REALITY inbound, put /anytext in the spider field.<br />


https://github.com/GFW4Fun/x-ui-pro<br />
This script will install the panel, and configure nginx for url-path-based routing.<br />
3x-ui web panel will be accessible without a /port/random-path/ but only /random-path/ is enough and will access the panel. CDN configs with random port paths: /port/anytext but the port in the client config should be 443, use an external proxy to set that.<br />



Use shortids(It is now used by default in 3x-ui).<br />

**Important!
**<br />**Do not forget to choose xtls-rprx-vision in clients section.**<br />


In the following picture, hohenheims are on Cloudflare CDN, Last two configs are direct to vps with domain. All on the same vps at the same time.


![Screenshot 2024-11-03 083003](https://github.com/user-attachments/assets/820e9639-91df-4626-b780-7cddc20d658e)


You can use all the CDNs you want simultaneously for different inbounds.<br />
The downside is that you are limited to a single REALITY inbound.<br />


![Screenshot 2024-11-03 084400](https://github.com/user-attachments/assets/4b8cde2e-7276-4778-bca4-c6ada979c2eb)


# Setup

0: optimize server and Disable ufw
```
ufw disable
```



Use the gfw4fun script to install 3x-ui and nginx with its configuration.
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -panel 1 -cdn off"
```
BE AWARE: his script set crontab to restart nginx, renew certificates, and restart xray. edit crontab if you don't want them.
This also installs tor and warp and psiphone, diable services.<br />
Also be carefull to turn off cdn for cdn domains when crontab renew certificates.


Now install Html website.
```
bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -RandomTemplate yes
```


Now we should edit the nginx domain config.
```
sudo nano /etc/nginx/sites-available/yourdomain.com
```
<br />

You will need to do this for any new domain installed by gfw4fun, yes his script can be used many times for new domains, like for different cdns.<br />
change the 443 to 8443 on the first lines. From this:
(you can use unix domain sockets, xtls wiki and linux says they are better)

![Screenshot 2024-11-03 091246](https://github.com/user-attachments/assets/292dc711-22a8-470f-8012-6aa05c1c55a9)


To this:


![Screenshot 2024-11-03 091207](https://github.com/user-attachments/assets/e395e357-5959-4c1d-b7ce-1c21de24cbc4)


Test new config and reload
```
ln -fs "/etc/nginx/sites-available/yourdomain.com" "/etc/nginx/sites-enabled/"
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl restart nginx
```


Encountered Error?
```
sudo fuser -k 80/tcp
sudo fuser -k 443/tcp
sudo fuser -k 8443/tcp
sudo systemctl start nginx
```
Then repeat previous sections' commands.

# Xray 

Create REALITY inbound<br />
This picture, was from previous setup, you should create the reality inbound on 443.


![image](https://github.com/user-attachments/assets/d987f9fb-d11b-4c56-a61f-230b74432b5c)
![Screenshot 2024-11-03 093431](https://github.com/user-attachments/assets/713697e2-153e-4a34-a511-3bb76b476de2)


CDN inbounds:<br />


From https://github.com/GFW4Fun/x-ui-pro#server-configuration-wrench <br />


![image](https://github.com/user-attachments/assets/4db3a6c3-62a1-4abe-bda6-171cf0ad2bc7)
![image](https://github.com/user-attachments/assets/e9ab2e33-71b1-45f0-9b77-0469cd69d81f)
![image](https://github.com/user-attachments/assets/7f914763-564d-464f-a10d-c7bdde12e5cb)




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

UFW at last
```
ufw enable
```


