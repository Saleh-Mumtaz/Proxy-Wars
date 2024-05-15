```
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      },
      "tag": "api"
    }
  ],
  "log": {
    "access": "none",
    "dnsLog": false,
    "error": "./error.log",
    "loglevel": "warning"
  },
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {},
      "tag": "direct"
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
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
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
        "domain": [
          "regexp:.*\\.ir$",
          "regexp:.*\\.xn--mgba3a4f16a$",
          "ext:geosite_IR.dat:ir"
        ]
      },
      {
        "outboundTag": "reverse-0",
        "inboundTag": [
          "inbound-80"
        ],
        "domain": [
          "full:reverse.xui"
        ],
        "type": "field"
      },
      {
        "outboundTag": "reverse-0",
        "inboundTag": [
          "inbound-80"
        ],
        "type": "field"
      }
    ]
  },
  "stats": {},
  "reverse": {
    "portals": [
      {
        "tag": "reverse-0",
        "domain": "reverse.xui"
      }
    ]
  }
}
```
