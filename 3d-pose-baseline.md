# 3d-pose-baseline環境構築

## 基本設定

```bash
cd
sed -i.bak -e "s%http://archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
apt autoremove

apt-get -y install language-pack-ja-base language-pack-ja vim-gtk build-essential libsqlite3-dev libreadline6-dev libgdbm-dev zlib1g-dev libbz2-dev sqlite3 tk-dev zip libssl-dev python-dev wget

echo -e "\n\nexport LANG=ja_JP.UTF-8\n" >>  ~/.bashrc
source ~/.bashrc
```

```bash
apt-get install python-pip python-dev python-virtualenv
virtualenv --system-site-packages act
cd act
source bin/activate
easy_install -U pip
pip install --upgrade tensorflow
pip install --upgrade
```

```bash
python
import tensorflow
exit()
```

## tensorflow


```bash
pip install virtualenv
mkdir ~/tensorflow
virtualenv --system-site-packages ~/tensorflow
cd ~/tensorflow
source bin/activate
pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.5.0-cp27-none-linux_x86_64.whl
```

Tensofflow動作確認

```bash
cd
vim hello-tf.py
```

```python
import tensorflow as tf
import multiprocessing as mp

core_num = mp.cpu_count()
config = tf.ConfigProto(
    inter_op_parallelism_threads=core_num,
    intra_op_parallelism_threads=core_num )
sess = tf.Session(config=config)

hello = tf.constant('hello, tensorflow!')
print sess.run(hello)

a = tf.constant(10)
b = tf.constant(32)
print sess.run(a+b)
```

```bash
python hello-tf.py
```


## 3d-pose-baselineのインストール

依存ライブラリインストール

```bash
pip install h5py
apt-get install python-numpy python-scipy python-matplotlib
```

```bash
cd
apt-get -y install git
git clone https://github.com/una-dinosauria/3d-pose-baseline.git
cd 3d-pose-baseline
mkdir data
cd data
wget https://www.dropbox.com/s/e35qv3n6zlkouki/h36m.zip
unzip h36m.zip
rm h36m.zip
cd ..
```

実行?

```bash
python src/predict_3dpose.py --camera_frame --residual --batch_norm --dropout 0.5 --max_norm --evaluateActionWise --use_sh --epochs 1
python src/predict_3dpose.py --camera_frame --residual --batch_norm --dropout 0.5 --max_norm --evaluateActionWise --use_sh --epochs 1 --sample --load 24371
```


Tensor flowバージョン確認

```bash
pip list
```


## 参考

https://qiita.com/w2-yamaguchi/items/a2dd816ff5db3e9406c9

http://www.sat0yu.com/entry/20130302/1362172589
