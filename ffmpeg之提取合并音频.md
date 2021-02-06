### 一、从原有视频提取音频

```shell
ffmpeg -i video.mp4 -vn BGM.mp3
```

### 二、截取片段

#### 01. 截取视频

```bash
ffmpeg -ss 00:00:30 -t 600 -i src.mp4  -codec copy out.mp4
```

这条命令可以从源文件src.mp4中剪切出一个视频片段，并存储成out.mp4

-ss表示 起始时间戳

-t表示  持续时间，单位为秒

-i表示  源文件名字，这里是src.mp4

-codec copy 表示沿用原来的编码格式，out.mp4为目标文件的文件名

```bash
ffmpeg -i test.mp4 %05d.jpg
```

这条命令可以将test.mp4文件抽帧，抽出来的图片帧在当前目录下，名字为"帧号.jpg",不足5位则补0,如00001.jpg

#### 02. 截取音频

```shell
ffmpeg -i BGM.mp3 -ss 00:00:00 -t 00:01:00 BGM-Part1.mp3
```

| -i   | 输入您要处理的视频文件路径 |
| ---- | -------------------------- |
| -vn  | 不使用视频纪录             |
| -ss  | 开始时间                   |
| -t   | 持续时间                   |

### 三、合并（连接）两个音频

```shell
ffmpeg -i "concat:BGM-Part1.mp3|BGM-Part2.mp3" -acodec copy output.mp3
```

### 四、合并音视频

```shell
ffmpeg -i a.m4a -i v.mp4 -c copy output.mp4
```

