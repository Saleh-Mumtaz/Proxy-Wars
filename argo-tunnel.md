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
