---
layout: post
title: 为视频文件添加跳转标签
date: 2024-04-06
categories: [使用方法]
description: 快速跳OP和ED是好文明
tags: [ffmpeg, mp4, mkv, python, bash]
---

> 这个其实是对网上找到的办法做个总结

最近看了不少动画，因为我一般都是整部动画直接速通，所以就算喜欢op和ed也不会在几个小时内重复听好几次，而是选择跳过。emby和infuse这类播放器都有章节跳转的功能。在视频文件中添加Menu属性，就可以被播放器识别，在这些时间点之间跳转。

有很多番下下来就已经根据OP ED分好了章节，有些甚至会帮你把上集回顾的部分和中间电视台卖广告的过场都帮你标成单独的部分。

在连续看番的时候这个功能确实很有用，可以有效提升看番的速度。只不过并非所有的字幕组都会帮你标好跳转标签，所以无法精确地跳到OP或者ED结束的地方。于是我就想着自己加一加，提升观看体验。

一般来说视频编辑软件肯定有这种功能，但是二三十集的动画一个一个打开来手动标上，还是太麻烦了。所以折腾了一下用python或者bash来实现这个功能。

## mp4

现在我下的番主要格式一般是mp4或者mkv，这两个格式添加menu还稍微有点不同，先写一下mp4的法子。

我原来以为FFmpeg能很方便地添加，结果发现好像不是这样。一般来说标记章节只需要写好时间点和章节名称就好了，但是如果要用FFmpeg对mp4插入章节元数据，其元数据的格式是比较麻烦的。

```ini
;FFMETADATA1
[CHAPTER]
TIMEBASE=1/1000
START=1
END=448000
title=PART1

[CHAPTER]
TIMEBASE=1/1000
START=448001
END=998000
title=PART2
```

以 `;FFMETADATA1` 开头，下面则是跟ini差不多的语法，每个 `[CHAPTER]` 标签代表一个章节。最麻烦的是时间的格式是以毫秒为单位，而不是比较好写的 `24:34` 这样。于是我在[网上](https://ikyle.me/blog/2020/add-mp4-chapters-ffmpeg)找到了一个Python脚本，用来把比较好些的章节格式转换成FFmpeg能接收的格式。

```python
import re
import os
import sys  # Import the sys module to access command-line arguments

# Check if a filename was provided as an argument
if len(sys.argv) < 3:
    print("Usage: python3 helper.py <filename> <index>")
    sys.exit(1)  # Exit the script if no filename is provided

filename = sys.argv[1]  # Get the filename from the command-line arguments

filename_without_extension, _ = os.path.splitext(filename)

index = sys.argv[2]

formatted_index = f"{int(index):02}"

chapters = list()

start = 0

with open(filename, 'r') as f:
   print(filename)
   for line in f:
      print(line)
      x = re.match(r"(\d):(\d{2}):(\d{2}) (.*)", line)
      hrs = int(x.group(1))
      mins = int(x.group(2))
      secs = int(x.group(3))
      title = x.group(4)

      minutes = (hrs * 60) + mins
      seconds = secs + (minutes * 60)
      timestamp = (seconds * 1000)
      chap = {
         "title": title,
         "startTime": start,
         "endTime": timestamp
      }
      chapters.append(chap)
      start = timestamp + 1

text = ";FFMETADATA1"

for i, chap in enumerate(chapters):
   title = chap['title']
   start = chap['startTime']
   end = chap['endTime']
   text += f"""
[CHAPTER]
TIMEBASE=1/1000
START={start}
END={end}
title={title}
"""
   print(i, ":", text)

# Construct the output filename using the index
output_filename = f"FFMETADATAFILE{filename_without_extension}.meta"

print(output_filename)

with open(output_filename, "a") as myfile:
    myfile.write(text)
```

稍微修改了一下网上的写法，把成品输出成以 `.meta` 结尾的文件，方便接下来批量处理的时候检索。

为每一个视频文件写一个跟视频文件名相同的 `txt` 文件，在里面写上章节信息，比如说：

```
0:01:43 OP
0:24:01 ED
0:24:23 ENDING
```

不过其实这里有个比较反直觉的事情，我虽然只需要快速跳转到op结束和ed结束的地方，但是却需要添加三个时间点。名称为OP和ED的时间点分别是OP和ED结束的时间点，最后一个时间点则是视频结束的时间点。如果不这么添加，那么名称ED的时间点就不会被认为是一个可以跳转的点，导致没法跳转到ED结束的地方去看下集预告。

要处理的番少的有十二集，多的有将近30集，所以写了一个脚本来批量跑这个Python脚本。不过既然都Python了，其实全用Python来写也是没啥问题的。

批量生成了 `.meta` 文件之后，就可以开始使用FFmpeg往视频文件里塞metadata了。命令如下：

```bash
ffmpeg -i intput.mp4 -i FFMETADATAFILE.meta -map_metadata 1 -codec copy output.mp4
```

`-map_metadata 1` ：这个选项将元数据从第二个输入文件（在此案例中为 `FFMETADATAFILE.meta`，因为 FFmpeg 从0开始索引输入）映射到输出文件。本质上，它告诉 FFmpeg 使用 .meta 文件中的元数据作为输出视频文件的元数据。

`-codec copy` ：这个选项指定视频和音频编解码器应直接复制而无需重新编码。这用于保持视频和音频流的原始质量，并且由于不需要重新编码，所以使过程更快。

同样也是写了个脚本来批量运行这个命令，之前制作 `.meta` 文件的时候，在文件名中包含了和视频文件一样的名称就是为了批量运行。

## mkv

本来以为mkv也要跟上面一样折腾一番，没想到搜了一下发现mkv有专门的工具干这个事情。

```bash
sudo apt install mkvtoolnix
```

并且这个工具的标注格式也比FFmpeg更加友好：

```plan
CHAPTER01=00:00:00.000
CHAPTER01NAME=OP
CHAPTER02=00:01:30.000
CHAPTER02NAME=MAIN
CHAPTER03=00:22:34.300
CHAPTER03NAME=YOKOKU
```

只需要使用如下命令即可为mkv文件添加章节了：

```bash
mkvmerge --chapters chapters.txt -o output.mkv input-file.mkv
```

因为其实一部番里，op和ed结束的时间点大部分是相似的，只有少部分集数不同，所以只要确认不同的集数就可以了。对于时间点相同的集数，写了个脚本来拷贝相同的txt文件，方便之后批量运行 `mkvmerge` 命令。改一下for循环的范围就可以决定为哪些集数准备相同的txt了。

```bash
# Loop through the indexes 04 to 12
for i in {14..24}; do
  # Construct the target file name
  targetFileName="[Nekomoe kissaten&LoliHouse] Kusuriya no Hitorigoto - ${i} [WebRip 1080p HEVC-10bit AAC ASSx2].txt"

  # Copy the content of "filename01.txt" to the target file
  cp "[Nekomoe kissaten&LoliHouse] Kusuriya no Hitorigoto - 01 [WebRip 1080p HEVC-10bit AAC ASSx2].txt" "$targetFileName"

  echo "Copied content to $targetFileName"
done
```

mkv的这个工具不知道为什么，比ffmpeg操作mp4要快很多。

## 碎碎念

会做这个其实还是因为「葬送的芙莉莲」和「药屋少女的呢喃」这两部番我最近连着看了好多遍，正好这两部番一部是mp4，另一部是mkv，并且都没有为op ed结尾添加标签。还是希望观看体验好一点，所以折腾了一下。
