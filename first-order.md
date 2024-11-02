# XRAY-REALITY-VISION-SELFSTEAL/TLS-WS/TLS-gRPC/... + NGINX + WEBSITE + SSLH + UFW ALL ON 443

Goal: VPS to expose only 80 and 443 ports. Have XRAY TCP/WS/HTTPUPGRADE/SPLITHTTP REALITY/TLS WEBSITE NGINX-PATH-ROUTING SSH on 443.
How?
SSLH will handle port 443, detects ssh and https connections(include REALITY) and forwards them to XRAY OR SSH.
xray REALITY inbound on port 4443, nginx on port 8443.
REALITY SNI will be our own domain which we get ssl certificates on our own VPS. Important: CDN/Cloud-sign should be off! Domain should resolve into our VPS'S IP.
The first SNI in this field, should be the A record without CDN proxy. Other SNIs with different CDNs should be after that. 
Before receiving certs do not activate CDN proxy on CDN SNIs.
dest of REALITY wil be 127.0.0.1:8443 . XRAY will forward any non-REALITY traffic without any changes to 8443, nginx.
That will contain the whole URL. path of the site, panel or CDN configs, will be deciphered by nginx, and routed back to xray inbound with random port, or to it's own site or x-ui pannel.
Here in gfw4fun script, we have /port/anytext path for cdn configs.
In REALITY inbound, put /anytext in spider field.
Use shortids(It is now used by default in 3x-ui).
Don't forget to choose xtls-rprx-vision in clients section.
