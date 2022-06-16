---
title: 微软暑期实习 leader 面
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2022-03-11 15:45:00
categories:
  - interview
tags:
hidden: true
message: 责令删除
password: "******"
description: 就写了道题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/MS_interview2/cover.jpg
---
# 面试

面试官迟到了几分钟，还不开摄像头

也没叫我自我介绍，问了一下项目，13:07 直接发个链接开始写题，是个系统设计题

设计一个朋友圈，有四个函数，用户和朋友圈都用 int 表示，其中朋友圈按时间id递增

1. 发一条朋友圈 `void(int userId, int momentId)`
2. 关注一个人 `void(int followerId, int follweeId)`
3. 取关一个人 `void(int followerId, int follweeId)`
4. 获取所有关注者(包括自己)最新的10条朋友圈 `list<int>(int userId)`

用 `map<int, set<int>>` 表示 用户 -> 关注者

用 `map<int, vector<int>>` 表示 用户 -> 朋友圈

1，2，3 非常容易，至于 4 则遍历所有关注者(以及自己)，将这人的最新朋友圈放入优先队列，维护并取出10个即可

`priority_queue<array<int, 3>>` 其中 `array<int, 3> {momentId, userId, idx_of_monment}`

大根堆 ，第一关键字是朋友圈id，还有存用户id，以及这条朋友圈是用户的第几条朋友圈 idx

以上，细节不再赘述

一开始他有问为什么想到优先队列。额，如果不是暴力遍历，我就只想到这个了呗(不知道怎么解释，就是想到了嘛)

写完后问我内存上有没有可以优化的地方，我没想出来

他提示我说朋友圈只存最近十条就够了，于是存朋友圈的改为 `deque<int>` 或 `list<int>` 这样

然后经过他点播，我想到优先队列里面也是只存十个就够了, 把 `priority_queue` 换成 `set`

结束了大概 13:35 左右的样子，问我有什么问题，我就问他们在做那块，他说是 bing ad 扯到 13:40 多点就结束了，迟到早退，太喜欢这种上来写道题完事的！

# 结果

结果就挂了呜呜呜，收到了感谢信