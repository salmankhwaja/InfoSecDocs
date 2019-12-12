# Enable TLS 1.3 in Apache

1. Open your Apache virtual host configuration file. 


```
# vi /etc/httpd/conf.d/vhost.conf
OR
# vi /etc/apache2/apache2.conf
```
2. and locate **ssl_protocols** directive and append **TLSv1.3** at the end of the line as shown below.

```
<VirtualHost *:443>
SSLEngine On

# RSA
ssl_certificate /etc/letsencrypt/example.com/fullchain.cer;
ssl_certificate_key /etc/letsencrypt/example.com/example.com.key;
# ECDSA
ssl_certificate /etc/letsencrypt/example.com_ecc/fullchain.cer;
ssl_certificate_key /etc/letsencrypt/example.com_ecc/example.com.key;

** ssl_protocols TLSv1.2 TLSv1.3**
ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
ssl_prefer_server_ciphers on;
SSLCertificateFile /pathtoCert/cert.pem
SSLCertificateKeyFile /pathtpriv/privkey.pem
SSLCertificateChainFile /pathtokey/chain.pem

     ServerAdmin admin@example.com
     ServerName www.example.com
     ServerAlias example.com
    #DocumentRoot /data/httpd/htdocs/example.com/
    DocumentRoot /data/httpd/htdocs/example_hueman/
  # Log file locations
  LogLevel warn
  ErrorLog  /var/log/httpd/example.com/httpserror.log
  CustomLog "|/usr/sbin/rotatelogs /var/log/httpd/example.com/httpsaccess.log.%Y-%m-%d 86400" combined
</VirtualHost>
```
3. Finally, verify the configuration and reload Apache.
```
-------------- On Debian/Ubuntu -------------- 
# apache2 -t
# systemctl reload apache2.service

-------------- On RHEL/CentOS/Fedora --------------
# httpd -t
# systemctl reload httpd.service
```


Read https://ssl-config.mozilla.org/ for further details if needed
