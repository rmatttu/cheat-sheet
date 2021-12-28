# awk

FSをawkコマンドのオプションとして記述

```bash
cat test.txt | awk -F '\t' '{print $1}'
```

FSをawkスクリプトとして記述

```bash
cat test.txt | awk 'FS="\t" {print $1}'
```


## 参考

* [awkで区切り文字がタブ(tab)のファイルを読み込む方法 | ITを使っていこう](https://it-ojisan.tokyo/awk-tab/)
* [awk システム変数を理解（FS　OFS　RS　ORS　NF　NR　FILENAME） - おぼえがき](https://blog.goo.ne.jp/_memento/e/5666998cdc00d4d69389c343457d1cd9)
