# Let's Encrypt で https 対応
## environment
os: Ubuntu 18.04.5 LTS

## process
```
$ curl -O https://dl.eff.org/certbot-auto
$ sudo mv certbot-auto /usr/local/bin/certbot-auto
$ sudo chown root /usr/local/bin/certbot-auto
$ sudo chmod 0755 /usr/local/bin/certbot-auto
$ sudo service apache2 stop
$ sudo /usr/local/bin/certbot-auto
```
指示に従う

証明書ファイルが保存される
```
$ sudo ls -la /etc/letsencrypt/live/%{subdomain}
```

起動
```
$ sudo service apache2 start
$ sudo a2enmod ssl
$ sudo a2ensite default-ssl
$ sudo service apache2 restart
```

一応
```
$ sudo /usr/local/bin/certbot-auto
(choose domain)
(1. Attempt to reinstall this existing certificate)
```
