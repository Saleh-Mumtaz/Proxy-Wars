first start
check dns at the beginning
```
resolvectl status
sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'
apt-get install -y software-properties-common ufw wget curl git socat cron busybox bash-completion locales nano apt-utils
```
Timezone
```
sudo localectl set-locale LC_TIME=C.UTF-8
```
or
```
env | egrep '^(LANG|LC_)'
```
```
vi /etc/default/locale
```
```
LANG=C.UTF-8
LC_ALL=C.UTF-8
LC_TIME=C.UTF-8
```
```
timedatectl list-timezones | grep Tehran
sudo timedatectl set-timezone Asia/Tehran
```
then reboot

---
NGINX
```
sudo apt update
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```
```
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
| sudo tee /etc/apt/sources.list.d/nginx.list
```
```
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
| sudo tee /etc/apt/preferences.d/99nginx
```
```
sudo apt update
sudo apt install nginx
```
bbr
```
vi /etc/sysctl.conf
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
sysctl -p
```
SSH
```
vi /etc/ssh/sshd_config
sudo systemctl daemon-reload && sudo systemctl restart ssh.socket
```


x-ui-pro
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -panel 1 -xuiver last -cdn off -secure no -country xx"
sudo sh -c "$(wget -qO- https://github.com/v2rayA/v2rayA-installer/raw/main/uninstaller.sh)"
rm -r /usr/local/etc/v2raya && systemctl stop warp-plus && systemctl disable warp-plus && rm -rf /etc/warp-plus/ && systemctl stop tor && systemctl disable tor && apt remove -y tor
crontab -e
30 10 * * * sudo apt update && sudo apt upgrade -y
0 11 * * * /sbin/shutdown -r +5
```
change the credentials with x-ui command menu then set other things in panel itself. port path certs session duration
in xray config disable protection shield and delete routings. correct statistics and log
reality's dest is the socket nginx is listening on put ```/etc/xray.socket``` in corresponding field</br></br>
if you enabled ip limit, use the following command to clear the log time to time
```
echo "" > /usr/local/x-ui/access.log
```

changing the html site
```
wget https://github.com/pathumd/2021_personal_site/archive/refs/heads/main.zip && unzip main.zip && rm -r /var/www/html/* && cp -r /root/2021_personal_site-main/. /var/www/html/ && rm main.zip && rm -r 2021_personal_site-main
```

Nginx configuration
```
vim /lib/systemd/system/nginx.service
ExecStartPre=/bin/rm -f /etc/xray.socket
vim /etc/nginx/sites-available/
unix:/etc/xray.socket
ln -fs "/etc/nginx/sites-available/" "/etc/nginx/sites-enabled/"
sudo nginx -t
systemctl daemon-reload && sudo systemctl reload nginx && systemctl restart nginx && systemctl status nginx
```
if entered linking command wrongly
```
rm /etc/nginx/sites-enabled/
```
if got any problem with ports check the process listening on them
t and u are tcp and udp, can use -tlpn and -ulpn to check only tcp or udp ports
```
sudo ss -tulpn | grep ":80 "
```
then kill the process with
socket file should be created and deleted by process, remove it manually if it didn't, it shouldn't be present before process begins that's why we added a pre start command to nginx service file earlier
```
sudo fuser -k 80/tcp
sudo fuser -k /etc/xray.socket
sudo rm -f /etc/xray.socket
#or
sudo lsof | grep /etc/xray.socket
sudo kill -9 <PID>
```
Hy2
```
HYSTERIA_USER=root bash <(curl -fsSL https://get.hy2.sh/)
vim /etc/hysteria/config.yaml
```
Hy2 conf
```
listen: :443

tls:
  cert: /etc/letsencrypt/live/d/fullchain.pem
  key: /etc/letsencrypt/live/d/privkey.pem
  sniGuard: disable

obfs:
  type: salamander 
  salamander:
    password: cry_me_a_r1ver

quic:
  initStreamReceiveWindow: 8388608
  maxStreamReceiveWindow: 8388608
  initConnReceiveWindow: 20971520
  maxConnReceiveWindow: 20971520
  maxIdleTimeout: 30s
  maxIncomingStreams: 1024
  disablePathMTUDiscovery: false

speedTest: false

disableUDP: false

udpIdleTimeout: 60s

ignoreClientBandwidth: false

auth:
  type: password
  password: randstr
```
obfs will override masq so there is no point in defining obfs and masq at the same time. reslover will be system's dns, if not set.
restarting and enabling hy2 service
```
sudo systemctl restart hysteria-server.service && sleep 3 && systemctl status hysteria-server.service
systemctl enable --now hysteria-server.service
```
use following command to see hy log in real time instead of systemctl status.
```
journalctl -u hysteria-server.service -f
```
sb subscription.json
```
vim /var/www/html/subscription.json
```
```
{
    "dns": {
      "final": "local-dns",
      "rules": [
        {
          "action": "route",
          "clash_mode": "Global",
          "server": "proxy-dns",
          "source_ip_cidr": [
            "172.19.0.0/30"
          ]
        },
        {
          "action": "route",
          "server": "proxy-dns",
          "source_ip_cidr": [
            "172.19.0.0/30"
          ]
        },
        {
          "action": "route",
          "clash_mode": "Direct",
          "server": "direct-dns"
        },
        {
          "action": "route",
          "rule_set": [
            "geosite-ir"
          ],
          "server": "direct-dns"
        }
      ],
      "servers": [
        {
          "address": "tcp://8.8.8.8",
          "address_resolver": "local-dns",
          "detour": "proxy",
          "tag": "proxy-dns"
        },
        {
          "address": "local",
          "detour": "direct",
          "tag": "local-dns"
        },
        {
          "address": "local",
          "detour": "direct",
          "tag": "direct-dns"
        }
      ],
      "strategy": "prefer_ipv4"
    },
    "inbounds": [
      {
        "address": [
          "172.19.0.1/30"
        ],
        "auto_route": true,
        "endpoint_independent_nat": false,
        "mtu": 1500,
        "platform": {
          "http_proxy": {
            "enabled": true,
            "server": "127.0.0.1",
            "server_port": 2080
          }
        },
        "stack": "system",
        "strict_route": false,
        "type": "tun"
      },
      {
        "listen": "127.0.0.1",
        "listen_port": 2080,
        "type": "mixed",
        "users": []
      }
    ],
    "outbounds": [
      {
        "outbounds": [
          "auto",
          "direct",
          "hy2"
        ],
        "tag": "proxy",
        "type": "selector"
      },
      {
        "interval": "10m",
        "outbounds": [
          "hy2"
        ],
        "tag": "auto",
        "tolerance": 50,
        "type": "urltest",
        "url": "http://www.gstatic.com/generate_204"
      },
      {
        "tag": "direct",
        "type": "direct"
      },
      {
        "password": "randstr",
        "server": "ip",
        "server_port": 443,
        "tag": "hy2",
        "tls": {
          "cipher_suites": [
              "TLS_AES_128_GCM_SHA256"
          ],
           "enabled": true,
           "min_version": "1.3",
           "server_name": "d"
        },
        "type": "hysteria2"
      }
    ],
    "route": {
      "auto_detect_interface": true,
      "final": "proxy",
      "rule_set": [
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-ads",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ads-all.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-private",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/private.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-ir",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ir.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geoip-private",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/private.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geoip-ir",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/ir.srs"
        }
    ],
    "rules": [
        {
            "action": "sniff"
        },
        {
            "action": "route",
            "clash_mode": "Direct",
            "outbound": "direct"
        },
        {
            "action": "route",
            "clash_mode": "Global",
            "outbound": "proxy"
        },
        {
            "action": "hijack-dns",
            "protocol": "dns"
        },
        {
            "action": "route",
            "outbound": "direct",
            "rule_set": [
                "geoip-private",
                "geosite-private",
                "geosite-ir",
                "geoip-ir"
            ]
        },
        {
            "action": "reject",
            "rule_set": [
                "geosite-ads"
            ]
        }
      ]
    }
  }
```
In case of port hopping
```
sudo ufw reset
sudo ufw disable
ip a
vim /etc/ufw/before.rules
vim /etc/ufw/before6.rules
```
my network interface name is ens3
find yours with ```ip a``` and replace mine with yours
put the following behind ```*filter```
```
*nat
:PREROUTING ACCEPT [0:0]
-A PREROUTING -i ens3 -p udp --dport 10000:60000 -j REDIRECT --to-ports 443
COMMIT
```
ufw http https ssh udp
with each ufw reset, before rules we added will be deleted too
```
ufw default deny incoming && ufw default allow outgoing && sudo ufw allow 443
ufw ports/tcp
ufw allow 10000:60000/udp
ufw enable
ufw status
```
port hopping sb subscription
```
vim /var/www/html/subscription.json
```
```
{
    "dns": {
      "final": "local-dns",
      "rules": [
        {
          "action": "route",
          "clash_mode": "Global",
          "server": "proxy-dns",
          "source_ip_cidr": [
            "172.19.0.0/30"
          ]
        },
        {
          "action": "route",
          "server": "proxy-dns",
          "source_ip_cidr": [
            "172.19.0.0/30"
          ]
        },
        {
          "action": "route",
          "clash_mode": "Direct",
          "server": "direct-dns"
        },
        {
          "action": "route",
          "rule_set": [
            "geosite-ir"
          ],
          "server": "direct-dns"
        }
      ],
      "servers": [
        {
          "address": "tcp://8.8.8.8",
          "address_resolver": "local-dns",
          "detour": "proxy",
          "tag": "proxy-dns"
        },
        {
          "address": "local",
          "detour": "direct",
          "tag": "local-dns"
        },
        {
          "address": "local",
          "detour": "direct",
          "tag": "direct-dns"
        }
      ],
      "strategy": "prefer_ipv4"
    },
    "inbounds": [
      {
        "address": [
          "172.19.0.1/30"
        ],
        "auto_route": true,
        "endpoint_independent_nat": false,
        "mtu": 1500,
        "platform": {
          "http_proxy": {
            "enabled": true,
            "server": "127.0.0.1",
            "server_port": 2080
          }
        },
        "stack": "system",
        "strict_route": false,
        "type": "tun"
      },
      {
        "listen": "127.0.0.1",
        "listen_port": 2080,
        "type": "mixed",
        "users": []
      }
    ],
    "outbounds": [
      {
        "outbounds": [
          "auto",
          "direct",
          "hy2"
        ],
        "tag": "proxy",
        "type": "selector"
      },
      {
        "interval": "10m",
        "outbounds": [
          "hy2"
        ],
        "tag": "auto",
        "tolerance": 50,
        "type": "urltest",
        "url": "http://www.gstatic.com/generate_204"
      },
      {
        "tag": "direct",
        "type": "direct"
      },
      {
        "password": "randstr",
        "server": "ip",
        "server_ports": [
      "10000:60000"
        ],
        "hop_interval": "30s",
        "tag": "hy2",
        "tls": {
          "cipher_suites": [
              "TLS_AES_128_GCM_SHA256"
          ],
           "enabled": true,
           "min_version": "1.3",
           "server_name": "d"
        },
        "type": "hysteria2"
      }
    ],
    "route": {
      "auto_detect_interface": true,
      "final": "proxy",
      "rule_set": [
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-ads",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ads-all.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-private",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/private.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geosite-ir",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ir.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geoip-private",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/private.srs"
        },
        {
            "download_detour": "direct",
            "format": "binary",
            "tag": "geoip-ir",
            "type": "remote",
            "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/ir.srs"
        }
    ],
    "rules": [
        {
            "action": "sniff"
        },
        {
            "action": "route",
            "clash_mode": "Direct",
            "outbound": "direct"
        },
        {
            "action": "route",
            "clash_mode": "Global",
            "outbound": "proxy"
        },
        {
            "action": "hijack-dns",
            "protocol": "dns"
        },
        {
            "action": "route",
            "outbound": "direct",
            "rule_set": [
                "geoip-private",
                "geosite-private",
                "geosite-ir",
                "geoip-ir"
            ]
        },
        {
            "action": "reject",
            "rule_set": [
                "geosite-ads"
            ]
        }
      ]
    }
  }
```
bat for windows client side to use singbox for windows
```
@echo off
REM Download JSON content from the URL and save it to config.json
curl -o config.json https://example.com/subscription.json

REM Check if the download was successful
if %errorlevel% neq 0 (
    echo Failed to download config.json
    pause
    exit /b
)

REM Execute sing-box.exe to run
start /B sing-box.exe run

REM Close the window after sing-box.exe finishes
exit
```
client side to use on windows with hy2 program itself
beware that if client has ipv6 but server does not, there will be problems in opening such sites.
```
server: *

auth: *

tls:
  sni: *
  insecure: false 

transport:
  type: udp

obfs:
  type: salamander 
  salamander:
    password: *

quic:
  initStreamReceiveWindow: 8388608 
  maxStreamReceiveWindow: 8388608 
  initConnReceiveWindow: 20971520 
  maxConnReceiveWindow: 20971520 
  maxIdleTimeout: 30s 
  keepAlivePeriod: 5s 
  disablePathMTUDiscovery: false

fastOpen: true

tun:
  name: "hytun" 
  mtu: 1500 
  timeout: 5m 
  address: 
    ipv4: 100.100.100.101/30
    ipv6: 2001::ffff:ffff:ffff:fff1/126
  route: 
    ipv4: [0.0.0.0/0] 
    ipv6: ["2000::/3"] 
    ipv4Exclude: [192.168.0.0/16,10.0.0.0/8,172.16.0.0/12,*] 
    ipv6Exclude: ["::/0"]
```
client side to use on android with hy2 program itself
```
server: *

auth: *

tls:
  sni: *
  insecure: false

transport:
  type: udp

obfs:
  type: salamander
  salamander:
    password: *

quic:
  initStreamReceiveWindow: 8388608
  maxStreamReceiveWindow: 8388608
  initConnReceiveWindow: 20971520
  maxConnReceiveWindow: 20971520
  maxIdleTimeout: 30s
  keepAlivePeriod: 10s
  disablePathMTUDiscovery: false

socks5:
  listen: 127.0.0.1:10808
  disableUDP: false
```
docker installation
docker container ports will bypass ufw so do not change ufw rules
```
sudo apt-get update && \
sudo apt-get install -y ca-certificates curl && \
sudo install -m 0755 -d /etc/apt/keyrings && \
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
sudo chmod a+r /etc/apt/keyrings/docker.asc && \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
sudo apt-get update && \
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
sudo docker run hello-world
```
telegram bot
```
mkdir /etc/tgbot && cd /etc/tgbot && vim compose.yml
```
conf
replace * in TELEGRAM_BOT_TOKEN WHITELISTED_IDS ADMIN_ID TELEGRAM_API_ID TELEGRAM_API_HASH with your own
```
name: telegram-ytdl

services:
  telegram-bot:
    image: ghcr.io/vaaski/telegram-ytdl:latest
    restart: unless-stopped
    volumes:
      - ./vaaski-telegram-ytdl:/app/storage
    environment:
      # get your bot token from @BotFather
      TELEGRAM_BOT_TOKEN: "*"

      # comma separated list of telegram user ids, leave empty to allow all users
      WHITELISTED_IDS: "*"

      # admin user id
      ADMIN_ID: "*"
      ALLOW_GROUPS: 'false'

      TELEGRAM_API_ROOT: "http://telegram-bot-api:8081"
      TELEGRAM_WEBHOOK_PORT: "8443"
      TELEGRAM_WEBHOOK_URL: "http://telegram-bot:8443"

      # whether to automatically update yt-dlp
      # defaults to "true", set to "false" to disable
      YTDL_AUTOUPDATE: "true"

      # provide your OpenAI API key to enable auto-translation (optional)
      OPENAI_API_KEY: ""

      # provide your cobalt instance url (optional, can be left as-is for the default compose.yml)
      COBALT_INSTANCE_URL: "http://cobalt:9000"

    depends_on:
      telegram-bot-api:
        condition: service_healthy
      cobalt:
        condition: service_healthy

  telegram-bot-api:
    image: ghcr.io/bots-house/docker-telegram-bot-api:latest
    restart: unless-stopped
    environment:
      # get these values from https://core.telegram.org/api/obtaining_api_id
      TELEGRAM_API_ID: "*"
      TELEGRAM_API_HASH: "*"
    command:
      - --dir=/var/lib/telegram-bot-api
      - --local
    volumes:
      - telegram-bot-server-data:/var/lib/telegram-bot-api
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 8081 || exit 1"]
      interval: 5s
      timeout: 30s
      retries: 3

  cobalt:
    image: ghcr.io/imputnet/cobalt:10
    restart: unless-stopped
    environment:
      - API_URL=http://cobalt:9000
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:9000"]
      interval: 5s
      timeout: 30s
      retries: 3

volumes:
  telegram-bot-server-data:
    driver: local

```
starting the bot
```
docker compose up -d
```
for cookies use seal android downloader to generate them
```
vim /etc/tgbot/vaaski-telegram-ytdl/cookies.txt
```
to delete everything related to all containers
```
docker kill $(docker ps -q) && docker rm $(docker ps -a -q) && docker volume rm $(docker volume ls -q) && docker rmi $(docker images -q)
```
warp interface
normal warp
```
# first run
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh
# menu
warp
```
on ubuntu 24 warp can not get ip so we use warp-go and choose ```WARP Non-global dualstack ipv4 priority``` option
```
# first run
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/warp-go.sh && bash warp-go.sh
# menu
warp-go
```
in panel go to advanced add this in outbounds section
```
    {
      "tag": "warp",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv4"
      },
      "streamSettings": {
        "sockopt": {
          "interface": "WARP",
          "tcpFastOpen": true
        }
      }
    }
```
and add routing rule network tcp,udp outbound warp or source port 0-65535 outbound warp
