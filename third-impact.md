first start

```
sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'
apt-get install -y software-properties-common ufw wget curl git socat cron busybox bash-completion locales nano apt-utils
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
sudo systemctl daemon-reload
sudo systemctl restart ssh.socket
```


x-ui-pro
```
sudo su -c "$(command -v apt||echo dnf) -y install wget;bash <(wget -qO- raw.githubusercontent.com/GFW4Fun/x-ui-pro/master/x-ui-pro.sh) -panel 1 -xuiver last -cdn off -secure no -country xx"
sudo sh -c "$(wget -qO- https://github.com/v2rayA/v2rayA-installer/raw/main/uninstaller.sh)"
rm -r /usr/local/etc/v2raya
systemctl stop warp-plus
systemctl disable warp-plus
rm -rf /etc/warp-plus/
systemctl stop tor
systemctl disable tor
apt remove -y tor
crontab -e
0 11 * * * /sbin/shutdown -r +5
```


changing the html site
```
wget https://github.com/pathumd/2021_personal_site/archive/refs/heads/main.zip
```
```
unzip main.zip
```
```
rm -r /var/www/html/*
```
```
cp -r /root/2021_personal_site-main/. /var/www/html/
```
```
rm main.zip
```
```
rm -r 2021_personal_site-main
```


Nginx configuration
```

```
