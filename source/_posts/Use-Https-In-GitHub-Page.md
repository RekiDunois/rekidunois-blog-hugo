---
layout: post
title:  我的 Https 之旅--GitHub Page 更换域名
date: 2019-06-30
categories: [使用方法, Web 端开发]
description: 小绿锁，yes！
tags: [GitHub Page, https, cloudflare]
---

> 在GitHub Page使用自己的域名

fork原博客的时候我就发现可以通过指定 `CNAME` 来让 `username.github.io` 的url跳转到你指定的网址. 所以在咨询了朋友之后, 便决定将这个博客的域名换成自己的域名 `rekidunois.cn`.

首先是在仓库里创建 `CNAME` 文件, 在里面设定域名 `blog.rekidunois.cn` (后来发现其实也可以直接在sitting里设置). 之后在域名的dns服务提供商里设置域名解析. 这里我使用了cloudflare作为我的dns服务提供商. 在使用它的dns服务之前, 我们需要先去域名注册机构(比如我在腾讯云购买的域名)将 `rekidunois.cn` 的dns设置为cloudflare的dns, 这样cf的域名解析服务就能使用了.

添加域名解析中的CNAME记录, 将其指向 `rekidunois.github.io`, 这时候应该可以使用 `blog.rekidunois.cn` 来访问博客了. 只是我们需要在cf中做https相关设置后, 浏览器才不会将我的博客标记为不安全. 查询了一些资料之后, 我在Crypto中将SSL选项置为 `Fleible` 方式. 并且开启了 `Always Use HTTPS` 选项. 这时我发现GitHub库中的设置里还有一个选项是启用https, 我便将它勾选. 过了一段时间之后, `blog.rekidunois.cn` 就能够正常访问并且浏览器里出现那把锁了.

这里可能会遇到一些坑, 比方说页面中如果有一些引用的脚本, 脚本所创建的图片可能会无法加载. 博客标签页的icon也可能会无法显示. 而通过引用脚本启用的功能(如gitalk)也会出现无法加载的情况. 而如果浏览器地址栏出现了『该网站试图从不安全的来源加载脚本』(我记不清具体的表述, 反正大概是这个意思)这样的提示的话, 请不要手动让浏览器加载这些脚本, 因为这会让浏览器将你的网站标记为不安全.

总的来说这个https还算是比较简单的设置, 我甚至没有敲任何一个命令就解决了问题. 下一个目标是将部署好的ttrss完全https化. 成功之后大概也会写一篇文章来记录整个ttrss的部署历程吧.
