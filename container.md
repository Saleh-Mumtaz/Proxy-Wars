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
      "tag": "SE-CF-WS-saleh",
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "saleh-mumtaz.ir",
            "port": 2053,
            "users": [
              {
                "id": "*",
                "flow": "",
                "encryption": "none"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": {
          "serverName": "DPfusNGjahiKxxABvFnxxSqGCZbhOvOdNWeFagxYWcDfXrzosw.Magi.AnjomanSUt.iR",
          "alpn": [
            "h2",
            "http/1.1"
          ],
          "fingerprint": "chrome",
          "allowInsecure": false
        },
        "wsSettings": {
          "path": "/?ed=2048",
          "headers": {
            "Host": "DPfusNGjahiKxxABvFnxxSqGCZbhOvOdNWeFagxYWcDfXrzosw.Magi.AnjomanSUt.iR"
          }
        },
        "sockopt": {
          "dialerProxy": "fragment",
          "tcpKeepAliveIdle": 100,
          "mark": 255,
          "tcpNoDelay": true
        }
      },
      "mux": {
        "enabled": true,
        "concurrency": 8,
        "xudpConcurrency": 8,
        "xudpProxyUDP443": "reject"
      }
    },
    {
      "tag": "fragment",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "AsIs",
        "fragment": {
          "packets": "tlshello",
          "length": "60-65",
          "interval": "1-2"
        }
      },
      "streamSettings": {
        "sockopt": {
          "tcpNoDelay": true,
          "tcpKeepAliveIdle": 100
        }
      }
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
        "outboundTag": "direct",
        "domain": [
          "regexp:.*\\.ir$",
          "regexp:.*\\.xn--mgba3a4f16a$",
          "ext:geosite_IR.dat:ir",
          "geosite:category-ir"
        ]
      },
      {
        "type": "field",
        "outboundTag": "direct",
        "ip": [
          "ext:geoip_IR.dat:ir"
        ]
      },
      {
        "type": "field",
        "outboundTag": "direct",
        "ip": [
          "geoip:ir"
        ]
      },
      {
        "type": "field",
        "inboundTag": [
          "inbound-80"
        ],
        "outboundTag": "SE-CF-WS-saleh"
      },
      {
        "type": "field",
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api"
      }
    ]
  },
  "stats": {}
}
```
