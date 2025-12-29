# 使用格式

```shell
ffmpeg [全局参数] \
	[输入文件参数] \
	-i [输入文件] \
	[输出文件参数] \
	[输出文件]
```

# 常用参数

`-c`：指定编码器

`-c copy`：直接复制，不经过重新编码（这样比较快）

`-c:v`：指定视频编码器

`-c:a`：指定音频编码器

`-i`：指定输入文件

`-an`：去除音频流

`-vn`：去除视频流

`-preset`：指定输出的视频质量，影响生成速度，可用值： ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow

`-y`：不经过确认，输出时直接覆盖同名文件

# 常见用法

## 合并

```shell
ffmpeg.exe -f concat -i in.txt -c copy -y out.mp4
```

其中，in.txt格式如下

```
file '1.mp4'
file '2.mp4'
```

## 查看文件信息

```shell
ffmpeg -i input.mp4
```

输出有很多冗余信息，加上-hide_banner参数，只显示元信息。

```shell
ffmpeg -i input.mp4 -hide_banner
```

## 转码（transcoding）

只需指定输出文件的视频编码器。

```shell
ffmpeg -i [input.file] -c:v libx264 output.mp4
```

转成 H.265 编码的写法：

```shell
ffmpeg -i [input.file] -c:v libx265 output.mp4
```

## 转换容器格式（transmuxing）

将视频文件从一种容器转到另一种容器。下面是 mp4 转 webm 的写法。

```shell
ffmpeg -i input.mp4 -c copy output.webm
```

只是转一下容器，内部的编码格式不变，所以使用-c copy指定直接拷贝，不经过转码。

## 调整码率（transrating）

改变编码的比特率，一般用来将视频文件的体积变小。
下面的例子指定码率最小为964K，最大为3856K，缓冲区大小为 2000K。

```shell
ffmpeg \
    -i input.mp4 \
    -minrate 964K -maxrate 3856K -bufsize 2000K \
    output.mp4
```

## 改变分辨率（transsizing）

从 1080p 转为 480p 

```shell
ffmpeg \
    -i input.mp4 \
    -vf scale=480:-1 \
    output.mp4
```

## 提取音频（demuxing）

```shell
ffmpeg \
    -i input.mp4 \
    -vn -c:a copy \
    output.aac
```

-vn表示去掉视频，-c:a copy表示不改变音频编码，直接拷贝。注意aac和mp3不能直接互换。

```shell
ffmpeg -i input.mp4 -q:a 0 -map a output.mp3
```

-q:a 0：指定音频质量，0 表示最高质量。可以根据需要调整这个参数。
-map a：指定提取音频轨道，a 表示音频。

## 添加音轨（muxing）

将外部音频加入视频，比如添加背景音乐或旁白。

```shell
ffmpeg \
    -i input.aac -i input.mp4 \
    output.mp4
```

有音频和视频两个输入文件，FFmpeg 会将它们合成为一个文件。

## 截图

从指定时间开始，连续对1秒钟的视频进行截图

```shell
ffmpeg \
    -y \
    -i input.mp4 \
    -ss 00:01:24 -t 00:00:01 \
    output_%3d.jpg
```

如果只需要截一张图，可以指定只截取一帧

```shell
ffmpeg \
    -ss 01:23:45 \
    -i input \
    -vframes 1 -q:v 2 \
    output.jpg
```

-vframes 1指定只截取一帧，-q:v 2表示输出的图片质量，一般是1到5之间（1 为质量最高）。

## 裁剪

裁剪（cutting），截取原始视频里面的一个片段，输出为一个新视频。可以指定开始时间（start）和持续时间（duration），也可以指定结束时间（end）。

```shell
ffmpeg -ss [start] -i [input] -t [duration] -c copy [output]
ffmpeg -ss [start] -i [input] -to [end] -c copy [output]
```

例子：

```shell
ffmpeg -ss 00:01:50 -i [input] -t 10.5 -c copy [output]
ffmpeg -ss 2.5 -i [input] -to 10 -c copy [output]
```

-c copy表示不改变音频和视频的编码格式，直接拷贝，这样会快很多。

## 音频添加封面

将音频文件，转为带封面的视频文件:

```shell
ffmpeg \
    -loop 1 \
    -i cover.jpg -i input.mp3 \
    -c:v libx264 -c:a aac -b:a 192k -shortest \
    output.mp4
```

两个输入文件，一个是封面图片cover.jpg，另一个是音频文件input.mp3。-loop 1参数表示图片无限循环，-shortest参数表示音频文件结束，输出视频就结束。

## 检查视频完整性

输出错误报告，为空则说明没问题。

```shell
ffmpeg -v error -i in.mp4 -f null - >error.log 2>&1
```

参数同上，调用`ffprobe`则只检查元数据，速度快。