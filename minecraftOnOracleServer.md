
# Oracle Cloud インスタンス作成


# 環境確認
```
# メモリ確認
free -m

```
# ポート開放
インスタンスに関してのポート開放が `ufw` コマンドじゃ適用されてくれなくて途方に暮れた。 `iptables` を使ったらいけた。
```
sudo iptables -I INPUT 5 -p tcp --dport 25565 -j ACCEPT
```

# 実行ユーザー作成
どうせだし。
```
sudo adduser minecraft
# 削除は `deluser`
# マイクラサーバーデータ他から持ってくるなら
# `chown -R minecraft:minecraft`
```

# サービス化
- https://qiita.com/DQNEO/items/0b5d0bc5d3cf407cb7ff
- https://k5342.hatenablog.com/entry/2018/04/23/111331

/home/minecraft/minecraft.service
```
[Unit]
Description = Minecraft Server

[Service]
ExecStart = /home/minecraft/server/start.sh
Restart = always
Type = simple
User = minecraft

[Install]
WantedBy = multi-user.target
```
を書いて

```
sudo ln -s /home/minecraft/minecraft.service /etc/systemd/system/minecraft.service
sudo systemctl list-unit-files --type=service | grep minecraft
# 検索結果に `minecraft.service` が出れば成功
sudo systemctl enable minecraft
# enable コマンドには serviceコマンド版 ない感じっぽい？
sudo service minecraft start
sudo service minecraft status
```
