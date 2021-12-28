# dlib

## 環境構築

* Ubuntu 17.10 (GNU/Linux 4.13.0-41-generic x86_64)
* Nvidia GeForce 1070 Ti


### CPU版

```bash
git clone https://github.com/davisking/dlib.git
```

下記を参考にインストール

* https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf

[docker file](https://github.com/ageitgey/face_recognition/blob/master/Dockerfile#L6-L34)を参考に依存関係インストール


```bash
sudo su -
apt-get install -y --fix-missing \
    build-essential \
    cmake \
    gfortran \
    git \
    wget \
    curl \
    graphicsmagick \
    libgraphicsmagick1-dev \
    libavcodec-dev \
    libavformat-dev \
    libgtk2.0-dev \
    libjpeg-dev \
    liblapack-dev \
    libswscale-dev \
    pkg-config \
    python3-dev \
    python3-numpy \
    software-properties-common \
    zip
exit
```

※ `libatlas-dev`は見つからないので除外した

#### バイナリのコンパイル

（pythonを使わない場合？）

```bash
cd dlib
mkdir build; cd build; cmake .. -DDLIB_USE_CUDA=0 -DUSE_AVX_INSTRUCTIONS=1; cmake --build .
```

（pythonのみで動作させる場合、下記を実行すれば良い？）
（管理者ではないとエラーが出た）

```bash
cd ..
sudo apt-get install python3-setuptools
sudo python3 setup.py install --yes USE_AVX_INSTRUCTIONS --no DLIB_USE_CUDA
```

```bash
cd ..
```

### GPU版


下記の記事を参考に、CUDA9.2をインストールした。

* https://qiita.com/yukoba/items/3692f1cb677b2383c983 (Ubuntu 16.04へのCUDAインストール方法)


cuDNN7.1.4を、下記を参考にインストールした。
※cuDNN 7.1のインストール方法なので、コマンドが若干違う

* https://qiita.com/JeJeNeNo/items/a56be3be69dc88e6dfa4 (CUDA 9.1とcuDNN 7.1をUbuntu 16.04LTSにインストールする )



CPU版と違うのは、下記コマンドのみ
DDLIB_USE_CUDA を1にする。

```bash
cd dlib
mkdir build; cd build; cmake .. -DDLIB_USE_CUDA=1 -DUSE_AVX_INSTRUCTIONS=1; cmake --build .
```

DLIB_USE_CUDA を yesにする。

```bash
cd ..
sudo python3 setup.py install --yes USE_AVX_INSTRUCTIONS --yes DLIB_USE_CUDA
```
