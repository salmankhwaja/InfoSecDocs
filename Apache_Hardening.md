# Apache Hardening Instructions.

- [Apache Hardening Instructions.](#apache-hardening-instructions)
    + [Add the Popular Security Headers to Apache Server configuration](#add-the-popular-security-headers-to-apache-server-configuration)
    + [Disable HTTP 1.0 Protocol](#disable-http-10-protocol)
    + [Disable OPTIONS Method in Apache](#Disable-OPTIONS-Method-in-Apache)
    + [Disable Apache server giving our information about which modules are loaded.](#disable-apache-server-giving-our-information-about-which-modules-are-loaded)
    + [Use An Appropriate User and Group](#use-an-appropriate-user-and-group)
    + [Disable Directory Listing in Apache](#disable-directory-listing-in-apache)
    + [Use Mod_Security and Mod_Evasive Modules to secure Server. (Advanced)](#use-mod-security-and-mod-evasive-modules-to-secure-server--advanced-)
    + [Securing Apache with SSL Certificates](#securing-apache-with-ssl-certificates)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



Apache is web server, which is generally controlled by it's config file. The following are the assumptions I will be making in this guide. 
- You know where is your Apache configuration file. 
  - Main Configuration file: /etc/httpd/conf/httpd.conf (RHEL/CentOS/Fedora) and /etc/apache2/apache2.conf (Debian/Ubuntu).
  - Main Configuration file Windows:  C:/Apache/Apache/conf/httpd.conf  (Generally this is the default location, if installation is done with default values) 

Edit the Apache Configuration file using these commands. 

```
# vim /etc/httpd/conf/httpd.conf (RHEL/CentOS/Fedora)
# vim /etc/apache2/apache2.conf (Debian/Ubuntu)
```

### Add the Popular Security Headers to Apache Server configuration

Search and replace the following attributes and use these values for better Server Hardening. I have also tried to document the main functionalities of which line will do what. 


```
# This will disable apache spitting out information in it's headers sent to browsers
ServerSignature Off
ServerTokens Prod
FileETag None

# Important headers to be set so that Proper OWASP Related security could be achieved. 

Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
Header set X-Content-Type-Options nosniff
Header set Content-Security-Policy "default-src 'self';"
Header set Referrer-Policy "no-referrer"
Header unset X-Powered-By

# This will enable all cookies to be httponly, and have secure attribute
Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure

# this will disable CGI processing
Options -Includes
Options -ExecCGI

# This will disable the TRACE functionality of Apache Server
TraceEnable off

```

### Disable HTTP 1.0 Protocol
HTTP 1.0 has security weakness related to session hijacking. We can disable this by using the mod_rewrite module.

Ensure to load *mod_rewrite module* in httpd.conf file
Enable RewriteEngine directive as following and add Rewrite condition to allow only HTTP 1.1

``` 
RewriteEngine On
RewriteCond %{THE_REQUEST} !HTTP/1.1$
RewriteRule .* - [F]
```

### Disable OPTIONS Method in Apache. 

1. Edit the httpd.conf file for the HTTP server.  This is typically in directory /www/<instanceName>/conf/httpd.conf
2. Add these three lines in the httpd.conf file.

```
RewriteEngine On
RewriteCond %{REQUEST_METHOD} ^OPTIONS
RewriteRule .* - [F]
```


### Disable Apache server giving our information about which modules are loaded. 

Comment out the following in Apache Configuration file. 

```
#LoadModule info_module modules/mod_info.so
```

### Use An Appropriate User and Group
By default, Apache runs under the daemon user and group. However, it is best practice to run Apache using a non-privileged account. Furthermore, if two processes (such as Apache and MySQL) are running using the same user and group, issues in one process might lead to exploits in the other process. To change Apache user and group, you need to change the User and Group directives in the Apache httpd.conf configuration file.

```
User apache
Group apache
```

### Disable Directory Listing in Apache

Search and replace the Directory XML Tag with this. 

```
<Directory /var/www/html>
    Options -Indexes
</Directory>
```

For anyone curious to find out more about Apache Hardening, please do browse through the following links 

- https://www.apachecon.eu/
- https://hackernoon.com/apache-web-server-hardening-how-to-protect-your-server-from-attacks-tc1t3umm
- https://www.acunetix.com/blog/articles/10-tips-secure-apache-installation/
- https://www.acunetix.com/blog/articles/10-tips-secure-apache-installation/
- https://geekflare.com/apache-web-server-hardening-security/
- https://geekflare.com/http-header-implementation/
- https://blog.insiderattack.net/apache-security-configuring-secure-response-headers-e8a83b72ce5e
- https://htaccessbook.com/important-security-headers/


### Use Mod_Security and Mod_Evasive Modules to secure Server. (Advanced)
 Where mod_security works as a firewall for our web applications and allows us to monitor traffic on a real time basis. It also helps us to protect our websites or web server from brute force attacks. You can simply install mod_security on your server with the help of your default package installers.

 Install mod_security on Ubuntu/Debian
```
$ sudo apt-get install libapache2-modsecurity
$ sudo a2enmod mod-security
$ sudo /etc/init.d/apache2 force-reload
```
Install mod_security on RHEL/CentOS/Fedora/
```
# yum install mod_security
# /etc/init.d/httpd restart
```

mod_evasive works very efficiently, it takes one request to process and processes it very well. It prevents DDOS attacks from doing as much damage. This feature of mod_evasive enables it to handle the HTTP brute force and Dos or DDos attack. This module detects attacks with three methods.

- If so many requests come to a same page in a few times per second.
- If any child process trying to make more than 50 concurrent requests.
- If any IP still trying to make new requests when its temporarily blacklisted.

Enable Mod_Security and Mod_Evasice from apache config file

```
LoadModule evasive20_module modules/mod_evasive24.so
LoadModule security2_module modules/mod_security2.so
```

More on Mod_Security and Mod_Evasive here
https://www.tecmint.com/protect-apache-using-mod_security-and-mod_evasive-on-rhel-centos-fedora/


### Securing Apache with SSL Certificates

Last, but not the least SSL certificates, you can secure your all the communication in an encrypted manner over the Internet with SSL certificate. Suppose you have a website in which people login by proving their Login credentials or you have an E- Commerce website where people provides their bank details or Debit/Credit card details to purchase products, by default your web server send these details in plain – text format but when you use SSL certificates to your websites, Apache sends all this information in encrypted text.

Once your certificate has been created and signed. Now you need to add this in Apache configuration. Open main configuration file with vim editor and add the following lines and restart the service.

Use your options instead of the options listed here. 

```
<VirtualHost 172.16.25.125:443>
        SSLEngine on
        SSLCertificateFile /etc/pki/tls/certs/example.com.crt
        SSLCertificateKeyFile /etc/pki/tls/certs/example.com.key
        SSLCertificateChainFile /etc/pki/tls/certs/sf_bundle.crt
        ServerAdmin ravi.saive@example.com
        ServerName example.com
        DocumentRoot /var/www/html/example/
        ErrorLog /var/log/httpd/example.com-error_log
        CustomLog /var/log/httpd/example.com-access_log common
</VirtualHost>
```


Last but not the least, read the source of Apache Server Configuration guide and harden your server. 

https://httpd.apache.org/docs/2.4/misc/security_tips.html
