# mysql

コマンド備忘録

## Client

ログイン

```bash
mysql -u <user> -p [schema-name]
```

## SQL

### 基本

```sql
show databases;
use database01;
show tables;
show columns from tagle01;
show index from table01;

create index idx_name on tags(name);
create index idx_translated_name on tags(translated_name);

drop database main;
create database main;
```

### パフォーマンス

```sql
SET tmp_table_size=4*1024*1024*1024;
SET max_heap_table_size=4*1024*1024*1024;
```

### ユーザ

追加

```bash
CREATE USER 'app'@'%' IDENTIFIED BY 'secret';
GRANT ALL PRIVILEGES ON *.* TO 'app'@'%';
FLUSH PRIVILEGES;
```

### エクスポート

mysqldumpを使用する

```bash
mysqlhost=localhost
mysqlschema=main
output_filename=${mysqlhost}-${mysqlschema}-`date +%Y-%m-%d_%H-%M-%S`.sql

mysqldump \
  -u root \
  -psecret \
  -h ${mysqlhost} \
  --single-transaction --routines --triggers --events --hex-blob=TRUE --lock-tables=FALSE \
  ${mysqlschema} > ${output_filename}
gzip ${output_filename}
```

### インポート

```bash
mysql -uroot -psecret main < main.sql
```

docker-composeの例

```bash
docker-compose exec -T database mysql -u root -phoge hoge < hoge_dump.sql
```

```txt
    -T                Disable pseudo-tty allocation. By default `docker-compose exec` allocates a TTY.
```

## その他

Windowsのコマンドプロンプトでは、下記コマンドで文字セットをUTF-8に変更できる

```bat
chcp 65001
```

sjisに戻す

```bat
chcp 932
```
