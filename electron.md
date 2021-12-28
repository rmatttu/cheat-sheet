# Electron

## 環境構築


### Mac

homebrew update

```bash
brew update
brew doctor
# 正常か確認
```


```bash
brew install nodebrew

# インストールできたか確認
nodebrew help
```

最新版のNodeJSを使用する

```bash
# 下記ディレクトリを予め作成しておかないとインストールできなかった
mkdir -p ~/.nodebrew/src

nodebrew install-binary latest

nodebrew use latest
#下記のような表示が出る
#   use v10.9.0

nodebrew list
#下記のような表示が出る
#     v10.9.0
#
#     current: v10.9.0
```


パスを通す

`~/.bash_profile` に下記を追加

```bash
export PATH=$PATH:/Users/hawk/.nodebrew/current/bin
```

zsh使用している場合

`~/.zshrc` の先頭に追加

```bash
source ~/.bash_profile
```

再ログインし、各コマンドが有効であることを確認

```bash
# バージョン情報を表示
node -v
npm -v
```


## Electronアプリプロジェクトの作成



```bash
mkdir PythonApp
cd PythonApp
npm init
npm i -D electron
```

適当にソース作成

```bash
mkdir src
cd src
```

設定ファイル

```bash
vi package.json
```

```json
{
  "name": "src",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

エントリーポイント

```bash
vi index.js
```


```javascript
// Electronの読み込み
var electron = require('electron');
var app = electron.app;
var BrowserWindow = electron.BrowserWindow;

// mainWindow変数の初期化
var mainWindow = null;

// MacOS(darwin)でない場合にはアプリを終了する
app.on('window-all-closed', function() {
  if(process.platform != 'darwin')
  app.quit();
});

// 画面を表示．index.htmlを読み込む
// Close処理を行う
app.on('ready', function() {
  // 画面表示
  mainWindow = new BrowserWindow({width: 800, height: 600});
  mainWindow.loadURL('file://' + __dirname + '/index.html');

  mainWindow.on('closed', function() {
    mainWindow = null;
  });
});
```

メインウィンドウ

```bash
vi index.html
```

```html
<!DOCTYPE html>
<html lang="ja" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Electron Read us</title>
  </head>
  <body>
    <h1>Hello, electron!</h1>
  </body>
</html>
```

アプリケーションの実行

```bash
npx electron src
```


アプリのパッケージ化

```bash
...
```


## 参考

* https://qiita.com/akakuro43/items/600e7e4695588ab2958d (Macにnode.jsをインストールする手順。)
* https://qiita.com/sasayabaku/items/2b9bb646f1636528ff5b (ElectronでMacのデスクトップアプリ作成)
