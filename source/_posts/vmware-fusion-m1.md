---
layout: post
title: 在 M1 Mac 上使用 VMWare fusion 安装 Windows 虚拟机
date: 2022-01-20
categories: [折腾日记, MacOS]
description: 为了打游戏煞费苦心
tag: [MacOS, VMWare, Windows11 arm, 虚拟机, 命令行, 脚本]
---

## 前言

由于正在使用的方舟脚本有一个依赖还没有移植到 m1 平台上，所以我打算装一个 Windows 虚拟机来跑。

Mac 上的 Windows 虚拟机其实不少，如果不差钱其实可以直接 Parallels Desktop，少点折腾也是好事。VMWare Fusion 的优势是免费，但是 m1 版本尚在测试中，如果有正经的生产需求，我建议还是三思。

关于安装 VMWare Fusion 并且在其中安装 Windows11 arm 的方法，少数派的 [这篇文章](https://sspai.com/post/69659) 介绍得很详细了。其中提到两种获取 windows11 arm 的方式，推荐使用第二种，即从 Hyper-V 格式的虚拟硬盘转换后，直接挂载虚拟硬盘，比从 UUP 文件打包镜像要方便很多。

## Windows 上的折腾

要跑 python 脚本，第一步肯定是要安装 python。 Windows11 的商店其实就能直接安装 python 依赖，但是如果你和我一样从 Hyper-V 格式的虚拟硬盘转换之后，启动虚拟机，你会发现一开始没有商店应用。

不过当然是可以装的。建议先直接安装 Powershell 或者你熟悉的命令行，会让接下来的配置体验好不少。

但是我试了不少方法，最后还是直接运行了 `wsreset.exe -i` , 重启虚拟机之后，商店就出现了。

出现了之后，就可以在里面直接搜索 `python` 然后安装了。之后就是 `pip3 install` 想要的东西，然后就可以愉快地跑方舟脚本了。

## 还不够

Windows 虚拟机的分辨率非常低，当然 VMWare 有许多教程教你如何让虚拟机可以跑在高分辨率下，但是我其实不怎么需要 Windows 的图形界面。毕竟我只需要跑脚本，理论上只需要一个命令行界面就够了。所以我决定折腾一下 Windows 的 ssh，并且尝试后台启动虚拟机。

### ssh

折腾的过程意外的顺利，Windows11 的 OpenSSH 默认就是开启的，直接在 Mac 上使用 `ssh user@host` 连接就可以了。其中 ip 地址可以在 Windows 的设置页面中查看。但是这时候脸上去默认进入的是 cmd 环境，体验很差。所以我们需要在注册表编辑器中修改 ssh 的默认命令行环境。

在注册表编辑器中，找到 `HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH` 目录，在其中新建 string 项 `DefaultShell`，然后里面填入你喜欢的终端所在路径，比方说我用 PowerShell，就可以输入 `C:\Program Files\PowerShell\7\pwsh.exe`，这样使用 ssh 连接上之后，就会进入 PowerShell 环境中，获得一些基本的历史和补全能力。如果有兴趣，进一步折腾 PowerShell 的配置文件也可以，所有的修改都可以在 ssh 连接上之后得到，非常方便。

### 后台启动虚拟机

VMWare 本身就提供这一能力，但是需要使用命令行来启动。在 `VMWare Fusion` 的 app 包目录下，找到目录 `Contents/Library`，里面的 vmrun 可执行文件就可以用来启动虚拟机。我们可以使用如下命令：

```bash
vmrun -T fusion start /Path/To/Your/vm/vmx/file nogui
```

如果你按照前面少数派文章中的指引创建了虚拟机文件，那你应该知道那个 vmx 在哪里找，就在 vmwarevm 文件夹下。`nogui` 可以让虚拟机在后台启动。

## 自动化

我不想每天一条条敲命令。于是我写了个脚本，并且给 iTerm 创建了一个 profile，当我启动 profile 时，运行这个脚本，自动在后台启动虚拟机并且 ssh 连接上去。

脚本的内容就是上面所写的启动虚拟机并 ssh，没什么好说的。在 iTerm 的 Preferences->profile 页面，创建一个新的 profile，将启动选项选择为 Command，然后输入 `sh /path/to/script.sh`，这样当你新建这个 profile 的窗口时，就会自动运行这个脚本了。

本来我以为需要额外的配置来关闭虚拟机，没想到如果我创建了这个窗口后，直接在 ssh 中输入 exit 或者关闭 iTerm 的窗口，虚拟机就会自动退出。这样省去了很多繁琐的操作，使用起来更加方便了。

## 后记

其实这个还是很有必要的。以后出门应该只会带 MacBook 了。现在没有 Windows 机器的情况下，我也能让方舟脚本帮我刷体力，不至于要自己用手刷，然后由于懒惰而放弃了。

本来以为今年是可以就地过年，没想到深圳卫健委操作还是挺不错的。基本上中高风险地区只控制在了别的几个区里，而且老家没有搞一刀切，从低风险但是有病例的省内市回去只需要居家隔离三天就可以了。

明明是两件令人快乐的事情重合在一起，为什么会变成这样呢（指过年不能宅在出租屋白金死亡搁浅）
