` DNSTT and SlipStream `
-
# DNSTT Server
After creating A and NS records, A record pointing to the server IP and NS record pointing to A record.
```
bash <(curl -Ls https://raw.githubusercontent.com/bugfloyd/dnstt-deploy/main/dnstt-deploy.sh)
```
# DNSTT Client
windows:
```
.\dnstt-client-windows-amd64.exe -udp 8.8.8.8:53 -pubkey-file pub.key dns.example.com 127.0.0.1:7000
```
linux
```
mkdir dnstt && cd dnstt\
```
```
wget https://dnstt.network/dnstt-client-linux-amd64
```
```
chmod +x dnstt-client-linux-amd64
```
Test
```
./dnstt-client-linux-amd64 -udp 8.8.8.8:53 -pubkey-file pub.key t.iran123.online 127.0.0.1:1080
```
As Service
```
vi /etc/systemd/system/dnstt-client.service
```
```
[Unit]
Description=DNSTT Client Tunnel Service
After=network.target network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/dnstt
ExecStart=/root/dnstt/dnstt-client-linux-amd64 -udp 8.8.8.8:53 -pubkey-file pub.key dns.example.com 127.0.0.1:1080
Restart=on-failure
RestartSec=5s
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl enable dnstt-client.service
sudo systemctl start dnstt-client.service
sudo systemctl status dnstt-client.service

```

# SlipStream Server

dnstt but optimized

```
bash <(curl -Ls https://raw.githubusercontent.com/AliRezaBeigy/slipstream-rust-deploy/master/slipstream-rust-deploy.sh)
```

# SlipStream   Server

Windows
```
.\slipstream-client-windows-amd64.exe  --resolver 8.8.8.8:53 --domain dns.example.com --tcp-listen-port 5201
```
Linux as service
```
mkdir slipstream && cd slipstream
```
```
wget https://github.com/AliRezaBeigy/slipstream-rust-deploy/releases/latest/download/slipstream-client-linux-amd64
```
```
chmod +x slipstream-client-linux-amd64
```
```
vi /etc/systemd/system/slipstream.service
```
```
[Unit]
Description=SlipStream Client Tunnel Service
After=network.target network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/slipstream
ExecStart=/root/slipstream/slipstream-client-linux-amd64 --resolver 8.8.8.8:53 --domain dns.example.com --tcp-listen-port 5201
Restart=on-failure
RestartSec=5s
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl enable slipstream.service
sudo systemctl start slipstream.service
sudo systemctl status slipstream.service
```
