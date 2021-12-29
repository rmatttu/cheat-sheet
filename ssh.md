# ssh


## .ssh/config


```bash
cd ~
mkdir .ssh
chmod 700 .ssh
cd .ssh
# 既存の鍵をここへコピー→id_rsa
chmod 600 id_rsa
vi config
```

下記ファイルを作成

```
Host github github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa
    User git
Host gitlab.com
    User git
    HostName gitlab.com
    TCPKeepAlive yes
    identitiesonly yes
    identityFile ~/.ssh/id_rsa
Host bitbucket.org
    HostName bitbucket.org
    IdentityFile ~/.ssh/id_rsa
    User git
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes
```

確認

```bash
ssh -T github.com
ssh -T gitlab.com
ssh -T bitbucket.org
```


## SSHキー設定方法


Mac（クライアント側）でキーペアを作成し、Ubuntu(サーバ側)へログイン用鍵を設定する


### SSHキー作成

暗号強度4096で作成

```bash
# キー生成
ssh-keygen -t rsa -b 4096 -C "this is memo"
```

必要があれば、下記を実施

```bash
# キー設置
mkdir .ssh
chmod 700 .ssh
mv id_rsa id_rsa.pub ~/.ssh

# 既に600かもしれない、一応実行
chmod 0600 ~/.ssh/id_rsa
chmod 0600 ~/.ssh/id_rsa.pub
```


### サーバへ鍵登録

```bash
# 公開鍵コピー(macOS)
pbcopy < ~/.ssh/id_rsa.pub

# サーバへログイン
ssh {ユーザ名}@{サーバIP} -p {SSHポート}

# 下記はサーバでの操作
mkdir .ssh
chmod 700 .ssh
cd .ssh

# 公開鍵貼り付け
vim authorized_keys

chmod 0600 ~/.ssh/authorized_keys

sudo su -
vi /etc/ssh/sshd_config
```

下記項目を修正

```
Port 22

...

PubkeyAuthentication yes
AuthorizedKeysFile     .ssh/authorized_keys

...

PasswordAuthentication no
```

sshd再起動
```bash
which sshd
# 出力例: /usr/sbin/sshd
/usr/sbin/sshd -t
# （設定ファイルに間違いがなければ何も表示されない）
service sshd restart
```

パスワードではログインできなくなる。
このターミナルはログアウトせず、別ターミナルを立ち上げ、設定した秘密鍵でログインできることを確認。


```bash
# 接続確認
ssh -i ~/.ssh/id_rsa {ユーザ名}@{サーバIP} -p {SSHポート}
```
### .ssh/configへ追記

クライアント側の`.ssh/config`に接続設定を記述する

```
Host my-server
    User username
    HostName 192.168.123.456
    Port 22
    IdentityFile ~/.ssh/id_rsa
```

今後は下記コマンドで接続できる

```bash
ssh my-server
```

scpでファイル転送する際も使用できる

```bash
# -i でssh鍵を指定しなくて良い
scp my-server:/home/username/test.zip .
```


参考

* https://qiita.com/suthio/items/2760e4cff0e185fe2db9 (お前らのSSH Keysの作り方は間違っている)
* https://qiita.com/gotohiro55/items/36a22516de2b381b3c6e (SSHの鍵認証設定)


## ポートフォワーディング

* サーバ上の33000番ポートで公開されているWebページを閲覧
* サーバ上の33000番ポートは開いていない

### コマンド

```bash
ssh -L 8080:localhost:33000 test@12.34.56.78 -i private_auth_key -p 2020
```

ローカルマシンでブラウザを開き、`localhost:8080`へアクセス

### config設定での接続

```bash
vi ~/.ssh/config
```

```conf
Host port-forward
    User test
    HostName 12.34.56.78
    Port 2020
    IdentityFile ~/.ssh/private_auth_key
    LocalForward 33000 localhost:33000
```

```bash
ssh -N myuser-test
```

ローカルマシンでブラウザを開き、`localhost:8080`へアクセス

### その他オプション

* -f : バックグラウンドで動作
* -N : コマンド実行無し

## `known_hosts`

```bash
ssh-keygen -R example.com
ssh-keygen -R '[example.com]:10022'
```

## ツール

自分のグローバルIPを取得

```bash
curl -s ipinfo.io | jq -r '.ip'
curl ifconfig.me
curl http://checkip.amazonaws.com/
```

自分のローカルIPを取得

```bash
ifconfig en0 | grep inet
```
