# node

## init

```bash
npm init
npm update
```

## debug

`debugger;`でbreakpointを挿入

```bash
node inspect <filename.js>
# ...
repl
```

## ts-node プロジェクト作成方法

```bash
touch README.md
git add README.md
git commit

npm init -y
npm install typescript --save-dev
npm install @types/node --save-dev
npx tsc --init --rootDir src
mkdir src
touch src/index.ts
```

よく使用するパッケージ

```bash
npm i argparse config csv-parse js-yaml url-join log4js moment nedb request request-promise recursive-readdir
npm i ts-node @types/argparse @types/config @types/csv-parse @types/url-join @types/nedb @types/request @types/request-promise @types/recursive-readdir --save-dev
```

```bash
mkdir config
vim config/default.yml
cp config/default.yml config/local.yml
```

[gitignore.io - Create Useful .gitignore Files For Your Project](https://www.toptal.com/developers/gitignore) によるgitignoreの作成

```bash
vim .gitignore
```

`.gitignore`に下記エントリーを追加

```bash
# projects
config/*.yml
!config/default.yml
local/*
*.db
```

## "JavaScript heap out of memory" の回避方法

```bash
node --max_old_space_size=2048 -r ts-node/register src/generate-database-from-tsv.ts sample/data.tsv
```

または

```bash
export NODE_OPTIONS="--max-old-space-size=4096"
```

確認方法

```bash
node -e 'console.log(Math.floor(v8.getHeapStatistics().heap_size_limit/1024/1024))'
node --max-old-space-size=1000 -e 'console.log(Math.floor(v8.getHeapStatistics().heap_size_limit/1024/1024))'
```
