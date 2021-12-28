# Docker Ubuntu固有の環境構築


```bash
# apt-get取得先を日本のサーバーに変更 > 参考 https://qiita.com/fkshom/items/53de3a9b9278cd524099
sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
apt -y autoremove

# 日本語パック（インストールしないと日本語が入力できない）
apt-get -y install language-pack-ja-base language-pack-ja
echo -e "\nexport LANG=ja_JP.UTF-8" >>  ~/.bashrc
source ~/.bashrc

# タイムゾーン変更 > 参考 https://qiita.com/windyakin/items/d73157966d414978b7cf
export TZ="Asia/Tokyo"
apt-get update \
  && apt-get install -y tzdata \
  && rm -rf /var/lib/apt/lists/* \
  && echo "${TZ}" > /etc/timezone \
  && rm /etc/localtime \
  && ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
  && dpkg-reconfigure -f noninteractive tzdata
```

## Append

下記は必要に応じて

```bash
apt-get -y install git zip wget curl vim-gtk

# pingなどのコマンド
apt-get install -y iputils-ping net-tools
apt-get install -y curl
```

Gitログなどの文字化けを直す

```
# gitの文字コード設定
echo -e "\n\nexport LESSCHARSET=utf-8\n\n" >>  ~/.bashrc
# 即反映
source ~/.bashrc
```


Gitのコミット時のエディターをvimにする

```
git config --global core.editor "vi"
```


python2.7環境

```bash
build-essential libsqlite3-dev libreadline6-dev libgdbm-dev zlib1g-dev libbz2-dev sqlite3 tk-dev libssl-dev python-dev
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```

python-mecab環境

```bash
apt-get install mecab mecab-ipadic-utf8
apt-get install python-mecab
python
cd /Library/Python/2.7/site-packages/
echo -e "\nimport sys\nsys.setdefaultencoding('utf-8')" >> /usr/lib/python2.7/sitecustomize.py
```

