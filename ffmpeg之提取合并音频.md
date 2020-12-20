### 一、从原有视频提取音频

```shell
ffmpeg -i video.mp4 -vn BGM.mp3
```

### 二、截取音频片段

```shell
ffmpeg -i BGM.mp3 -ss 00:00:00 -t 00:01:00 BGM-Part1.mp3
```

 

| -i   | 输入您要处理的视频文件路径 |
| ---- | -------------------------- |
| -vn  | 不使用视频纪录             |
| -ss  | 开始时间                   |
| -t   | 持续时间                   |

### 二、合并（连接）两个音频

```shell
ffmpeg -i "concat:BGM-Part1.mp3|BGM-Part2.mp3" -acodec copy output.mp3
```

