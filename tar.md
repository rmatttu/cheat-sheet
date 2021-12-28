# tar

## 解凍

拡張子から自動認識してくれる？

```bash
tar xf archive.tar.gz
tar xf archive.tar.bz2
tar xf archive.tar
```

## 圧縮

### tar

```bash
tar cf archive.tar mydir/
```

### tar.gz

```bash
tar zcf archive.tar.gz mydir/
```

### tar.bz2

```bash
tar Jcf tarchive.tar.bz2 mydir/
```
## 確認

解答せず、ファイルのみ閲覧

```bash
tar tf hoge.tar.gz
tar tvf hoge.tar.gz
```
