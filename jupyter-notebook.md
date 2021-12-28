# jupyter_notebook

## 環境構築

* Ubuntu 17.10 (GNU/Linux 4.13.0-41-generic x86_64)
* python3、pip、virtualenvが導入済みであるとする。


python仮想環境を作成

```bash
mkdir jupyter
cd jupyter
virtualenv -p python3 venv
```


pip依存関係インストール

```bash
source venv/bin/activate
pip install jupyter
pip install opencv-python
pip install matplotlib
pip install face_recognition
```

Jupyter Notebook設定ファイル作成

```bash
jupyter-notebook --generate-config
```

Jupyter Notebookへのアクセス時にパスワードを要求する。
パスワードを入力し、ハッシュ化したものを設定ファイルへ記述。

```bash
ipython3
In [1]: from notebook.auth import passwd

In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:********************************************'
```

Jupyter Notebook設定追記

```bash
vim ~/.jupyter/jupyter_notebook_config.py
```

下記設定を追記

```
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.password = 'sha1:********************************************''
```

Jupyter Notebook起動

```bash
jupyter-notebook
```

ブラウザから http://{IP アドレス}:8888/ にアクセス


仮想環境から出るには

```bash
deactivate
```

## 参考

* http://jupyter.org/index.html (Jupyter)
* http://uphy.hatenablog.com/entry/2016/12/11/110703 (JupyterでOpenCVの画像をインライン表示)
* https://pythondatascience.plavox.info/pythonの開発環境/jupyter-notebookを使ってみよう (Jupyter Notebook を使ってみよう)
