We use this to connect our HASS system to another server in the cloud so we can reverse proxy to HASS.

OpenVPN server config looks like this
```
dev tun
ifconfig 10.1.0.2 10.1.0.1
secret ha.key
port 1194
comp-lzo
ping 15
ping-restart 45
ping-timer-rem
verb 3
```

Plugin addon looks like this
```
{
  "config": "dev tun\nifconfig 10.1.0.2 10.1.0.1\nport 1194\ncomp-lzo\nping 15\nping-restart 45\nping-timer-rem\nremote server.domaname\nverb 3\n",
  "key": "#\n# 2048 bit OpenVPN static key\n#\n-----BEGIN OpenVPN Static key V1-----\n\n-----END OpenVPN Static key V1-----\n"
}
```

Nginx config is
```
server {
    server_name servername;
	listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live//fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live//privkey.pem;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_prefer_server_ciphers on;
	ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
	ssl_ecdh_curve secp384r1; # Requires nginx >= 1.1.0
	ssl_session_cache shared:SSL:10m;
	ssl_session_tickets off; # Requires nginx >= 1.5.9
	ssl_stapling on; # Requires nginx >= 1.3.7
	ssl_stapling_verify on; # Requires nginx => 1.3.7
	add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
	add_header X-Frame-Options DENY;
	add_header X-Content-Type-Options nosniff;
	proxy_buffering off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header Authorization $http_authorization;
	location / {
		proxy_pass http://10.1.0.1:8123;
	}
    location ~ /.well-known {
            allow all;
    }
}
```