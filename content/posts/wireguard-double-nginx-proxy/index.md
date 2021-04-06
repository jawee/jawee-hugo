---
title: "Wireguard tunnel from VPS to home network"
date: 2021-04-04T13:02:20+02:00
draft: false
---
Inspiration from [https://theorangeone.net/posts/wireguard-haproxy-gateway/](https://theorangeone.net/posts/wireguard-haproxy-gateway/).

## Install wireguard on both the VPS and the internal server
```bash
sudo apt install wireguard
```

## Generate keys for both VPS and internal server
```bash
wg genkey | tee privatekey | wg pubkey > publickey
```

## Wireguard configurations

___VPS configuration (wg0.conf)___
```bash
[Interface]
Address = 10.1.10.1
PrivateKey = <Server private key>
ListenPort = 51820

[Peer]
PublicKey = <Client public key>
AllowedIPs = 10.1.10.2/32
```

___Internal server configuration (wg0.conf)___
```bash
[Interface]
Address = 10.1.10.2
PrivateKey = <Client private key>

[Peer]
PublicKey = <Server public key>
Endpoint = <hostname>:51820
AllowedIPs = 10.1.10.2/24

PersistentKeepalive = 25
```



## Enable automatic connections:
```bash
systemctl enable wg-quick@wg0.service
```

## Nginx configuration
___VPS nginx config___
```bash
server {
    listen 80;
    listen [::]:80;
    server_name subdomain.domain.tld;

    location ^~ /.well-known/acme-challenge/ {
        default_type "text/plain";
        root /home/<user>/public/letsencrypt;
    }

    location / {
        proxy_pass http://10.1.10.2/;
        proxy_buffering off;
        proxy_set_header Host             $host;
        proxy_set_header X-Real-IP        $remote_addr;
        proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```

___Internal server nginx config___
```bash
server {
    listen 80;
    server_name subdomain.domain.tld;

    location / {
        proxy_pass http://<internal-ip-to-point-at>:<port>;
        proxy_buffering off;
    }
}
```

## Letsencrypt certificate on VPS
```bash
sudo certbot --nginx -d subdomain.domain.tld
```

## Further reading:
* https://medium.com/tangram-visions/what-they-dont-tell-you-about-setting-up-a-wireguard-vpn-46f7bd168478
* https://theorangeone.net/posts/wireguard-getting-started/
* https://theorangeone.net/posts/wireguard-haproxy-gateway/
