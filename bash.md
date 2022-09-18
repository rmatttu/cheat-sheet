# bash

## exec

今後実行されるコマンドの標準出力・標準エラー出力をファイルにも書き出す

```bash
exec &> >(tee --append $log_name)
```

## Find

```bash
find /path/to/find -type f
find /path/to/directory -type d

# time filter
find -newermt "2022-09-15 17:21:44" -type f

# file size filter
find -type f -size +0c
```

Remove "./"

```bash
find -type f
# ./a.txt
# ./b.txt
# ./c.txt

find -type f -printf '%P\n'
# a.txt
# b.txt
# c.txt
```

## Shebang

```bash
#!/bin/sh
#!/bin/bash
#!/usr/bin/env bash
```

## Time

Elapsed time

```bash
start_time=`date +%s`
sleep 3.5
echo "Elapsed time: $((`date +%s` - start_time)) s"
```

Datetime

```bash
now=`date +%Y-%m-%d_%H-%M-%S`
# 2021-05-16_11-07-01
today=`date +%Y-%m-%d`
# 2021-05-16
```

## Loop

for

```bash
max=10
for ((i=0; i < $max; i++)); do
  echo $i
done
```

fori

```bash
for i in 0 1 3 5 8 9; do
  echo $i
done
```

for line

```bash
# IFS で区切り文字を指定
# IFS を指定しないとスペースとタブでも区切られてしまう
IFS=$'\n'
for i in `cat filelist.txt`; do
  dirname $i
  basename $i
  echo $i
done
```

Infinity loop

```bash
while do
  echo "Press [CTRL+C] to stop..."
  sleep 1
done
```

## xargs

```bash
find ./ -newermt "2021-01-01" -type f | xargs -I{} echo {}
```

## Function

```bash
f () {
  echo Show args $1 $2 $3
}
```

## iconv

sjis to utf8

```bash
iconv -f sjis -t utf8 txt2wav.c >txt2wav.utf8.c
iconv -f cp932 -t utf8 txt2wav.c >txt2wav.utf8.c
```

## diff

Like "git diff"

```bash
diff -U0 a.txt b.txt
```

## sed

```bash
cat windows_file_pathes.json | sed -e "s;\\\\\\\\;/;g" > posix_file_pathes.json
```

## sort

```bash
sort -k 2 -n filename
sort --key 2 --numeric-sort filename
sort -t$'\t' -k2 -n FILE
```

## jq

### substr

文字列置換

```bash
cat windows_files.jsonl | jq '.path |= gsub("\\\\"; "/")' > posix_files.jsonl
```

### sort

```bash
cat db.jsonl | jq -cs | jq '. | sort_by(.total_score) | reverse | .[]' > ranking.json
```

## touch

```bash
touch --date "2021/12/09 00:00"  test.txt
touch --date "2021/11/09 00:00"  test2.txt
touch --date "2021/01/09 00:00"  test3.txt
```


## Template

```bash
#!/usr/bin/env bash
# Usage
#

set -e
set -u
start_time=`date +%s`
cd `dirname $0`
script_dir_path=$(dirname $(readlink -f $0))

# some code

echo "Elapsed time: $((`date +%s` - start_time)) s"
```

## Path expansion

```bash
full_path='/path/to/file/target.txt'

# Path without extension
echo ${full_path%.*}
# /path/to/file/target

# Get filename
echo ${full_path##*/}
# target.txt

# Filename without extension
name=${full_path##*/}
echo ${name%.*}
# target

# Extension
echo ${full_path##*.}
# txt
```

## Variable Is Empty Or Not

```bash
if [ -z "$var" ]
then
  echo "\$var is empty"
else
  echo "\$var is NOT empty"
fi
```

Or

```bash
if test -z "$var"
then
  echo "\$var is empty"
else
  echo "\$var is NOT empty"
fi
```

Not

```bash
if [ ! -z "$var" ] ; then
  echo "\$var is NOT empty"
fi
```

## Check unset variable

`set -u`セット時でも変数を確認

```bash
set -u

if [ -z "${var-}" ] ; then
  echo "\$var is empty"
else
  echo "\$var is NOT empty"
fi
```

参考

[How To Bash Shell Find Out If a Variable Is Empty Or Not - nixCraft](https://www.cyberciti.biz/faq/unix-linux-bash-script-check-if-variable-is-empty/)

## Log

目立つように

```bash
print_log () {
  echo -e "\e[31m"
  echo "**************************************"
  echo $1
  echo "**************************************"
  echo -e "\e[m"
}
```

1行で

```bash
print_log () {
  script_name=`basename $0`
  echo -e "\e[31m`date +%Y-%m-%d_%H-%M-%S`\t$script_name\t$1\e[m"
}
```

## .env

```bash
$ cat << EOD > .env
DOT_ENV_VALUE_TEST="this is test"
DOT_ENV_VALUE_MEMO=staging
EOD
```

run.sh

```bash
#!/usr/bin/env bash

set -e
set -u

eval "$(cat .env <(echo) <(declare -x))"

echo $DOT_ENV_VALUE_TEST
echo $DOT_ENV_VALUE_MEMO
```

実行例

```bash
./run.sh
# this is test
# staging

DOT_ENV_VALUE_MEMO=development ./run.sh
# this is test
# development
```

## Here document

```bash
cat << EOS >>/root/.bashrc
# switch user node
su node
cd ~/
EOS
```

`'EOS'`

```bash
cat << 'EOS' > ~/.docker/config.json
{"detachKeys": "ctrl-\\"}
EOS
```

echo

```bash
{ \
  echo '# switch user "node"'; \
  echo 'su node'; \
  echo 'cd ~/'; \
} >> /root/.bashrc
```

## ログ出力Template

launcher.sh

```bash
#!/usr/bin/env bash

set -e
set -u

start_time=`date +%s`
cd `dirname $0`
script_dir_path=$(dirname $(readlink -f $0))
log_name=$script_dir_path/log.log

./main.sh 2>&1 | tee -a $log_name
echo "Elapsed time: $((`date +%s` - start_time)) s" | tee -a $log_name
```

## 保守・監視テスト用

CPU

```bash
yes > /dev/null
```

Storage

```bash
# 1Gbyte のファイルを作成
dd if=/dev/zero of=1G.dummy bs=1M count=1000

# 9Mbyte のファイルを作成
dd if=/dev/zero of=test9M bs=1M count=9
```

## 応用

自力でsize順ソートしてみる

```bash
ls -l --time-style=long-iso | awk 'BEGIN {OFS="\t" }{print $1" "$2, $5, $6" "$7" "$8, $9}' | sort -t $'\t' -k 2  -n > ls.tsv
```

* [\[ sort \]tab区切りデリミタ指定 - Qiita](https://qiita.com/penguin_dream/items/0de59b6682c84b4d9395)
* [ls コマンドのタイムスタンプの書式を指定する - 知に至る病](https://amano41.hatenablog.jp/entry/ls-timestamp-using-date-format)

## awk CSV file

```bash
awk 'FS="," {print $1}' test.csv > column1-data.txt
```

## grep

With regex

```bash
# Get no-data in column2.
awk 'FS="," {print $1 $2}' id-file-list-in-windows.csv | grep -E $'\t$' > no-data-rows.tsv
```

Print only matching.

```bash
cat id-file-list.txt
# dir-a/111.txt
# dir-a/101.csv
# dir-b/223.jpg
# dir-c/32.png

cat id-file-list.txt | grep -o -E '[0-9]+\.'
# 111
# 101
# 223
# 32
```

## sed

```bash
awk 'FS="," {print $1 $2}' id-file-list-in-windows.csv | grep -o -E $'\t$' > no-data-rows.tsv
cat filelist-dos.txt | sed "s/\\\//g" > filelist-posix.txt
```

## Read tsv

1行1行処理する

```bash
while IFS=$'\t' read index name created_at updated_at
do
  echo $index, $name, $created_at, $updated_at
done < input.tsv
```

## ps, kill

list processes

```bash
ps aux
ps auxwwf
```

kill <PID>

```bash
kill 1234
```

force

```bash
sudo kill -p 1234
```


## References

* [bashでのPID取得方法まとめ($、$PPID、$!、$BASHPID) - Qiita](https://qiita.com/laikuaut/items/1daa06900ad045d119b4)
* [【 kill 】コマンド／【 killall 】コマンド――実行中のプロセスを終了させる：Linux基本コマンドTips（8）（1/2 ページ） - ＠IT](https://www.atmarkit.co.jp/ait/articles/1604/05/news022.html)
* [Linux / Unix: bg Command Examples - nixCraft](https://www.cyberciti.biz/faq/unix-linux-bg-command-examples-usage-syntax/)
