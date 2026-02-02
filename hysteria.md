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
