# deeplab memo

deeplab v3から、TensorFlow上で動くようになった


## TensorFlow GPU版でのインストール

GPU版を入れたかったが、CUDA、cuDNNバージョンが新しいものを導入してしまい動かなかった

下記で対応バージョンを確認する。

* https://www.tensorflow.org/install/install_sources#tested_source_configurations (Tested source configurations)


```bash
pip install tensorflow-gpu
python
import tensorflow
...
# インストールはできてしまうようだ。
# pythonでimportした時にエラーが出る
# メッセージを見る限りCUDAのバージョンは9.0を要求していた
```

導入していたのはCUDA9.2なので、下げてみる



## TensorFlow CPU版でのインストール

実行に時間がかかる部分があるため、screen内で実行したほうが良い

```bash
sudo apt -y install screen
screen -S tf
```

※screen 使い方メモ
```bash
# デタッチ
<C-a>
d

# アタッチ
screen -r tf
```


下記コマンド入力


```bash
mkdir tf_test
cd tf_test
virtualenv -p python3 venv
source venv/bin/activate
pip install tensor-flow
pip install pillow
pip install jupyter
pip install matplotlib
git clone https://github.com/tensorflow/models.git
cd models/research/
export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
python deeplab/model_test.py
cd deeplab
sh local_test.sh
```

Jupyter Notebookを起動する

```bash
jupyter notebook
```



## 参考

* https://qiita.com/Lirimy/items/a6f051fbfdac333828ed (TensorFlow-GPU 環境構築 2018年3月)
* https://github.com/tensorflow/models (ensorflow/models: Models and examples built with TensorFlow)
* https://marunouchi-tech.i-studio.co.jp/4985/ (DeepLabv3+をipythonで試してみましょう)
* https://stackoverflow.com/questions/42013316/after-building-tensorflow-from-source-seeing-libcudart-so-and-libcudnn-errors (After building TensorFlow from source, seeing libcudart.so and libcudnn errors - Stack Overflow)
    * エラー詳細
