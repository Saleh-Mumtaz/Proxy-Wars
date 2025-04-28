# Hysteria2 + port hopping - Hysteria server - Sing-box client

```
apt update && apt upgrade -y
bash <(curl -fsSL https://get.hy2.sh/)
HYSTERIA_USER=root bash <(curl -fsSL https://get.hy2.sh/)
```
برای تک پورت اون هم روی ۴۴۳ دستور زیر رو بزنید، اگر کانفیگ های v2ray روی ۴۴۳ دارید، اونها روی پورت ۴۴۳ tcp هستند، هیستریا پورت ۴۴۳ udp رو استفاده میکنه.
```
ufw allow 443/udp
```

configuration
```
nano /etc/hysteria/config.yaml
```

```
listen: :443

tls:
  cert: /path/cert.pem
  key: /path/key.pem

obfs:
  type: salamander
  salamander:
    password: random_string

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

auth:
  type: password
  password: random_string

resolver:
  type: udp
  tcp:
    addr: 8.8.8.8:53
    timeout: 4s
  udp:
    addr: 8.8.4.4:53
    timeout: 4s
  tls:
    addr: 1.1.1.1:853
    timeout: 10s
    sni: cloudflare-dns.com
    insecure: false
  https:
    addr: 1.1.1.1:443
    timeout: 10s
    sni: cloudflare-dns.com
    insecure: false

sniff:
  enable: true
  timeout: 2s
  rewriteDomain: false
  tcpPorts: 80,443,8000-9000
  udpPorts: all

masquerade:
  type: proxy
  proxy:
    url: https://news.ycombinator.com/
    rewriteHost: true
```
sub
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
          "172.19.0.0/30",
          "fdfe:dcba:9876::1/126"
        ]
      },
      {
        "action": "route",
        "server": "proxy-dns",
        "source_ip_cidr": [
          "172.19.0.0/30",
          "fdfe:dcba:9876::1/126"
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
        "172.19.0.1/30",
          "fdfe:dcba:9876::1/126"
      ],
      "auto_route": true,
      "endpoint_independent_nat": false,
      "mtu": 9000,
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
      "obfs": {
        "password": "random_string",
        "type": "salamander"
      },
      "password": "random_string",
      "server": "ip_address",
      "server_ports": [
	"10000:60000"
      ],
      "hop_interval": "10s",
      "tag": "hy2",
      "tls": {
	 "cipher_suites": [
          "TLS_AES_128_GCM_SHA256"
        ],
        "enabled": true,
        "insecure": true,
	"min_version": "1.3",
        "server_name": "domain_name"
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
        "tag": "geosite-ir",
        "type": "remote",
        "url": "https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/category-ir.srs"
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
