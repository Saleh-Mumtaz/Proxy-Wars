# Proxy Wars
Endless battles between GFW and freedom brigade

---

**Optional BBRv3 and zanmod kernel**
```
apt install curl -y && bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/optimizer.sh --ipv4)
```

```
bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/bbrv3.sh --ipv4)
```
---
**Step 0-1 Change DNS to cloudflare and google**

```
sudo apt install resolvconf
```
```
sudo systemctl start resolvconf.service
```
```
sudo systemctl enable resolvconf.service
```
```
sudo systemctl status resolvconf.service
```

```
vi /etc/resolvconf/resolv.conf.d/head
```
```
nameserver 1.1.1.1
nameserver 8.8.8.8
```
```
sudo resolvconf --enable-updates
```
```
sudo resolvconf -u
```
```
sudo systemctl restart resolvconf.service
```
```
sudo systemctl restart systemd-resolved.service
```
```
resolvectl status
```
**Step 0-2 Change DNS to cloudflare and google**

```
mkdir /etc/systemd/resolved.conf.d/
```
```
vi /etc/systemd/resolved.conf.d/dns_servers.conf
```
```
[Resolve]
DNS=1.1.1.1 8.8.8.8
```
```
systemctl restart systemd-resolved
```
```
resolvectl status
```
```
systemd-resolve --status
```

---

**Step 1 Optimizing**
```
sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'
```
```
apt-get install -y software-properties-common ufw wget curl git socat cron busybox bash-completion locales nano apt-utils
```
---
Unlimiting

```
vi /etc/security/limits.conf
```
```
* soft nofile 51200
* hard nofile 51200
```

```
ulimit -n 51200
```
---
```
sudo apt install speedtest-cli
```
```
speedtest-cli
```
---

**Step 2 Hybla or BBR TCP Congestion Control**
```
vi /etc/sysctl.conf
```

```
fs.file-max = 51200
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_congestion_control = hybla
```
OR BBR
```
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
```
```
sysctl -p
```
```
sysctl net.ipv4.tcp_congestion_control
```
---

**Step 3 Change SSH Port**
```
vi /etc/ssh/sshd_config
```
```
#Port 22
```
```
systemctl reload sshd
```
---

**Step 4 x-ui**
```
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```
---

**Step 5 Warp WireProxy**
```
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh
```
warp i
ipv4 
de nl

 warp h (help)
 
 warp n (Get the WARP IP)
 
 warp o (Turn off WARP temporarily)
 
 warp u (Turn off and uninstall WARP interface and Socks5 Linux Client)
 
 warp b (Upgrade kernel, turn on BBR, change Linux system)
 
 warp a (Change account to Free, WARP+ or Teams)
 
 warp p (Getting WARP+ quota by scripts)
 
 warp v (Sync the latest version)
 
 warp r (Connect/Disconnect WARP Linux Client)
 
 warp 4/6 (Add WARP IPv4/IPv6 interface)
 
 warp d (Add WARP dualstack interface IPv4 + IPv6)
 
 warp c (Install WARP Linux Client and set to proxy mode)
 
 warp l (Install WARP Linux Client and set to WARP mode)
 
 warp i (Change the WARP IP to support Netflix)
 
 warp e (Install Iptables + dnsmasq + ipset solution)
 
 warp w (Install WireProxy solution)
 
 warp y (Connect/Disconnect WireProxy socks5)
 
 warp k (Switch between kernel and wireguard-go-reserved)
 
 warp g (Switch between warp global and non-global)
 
 warp s 4/6/d (Set stack proiority: IPv4 / IPv6 / VPS default)

or **Warp Interface**
```
{
      "tag": "warp-interface",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv4"
      },
      "streamSettings": {
        "sockopt": {
          "interface": "warp",
          "tcpFastOpen": true
        }
      }
    }
```

---

**Step 6 Clear History**
```
history -c
```
```
history -w
```
---

**Step 7 Download iran.dat file**
```
cd /usr/local/x-ui/bin/
```
```
wget https://github.com/bootmortis/iran-hosted-domains/releases/download/202401150027/iran.dat
```
---

**Step 8 Routing Rules**
```
{
  "api": {
    "tag": "api",
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ]
  },
  "inbounds": [
    {
      "tag": "api",
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
             "domainStrategy": "UseIPv4"
      }
    },
    {
      "tag": "WARP",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "127.0.0.1",
            "port": 40000
          }
        ]
      },
      "streamSettings": {
        "sockopt": {
          "tcpFastOpen": true,
          "tcpNoDelay": true,
          "tcpKeepAliveIdle": 100
        }
      }
    },
    {
            "tag":"WARP-v4",
            "protocol":"freedom",
            "settings":{
                "domainStrategy":"UseIPv4"
            },
            "proxySettings":{
                "tag":"WARP"
            }
     },
     {
      "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
     }
  ],
  "policy": {
    "levels": {
      "0": {
        "statsUserDownlink": true,
        "statsUserUplink": true
      }
    },
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true,
      "statsOutboundDownlink": true,
      "statsOutboundUplink": true
    }
  },
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "outboundTag": "WARP-v4",
        "domain": [
          "regexp:.*\\.ir$",
          "ext:geoip_IR.dat:ir",
          "geosite:category-ir",
          "geosite:speedtest",
          "geosite:google",
          "geosite:openai",
          "geosite:netflix",
          "geosite:spotify"
        ]
      },
      {
        "type": "field",
        "outboundTag": "WARP-v4",
        "ip": [
          "geoip:ir",
          "geoip:us"
        ]
      },
      {
        "type": "field",
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api"
      },
      {
        "type": "field",
        "outboundTag": "blocked",
        "ip": [
          "geoip:private"
        ]
      }
    ]
  },
  "stats": {}
}
```
---

**Step 9 Brush IPv4 tp get lower ping**
```
warp i
```
default

need to be done after each restart I guess

maybe it was dns i guess?
---

**Step 10 Telegram MTProto proxy**
```
https://telegra.ph/How-Run-MTProto-Proxy-Telegram-with-iSegaro-05-18
```
```
curl -L -o mtp_install.sh https://git.io/fj5ru && bash mtp_install.sh
```
dd-only: no

tls-only: no

rtt worked: yes 
```
sudo systemctl status mtproto-proxy.service
```
Uninstalling MtProto Proxy
```
sudo systemctl stop mtproto-proxy
```
```
sudo systemctl disable mtproto-proxy
```
```
cd mtproto_proxy
```
```
sudo make uninstall
```
RTT
-

root
```
wget  "https://raw.githubusercontent.com/radkesvat/ReverseTlsTunnel/master/scripts/install.sh" -O install.sh && chmod +x install.sh && bash install.sh 
```
```
cd /etc/systemd/system
```
```
vi sysevent.service
```

**RTT - IRAN**
```
[Unit]
Description=Storj-cli helps you to connect to storj's globally distributed storage network via connecting your own servers to our nodes!Define your switches in your account, get your peer node ip, define port range and a thirdparty webstie for handshaking and connecting your server to our node!

[Service]
Type=idle
User=root
WorkingDirectory=/root
ExecStart=/root/storj-cli --iran --peer:* --lport:443 --add-port:8443 --add-port:8880 --sni:zula.ir --password:!@#$%^&* --keep-ufw --log:0 --connection-age:4800 --terminate:24
Restart=always

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl start sysevent.service
```
```
sudo systemctl enable sysevent.service
```
```
sudo systemctl status sysevent.service
```
```
sudo systemctl stop sysevent.service
```
```
sudo systemctl disable sysevent.service
```
```
journalctl -u sysevent.service
```

---

**RTT - ABROAD**
```
cd /etc/systemd/system
```
```
vi tunnel.service
```
```
[Unit]
Description=Reverse TLS Tunnel


[Service]
Type=idle
User=root
WorkingDirectory=/root
ExecStart=/root/RTT --kharej --iran-ip:* --iran-port:443 --toip:127.0.0.1 --toport:multiport --password:!@#$%^&* --sni:zula.ir  --keep-ufw --log:0 --connection-age:4800 --terminate:24
Restart=always

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl start tunnel.service
```
```
sudo systemctl enable tunnel.service
```
```
sudo systemctl status tunnel.service
```
```
sudo systemctl stop tunnel.service
```
```
sudo systemctl disable tunnel.service
```
```
journalctl -u tunnel.service
```

---

**Creating WARP Inbound**

Add the following code to the routing rules
```
{
    "type": "field",
    "inboundTag": [
      "inbound-portnumber"
    ],
    "outboundTag": "warp"
  }
```
---

**RTCF**
```
cat /proc/cpuinfo
```

multi-thread: 
```
wget  "https://raw.githubusercontent.com/radkesvat/RTCF/master/scripts/install_mt.sh" -O install_mt.sh && chmod +x install_mt.sh && bash install_mt.sh
```

single-thread: 
```
wget  "https://raw.githubusercontent.com/radkesvat/RTCF/master/scripts/install_st.sh" -O install_st.sh && chmod +x install_st.sh && bash install_st.sh
```
---

**RTCF - IRAN**
```
cd /etc/systemd/system
```
```
nano tunnel.service
```
```
[Unit]
Description=RTCF

[Service]
Type=idle
User=root
WorkingDirectory=/root
ExecStart=/root/RTCF --iran --auto:off --cert:/root/mycert.cert --pkey:/root/mykey.key --domain:test.magi.anjomansut.ir --lport:23-65535  --password:124578
Restart=always

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl start tunnel.service
```
```
sudo systemctl enable tunnel.service
```
---

**RTCF - ABROAD**
```
[Unit]
Description=Reverse Tunnel with CDN (aka cloudflrae) True tls support


[Service]
Type=idle
User=root
WorkingDirectory=/root
ExecStart=/root/RTCF --kharej --auto:off --domain:test.magi.anjomansut.ir --iran-port:443 --toip:127.0.0.1 --toport:multiport --password:124578
Restart=always

[Install]
WantedBy=multi-user.target
```
---

Certificate with certbot
-
```
sudo apt install software-properties-common
```
```
sudo add-apt-repository ppa:certbot/certbot
```
```
sudo apt-get install certbot -y
```
```
sudo certbot certonly --standalone --preferred-challenges http --agree-tos --email saleh.mumtaz@proton.me -d 
```
certbot certonly --force-renew -d 

https://docs.digitalocean.com/support/how-can-i-renew-lets-encrypt-certificates/

---

UFW
-
see all currently used ports
```
sudo lsof -i -P -n | grep LISTEN | grep "*"
```
**Default Modes**
```
ufw default deny incoming
```
```
ufw default allow outgoing
```
```
ufw allow 1605,8443,443,80,2053,2083,2087,2096/tcp
```
```
ufw allow 1605,8443,443,80,2053,2083,2087,2096/udp
```
```
ufw show added
```
```
ufw enable
```
```
ufw status
```
```
sudo ufw --force disable
```
```
sudo ufw --force reset
```
Deny ping requests
-
```
cp /etc/ufw/before.rules /etc/ufw/before.rules.bak
```
```
sed -i -e '/-A ufw-before-input -p icmp --icmp-type destination-unreachable -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-input -p icmp --icmp-type time-exceeded -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-input -p icmp --icmp-type parameter-problem -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-input -p icmp --icmp-type echo-request -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-forward -p icmp --icmp-type destination-unreachable -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-forward -p icmp --icmp-type time-exceeded -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-forward -p icmp --icmp-type parameter-problem -j ACCEPT/s/ACCEPT/DROP/' \
       -e '/-A ufw-before-forward -p icmp --icmp-type echo-request -j ACCEPT/s/ACCEPT/DROP/' \
       /etc/ufw/before.rules
```
```
ufw disable && ufw enable
```
**Or as root user:**
```
vi /etc/sysctl.conf
```
add the following:
```
net.ipv4.icmp_echo_ignore_all=1
```
Then run:
```
sysctl -p
```
These methods will not work for servers behind anycast network. 
ICMP packets in anycast network can pass through even with icmp packets disabled at the system level, this is normal.

# WaterWall
```
wget https://github.com/radkesvat/WaterWall/releases/download/vX.X/Waterwall-linux-64.zip
```
```
apt install unzip
```
```
unzip Waterwall-linux-64.zip
```
```
chmod +rwx Waterwall
```
https://github.com/radkesvat/WaterWall/wiki
