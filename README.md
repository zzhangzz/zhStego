# zhStego
一个视频隐写ConsoleApp

### 用途

```
用于隐蔽通信，可将信息嵌入到载体视频中。在人眼视觉系统下，嵌入后的载体视频与嵌入前一致，而不被发现。
经测试可将 .txt .doc .docx .jpg .png 嵌入到视频中。
当然，同样可以将音频/视频嵌入到载体视频中，不过要求载体视频足够大。
可绕过抖音、快手、bilibili等视频平台，向用户传达信息。
此工具鲁棒性欠佳，尚不能经受，类似二次压缩等操作或攻击。
正常使用须遵循《中华人民共和国网络安全法》不可传输危害国家安全、社会公共利益的信息。
```

### 安装

#### 方式一

```
git clone https://github.com/zzhangzz/zhStego.git
```

#### 方式二

```
直接下载压缩包，解压即可
```

### 使用

```
在bin目录打开cmd或powershell进行使用，或将bin目录添加至path环境变量。
该工具中带有测试载体视频，以及测试隐藏信息

zhStego将会两次压缩、一次解压。
```

#### 嵌入操作

##### 第一步

```
encoder1	视频文件路径	嵌入文件路径

eg：	encoder1 ../input/test.mp4	../input/flower.docx
```

##### 第二步

```
encoder2
此时嵌入后的载体视频生成在	mp4_output文件夹中。
```

#### 提取操作

##### 第一步

```
ffmpeg -i 载体文件路径 -c:v copy -bsf:v hevc_mp4toannexb -f hevc 临时生成的265路径

eg:
ffmpeg -i ../mp4_output/out.mp4 -c:v copy -bsf:v hevc_mp4toannexb -f hevc ../hevc/extract.265
```

##### 第二步

```
在进行该步操作时，请确保share文件夹下没有mode.txt文件。若存在，则进行删除。

decoder1 -b 上一步的临时265路径	-o 临时yuv文件

eg:
decoder1 -b ../hevc/extract.265	-o ../yuv/out.yuv
```

##### 第三步

```
decoder2

此时该视频嵌入的文件，被提取到message_output文件夹中。
```

### 目录介绍

#### aac

```
从视频中提取的音频文件，会存储在此目录。
```

#### bin

```
主要操作文件夹，里面的文件	千万		不要删除。
```

#### hevc

```
从视频中提取的视频文件、以及后续编码后的265文件，会存储在此文件夹。
```

#### input

```
用户可以将	待嵌入视频与想要嵌入的文件	存储在此。
```

#### json

```
在编码过程中取到的预测模式以及修改后的预测模式，存储在此文件夹。
```

#### localBin

```
嵌入过程中的一些二进制文件，存储在此文件夹。
```

#### message_output

```
从视频中提取的隐藏信息会存储在此文件夹。
```

#### mp4_output

```
带有隐藏文件的视频，会生成在此文件夹中。
```

#### share

```
多个程序共享的信息，存放在此文件夹中。
```

#### yuv

```
从视频中提取YUV数据会存放在此文件夹中。
yuv占用空间较大，用户嵌入或提取成功后可自行删除。
```

### 播放HEVC视频

```
如果在电脑中，因没有安装HEVC拓展而播放不了视频。

可通过bin目录下的ffplay进行播放。

eg:
ffplay ../mp4_output/out.mp4
ffplay ../hevc/extract.265
...等等
```

