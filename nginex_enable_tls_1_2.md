# Enable TLS 1.2 in nginx and remediate Lucky13 vulnerability.

1. Now start, enable and verify the nginx installation.
```
# systemctl start nginx.service
# systemctl enable nginx.service
# systemctl status nginx.service
```

2. Open the nginx vhost configuration **/etc/nginx/conf.d/example.com.conf** file using your favorite editor. Substitute your site name with **example.com**

```
# vi /etc/nginx/conf.d/example.com.conf
```
3. Locate **ssl_protocols** directive and append **TLSv1.3** at the end of the line as shown below

```
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;

  server_name example.com;

  # RSA
  ssl_certificate /etc/letsencrypt/example.com/fullchain.cer;
  ssl_certificate_key /etc/letsencrypt/example.com/example.com.key;
  # ECDSA
  ssl_certificate /etc/letsencrypt/example.com_ecc/fullchain.cer;
  ssl_certificate_key /etc/letsencrypt/example.com_ecc/example.com.key;

  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_prefer_server_ciphers on;
}
```
4. Finally, verify the configuration and reload Nginx.

```
# nginx -t
# systemctl reload nginx.service
```


In other words, you have to locate the correct configuration file for nginex Website, and add the foloowing snippet. This will also remidaite the Lucky13 vulnerability in nginex.

```
ssl_protocols TLSv1.2;
ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
ssl_prefer_server_ciphers on;
```

Read https://ssl-config.mozilla.org/ for further details if needed
