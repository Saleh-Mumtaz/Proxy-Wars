**In root directory**

```
mkdir Argo && cd Argo
```

```
wget https://github.com/cloudflare/cloudflared/releases/download/2024.5.0/cloudflared-linux-amd64 && chmod +x cloudflared-linux-amd64
```

```
./cloudflared-linux-amd64 tunnel login
```

```
./cloudflared-linux-amd64 tunnel create argos
```

```
./cloudflared-linux-amd64 tunnel route dns argos argos.domain.com
```

**tmux**

```
./cloudflared-linux-amd64 tunnel --url localhost:2053 run argos
```
Exiting Session without killing it

ctrl + d then b

Getting back to session
```
tmux list-sessions
```
```
tmux attach -t n
```
**Run as service**
```
cd /etc/systemd/system
```
```
vi argos.service
```
```
[Unit]
Description=Argo Tunnel

[Service]
Type=idle
User=root
WorkingDirectory=/root/Argo
ExecStart=/root/Argo/cloudflared-linux-amd64 tunnel --url localhost:2053 run argos
Restart=always

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
```
```
sudo systemctl start argos.service
```
```
sudo systemctl enable argos.service
```
```
sudo systemctl status argos.service
```
```
sudo systemctl stop argos.service
```

```
sudo systemctl disable argos.service
```
