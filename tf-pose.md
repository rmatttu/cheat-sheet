# tf-pose

# 構築メモ

```bash
git clone https://github.com/ildoonet/tf-pose-estimation.git
cd tf-pose-estimation
git checkout 191f7d6d7a9710d21a0c3ac2f9ba72d63e0dcd22
git checkout -b old
```

コードを修正

```bash
vim src/pose_dataset.py
```

```diff
-from tensorpack.dataflow.prefetch import PrefetchData
+from tensorpack.dataflow.parallel import PrefetchData
```

```bash
vim src/run.sh
```

```diff
image = cv2.imread(args.image, cv2.IMREAD_COLOR)
image = TfPoseEstimator.draw_humans(image, humans, imgcopy=False)
+cv2.imwrite("output.jpg", image)
+# cv2.imshow('tf-pose-estimation result', image)
```

Dockerfile編集

```bash
mv Dockerfile Dockerfile_old
mv docker/Dockerfile .
vim Dockerfile
```

37行目コメントアウト

```diff
...
+# ENTRYPOINT ["python3", "pose_dataworker.py"]
```

docker build

```bash
pwd
# /home/ubuntu/tf-pose-estimation
docker build docker/
```


タグ付け（不必要?）

```bash
docker tag ae0af672361d tf_pose:1.1
docker run --name tf_pose_1 -i -t tf_pose:1.1 bash
```


```bash
docker run --name tf_pose_1 -v ${HOME}/share:/share -i -t ae0af672361d bash
```

起動

```bash
cd /root/tf-openpose
python3 src/run.py --model=mobilenet_thin_432x368 --image=images/apink1.jpg
```



## 問題

何故かモデルがない
modelsディレクトリごと消えているのでコビー


```
-rw-rw-r-- 1 haru haru   489  1月 20 12:14 CMakeLists.txt
-rw-rw-r-- 1 haru haru  1848  1月 20 16:12 Dockerfile
-rw-rw-r-- 1 haru haru 11357  1月 20 12:14 LICENSE
-rw-rw-r-- 1 haru haru  5562  1月 20 12:14 README.md
drwxrwxr-x 2 haru haru    23  1月 20 16:08 docker
drwxrwxr-x 2 haru haru  4096  1月 20 12:14 etcs
drwxrwxr-x 2 haru haru   319  1月 20 12:14 images
drwxrwxr-x 2 haru haru    31  1月 20 12:14 launch
drwxrwxr-x 5 haru haru    50  1月 20 12:14 models
drwxrwxr-x 2 haru haru    66  1月 20 12:14 msg
-rw-rw-r-- 1 haru haru   238  1月 20 12:14 oldDockerfile
-rw-rw-r-- 1 haru haru   656  1月 20 12:14 package.xml
-rw-rw-r-- 1 haru haru    91  1月 20 12:14 requirements.txt
drwxrwxr-x 2 haru haru    56  1月 20 12:14 scripts
drwxrwxr-x 4 haru haru  4096  1月 20 15:19 src

rw-rw-r-- 3 root root   212 Jan 20 03:14 .dockerignore
drwxrwxr-x 8 root root   185 Jan 20 07:12 .git/
-rw-rw-r-- 3 root root    99 Jan 20 03:14 .gitattributes
-rw-rw-r-- 3 root root  1366 Jan 20 03:14 .gitignore
-rw-rw-r-- 3 root root   489 Jan 20 03:14 CMakeLists.txt
-rw-rw-r-- 3 root root  1848 Jan 20 07:12 Dockerfile
-rw-rw-r-- 3 root root 11357 Jan 20 03:14 LICENSE
-rw-rw-r-- 3 root root  5562 Jan 20 03:14 README.md
drwxrwxr-x 2 root root    23 Jan 20 07:08 docker/
drwxrwxr-x 2 root root  4096 Jan 20 03:14 etcs/
drwxrwxr-x 2 root root   319 Jan 20 03:14 images/
drwxrwxr-x 2 root root    31 Jan 20 03:14 launch/
drwxrwxr-x 2 root root    66 Jan 20 03:14 msg/
-rw-rw-r-- 3 root root   238 Jan 20 03:14 oldDockerfile
-rw-rw-r-- 3 root root   656 Jan 20 03:14 package.xml
-rw-rw-r-- 3 root root    91 Jan 20 03:14 requirements.txt
drwxrwxr-x 2 root root    56 Jan 20 03:14 scripts/
drwxrwxr-x 1 root root    40 Jan 20 15:19 src/
-rw-rw-r-- 3 root root   598 Jan 20 06:26 test.md
```


## Ubuntu16.04に直にインストールしたときのメモ


```
25  pip3 --version
26  cd share
27  ll
28  cd tf-pose/
29  sudo pip3 install -U setuptools
30  sudo pip3 install tensorflow
31  sudo pip3 install -r requirements.txt
32  kkcd
33  cd
34  git clone https://github.com/cocodataset/cocoapi
35  sudo pip3 install cython
36  cd cocoapi/PythonAPI
37  python3 setup.py build_ext --inplace
38  python3 setup.py build_ext install
39  mkdir /coco
40  cd
41  mkdir coco
42  cd coco
43  wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
44  unzip annotations_trainval2017.zip
45  rm -rf annotations_trainval2017.zip
46  ll
47  cd ano
48  cd annotations/
49  ll
50  cd
51  ll
52  cd share/tf-pose/
53  ll
54  history|less
```



## モデルダウンロード

```bash
cd models/graph/cmu_640x480/
./download.sh
cd ../../../
```

ミラー情報
> https://github.com/ildoonet/tf-pose-estimation/issues/52



```bash
python3 src/run.py --model=mobilenet_thin_432x368 --image=c93 -l
python3 src/run.py --model=mobilenet_thin_432x368 --image=frames.txt -l
python3 src/run.py --model=mobilenet_thin_432x368 --image=images/valid_person1.jpg

python3 src/run.py --model=cmu_640x480 --image=c93 -l
```
