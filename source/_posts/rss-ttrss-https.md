---
layout: post
title: 我的优秀 RSS 体验折腾记录
date: 2019-11-12
categories: [使用方法, Web 端开发]
description: A W S L
tags: [RSS, TTRSS, HTTPS, Docker, Fever, Reeder]
---

> RSS 是目前为止数字屏幕上最优秀的信息阅读方式之一

自从发现 [RSSHub](https://docs.rsshub.app/) 这个优秀的项目之后, 我就开始琢磨如何同时在 Windows 和 IOS 手机上获得优秀的 RSS 体验, 同时也是为了远离 Windows 端糟糕的微博和知乎前端.

## 使用 RssHub 生成 Rss 订阅地址

### Rss 与 RssHub 的一点想法

由于 RSS 会让内容生产者的主站流量损失, 由于 RSS Feed 中无法投放广告为内容生产者产生收益, 由于内容生产者选择依附于强大的平台, 在闭环的生态环境中发布内容, 由于...RSS "死掉"的原因实在是数不胜数. 自从2013年 Google Reader 被关闭之后, 各家互联网厂商都开始倾向于搭建属于自己的 App 的生态闭环. 你会发现当你在手机上使用微信和 QQ 的时候, 里面的内容(比方说图片)几乎无法分享给其他的 App, 公众号中的文章也不允许添加其他的外部链接, 微信内置的浏览器也会按照腾讯自身的商业需要--而不是一些正常的正当理由--来封禁内容的传播. 比方说在微信中不允许查看其他 IM 软件的链接, 带头封禁官方都没有封禁的 [996.icu](https://996.icu/) 网站.(一起这么做的还有 360 手机浏览器等国产套壳产品) 对于读者和内容生产者来说, 这样的阅读环境和生产环境显然不能算优秀, 甚至根本就不及格. 无奈国内还是有许多平台上有优秀的内容产出中. 毕竟不是谁都有耐心去研究怎么建站(即使到今天, 建站已经完全没有了技术含量).

在许多内容都没有官方提供的 RSS 订阅源时, 由来自 B 站的前端工程师, <del>知名女装偶像</del> [@DiyGod](https://diygod.me) 发起了 [RssHub](https://docs.rsshub.app/) 项目, 旨在为没有官方 RSS 源的平台生成 RSS Feed. 截止到今天, 已经能够生成诸如微博, bilibili, 知乎, 豆瓣等大部分主流平台的 RSS Feed. 也有诸如[广州市停水通知](https://rsshub.app/tingshuitz/guangzhou)这样的针对性通知的 RSS Feed 可以生成. 基本上我所接触的所有信息都能找到办法通过 RSS 的方式订阅, RssHub, yes!

### 开始学习使用 RssHub

[RssHub](https://github.com/DIYgod/RSSHub) 的代码在 GitHub 上开源. 而[文档](https://docs.rsshub.app/)中也介绍了 RssHub 的基本使用方式. 但是为了规避平台的反爬机制, [@DiyGod](https://diygod.me) 推荐我们自己部署 RssHub 实例. [文档](https://docs.rsshub.app/)中也有 RssHub 的部署方法. 我这里选择的是最 Noob 的部署方法--使用 Heroku 进行部署.

Diygod 已经[为 RssHub 做了直接在 Heroku 上部署的链接](https://heroku.com/deploy?template=https%3A%2F%2Fgithub.com%2FDIYgod%2FRSSHub), 只要点击这个链接, 注册账号并登录就可以直接启动一个你自己的 RssHub 实例. Heroku 的每个账号能每个月有差不多400个小时的免费实例运行时间, 如果你愿意绑定一张信用卡, 这个时间就会增至1000个小时(具体时间我记得不是很清楚, 请登录[官方网站](https://heroku.com)查询). 基本来说绑定了信用卡, 就可以在 Heroku 免费运行 RssHub 实例一整个月.

![本月我的用量](/images/blog/2019-08-11-22-38-54.png)

部署成功之后, 我们还希望在 RssHub 项目代码进行更新之后同步这一更新. 这里可以先把主仓库的代码 fork 一份到自己账号下, 然后在自己的 repo 里设置 WebHook(毕竟你又不能让 Diygod 帮每个人弄钩子), 当自己的项目 repo 更新之后, 就触发 Heroku 实例的更新, 然后在网页上创建从主仓库到自己仓库的 pull request 来更新代码. 这样就可以保证自己建立的 Rsshub 能够得到更新, 修复 bug 和添加新功能.

![创建 pull request](/images/blog/2019-08-11-22-40-37.png)

![这时候 Heroku 就会开始拉去代码并重新构建](/images/blog/2019-08-11-22-42-47.png)

Heroku 的实例创建之后会自动生成一个链接供用户使用, 类似于 scboy.taobao.com 这样的链接. 然而, 你完全可以使用你自己的域名, 只需要在网页上做几个设置就好了.

首先, 在你的域名解析服务提供商处添加额外的域名解析服务. 比如说, 添加 `rsshub` 解析记录, 设置为指向原 Heroku 提供链接的 `CNAME` 记录, 这样就可以使用你自己的域名访问 RssHub 实例啦!

## 使用 ttrss 持续抓取 rss 并阅读

Rss 阅读器有个问题, 如果你的客户端不在线的时候, 你是没法接到 Rss 订阅源的更新的. 然后 Rss 并不会一直保留推送的文章, 如果时间久了新的文章就会把旧文章挤掉. 这样的话有可能会错过推送. 解决方法就是, 保持你的客户端7*24小时在线, 这样就不会漏掉任何一篇 Rss 推送的文章. 单靠本地阅读器显然是做不到的. 这里就需要 ttrss 出场了. 特别感谢 [@HenryQW](https://henry.wang/) 瑰宝打造了 [Awesome TTRSS](https://github.com/HenryQW/Awesome-TTRSS) 这个 ttrss 解决方案, 让我们可以非常容易地部署自己的 ttrss 实例

> [ttrss](https://tt-rss.org/)是一款基于 PHP 的免费开源 RSS 聚合阅读器。🐋 Awesome TTRSS 旨在提供一个 「一站式容器化」 的 Tiny Tiny RSS 解决方案，通过提供简易的部署方式以及一些额外插件，以提升用户体验。

部署 ttrss 非常方便, 直接使用 [@HenryQW](https://henry.wang/) 编写的 [docker-compose.yml](https://github.com/HenryQW/Awesome-TTRSS/blob/master/docker-compose.yml) 即可部署 ttrss 及其依赖. 比如 [PostgreSQL](https://hub.docker.com/r/sameersbn/postgresql) 数据库和其他插件. 部署方式这里我直接引用文档说明:

> 步骤
>
> 下载 docker-compose.yml 至任意目录。
>
> 更改 docker-compose.yml 中的设置，请务必更改 postgres 用户密码。
>
> 通过终端在同目录下运行 docker-compose up -d 后等待部署完成。
>
> 默认通过 181 端口访问 TTRSS，默认账户：admin 密码：password，请第一时间更改。
>
> wangqiru/mercury-parser-api 及 wangqiru/opencc-api-server 为支持高级功能而加入的可选服务类容器，删除不会影响 TTRSS 基础功能。

按照步骤部署之后, 应该可以直接使用你主机公网 IP:181 的方式访问 ttrss. 但这还远远不够.

### 绑定域名, 使用 https

自己部署的 ttrss 还需要在 nginx 上设置域名, 才能使用域名来访问 ttrss 服务.

我使用 `Let's Encrypt` 获取免费的 https 证书。它不但为人们提供免费的证书，还提供了一个可以傻瓜式生成证书并修改 `nginx` 配置的 bot。进入它的[官网](https://letsencrypt.org/)，点击 `Get Started`，根据提示操作，即使是新手也能轻松使用 [certbot](https://certbot.eff.org/) 来将你的站点变为 https 站点。在使用时输入你的域名，在服务器这边关于域名的配置就已经成功了。

但是这时候你还需要一个域名解析服务。我推荐 [CloudeFlare](https://www.cloudflare.com/) 的域名解析服务。他们还能为你的域名提供一个免费（聊胜于无）的 CDN 服务。我已经将这个博客、我的 rsshub 实例和我的 ttrss 实例都放到了 cf 上解析。

ttrss 实例应该设置为 Type A，`Name` 应该是 `www` 或者其他你想要的东西，比方说 `ttrss.example.com` 的网址，`Name` 就应该是 `ttrss`。

设置完域名解析服务后你还可以顺手打开 `SSL/TLS 加密`。关于这个我也不是很懂，我尝试过 `Flexible` 和 `Full`，在我的服务端配置下似乎都能正常使用。

操作完成之后，我的 ttrss 站点检测结果如下：

![ssllabs的检测结果](/images/blog/2019-08-11-19-41-44.png)

现在，ttrss 站点可以正常使用了！

### 在手机上使用

安卓上我不知道什么 rss 阅读器好用，而且还要支持 ttrss 账号。在 IOS 设备上，我使用 `Reeder` 来查看 rss 订阅。[Awesome-TTRSS](https://ttrss.henry.wang/) 中已经内置了 `Fever` 插件，只要在设置中开启即可。当然，别忘记在设置中选中 `允许外部客户端通过 API 来访问该账户`，这样才能让 `Reeder` 访问你的 ttrss 实例。

![Reeder 在手机上设置完之后的样子](/images/blog/2019-11-12-00-14-29.png)

![文章阅读界面](/images/blog/2019-11-12-00-16-46.png)

阅读效果相当舒服，还能在网页版和手机端之间同步阅读记录。这样在地铁上读过的文章你就不会在网页端发现它还是未读状态了。

## 总结

有一个比较遗憾的地方就是在 Windows 上我没有发现合适的可以使用 ttrss 同步的 rss 阅读器，所以在 Windows 端应该还是要用网页版来阅读 ttrss 文章。但总的来说我现在使用 rss 获取信息的方式让我非常满意。

## 后记

这篇文章从八月份开始一直到十一月才写完。中间这段时间我的工作也是慢慢变得多了起来，所以一直很忙也没时间写任何东西。但还好，我觉得我写博客的热情还没有被消磨掉。这段时间我买了二手的 LG G30 和联想拯救者 Y9000X，之后可能有时间会写一下这两个产品的使用体验。
