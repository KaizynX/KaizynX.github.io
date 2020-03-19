---
title: 博客进阶
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2018-10-12 21:00:00
categories:
  - 技术
tags:
  - Blog
description: 博客主题配置,自动部署
photos: 
---

# 进阶指南

## 序

自博客搭建初以来,就没什么时间进一步社会主义改造了

直到国庆期间(只有三天假期...),才有时间进行了一番大整改

---
## 主题选择

真的,被之前的主题坑了太久

直到我迫不得已改成了 [NexT](https://github.com/theme-next/hexo-theme-next) 主题

这个一直倍受大家推崇和喜爱的号称 star 第一的主题

虽然形式简单,但已经集成了很多插件等玩意,比配置之前那个不知道方便多少

于是我弃坑了,现在的是 NexT 6.4.1

---

## 标配 NexT

[NexT 使用文档](http://theme-next.iissnan.com/)

这个还有中午,还有官网,多么好.

安照上面的一步步来应该没什么问题

---

## 高配 NexT

百度 `next 配置`

其实就有很多~~其实相似度极高~~的配置教程

里面好玩的东西很多

坑点没怎么遇到过

---

## 插件安装和使用

一般也就在最顶层文件夹输入类似下面的指令

    npm install [插件名字] --save

如果要使得 `Travis CI` 上也能加载插件

只要在 `.travis.yml` 文件中的 `install:` 部分加上一行即可

```
 
# S: Build Lifecycle
before_install:
- npm install hexo-cli -g
install:
- npm install
- npm install hexo-deployer-git --save
- npm install [在这里] --save
  
script:
- hexo clean
- hexo generate
after_script:
# 略略略
# End: Build LifeCycle
```
---

## 提一下自动部署

这里丑化说在前头,还是老老实实本地 `hexo deploy` 比较好

自动部署坑太多,带来很多不必要的麻烦

因为你很难知道那边的环境到底哪里不同了

---

## 第三方集成服务

其他主题配置都按照文档来就好

这里提一下,我们在主题的 `_config.yml` 配置文件中看到很多集成的插件,例如

```cpp
 
# 分享条
# NeedMoreShare2
# Dependencies: https://github.com/theme-next/theme-next-needmoreshare2
# See: https://github.com/revir/need-more-share2
# Also see: https://github.com/DzmVasileusky/needShareButton
# iconStyle: default | box
# boxForm: horizontal | vertical
# position: top / middle / bottom + Left / Center / Right
# networks: Weibo,Wechat,Douban,QQZone,Twitter,Linkedin,Mailto,Reddit,
#           Delicious,StumbleUpon,Pinterest,Facebook,GooglePlus,Slashdot,
#           Technorati,Posterous,Tumblr,GoogleBookmarks,Newsvine,
#           Evernote,Friendfeed,Vkontakte,Odnoklassniki,Mailru
needmoreshare2:
  enable: false
  postbottom:
    enable: false
    options:
      iconStyle: box
      boxForm: horizontal
      position: bottomCenter
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook
  float:
    enable: false
    options:
      iconStyle: box
      boxForm: horizontal
      position: middleRight
      networks: Weibo,Wechat,Douban,QQZone,Twitter,Facebook

```

把 `enable: false` 改成 `enable: true` 

在按需求把剩下两个 `false` 改成 `true` 即可

其实不然,你点进去上面一些链接的 `github` 官网 (第三行的)

你要按照上面的代码操作,什么 `cd` 进某个目录再 `git clone`

就成功添加了

其实说的很清楚,按照上面的做就好

---

**坑点**

自动部署时还是没有!

1. 因为你 `clone` 来的文件夹被 `gitignore` 掉了

    修改在主题的文件夹里 `\主目录\themes\next\` 的 `.gitignore` 文件

    你会发现你克隆来的恰好在 `\source\lib`

    完美被忽略,注释掉即可

    我现在的 `.gitignore` 如下

    ```.gitignore
        
    .DS_Store
    .idea/
    *.log
    *.iml
    yarn.lock
    package-lock.json
    node_modules/

    # Ignore optional external libraries
    # 就是它,罪魁祸首
    # source/lib/*

    # Track internal libraries & Ignore unused verdors files
    source/lib/font-awesome/less/
    source/lib/font-awesome/scss/
    !source/lib/font-awesome/*

    !source/lib/jquery/

    source/lib/ua-parser-js/*
    !source/lib/ua-parser-js/dist/

    !source/lib/velocity/
    ```

    其实说实在的这个**全部删掉**都没大碍,不删反而出事了

2. 你克隆来的文件夹包括 `.git` 你 `git` 提交时会报错

    你在之前删掉这个文件夹就好了

    如果你没删之间用了 `git` 呢

    那你怕是再也加不上去了

    骗你的 [看这里](https://www.cnblogs.com/howdop/p/5583342.html)

        git rm --cached 那个文件夹

    又可以了

---

## 自动部署图标无法显示

以前自动部署时我的图标无法显示

就是首页啊归档啊那个旁边的图标

道理同上,被 `.gitignore` 掉了

这个图标系统的根应该在 `\themes\next\source\lib\font-awesome`

至于图标来源,网上,一个叫做 fontawesome 的网页

其实不是[这个](https://fontawesome.com/) 里的

我亲测应该是这个的[中国版](http://www.fontawesome.com.cn/)里的

至于具体用法,额...百度吧,~~我不太会~~

---

## 添加背景动画

这个属于**第三方集成服务**

也是需要 `clone` 过来才能用的

光按百度上说法直接改 `true` 无效

```yml
 
# 背景动画
# Canvas-nest
# 大部分都是这种,网络上应该能轻易找到样例
# Dependencies: https://github.com/theme-next/theme-next-canvas-nest
canvas_nest: false

# cpu占用太大
# JavaScript 3D library.
# Dependencies: https://github.com/theme-next/theme-next-three
# three_waves
# 点点波浪(像海面)cpu占用极高
three_waves: false
# canvas_lines
# 点线几何体,占用最低(还是高)
# 样例: https://notes.doublemine.me/ 其实我觉得这个最好看
canvas_lines: false
# canvas_sphere
# 球上凸点(柱?)占用高
canvas_sphere: false

# 背景彩色线条,这个不占cpu但是只适合pisces主题(唉)
# Only fit scheme Pisces
# Dependencies: https://github.com/theme-next/theme-next-canvas-ribbon
# Canvas-ribbon
# size: The width of the ribbon.
# alpha: The transparency of the ribbon.
# zIndex: The display level of the ribbon.
canvas_ribbon:
  enable: false
  size: 300
  alpha: 0.6
  zIndex: -1

```

我之所以没加这个特效是因为占用太大了

~~其实还是我不知道怎么魔改,看样例占用还行~~

---

## 添加 Valine 评论

我找了找还是这个评论比较好,样式简单,支持匿名

配合 [LeanCloud](https://leancloud.cn/)

额...找不到之前的教程了,自己找,满地都是

## 添加背景

[看这个](https://blog.csdn.net/mango_haoming/article/details/78473243)

然后这里说说使用本地图片的方法

```
 
body {
    background:url(/images/bg.jpg);
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:50% 50%;
}
```

放到 `/images` 文件夹即可

那个你放头像啊图标啊各种东西的文件夹

---

## 未完待续~~吧~~

主要还是百度骚配置

坑点大致如此
