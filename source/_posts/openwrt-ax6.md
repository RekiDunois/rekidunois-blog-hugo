---
layout: post
title: 年轻人的第一次 openwrt 刷机体验 feat. redmi.AX6
date: 2022-01-04
categories: [折腾日记, openwrt]
description: 博主犯蠢大赏
tag: [openwrt, 路由器, clash]
---

## 前言

AX6 买来的那时候，就已经说可以刷 openwrt 了。但是当时的固件似乎没有 160MHz 可用，而且我官固用着也还算可以，所以就没动。

搬家之后，还是觉得刷一刷比较好，毕竟功能更多，而且小米固件，还是多少有点不舒服。

正好最近连休，就开始折腾这些东西了。顺带一提，在刷 ax6 之前，我还顺手给 K30U 刷了 PE，能完美工作的指纹真是太爽了。

## 编译

我对交叉编译或者 openwrt 编译本身可谓是一窍不通。连 makefile 对我来说都是天书一样的东西。幸好，github 上可以使用 action 来编译 openwrt，你要做的只是搞清楚配置文件里的配置项是啥意思，然后作删改就好了。

搜了一下，有人专门为了红米系列的路由器的 openwrt 编译做了 action [仓库](https://github.com/jingleijack/AX6-AX3600_Almighty-Edition_Config)，热门型号都在里面了，fork 这个仓库，修改对应型号的 config 文件，然后进入仓库的 Action 页面，选中你想要编译的型号点击 `Run workflow`，然后，坐和放宽。

保守估计编译需要 2 小时 30 分左右，编译完成之后，action 会帮你生成一个 release，里面的东西基本上就是你刷入所需要的东西了。

## 刷入

有了固件，刷入就很方便了。恩山论坛里有非常详细的教程，从官固刷到 openwrt，或者从 openwrt 刷到，well，另一个 openwrt，甚至从 openwrt 刷回官固，都有。我这里稍微记录一下我用过的命令，以防自己以后需要再干这种事情。建议各位把所需要的文件存到一个文件夹里，放到网盘里备份起来。除了路由器，刷手机的时候也可以根据型号来建立备份文件夹，未来的你一定会感激现在的自己。

### 1.0 如果是官固

```shell
nvram set flag_last_success=0
nvram set flag_boot_rootfs=0
nvram set flag_boot_success=1
nvram set flag_try_sys1_failed=0
nvram set flag_try_sys2_failed=0
nvram set boot_wait=on
nvram set uart_en=1
nvram set telnet_en=1
nvram set ssh_en=1
nvram commit
```

### 1.01 如果是已经在 openwrt 状态下

```shell
fw_setenv flag_last_success 0
fw_setenv flag_boot_rootfs 0
reboot
```

[跳转到本文 1.3 节](#13-qsdk-固件刷入-mtd13)

### 1.1 QSDK 过度固件

```shell
mtd write /tmp/xiaomimtd12.bin rootfs
```

拔电重启，此时路由地址会变为 192.168.1.1，用户名 root，无密码。若光猫的地址也是这个，建议拔掉 wan 口，过会儿再插回来。

### 1.2 扩容分区（如果对自己使用的东西大小有明确认知，认为不用那么大，也可以选择不扩）

```shell
. /lib/upgrade/platform.sh
switch_layout boot; do_flash_failsafe_partition a6minbib "0:MIBIB"
```

拔电重启

### 1.3 QSDK 固件刷入 mtd13

```shell
ubiformat /dev/mtd13 -y -f /tmp/openwrt-ipq-ipq807x_64-xiaomi_ax6-squashfs-nand-factory.bin
fw_setenv flag_last_success 1
fw_setenv flag_boot_rootfs 1
reboot
```

其实我似乎没有做这一步。因为上面提到的编译产物并没有这个东西。我相信我现在是跑在 mtd13 分区，不知道有没有什么问题。..

### 1.4 openwrt 固件刷入 mtd12

```shell
ubiformat /dev/mtd12 -y -f /tmp/openwrt-5.10.63-redmi_ax6-squashfs-nand-factory.ubi
fw_setenv flag_last_success 0
fw_setenv flag_boot_rootfs 0
reboot
```

如果想要查看 fw_setenv 所设置的所有变量，可以使用 fw_printenv。

此时，路由器地址会变成 openwrt 代码中所设置的默认地址，应该也能在编译的时候修改？不是很清楚，但是可以在你使用的代码仓库里查看默认地址和密码。

然后，刷机就宣告结束了。

### one more thing

OpenWrt 和 QSDK 互相切换命令

OpenWrt 切换 QSDK

```shell
fw_setenv flag_last_success 1
fw_setenv flag_boot_rootfs 1
reboot
```

QSDK 切换 OpenWrt

```shell
fw_setenv flag_last_success 0
fw_setenv flag_boot_rootfs 0
reboot
```

本节参考文章 [红米 AX6 刷 openwrt 和 QSDK 双系统 （扩容）](https://www.right.com.cn/FORUM/thread-6054985-1-1.html)，[红米 AX6 刷 openwrt 教程｜软路由杀手来了](https://qust.me/post/redmi_ax6_openwrt/)，以及来自 [Rachel](https://blog.rachelt.one) 的大量帮助！（瑞秋秋编译 openwrt 本当上手，还做了文件存档，太感谢了）

## 然后就能正常使用了吗？

Well，yes and no.

第一次刷入算是基本正常，该工作的都工作，但是有点小问题：

- 无线速度使用 iperf3 测试，只有 400+，对于 wifi6 来说无论如何都太慢了。但是后来发现，是因为 mpb 走了有线导致测速变慢了（迫真）

- OpenClash 经常无故崩溃，然后网就断了。

- Adguard 无法更新，当然也无法进入配置页面

由于这些小问题，我打算重新编译 openwrt。一开始想要使用 lean 的源码，但是他有一个 commit 将所有米系设备都移出了支持列表。当我尝试在 Action 里去掉这个 commit 的时候，rebase 又出现了冲突。

于是我尝试 fork lean 的代码，然后在本地处理这些冲突，再 push 回去。这么操作确实成功了，但是 Action 却在编译的时候失败了。

```log
 ERROR: target/linux failed to build.
```

没有搞清楚发生什么事了，我把目光转向了 5.15 版本的固件。跑了这个版本的 Action，发现一切工作也正常，甚至感觉比 5.10 版本还要顺滑。

然而在使用的过程中，依旧遇到了一些奇怪的问题。在五次编译之后，openwrt 总算是稳定使用了。而中间遇到的问题，最终发现都是我自己的问题：

- 头一次刷进去，因为地址变成了 192.168.1.1，和光猫的地址一样，所以进了光猫的后台，一脸懵逼，拔掉线之后继续 config（然后设置 pppoe 的时候忘记插上去，又差点重新刷一次）

- 编译好了一版，对着 lan 口设置了半天 pppoe，折腾了差不多半个小时才反应过来，应该是在 wan 口设置。

- 上面提到的 openclash 的问题，是因为 openwrt 22.0 使用 nftables，而 openclash 和 shellclash 都需要用到 iptables，所以就算成功启动了，也无法走代理。

- 遇到了 argon 主题在 22.0 上不知为何无法显示侧边栏的问题，但是改成 5.15 内核版本之后就没这个问题了。argon 需要安装 luci-compat 和 luci-lib-ipkg 两个包，否则它的配置界面无法启动。

- 一开始以为空间不够用，打算找方法扩容，但是我手头没有空闲的 u 盘。后来发现，不是因为空间不够，是因为我传了张 10m 的图片打算当作登录界面。

（实在是被自己蠢到了

解决了这些问题，我的 openwrt 也算是基本正常运行了。再配置一下 Adguard 和访客网络，就可以基本不用动了。
