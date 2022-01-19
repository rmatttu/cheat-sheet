# Git

## ローカル設定

```bash
git config --local user.name "ユーザ名"
git config --local user.email "メールアドレス"
```

## リセット

追跡ファイルの変更をもとに戻す

```bash
git checkout .
```

追跡外ファイルをすべて削除

```bash
git clean -fdx
```

## リポジトリのバックアップ

バックアップ作成

```bash
mkdir sample-repo
cd sample-repo
git init
echo hello > hello.txt
git add hello.txt
git commit -m "First commit"
cd ../
git clone --mirror sample-repo backup.repo
tar cjf backup.repo.tar.bz2 backup.repo
```

クライアント端末で操作

```bash
scp backup.tar.bz2 ssh://somewhereelse
```

リストア

```bash
tar xf backup.repo.tar.bz2
git clone backup.repo sample-repo
cd sample-repo
```

## 表示

コミット差分

```bash
git diff HEAD^ HEAD
git diff @^ @
```

## Archive

```bash
git archive HEAD --output=hoge.zip
```

## 取り消し

現在の変更を取り消し

```bash
git reset --hard HEAD
git reset --hard @
```

コミットの取り消し

```bash
git reset --hard HEAD^
git reset --hard @^
```

(for Windows)

```dos
git reset --hard "HEAD^"
git reset --hard "@^"
```

## ブランチ

ローカルブランチ削除

```bash
git branch --delete hoge
git branch -d hoge
```

ローカルブランチ名変更

```bash
git branch -m hoge fuga
git branch -m huga
```

リモートブランチ削除

```bash
git push origin :hoge
```

ローカルブランチのプッシュ

```bash
git push origin fuga
```
prune

```bash
git remote prune origin
```

* 参考
  * [Gitのリモートブランチがローカルでゾンビ化した場合の対処法 - Qiita](https://qiita.com/hy3/items/96267f9cce825e685baf)

## タグ

HEADにタグを付与
```bash
git tag <タグ名>
```

タグのpush

```bash
git push origin <タグ名>
```

## リモート

origin URL確認

```bash
git remote -v
```

## Windows環境でファイルパーミッション変更をコミット

```dos
git update-index --add --chmod=+x script.sh
```

## Git info

```bash
script_dir_path=$(dirname $(readlink -f $0))
root_dir_name=`basename $script_dir_path`

# このプロジェクトのバージョン情報を出力
(
  echo -e "$root_dir_name\tGit_branch\t`git rev-parse --abbrev-ref HEAD`"
  echo -e "$root_dir_name\tGit_HEAD\t`git rev-parse HEAD`"
  echo -e "$root_dir_name\tGit_tags\t`git describe --tags`"
  echo ""
) >> git-info.txt
```
