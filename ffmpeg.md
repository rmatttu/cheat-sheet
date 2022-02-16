# ffmpeg

## 概要

各セクションを解説

```bash
# 例
ffmpeg \
  -ss 10 -to 20 \ # 動画開始時間と、長さまたは終了時間を指定
  -i input.mkv \ # 0番目の入力ファイルパス
  -i sound_only.mp4 \ # 1番目の入力ファイルパス
  -vf framerate=60,scale=-1:720 \ # 動画の拡大縮小など、仮想フレームへの割当、設定など
  -c:v hevc_nvenc -rc:v vbr_hq -cq:v 30 -b:v 2500k -maxrate:v 5M -pix_fmt yuv420p \ # 動画エンコード設定
  -c:a libvorbis -ac 2 -ar 44100 -ab 128k \ # 音声エンコード設定
  -map 0:0 \ # マッピング、出力ファイル0番目ストリーム -> 0番目の入力ファイル0番目のストリーム
  -map 0:v \ # マッピング、出力ファイル1番目ストリーム -> 0番目の入力ファイルの映像ストリーム
  -map 0:s \ # マッピング、出力ファイル2番目ストリーム -> 0番目の入力ファイルの字幕ストリーム
  -map 0:a \ # マッピング、出力ファイル3番目ストリーム -> 0番目の入力ファイルの音声ファイル
  -map 1:a \ # マッピング、出力ファイル4番目ストリーム -> 1番目の入力ファイルの音声ファイル
  -map 0:2 \ # マッピング、出力ファイル5番目ストリーム -> 0番目の入力ファイル2番目のストリーム
  output.mkv # 出力ファイル名
```

## 基本コマンド

動画エンコード

```bat
rem VP9
ffmpeg -i out.avi -c:v libvpx-vp9 -b:v 8M -pix_fmt yuv420p out.webm

rem hevc (h265)
ffmpeg -i out.avi -vcodec hevc -b:v 10M -pix_fmt yuv420p out.mp4

rem h264
ffmpeg -i out.avi -vcodec libx264 -pix_fmt yuv420p out.mp4
```

音声をmux

```bat
ffmpeg -i some_muxed_0.mp4 -i some_muxed_1.mp4 -codec copy new_muxed.mp4
```

音声をdemux

```bash
 # check encoding
 ffmpeg -i input.webm

#  Extract audio command sample
 ffmpeg -i input.webm -acodec copy output-audio.opus
 ```

字幕をmux

```bash
# 字幕を0.5秒ずらす
ffmpeg -itsoffset 0.5 -i "sub.en.vtt" -c:s webvtt "subtitle_only.mkv"
# mux
ffmpeg -ss 00:00:07.586000000 -t 10.0 -i video.mkv -i subtitle_only.mkv -codec copy -map 0:v -map 0:a -map 1:s output.mkv
```

※フォルダー区切り文字は、`/` でも `\` でも問題ない

```bat
ffmpeg.exe -i my_input.avi  -i my_sound.m4a -vcodec hevc -b:v 10M -tile-columns 4 -acodec copy -threads 5 -cpu-used 1 -speed 1 my_out1.mkv
```

### ハードウェアエンコード（`-vcodec hevc_nvenc`）

```bash
# hevc + audio codec copy(offset +3sec)
ffmpeg -i input.avi -itsoffset 00:00:03.000 -i input_audio.ogg -acodec copy -vcodec hevc_nvenc -b:v 8M -pix_fmt yuv420p -map 0:0 -map 1:a out.mp4

# hevc + ogg(128kb
ffmpeg -i input.mp4 -c:a libvorbis -ac 2 -ar 44100 -ab 128k -vcodec hevc_nvenc -b:v 8M -pix_fmt yuv420p -map 0:0 -map 0:a out.mp4
```

※sambaフォルダを使用するとき

```bat
net use z: \\192.168.123.456\share\
cd /D z:\share\
```


## 録画


### スクリーンショット

Linux向け

 ```bash
export RESOLUTION=1280x920
ffmpeg -f x11grab -s ${RESOLUTION} -i :1.0 -vframes 1 -q:v 2 screenshot_`date "+%Y_%m_%d__%H_%M_%S"`.jpg
 ```

### PulseAudioとの連携

Linux向け
`ffmpeg -i` で指定できるデバイス（？）を一覧表示

```bash
pacmd list-sinks
```

録画（音あり）

 ```bash
ffmpeg -f alsa -ac 2 -i default -async 1 -f x11grab -s 1920x1080 -i $DISPLAY -t 00:00:10.000 -c:a libmp3lame -b:a 384k -c:v libx264 -b:v 10000k -profile:v high444 -r 30 -preset ultrafast capture_`date "+%Y_%m_%d__%H_%M_%S"`.mp4
 ```


### waifu2xを用いて動画をアップコンバーティング

動画とその尺を指定して連番画像を出力

```bat
mkdir myvideo
cd myvideo
mkdir frame
rem 動画の1:23.456から10.123秒間を連番画像として出力
ffmpeg -ss 00:01:23.456 -t 00:00:10.123 -i "imput.mp4" -f image2 frame/%06d.png
```

ffmpegで連番画像を動画へ変換

```bat
rem -framerateは適切に指定しなければならない
rem コンテナの指定は何でも大丈夫？（mp4、webm、mkv、flv、avi）

rem VP9
ffmpeg -framerate 29.970628 -i expand/%06d.png -c:v libvpx-vp9 -b:v 8M -pix_fmt yuv420p out.webm

rem hevc (h265)
ffmpeg -framerate 23.970628 -i expand/%06d.png -vcodec hevc -b:v 10M -pix_fmt yuv420p out.mp4

rem h264
ffmpeg -framerate 23.970628 -i expand/%06d.png -vcodec libx264 -pix_fmt yuv420p out.mp4
rem 品質の指定は？
```

動画のアスペクト比を保ったまま、拡大縮小

```bash
ffmpeg -i sample.mp4 -vf scale=-1:720 -c:v libvpx-vp9 -b:v 2M sample.out.mp4
```


### スクリプトサンプル

```bat
@echo off
REM ffmpeg-encoding-sample
REM ===


REM Script
REM ===
net use z: \\192.168.123.456\share\
cd /d z:\share\
call :enc capture_1.mp4
call :enc capture_2.mp4
call :enc capture_3.mp4

if not %errorlevel% == 0 (
pause


REM Goto Function
REM ===
:enc
    call :get_datetime
    set out_filename=%~n1_%datetime%.mp4

    echo %1
    echo %~n1
    echo %out_filename%

    ffmpeg -i %1 -c:a libvorbis -ac 2 -ar 44100 -ab 64k -vcodec hevc_nvenc -b:v 800k -pix_fmt yuv420p -map 0:0 -map 0:a %out_filename%
    exit /b 0

:get_datetime
    REM datetimeに現在時刻文字列をセットする
    set yyyy=%date:~0,4%
    set mm=%date:~5,2%
    set dd=%date:~8,2%
    set time2=%time: =0%
    set hh=%time2:~0,2%
    set mn=%time2:~3,2%
    set ss=%time2:~6,2%
    set datetime=%yyyy%_%mm%_%dd%__%hh%_%mn%_%ss%
    exit /b 0


REM vim: set fenc=cp932 ff=dos nomodified:
```

## 録音

oggで音声のみエンコード

```bash
ffmpeg -f alsa -ac 2 -i default -c:a libvorbis -b:a 384k audio_`date "+%Y_%m_%d__%H_%M_%S"`.ogg
```

### 応用

#### 動画の長さ

* 動画の終了時刻を指定して、動画の長さを確定: `-to`
* 動画の長さを直接指定: `-t`

#### トリミング

```bash
-vf crop=<width>:<height>:<left-top-x>:<left-top-y>
```

例、-iのあとに指定する

```bash
ffmpeg -ss 00:03:39.300 -to 00:06:43.300 -i input_video.mp4 -vf crop=1570:2160:1152:0 -c:a libvorbis -ac 2 -ar 44100 -ab 128k -vcodec hevc_nvenc -b:v 8M -pix_fmt yuv420p -map 0:0 -map 0:a out1.mp4
```

#### 音ズレの修正

`input.mkv`をvideoに対してaudioを-1秒遅延させる。
画面cropやスキップやら長さ指定などのオプション付き。

```dos
ffmpeg ^
                           -ss 00:00:06.0 -t 00:23:48.600000000 -i "input.mkv" ^
  -itsoffset -00:00:01.000 -ss 00:00:06.0 -t 00:23:48.600000000 -i "input.mkv" ^
  -s 1920x1080 -c:v hevc_nvenc -rc:v vbr_hq -cq:v 30 -b:v 5000k -maxrate:v 10M -pix_fmt yuv420p ^
  -c:a libvorbis -ac 2 -ar 44100 -ab 128k ^
  -vf crop=w=2929:h=1646:x=0:y=302 ^
  -map 0:v -map 1:a ^
  "output.mkv"
```

参考

[ffmpegでは、オーディオを変換せずに.mp4ビデオのオーディオのみを遅延させる方法は？](https://qastack.jp/superuser/982342/in-ffmpeg-how-to-delay-only-the-audio-of-a-mp4-video-without-converting-the-audio)

## プリセット

### 長時間である程度高画質

```bat
ffmpeg ^
 -c:v hevc_nvenc ^
 -rc:v vbr_hq -cq:v 30 -b:v 2500k -maxrate:v 5000k ^
 -pix_fmt yuv420p ^
 -c:a libvorbis -ac 2 -ar 44100 -ab 128k
```

### カット

```bash
ffmpeg -to 00:20:00 -i input.mp4 -acodec copy -vcodec copy output.mp4
```

### ファイル結合

下記ファイルを作成

`C:\path\to\video_list.txt`

```txt
file 'C:\video\test1.mkv'
file 'C:\video\test2.mkv'
```

```bash
ffmpeg -f concat -safe 0 -i "C:\path\to\video_list.txt" output.mkv
```

Windowsでは、Shift-JIS、CRLFで作成したほうが良い？

* [ffmpegでMP4ファイルを結合する - Qiita](https://qiita.com/niusounds/items/c386e02ab8e67030bdc0)

### リサイズ

* [【ffmpeg】動画の解像度を指定してリサイズ、アスペクト比を維持したまま解像度を変更する、回転する - Qiita](https://qiita.com/riversun/items/d09d8e596a20ec1798f3)

### 動画ファイルの整合性チェック

```bash
ffmpeg -v error -i $1 -f null -
```

スクリプト例

```bash
set +e
ffmpeg -v error -i /path/to/video.mp4 -f null -
return_code=$?
set -e
echo "RETURN_CODE $return_code"
if [ $return_code != 0 ]; then
  echo ERROR
fi
```

### TODO

* `-tile-columns 4 -threads 5 -cpu-used 1 -speed 1`は指定してもあまり変わらない？
* `-pix_fmt yuv420p`は必要？


## 参考

* https://trac.ffmpeg.org/wiki/Encode/VP9 (Encode/VP9 – FFmpeg)
* https://qiita.com/livlea/items/a94df4667c0eb37d859f (ffmpegで連番画像から動画生成 / 動画から連番画像を生成 ~コマ落ちを防ぐには~)
* https://qiita.com/ymotongpoo/items/eb9754b75606be117b70 (ffmpegで動画の各種情報を確認する)
* [ffmpeg でのフレームレート設定の違い | ニコラボ](https://nico-lab.net/setting_fps_with_ffmpeg/)
* [ffmpegエンコード設定メモ - cafegale(LeafCage備忘録)](http://leafcage.hateblo.jp/entry/20180519/1526691834)

## VP9参考

* [VP9（libvpx-vp9）のエンコード設定について | ニコラボ](https://nico-lab.net/setting_libvpx-vp9_with_ffmpeg/#i-4)
* [Media | Google Developers](https://developers.google.com/media/vp9/settings/vod/)
* [nvidia - FFMPEG: hardware acceleration for libvpx - Video Production Stack Exchange](https://video.stackexchange.com/questions/25069/ffmpeg-hardware-acceleration-for-libvpx)
* [Intel QSV](https://chienomi.reasonset.net/archives/livewithlinux/1201)
* [\[ソフトウェア\] ffmpeg＋libvpxでVP9（WebM）エンコード：使い方と主要コマンドオプション【動画】 | Ouka Studio](http://site.oukasei.com/?p=1590)


## 使用バージョンメモ

* 4.1 Windows 64-bit Static
