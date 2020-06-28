---
layout: post
title: 本地编辑 Markdown 时粘贴图片自动插入 Markdown 图片格式
categories: [使用方法, VS Code]
description: 这可能是持续写整个博客的基础操作
keywords: VS Code, Picgo, Paste Image, markdown, github
---

> 这可能是持续写整个博客的基础操作

&ensp;&ensp;&ensp;&ensp;在折腾了那么久之后, 我还是回到使用静态网页搭建博客的方式来. 毕竟markdown那么好用, 我也不舍得放下这个工具. 在步入社会开始工作之后我就发现, 即使工作的时候没有直接要求你用你觉得很好用的东西, 你也可以自己去设法使用这些你认为用起来很舒服的道具. 在折腾成功之后会有莫大的满足感, 而且日常使用也会提升工作体验, 何乐而不为.

&ensp;&ensp;&ensp;&ensp;但是markdown暂时还没有能在我的工作中有所应用. 可能以后写文档能用上吧, 谁知道呢（ 那么, 简单讲一下这个博客的工作流程. 非常简单, 那就是在本地编辑markdown文件, 然后 `git commit` 并且push到GitHub里叫做 `UserName.github.io` 的库里, 这样你的新文章就会被解析并在 `https://UserName.github.io` 中可以访问. 之所以使用markdown, 有很多人给出了分析, 我这里就讲一点: markdown在写作的时候, 可以让你的双手几乎不离开键盘, 就完成排版和修改格式等等操作. 如果要在其他的文字工作方式中实现这一点, 就我所知可能只有ThinkPad的小红点可以做到让用户在工作的时候双手不离开键盘打字区.

&ensp;&ensp;&ensp;&ensp;那么, 有一个非常现实的问题就出现了: 我写文章并不光是只有文字（太长的文字也没人看鸭）, 还需要插入图片. 在markdown中是可以很方便地插入图片, 不管是本地的图片还是图床中的图片, 都可以插入. 问题在于, 手动输入图片的url或者本地路径怎么样都不是一个舒服而且高效率的方式. 在Windows上编辑文章, 图片还是以剪贴板的复制粘贴操作来插入图片最为舒服. 所以本篇文章的意义就是在于, 如何在VS Code中配置粘贴图片自动拷贝到本地的特定目录, 并且在markdown文本中输入引用图片的格式, 让图片可以被正确地显示.

## 寻找工具

&ensp;&ensp;&ensp;&ensp;既然是VS Code, 那么第一反应是使用插件来实现这一功能. 优秀的第三方插件支持也是VS Code得以立足于编辑器市场的重要特点. 在Google了一阵之后, 很快就发现了两款可以实现相应功能的插件: [Picgo](https://marketplace.visualstudio.com/items?itemName=Spades.vs-picgo) 和 [Paste Image](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image). Picgo是开源的作品, 在GitHub上也可以找到它的 [repo](https://github.com/PicGo/vs-picgo), 但是安装后发现它重点在于如何在VS Code编辑markdown文件时, 上传图片到图床中. 既然我们决定将整个博客都放入一个GitHub repo, 那么我们似乎不是很需要上传到其他的图床里.

&ensp;&ensp;&ensp;&ensp;另外一款插件Paste Image, 名字非常的直截了当, 功能也是基本能满足需求. 可以直接以快捷键的方式粘贴图片到markdown中, 通过一番设置之后, 达成了我只需要commit就可以完整更新博客文章的效果. 下面贴一下设置的方式:

![BasePath](/images/blog/2019-06-06-00-03-25.png)

放置图片的根目录. 其中变量 `${projectRoot}` 是插件提供, 这里提供的是你blog repo在本地的绝对路径, images是想要放置所有博客图片的文件夹. 这里会有一个问题, 我们下面继续说

![Image Path](/images/blog/2019-06-06-00-08-51.png)

其实本来, 所有的图片就放在一个文件夹里是可以的. 但是这个博客的文章还是有所分类, 并不是所有都是博客文章. 比方说有一些大段的工具使用方法, 就不适合作为博客文章发出, 而应该以wiki的形式储存. 所以在这个地方对图片进行分类也是有必要的. 问题就在于, 我没有找到方法可以让我在插入图片的时候根据编辑文件所在地址来决定`images`文件夹中图片要放到哪个子文件夹里. 所以这里也只能先写死.

![Insert Pattern](/images/blog/2019-06-06-00-12-26.png)

这个地方拼接的字符串就是在markdown文本中要输入的图片路径, 所以应该是相对路径而不是绝对路径, 那么 `${projectRoot}` 这个变量就没法使用了, 毕竟输出的是绝对路径. 然后 `images` 也没有其他变量可以生成这个路径, 所以也只能写死...

&ensp;&ensp;&ensp;&ensp;经过以上配置之后, 使用快捷键 `Ctrl + Alt + v` 就可以将剪贴板中的图片自动拷贝到指定路径, 并且在markdown文本中输入正确的显示字段了. 最后吐槽一下VS Code, 使用了Synax插件同步配置但是突然失效不说, 插件本身居然不告诉我没有拉取远程配置, 而是报告同步成功, 我???? 其实VS Code应该去用yaml之类的格式当配置文件的, 会舒服很多. 明天到了上班的地方还要在那里的电脑再弄一次.
