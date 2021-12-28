# screen

## インストール


```bash
sudo apt-get -y install screen
```

## 使い方

screenに名前をつけて新規作成しアタッチ

```bash
screen -S my_screen_name
```

screenをデタッチ

```bash
<ctrl> + <a>
d
```

screen セッション一覧表示

```bash
screen -ls
```

作成済みのscreenにアタッチ

```bash
screen -r my_screen_name
```

## 参考

* https://qiita.com/hnishi/items/3190f2901f88e2594a5f (Linux screenコマンド使い方)
* https://qiita.com/kmszk/items/9939b67e923dbb87f39c (.screenrcにこれだけは設定しとけっていうオススメ設定)

```bash
cat ~/.screenrc
```

```txt
Encoding
defutf8 on
defencoding utf8
encoding utf8 utf8

defbce on
term xterm-256color

# Prefix Key
escape ^Gg

# set scrollback
# メモリ量と相談
# defscrollback 1000000

# Delete sartt up screen
startup_message off

# Enable Auto detach
autodetach on

hardstatus alwayslastline "%{= rw} %H %{= wk}%-Lw%{= bw}%n%f* %t%{= wk}%+Lw %{= wk}%=%{= gk} LoadAVG [%l] %y/%m/%d %c "
termcapinfo xterm* ti@:te@
altscreen on
```

参考

* [作業がグッと楽になる screen を使おう！ - bacchi.me](https://bacchi.me/linux/how-2-use-screen/)
* [.screenrcにこれだけは設定しとけっていうオススメ設定 - Qiita](https://qiita.com/kamykn/items/9939b67e923dbb87f39c)
* [GNU 'screen' Status Line(s)](https://www.gilesorr.com/blog/screen-status-bar.html)
