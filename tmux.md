# tmux


## ペイン移動

ペインを別ウィンドウに持っていく

```bash
:join-pane -t [session名]:<windowインデックス値>
```

現在のウィンドウに別ウィンドウをペイントして追加

```bash
:join-pane -s [session名]:<windowインデックス値>
```

水平ペインを垂直ペインに変換

```bash
<Prefix> + <Space>
```


* [tmux: ペインの移動 - nju33](https://nju33.com/tmux/%E3%83%9A%E3%82%A4%E3%83%B3%E3%81%AE%E7%A7%BB%E5%8B%95)
* [tmuxのjoin-paneからpaneの指定方法を学ぶ (ターミナルマルチプレクサ Advent Calendar 2日目) - kozo2のはてなダイアリー](https://kozo2.hatenadiary.org/entry/20111202/1322827858)
