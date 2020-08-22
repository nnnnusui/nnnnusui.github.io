# Let's Encrypt で https 対応
## environment
os: Ubuntu 18.04.5 LTS

## process
### サーバー立ち上げ
```
$ sudo apt install apache2
$ sudo ufw allow 'Apache Full'
```

```
$ curl -O https://dl.eff.org/certbot-auto
$ sudo mv certbot-auto /usr/local/bin/certbot-auto
$ sudo chown root /usr/local/bin/certbot-auto
$ sudo chmod 0755 /usr/local/bin/certbot-auto
```

起動
```
$ sudo a2enmod ssl
$ sudo a2enmod rewrite
$ sudo service apache2 restart
```

Apacheのconfigを記述
```
$ cd /etc/apache2/sites-available/
$ sudo vi domain.com.conf
<VirtualHost *:80>
  ServerName domain.com
  DocumentRoot /var/www/html
</VirtualHost>
$ sudo vi sub.domain.com.conf
<VirtualHost *:80>
  ServerName sub.domain.com
  Redirect permanent / https://redirect.com/
</VirtualHost>
$ sudo a2ensite *
$ sudo service apache2 restart
```

### https化
```
$ sudo /usr/local/bin/certbot-auto
$ sudo service apache2 restart
```
受け付けている(Apacheのconfigに記述した)ドメインが一覧に出る。

一覧からドメインを選んでSSL化。

Apacheのconfigにredirectが自動で追記されるし、https通信用に新しく生成されたconfigも自動で `a2ensite` される。


証明書ファイルが保存されている
```
$ sudo ls -la /etc/letsencrypt/live/%{subdomain}
```

自動更新の設定
```
$ sudo /usr/local/bin/certbot-auto renew --post-hook "sudo service apache2 restart"
$ sudo vi /etc/cron.d/certbot
30 12 * * 1 root /usr/local/bin/certbot-auto renew --deploy-hook 2>&1 "sudo service apache2 restart" >&1 | mail -s "Let's Encrypt auto update" ${email}
$ service crond restart
```

その他のページの http -> https へのリダイレクト
```
$ sudo vi 002-httpsRedirect.conf
<VirtualHost *:80>
  RewriteEngine on
  RewriteRule ^.*$ https://%{HTTP_HOST}%{REQUEST_URI} [R,L]
</VirtualHost>
$ sudo a2ensite http-redirect
$ sudo service apache2 restart
```

### 他、コマンドメモ
```
$ sudo a2dissite ${.confのファイル名}
.confを適用範囲から除外
$ sudo a2dismod ${mod名}
modの停止
```


## references
- https://qiita.com/yuichi1992_west/items/a021b078c9aa8cc25b76
- https://deep-blog.jp/engineer/11632/
- http://li-one.hatenablog.jp/entry/2016/07/04/110547
