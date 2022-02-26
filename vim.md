# vim

## Unite

* Unite buffer: `tb`
* Unite file mru: `tz`

## Batch

Convert to uft-8 LN

```bash
ls -1 *.txt | xargs -I{} vim -c 'set ff=unix' -c 'set fenc=utf-8' -c 'wq' {}
```

## LF to CRLF

```bash
vi -c ":set ff=dos" -c ":wq" src.txt
```

参考

* [vimで文字コードを指定してファイルを開くコマンドラインオプション | IT業務で使えるプログラミングテクニック](https://kekaku.addisteria.com/wp/20190321045617)

## tac (tail -r)

```vim
:g/^/m0
```

## 連番の作成

```txt
1.
1.
1.
```

この状態で、2～3行目を選択し`g<C-A>`

[vim-jp » Visual モード時の CTRL-A/CTRL-X について](http://vim-jp.org/blog/2015/06/30/visual-ctrl-a-ctrl-x.html)

## 便利メモ

* 小カッコ内で`ci(`または`ci)`
* アンダーバー区切りの変数等で`ct_`
