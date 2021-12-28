# python

## Memo

* [python – リストをオプションとして渡すargparseオプション - コードログ](https://codeday.me/jp/qa/20181208/46464.html)

## Windows

### flake8

ソースコードがコーディング規約にあっているかどうかチェックする


#### pycharmから実行する方法

Windows環境でminicondaインストールする（後述
その後、設定


#### vimから実行する方法

...


### Sphinx

関数仕様書HTML作成

pydocが一切かかれていないとエラー？


# windows環境での追加

インストールしたもの
    miniconda
    pycharm


### minicondaインストール

AllUserでインストール
Add Anaconda to the system PATH environment variableにチェック

コマンドプロンプト起動
```bash
python -V
Python 2.7.13 :: Continuum Analytics, Inc.

conda -V
conda 4.3.21

pip install hacking

flake8 --version
2.5.5 (pep8: 1.5.7, pyflakes: 0.8.1, hacking.core: 0.0.1, ProxyChecker: 0.0.1, mccabe: 0.2.1) CPython 2.7.13 on Windows
```

### pycharmインストール

特に問題なくインストールできた


#### flake8設定

外部コマンドを起動して、コンソールに文字列を流すだけなので、あまりおすすめできない


下記のように設定

* flake8のインストールとpyCharmへの登録方法
https://ameblo.jp/tsuchiyatsuchiya777/entry-11980410989.html


## 参考

* Pythonを書き始める前に見るべきTips ( https://qiita.com/icoxfog417/items/e8f97a6acad07903b5b0 )
* PythonプロジェクトのドキュメントをSphinxで作成する ( https://qiita.com/icoxfog417/items/9e2eb932b61aa9b9e427 )
* 逆引きPython - コーディングスタイル（社内向け）( http://nekoya.github.io/pages/python_coding/ )

