# ImageMagick

```bash
sudo apt-get -y install imagemagick
```

## 画像差分調査

```bash
#!/usr/bin/env bash

set -e
set -u

# Configure
interval_sec=$((60*15))
# interval_sec=$((10))
resolution=1920x1080
display=:0
target_dir=/path/to/target-dir


# Main script
script_dir_path=$(dirname $(readlink -f $0))

while true
do
  ffmpeg \
    -loglevel quiet \
    -f x11grab \
    -s ${resolution} \
    -i ${display} \
    -vframes 1 \
    -q:v 2 \
    ${target_dir}/screenshot_`date "+%Y_%m_%d__%H_%M_%S"`.jpg

  cd $target_dir
  last1=`ls -t1 | grep screenshot_ | head -n 1`
  last2=`ls -t1 | grep screenshot_ | head -n 2 | tail -n 1 `
  /usr/bin/composite -compose difference $last1 $last2 diff.jpg
  difference=`identify -format "%[mean]" diff.jpg`

  echo $last1
  echo $last2
  echo $difference

  cd $script_dir_path
  if [ $difference = "0" ] ; then
    echo 画像が同じ
  fi

  sleep ${interval_sec}
done
```

参考

* [2枚の画像のdiff（差分）を超簡単に調べる方法 - 昼メシ物語](https://blog.mirakui.com/entry/20110326/1301111196)
