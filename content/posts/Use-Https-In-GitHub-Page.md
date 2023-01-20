---
layout: post
title:  我的 Https 之旅--GitHub Page 更换域名
date: 2019-06-30
categories: [使用方法, Web 端开发]
description: 小绿锁，yes！
tags: [GitHub Page, https, cloudflare]
---

> 在 GitHub Page 使用自己的域名

fork 原博客的时候我就发现可以通过指定 `CNAME` 来让 `username.github.io` 的 url 跳转到你指定的网址。所以在咨询了朋友之后，便决定将这个博客的域名换成自己的域名 `rekidunois.cn`.

首先是在仓库里创建 `CNAME` 文件，在里面设定域名 `blog.rekidunois.cn` （后来发现其实也可以直接在 sitting 里设置）. 之后在域名的 dns 服务提供商里设置域名解析。这里我使用了 cloudflare 作为我的 dns 服务提供商。在使用它的 dns 服务之前，我们需要先去域名注册机构（比如我在腾讯云购买的域名）将 `rekidunois.cn` 的 dns 设置为 cloudflare 的 dns, 这样 cf 的域名解析服务就能使用了。
添加域名解析中的 CNAME 记录，将其指向 `rekidunois.github.io`, 这时候应该可以使用 `blog.rekidunois.cn` 来访问博客了。只是我们需要在 cf 中做 https 相关设置后，浏览器才不会将我的博客标记为不安全。查询了一些资料之后，我在 Crypto 中将 SSL 选项置为 `Fleible` 方式。并且开启了 `Always Use HTTPS` 选项。这时我发现 GitHub 库中的设置里还有一个选项是启用 https, 我便将它勾选。过了一段时间之后，`blog.rekidunois.cn` 就能够正常访问并且浏览器里出现那把锁了。
这里可能会遇到一些坑，比方说页面中如果有一些引用的脚本，脚本所创建的图片可能会无法加载。博客标签页的 icon 也可能会无法显示。而通过引用脚本启用的功能（如 gitalk) 也会出现无法加载的情况。而如果浏览器地址栏出现了『该网站试图从不安全的来源加载脚本』（我记不清具体的表述，反正大概是这个意思）这样的提示的话，请不要手动让浏览器加载这些脚本，因为这会让浏览器将你的网站标记为不安全。
总的来说这个 https 还算是比较简单的设置，我甚至没有敲任何一个命令就解决了问题。下一个目标是将部署好的 ttrss 完全 https 化。成功之后大概也会写一篇文章来记录整个 ttrss 的部署历程吧。