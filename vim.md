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
