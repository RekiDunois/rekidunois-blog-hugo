---
layout: post
title: 正式工作一年半（实习以来两年半）的一点碎碎念
date: 2020-11-28
categories: [阶段总结]
description: 我花了一年多的时间来取悦自己，然后发现自己是真的菜。
tags: [杂谈, 时间, 工作, 胡言乱语]
---

> Tempora mutantur, nos et mutamur in illis;

大学以前我最想要的东西就是一台个人电脑。但是家里只有一台老旧的电脑，还是和爸妈共用的，为了多少能体验一下电子设备，省吃俭用买了台三百块的手机，爸妈知道后我还被骂了一顿。

大学以后我最想要的就是自己一个人住的单间，因为学校宿舍的条件实在太过恶劣。但是在大学我还是用电脑做了很多事情，虽然那台电脑特别破。

实习被我认为是逃离垃圾大学的第一步。实习之前有个和学校合作的培训公司跑过来给全班人强制培训，因为内容是硬件所以我毫无兴趣（并且据熟悉硬件的舍友说，他教的东西大一学生都会做，而且他还真的让他一个大一学弟来顶替他上课）。这时候接到了现在这家公司的面试请求，于是请假跑去深圳面试，住了一天然后回来（这样可以请两天假不去培训）。最开始我不觉得我能过，因为我以为他们是用 c# 和 .Net 的公司，结果面试题是 c。然后我 c 基本学的一塌糊涂，cs 基础几乎等于没有（因为学校都没教，我也不知道该学啥）。唯一会的就是链表，而且还很多错。

没想到最后居然同意我实习了。同期有两个毕业生，他们试用期我实习期。最开始的日子非常悠闲，甚至没有布置任务。由于旧的办公室人坐满了，我们三个是在隔壁楼新租的办公室。没有上司看着你工作，再加上任务没布置/布置了也没有很重，同期的一个人甚至买了台 Xbox 放办公室里，接办公室的 4k 屏玩大镖客。

实习的结果，我没有按时完成我的第一个任务。虽然相关代码在 dead line 前两天才给我，我要从其中一个项目迁移到另一个项目中。开发途中的需求也不断修改，而且事后我觉得，这个需求并不是非常好做。我当时害怕及了，HR 并没有跟我聊有关转正的事情，我已经做好了毕业去重新找工作的准备了。结果之后我还是安全转正了，并且顺利负责了其他的项目。

在经历了蠢的要死的毕业设计，跟副院长斗智斗勇之后，总算是顺利毕业了。成为了正式工，我却感觉和实习的时候没有特别大的区别。给我分配的项目依旧很少，以至于很多需要提供的需求在 deadline 之前几天才会给我。而我秉承不想加班的理念，很多项目被几个人一起顺理成章地拖慢了。这时候我渐渐发现了公司代码的一些问题。

提报的闪退，崩溃问题非常多，但是我们的日志输出是很乱的，并且崩溃在日志上也不一定能看得出来。我觉得 c++ 要规避很多崩溃问题，肯定需要添加单元测试。但是整个公司的 c++ 项目使用的还是 msvc 2013 版本的编译器，并且使用比较旧版本的 Qt，新的测试项目引用这些代码之后不能通过编译，添加单元测试的想法一直停留在想法里。我不确定我这个项目能不能升级，升级之后其他的项目要不要一起升级，愿不愿意升级。总之就是和大多数代码故事里说的一样，屎山依旧是屎山，很难改变。

我想了很多办法提升开发体验。甚至想过用 .Net 重构一个项目。其实最近需要上架微软商城的需求，就是重构的好时机，但是上司直接说：我们不可能用微软的技术栈重新写一个项目。所以这条路就死了。停留在旧代码的这个项目，在之后的迭代中会发生什么事情真的难以预料。

我每修改一行代码就在担心，这样会不会导致崩溃，会不会功能不正确，会不会让我的评价进一步降低？之前 hr 已经和我聊过说测试认为我发的版本质量比其他人都低，我说没办法啊，要么你加班自己测试各种情况测出来再改，改完了再发版本。要么写单元测试代码，规避常见错误的同时，对于以前已经发生过的错误也能保证修改代码后不会再次出现。

但是说实话，我自己有几斤几两很清楚。我是个下决心要学 WPF 然后研究了半天项目结构如何组织，最后慢慢就放弃了的菜鸡程序员。崩溃问题我自己写出来的也有很多，低级错误也没少犯。以至于到今天我写每一行代码都还是战战兢兢，思前想后，畏首畏尾。发的每一个版本都害怕会不会出现问题，奇奇怪怪的 Windows Api 经常看半天不知道他要干嘛。自己喜欢的技术，也经常觉得自己是叶公好龙，看半天一个能看的玩意儿都做不出来。渐渐的我也失去了追求更好的技术的信心，每天除了工作就只是打游戏。

最近开始研究 WinUI，发现里面提供了非常多开箱即用的东西，而且不一定非得写 UWP，可以写 Win32，感觉之前碎片化学习的东西，慢慢都能一点点组合起来用了。感觉重拾了一点点信心，开始搞很久以前就想做的音乐播放器了。希望这一次不要半途而废吧。

这三年来我玩的游戏可能没有大学三年多，但是玩的时间说不定比大学三年长，而且常玩的游戏居然没什么特别大的变化。砍口垒，wows，fgo，风暴英雄，文明6。最近开始玩方舟，塔防游戏也能算是我很喜欢的游戏类型，唯一玩的变少的游戏可能是音游了吧。感觉自己反应真的不如高考完那时候好了，很多当时能 fc 的曲子现在怎么打怎么死。

其实对我来说，游戏和编程区别也不算大。都是我的爱好，只是编程是有学习曲线的，你在其中遇到的挫折会比游戏多得多。而游戏开发者肯定希望你花更多的时间在游戏上，所以你可能遇到的挫折更少，或者更容易克服。虽然我游戏玩的也不是很好，但我还是希望我编程也能像游戏一样，总能找到令我满意的解。

出来社会一年多，其实我内心感觉以前的朋友门混的大概都比我好。爸妈嘴巴上说着你开心就好，实际上买房买车找老婆，交家用，这些其他人要考虑的事情他们一个也没有少想。只是我们之间交流不如其他父母和孩子之间深入，我又是能不说话就不说话的性格，所以聊的不是很多吧。很多事情我有默默在做，但是非让我说出来我更倾向于保持沉默。「老死不相往来」是我觉得我对现实人际关系的一种犬儒。先不论父母，朋友什么的我觉得我已经伤害了很多人的感情了，与其继续接触新的朋友然后再伤害他们，不如不要和更多的人交流，以免有人受伤。

毕业的时候我与自己顶下了三年之约，三年后的自己要更加优秀，让自己更满意，找到更好的工作。三年过半，我觉得这个约定可能非常难实现了。但是好在自己也不能把自己怎么样，这个约定不管是三年还是五年，我都会一直往前走，直到真正实现这个约定为止。

> 希望与未来的自己相遇在更加广阔的海洋上

The end
