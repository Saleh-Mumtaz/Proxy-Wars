Debloating x-ui-pro
```
sudo sh -c "$(wget -qO- https://github.com/v2rayA/v2rayA-installer/raw/main/uninstaller.sh)"
```
```
rm -r /usr/local/etc/v2raya
```
```
systemctl stop warp-plus
```
```
systemctl disable warp-plus
```
```
rm -rf /etc/warp-plus/
```
```
systemctl stop tor
```
```
systemctl disable tor
```
```
apt remove -y tor
```
```
crontab -e
```
```
0 11 * * * /sbin/shutdown -r +5
```
