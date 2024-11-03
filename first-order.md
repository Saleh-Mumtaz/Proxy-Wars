# All-in-443
XRAY-REALITY-VISION-SELF-STEAL/TLS over TCP/WS/gRPC/... + CDN Inbounds + NGINX + WEBSITE + SSH to server + Webpannel access + UFW ALL ON port 443
<img width="738" alt="schematic" src="https://github.com/user-attachments/assets/3caec1f7-889d-47c6-9bb9-28f59ce7fd4a">


Goal:<br /> VPS to expose only 80 and 443 ports. Have XRAY TCP/WS/HTTPUPGRADE/SPLITHTTP REALITY/TLS WEBSITE NGINX-PATH-ROUTING SSH on 443.
How?


SSLH will handle port 443, detects ssh and https connections(include REALITY) and forwards them to XRAY OR SSH.


xray REALITY inbound on port 4443, nginx on port 8443.


REALITY SNI will be our own domain which we get ssl certificates on our own VPS.<br />Important: CDN/Cloud-sign should be off! Domain should resolve into our VPS'S IP.


The first SNI in this field, should be the A record without CDN proxy. Other SNIs with different CDNs should be after that.<br />
Before receiving certs do not activate CDN proxy on CDN SNIs.


dest of REALITY wil be 127.0.0.1:8443 . XRAY will forward any non-REALITY traffic without any changes to 8443, nginx.<br />
That will contain the whole URL. path of the site, panel or CDN configs, will be deciphered by nginx, and routed back to xray inbound with random port, or to it's own site or x-ui pannel.


Here in **gfw4fun** script, we have /port/anytext path for cdn configs.<br />
In REALITY inbound, put /anytext in spider field.<br />


https://github.com/GFW4Fun/x-ui-pro<br />
This script will install panel, configure nginx for url-path-based routing.<br />
3x-ui web pannel will be accessible without a /port/random-path/ but only /rndm-path/ is enough and will access the panel. CDN configs with random port paths: /port/anytext but the port in client config should be 443, use external proxy to set that.<br />



Use shortids(It is now used by default in 3x-ui).<br />

**Impoertant!
**<br />**Do not forget to choose xtls-rprx-vision in clients section.**<br />


In the foloowing picture, hohenheims are on Cloudflare CDN, Last two configs are direct to vps with domain. All on the same vps.


![Screenshot 2024-11-03 083003](https://github.com/user-attachments/assets/820e9639-91df-4626-b780-7cddc20d658e)


You can use all the CDNs you want at the same time for different inbounds.<br />
Downside is that you are limited to a single REALITY inbound.<br />


![Screenshot 2024-11-03 084400](https://github.com/user-attachments/assets/4b8cde2e-7276-4778-bca4-c6ada979c2eb)


# Setup

0: Disable ufw
```
ufw disable
```


Install SSLH
```
sudo apt-get update
sudo DEBIAN_FRONTEND=noninteractive apt-get install sslh
```


Use the gfw4fun script to install 3x-ui and nginx with its configuration.
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -install yes -panel 1 -cdn off"
```


Now install Html website.
```
sudo su -c "bash <(wget -qO- https://raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/randomfakehtml.sh)"
```


Now we should edit the nginx domain config.
```
sudo nano /etc/nginx/sites-available/yourdomain.com
```
<br />

You will need to do this for any new domain installed by gfw4fun, yes his script can be used many times for new domains, like for different cdns.<br />
change the 443 to 8443 on the first lines. From this:


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


# Now Xray 

Create REALITY inbound


![image](https://github.com/user-attachments/assets/d987f9fb-d11b-4c56-a61f-230b74432b5c)
![Screenshot 2024-11-03 093431](https://github.com/user-attachments/assets/713697e2-153e-4a34-a511-3bb76b476de2)


CDN inbounds:<br />


From gfw4fun<br />


![image](https://github.com/user-attachments/assets/4db3a6c3-62a1-4abe-bda6-171cf0ad2bc7)
![image](https://github.com/user-attachments/assets/e9ab2e33-71b1-45f0-9b77-0469cd69d81f)
![image](https://github.com/user-attachments/assets/7f914763-564d-464f-a10d-c7bdde12e5cb)



# SSLH

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
    # Forward SSH traffic to the local SSH server on port 1605
    { name: "ssh"; host: "127.0.0.1"; port: "2789"; },

    # Forward HTTPS (TLS) traffic to Xray on localhost:443
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
Description=SSL/SSH multiplexer
After=network.target
Documentation=man:sslh(8)

[Service]
User=root
RuntimeDirectory=sslh
EnvironmentFile=/etc/default/sslh
ExecStart=/usr/sbin/sslh --foreground --config /etc/sslh.cfg
Backlog=100
KillMode=process
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

For ufw

Important! 2789 is my ssh port, change yours and replace 2789 with it.
```
ufw status
ufw reset
ufw default deny incoming
ufw default allow outgoing
ufw allow 80,443,2789/tcp
ufw allow 80,443,2789/udp
ufw enable
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
Why did you keep ssh exposed? wasn't this for having ssh on 443?


Yes but to make sure everything works fine, we will let this setup work for a couple of days.<br />You can be reckless and delete your ssh port from ufw rules, but doesn't worth it, first check for a day, then do that.


# About SSLH

Wouldn't SSLH crash? if you configure it correctly, no, but nothing is 100 percent reliable.<br />Despite its documents which are not very clear for configuration, this program is very powerful and would not crash.<br />**A lot of companies and servers** which we don't know are using this to allow ssh to their servers on 443 port, becuase it is the only exposed port along 80.
