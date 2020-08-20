# Let's Encrypt で https 対応
## environment
os: Ubuntu 18.04.5 LTS

## process
```
$ sudo apt install apache2
$ sudo ufw allow 'Apache Full'
```

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

自動更新
```
$ sudo /usr/local/bin/certbot-auto renew --post-hook "sudo service apache2 restart"
$ sudo vi /etc/cron.d/certbot
30 12 * * 1 root /usr/local/bin/certbot-auto renew --deploy-hook 2>&1 "sudo service apache2 restart" >&1 | mail -s "Let's Encrypt auto update" ${email}
$ service crond restart
```

http -> https へのリダイレクト
```
$ sudo vi /etc/apache2/sites-available/http-redirect.conf
<VirtualHost *:80>
  Redirect permanent / https://${domain}/
</VirtualHost>
$ sudo a2ensite http-redirect
$ sudo service apache2 restart
```

お好み
```
$ sudo ufw allow 'Apache Secure'
```


## references
https://qiita.com/yuichi1992_west/items/a021b078c9aa8cc25b76
https://deep-blog.jp/engineer/11632/
http://li-one.hatenablog.jp/entry/2016/07/04/110547
