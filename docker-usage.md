# Docker操作方法

## docker

### 一覧の表示

```
# イメージ一覧
docker images

# コンテナ一覧
docker ps -a
```


### イメージからコンテナ作成

```bash
# 通常
docker run --name {作成されるコンテナ名} -i -t {対象のイメージID or イメージ名} bash

# 共有フォルダ作成（指定ディレクトリはホスト側・コンテナ側いずれも絶対パスでなければならない）
docker run --name {作成されるコンテナ名} -v <ホスト側のディレクトリ>:<コンテナ側のディレクトリ> -i -t {対象のイメージID or イメージ名} bash

# エントリーポイントを書き換えて、即終了しないようコマンドをかませる
# 起動したイメージに tail -f /dev/null を実行後、アタッチ
# -d オプションを入れると良い
docker run --rm -it --name test --entrypoint=tail {イメージ名} -f /dev/null
```


### コンテナ起動、終了

```bash
# コンテナ起動
nvidia-docker start {コンテナ名}

# 起動後即アタッチ
docker start -a -i {コンテナ名}

# 起動した{コンテナ名}に入る
docker attach {コンテナ名}

# コンテナから一時的に出る
Ctrl + p, Ctrl + q

# コンテナを終了
exit
```


### コンテナからイメージ作成

```bash
docker commit {コンテナID} {作成するイメージ名}
docker commit {コンテナID} {作成するイメージ名:タグ名}
```


### コンテナ・イメージ削除

```bash
docker rm {コンテナ名}
docker rmi [イメージID]
# ※環境が壊れる為、nvidia-dockerコマンドで消さないように
```

ワンライナー

```bash
# すべてのコンテナを削除
docker ps -aq   | xargs -I{} docker rm {}

# <None>のイメージを削除
docker images -f "dangling=true" -q | xargs -I{} docker rmi {}
```


### nvidia-docker関連確認

コンテナ内で下記コマンド入力

```bash
# GPU情報を表示
nvidia-smi

# Cudaバージョン情報を表示
nvcc -V
```

### dockerでvolumeをマウントしたときのファイルのowner問題解決方法

ホストのuser id、group idをチェック

```bash
id
# > uid=1000(user) gid=1000(user) groups=1000(user),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd),999(docker)
# > この例では、現在ログインしているUSER_IDは1000、GROUP_IDは1000
```

docker container内で、同じuser id、group idのユーザを作成

```bash
USER_ID=1000 # 先程確認したUSER_IDと同じ値を設定
GROUP_ID=1000 # 先程確認したUSER_IDと同じ値を設定
useradd -s /bin/bash user
usermod -u $USER_ID -o -m user
groupmod -g $GROUP_ID user
su user
```

* 参考
  * [dockerでvolumeをマウントしたときのファイルのowner問題 - Qiita](https://qiita.com/yohm/items/047b2e68d008ebb0f001)
  * [【 useradd 】コマンド――新規ユーザーを作成する：Linux基本コマンドTips（255） - ＠IT](https://www.atmarkit.co.jp/ait/articles/1811/02/news035.html)
  * [シェルで現在のユーザー名を取得する方法 | LFI](https://linuxfan.info/post-1586)


## Docker-compose


### コマンド集

Docker Composeで作ったコンテナ、イメージ、ボリューム、ネットワークを一括消去

```bash
docker-compose down --rmi all --volumes
```

停止コンテナ、タグ無しイメージ、未使用ボリューム、未使用ネットワーク一括削除

```bash
docker system prune
```

* 参考
   * [《滅びの呪文》Docker Composeで作ったコンテナ、イメージ、ボリューム、ネットワークを一括完全消去する便利コマンド - Qiita](https://qiita.com/suin/items/19d65e191b96a0079417)
   * [Docker一括削除コマンドまとめ - Qiita](https://qiita.com/boiyama/items/9972601ffc240553e1f3)

rootでbash起動

```bash
docker-compose exec --user root app bash
```

### Docker-compose設定例

```bash
vim docker-compose.yml
```

```bash
version: '2.3'
services:
    app:
        image: vvakame/review:2.5
        volumes:
            - ./share:/share
        ports:
            - "3306:3000"
        entrypoint: /bin/bash
        # entrypoint: /bin/sh
        command: tail -f /dev/null
        # command:
        #     - tail
        #     - -F
        #     - /dev/null
        logging: # ログ出力をsyslogへ変更（デフォルトはJSONで、docker-compose downで自動的に削除されてしまう）
          driver: syslog
          options:
            syslog-facility: daemon
            tag: sample-app/{{.Name}}/{{.ID}}
            # {{.Name}}: コンテナ名
            # {{.ID}}: コンテナID
```

docker-composeでの起動、削除

```bash
# バックグラウンドで起動
docker-compose up -d

# コンテナごと削除
docker-compose down

# 停止
docker-compose stop

# docker exec
docker-compose exec app bash
```

参考
* [Dockerのログをrsyslogで出力する - Qiita](https://qiita.com/Esfahan/items/5e5a9ae7882bb0eaaf5f)


## docker image確認

entrypoint確認

```bash
docker inspect -f "{{ .Config.Entrypoint  }}" node:latest
```

cmd確認

```bash
docker inspect node:latest --format='{{.Config.Cmd}}'
```

## その他設定

### detach-keys の変更

Ctrl-p について、キーバインドがdockerに乗っ取られているため、docker内bashのキーバインドがおかしくなる。
dockerユーザ設定で、detach-keyを変更する。


```
mkdir ~/.docker
vim ~/.docker/config.json
```

```json
{
    "detachKeys": "ctrl-\\"
}
```


### Ubuntu 16.04 日本語向け image

下記Dockerfileを作成

```
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
MAINTAINER Admin <admin@admin.com>

# apt-getのサーバを日本のサーバに変更
RUN sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list && \
    apt-get -y update && \
    apt-get -y upgrade

# 日本時間設定、日本語設定
RUN apt-get -y install language-pack-ja-base language-pack-ja && \
    echo "export LANG=ja_JP.UTF-8" >> ~/.bashrc && \
    echo "export LESSCHARSET=utf-8" >> ~/.bashrc && \
    apt-get -y update && \
    apt-get -y install tzdata && \
    rm -rf /var/lib/apt/lists/* && \
    echo "Asia/Tokyo" > /etc/timezone && \
    rm /etc/localtime && \
    ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get -y update

# 基本ソフトウェアインストール、git設定
RUN apt-get -y install \
    curl \
    wget \
    git \
    zip \
    vim-gtk
```

下記コマンドを実行

```bash
docker build -t admin/test .
```

`docker-compose.yml`を作成

```yml
version: '2.3'
services:
    app:
        image: admin/test
        runtime: nvidia
        command: tail -f /dev/null
        volumes:
            - ./share:/root/share
```

### Dockerfileの書き方

参考

* [Dockerfileを書くときに気をつけていること10選 - Qiita](https://qiita.com/c18t/items/f3a911ef01f124071c95)
* [Dockerfile のベストプラクティス — Docker-docs-ja 1.9.0b ドキュメント](http://docs.docker.jp/engine/articles/dockerfile_best-practice.html)
* [Dockerfile内のENTRYPOINTとCMD（とRUN） の使い分け - コード日進月歩](https://shinkufencer.hateblo.jp/entry/2019/01/28/225553)


### Docker Compose の設定ファイル名を環境変数で指定する

毎回 -f を指定するのは面倒なので、これは環境変数で指定することが出来る。

```bash
COMPOSE_FILE=docker-compose.yml docker-compose up

# 複数指定する場合は : で結合
COMPOSE_FILE=docker-compose.yml:docker-compose.override.yml
```

docker-compose コマンドはデフォルトで .env から環境変数を読み込むので、これを書いておけば毎回ファイル名を指定する必要がなくなる。

```bash
echo "COMPOSE_FILE=docker-compose.yml:docker-compose.override.yml" > .env
```

"マージ"の定義

> 1 つの値しか持たないオプション、たとえば image、command、mem_limit のようなものは、古い値が新しい値に置き換えられます。
> 複数の値を持つオプション、つまり ports、 expose、 external_links、 dns、 dns_search、 tmpfs では、両者の設定をつなぎ合わせます
> environment、 labels、 volumes、 devices の場合、Compose は設定内容を "マージ" して、ローカル定義の値が優先するようにします。


参考

[ファイル間、プロジェクト間での Compose 設定の共有 — Docker-docs-ja 19.03 ドキュメント](https://docs.docker.jp/compose/extends.html)


## dockerhub

### CIを設定し、自動ビルドする方法（推奨）

* 適当なGithub.comリポジトリを作成
  * docker-xxxx
  * `docker-`をプレフィックスにしたほうが良い？
* Dockerfileをプッシュ
* tagをつける
  * `1.0.0`など
* dockerhubにgithubのアカウントを連携しておく
* dockerhubでリポジトリ新規作成
  * 作成と同時にgithubのリポジトリを指定
* BuildsタグでCIを設定
  * Configure Automated Builds > Automated Builds
    * Master ブランチを指定
    * Dokerfile名を指定
    * Contextを指定


### イメージを直でプッシュする方法

```bash
docker login
~~~ ユーザ名、パスワード、メールアドレスを入力してログイン ~~~
```

今回付けたタグ名を指定してプッシュします。

```bash
docker push [Dockerhubユーザ名]/myfirstimage
```

### Tag list

```bash
wget -q https://registry.hub.docker.com/v1/repositories/debian/tags -O -  | sed -e 's/[][]//g' -e 's/"//g' -e 's/ //g' | tr '}' '\n'  | awk -F: '{print $3}'
```


# References

* [docker-compose.ymlが環境別に複数ある場合はCOMPOSE\_FILEを定義しておくと幸せになれる](https://suin.io/535)
* [ファイル間、プロジェクト間での Compose 設定の共有 — Docker-docs-ja 19.03 ドキュメント](https://docs.docker.jp/compose/extends.html)
* [ローカル環境用の設定をdocker-compose.override.ymlに書く](https://sunday-morning.app/posts/2019-10-23-docker-compose-override-yml)
* [How can I list all tags for a Docker image on a remote registry? - Stack Overflow](https://stackoverflow.com/questions/28320134/how-can-i-list-all-tags-for-a-docker-image-on-a-remote-registry)
