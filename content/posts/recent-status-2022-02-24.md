---
layout: post
title: 近况总结
date: 2022-02-24
categories: [阶段总结]
description: 没有阶段成果，但是有阶段目标
tags: [生活, 杂谈]
---

## 前言

昨天去找新房子的房东拿钥匙，我比他先到，所以在等。旁边有小摊在卖章鱼小丸子，刚好是饭点我也饿了，所以就点了一份。没想到摊主是一位聋哑人，打着手势一边告诉我 10 块钱一份，一边问我要放什么酱。

点完之后我还发现，摊主开着微信视频，看来是有人和摊主视频交流，顺便在客人无法交流的时候让视频对面的人帮忙说话。所以我在旁边一边吃，一边帮摊主招呼了一下其他的顾客。

摊主做的章鱼小丸子，说实话味道一般。有炸糊了的地方，感觉火候也煮过头了。撒料撒得特别多，海苔的味道都盖过了小丸子本身的味道。

摊子的后面就是一个商业中心，前面有一个喷水池，上面写着财富港三个字。喷水池的另一边，是一群人在排队等核酸检测。天气非常冷，风吹到手指和脚都失去了知觉，甚至我踩了十几分钟单车也无济于事。

回去的路上，我还迷路了，结果多花了半个多小时，回到家已经快八点半了。

第二天，公司写字楼表示只有 48 小时内核酸阴性者才能进入，大家聚集到楼下的麦当劳商量对策，HR 和行政发消息说，大家先去做核酸，今天远程办公，我和室友跑了挺远的，好不容易做好了，大冷天里衣服都湿透了。

然后我打的去交通局指定的一个医院，做了驾驶证体检，回家之后申请了更换驾驶证。

## 生活

新年之后我一直处于似忙非忙的状态。室友要离职搬家，所以我开始找房子，踩着单车整个区跑，好歹是看到了一个喜欢的房子，姑且是定下来了，但是还要等到三月中旬才可能会搬，到时候这边的房子也不知道房东那边会怎么处理，最坏的结果可能是押金无了。

## 世界局势

今天，俄罗斯总统普京宣布对乌克兰进行「特别军事行动」，整个早上都在刷相关新闻，一会儿说特种部队控制了基辅机场，一会儿说乌克兰军队指挥部被击毁。

一时间，战况与地狱笑话齐飞，我们见证了第一场有短视频直播的现代战争。

## 程序员的自我修养

学会了在 workers 里写代码，写了一个小 api，请求当天的必应壁纸并且返回，之后准备加上个宽高选项。

成功在 heroku 部署了 miniflux，虽然这个其实做过，但是我总算搞明白，如何防止免费 dyno 冻结 miniflux 了。其实也很简单，在 heroku 里部署一个定时的 add-on，每 10 分钟，指定 cookie 的情况下 curl miniflux 的刷新 url，有 cookie 的情况下就能正常触发 miniflux 的刷新行为，这样就不会因为被冻结而更新不及时了。

准备开始系统性学习如何使用 GitHub Action 来在自己的 vps 里部署数据库和自己写的 bot，达到提交代码后自动测试，构建 docker 并且部署的效果。

## 游戏

第四个号的麻将对局数达到了 70 个半庄战，升到了 2 级。系统数据显示 1 级玩家的平均对局数有 360 场，看来我还需要多打多练。

我 pc 彻底无法启动，现在不知道是内存的问题还是主板的问题，所以在 steam 上买了不少能在 mac 玩的小游戏。前段时间通关了风来之国，很好玩，但是流程确实也不算长，并且没有 end-game 玩法。

跟朋友合资租了一个 mc 服务器，可惜 1.18 版本的工业 2 有 bug，不能玩工业 2 了，有点缺少目标。但是我看他们玩原版的很多东西也做得很爽。

因为找房子太忙了错过了阴云火花活动。

## 阶段

自己今年过完生日也 25 岁了，感觉毕业之后时间过得真的飞快，每天盼着周末，过完周末盼下一个周末，盼着盼着三年就快过去了。毕业的时候给自己定的三年之约，我感觉自己确实比刚毕业那会儿懂的多了点，但是真要做点什么的时候，又感觉自己啥也不懂。但是害怕麻烦这个特点，似乎没怎么变过。

好消息是我写博客和阅读的习惯倒是坚持下来了。虽然博客没啥有用的内容，阅读也只是阅读一些新闻而已。总归不算是坏事吧。

下个月要搬家，我要好好布置一下桌面，可能要按照笔记本的使用习惯去修改布局。房间里其他的东西也要好好规划。然后就是要去体检，希望工作一切顺利吧。