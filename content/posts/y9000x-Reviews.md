---
layout: post
title: 码农的毕业笔记本——Y9000X
date: 2020-01-25
categories: [产品体验, 笔记本]
description: 满足两大爱好之一可能再也不需要买新的电脑了。
tags: [Laptop, Lenovo, Intel, Coding]
---

> 轻薄，标压，性能，"我的对手很少", 但是几乎没人能打得过我

笔记本电脑绝对是 PC 工业史上的一颗璀璨明珠。计算机从最开始占地一个房间，到可以摆在桌面上，到可以塞到手提袋里随时拿走，这样的发展历程已经过去了几十年。当影视作品中表现一个人在“工作”的时候，笔记本电脑也是高概率会出现的道具之一。
很长一段时间，我已经接受了这样的设定：想要轻薄，性能就不高；想要高性能，笔记本就会很厚重，并且续航很低。如果你又要轻薄，又要高续航，还需要高性能？请选择火星牌笔记本（雾）

但是其实这些需求中，“高性能”这一项对于每个人的定义可能都是不同的。如果你要玩游戏，或者机器学习什么的，你可能需要一个比较强的 GPU, 不过对于我来说，我只希望写代码的时候 IDE 能反应快一点，开浏览器的时候能多开几十个标签页，然后编译的时候花的时间能少一点而已。对，我可能只需要一颗强壮的 CPU.

只是市面上其实很少有这种只偏重 CPU 的产品。如果选择一款搭载标压移动版 CPU 的笔记本，第一它会非常厚重，第二它的散热还需要兼顾 GPU. 而且在我选购游戏本的那个时代，其实 16G 的内存和大号 SSD 其实非常的不普及，特别是 NVMe 的 SSD. 所以在日常使用中其实这类游戏本的体验，只能说是差强人意。如果我要稍微兼顾一点便携性，比方说过年的时候带回老家，就会非常痛苦。我一个朋友甚至因为带游戏本回家把背包都背得开裂了。

那么，有没有这么一款有强劲的 CPU 性能，没有独显，然后可以做到非常轻薄的笔记本呢？答案已经写在标题里了，那就是联想出品的 Y9000X.

## 基本配置

CPU 最高可以选配 i9-9880H, 我手上这款是 i7-9750H. 显卡则使用的集成显卡，没有独显。
内存最高可选配 32G 双通道版本。但是这款机型的所有显卡都是焊在主板上的，并且没有额外的内存插槽，所以无法自行升级内存。但是这款机型的最低配也是 16G 起步，一般来说是比较足够。但是即使加钱到 32G 也不贵，只是必须和 4k UHD 屏幕一起买就是了。

储存设备方面，它只有一块 SSD, 我手上的是来自三星的 1T NVMe SSD. 最高可选配 2T 版本。作为一个笔记本来说，这个大小应该是够用的。如果有大量冷数据或者视频之类的，建议考虑储存在 NAS 或者网盘中可能会比较合适。如果还是觉得不够，机器搭配了一个 M2 接口以供用户自行添加硬盘。

![CrystalDiskMark](/images/blog/2019-11-12-00-50-41.png)
> 应该是我用过的最好的硬盘了？

屏幕可以选择 1080P 或者 4k 分辨率的两个版本。我选择了 1080P 版本，其实稍微有点后悔，不过也还好。这块屏幕给人的感觉还是不错的，亮度很高。

![B site](/images/blog/2020-01-24-00-44-32.png)
> 拍照技术不好，不知道能不能看出来

IO 接口方面，它提供了两个 USB 3.1 Type-A 接口，其中一个支持关机充电。只是这两个接口都位于机身的后面，屁股的位置上。所以要插拔不是很容易，我平时都是用来插诸如无线鼠标一类的设备。

![屁股](/images/blog/2020-01-24-12-16-50.png)
> 左边那个是罗技 G304 的接收器，右边那个有电池标志的接口是支持关机充电的。
> 但是平时没啥用，所以拿个塞子塞住

然后在她的右边还有（非常先进的）3.5mm 耳机孔，还附带一个读卡器，这应该能方便摄影师朋友们导出摄影素材。

![right](/images/blog/2020-01-24-12-22-15.png)

左边它提供了轻薄本必备的雷电 3 接口，而且还有两个。这两个接口共享 40Gbps 的带宽。当然如果你只用其中一个，也可以独享 40Gbps 的带宽，因为一般我们会需要一个 Type-C 接口来充电。不然的话可能会让单个 Dock 非常烫。

![left](/images/blog/2020-01-24-12-23-26.png)

Y9000X 搭载的是全尺寸的键盘，除了有小键盘以外，还有尺寸正常的方向键，而不是通常的那种将上下压缩得很小的方向键。只是我感觉键盘似乎偏上了一点，下方的距离非常开阔，导致打字如果带了手表或者手环，下方会顶着这个东西，可能不是很舒服。手臂吊起来打字又会很累。

![keyboard](/images/blog/2020-01-24-12-24-36.png)

键盘手感还算可以，和机身的金属质感很搭。我个人比较喜欢她亲戚隔壁 ThinkPad 的手感，但是如果要打比方的话，这个东西有点像机械里的茶轴，然后 ThinkPad 的键盘有点像红轴。电源键同时也是指纹识别配件，可以搭配 Windows 10 的 Windows hello 使用。触控板中规中矩，是一整块的状态，但是不像 MacBook 一样到处都可以按压，属于那种隐藏式的左右键按压。

本机搭载的摄像头是那种位于屏幕下方的鼻孔摄像头，但是联想很贴心地为它配备了物理开关。这一点在 ThinkPad 系列似乎也成了标配，应该得到鼓励。

![webcam](/images/blog/2020-01-24-12-26-32.png)

由于没有搭载独显，也没有奇奇怪怪的认证，所以这台机器的 C 面非常干净。拿到手的时候只有两个贴纸，而且由于采用的是金属磨砂材质，这些贴纸撕起来也非常轻松，并且不会留下什么痕迹。对于强迫症来说是非常舒服的事情。

## 性能

这台机器的一大卖点是，只搭载标压移动 U 的情况下，还为其配备了四个散热风扇，散热用料非常足。（于是声音也非常大，简直起飞）.

随手跑了一下 CeniBench, 得分居然比 7700K 还要高

![CeniBench](/images/blog/2019-11-12-00-49-16.png)

16G 双通内存还是很爽的，减少访问虚拟内存的频率那就能减少卡顿。频率 2667 （咦不应该是 2666 嘛）, 不知道能不能艹一艹到 3200 （逃

![memory](/images/blog/2020-01-24-12-33-58.png)

## 使用感想

- 便携

这篇文章从我买 Y9000X 开始，大概是十一月份左右，到现在一月份，担任主力开发机两个半月。除了丢在公司写代码以外，偶尔也会蹭了公司网下载的东西然后拿回家里去。这一点我就发现她是真的占地方小而且轻啊。如果我不嫌插线拔线麻烦甚至可以一直拿着这个东西上下班通勤，不管是坐公共交通还是骑自行车，我觉得带着都不成问题。

- 发热

热嘛确实是热。不过在键盘区域感受不是很大。电源非常热，这个 95W 的电源烫的要死，而且还不好带。但是市面上所售这个功率的 PD 充电头其实并不多，我看唯一适合便携的似乎是苹果的那个新 16 寸 Macbook Pro 用的充电插头。但是那个要小六百块钱了，emmm 贫穷它缠绕着我。

- 续航

开着高性能但是还是用电池的时候这 CPU 还在疯狂睿频到 4.0 左右，于是续航就尿崩，大概一两个小时左右。然后开了节能和节电模式之后能续到四个小时左右，在高铁上玩一路大概没啥问题？（而且高铁上还都有插座）. 除此以外我似乎没有啥需要脱离电源使用的情况了。

## 总结

使用 Y9000X 也快三个月了。上一个使用的笔记本是我拿来当台式机用的神船 Z6, 使用三个月的时候我应该刚开始大学生活。而距离那时已经过去了四年多了，笔记本这一产品在我看来也有了相当程度的变化。特别是 SSD 以及内存颗粒价格的降低，将所有类型笔记本性能都增加到了非常高的一个高度。Y9000X 在这三个月里已经基本覆盖了我所有的使用场景，工作的时候用来写代码，然后团建的时候带出去旅游，春节回家带着她坐火车，都没有什么大问题。如果说一定要购买的配件，那我觉得可能需要一个显卡坞和一块 1660super 放在老家里，这样我现在也能快乐冰原了（逃
