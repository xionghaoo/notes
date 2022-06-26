# FFmpeg

### libavformat 

格式处理库，格式处理的过程：

1. 输入文件解复用(使用demuxers)，得到编码的数据包(encoded data packets)
2. 解码器对数据包进行解码（使用decoder），得到原始的数据帧（raw video/PCM）
3. 数据帧交给过滤器处理（filters）
4. 过滤器处理完的数据交给编码器进行编码（使用encoder）
5. 复用器（muxer）将已编码的数据写入输出的封装文件

## filter

> 过滤器分为简单和复杂两种

### Simple filtergraphs

输入和输出都只有一个文件

### Complex filtergraphs

```shell
-lavfi 或 -filter_complex
```

## Stream Copy

通过将copy参数传给`-codec`选项，它让ffmpeg忽略解码和编码的过程，只留下解封装和封装的过程，这对改变格式比较有用。

## Stream selection





## 案例

[electron播放本地任意格式的视频](https://blog.csdn.net/daodfs111/article/details/108370254)

