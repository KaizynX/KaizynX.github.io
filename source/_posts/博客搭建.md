---
title: 博客搭建
date: 2018-09-01 12:48:55
tags: 技术
---

----------
序
--

想做一个自己的博客，而网上的像CSDN之类广告太多，而且没法个性化设置，于是有了搭建自己博客的想法，恰巧在知乎上看到一个教程，**免费**，重点是能免费搞，于是乎……走上了一条不归路……

----------

起源
--

就是这个 [超详细Hexo+Github博客搭建小白教程][1] 我看的第一篇教程，还是相当详细了，于是就按部就班开始了。
这里用的是 [github][2] 和 [hexo][3]
其中github可以用[coding][4]等代替，hexo可以用Jekyll代替
这里在给一篇用coding的[文章][5]
总之我是用github+hexo的，这套应该算是主流配置哈
之后就是一系列的坑了

----------

建站
--

安装hexo是用命令行装的，而用的指令是node.js的，所以要先装node.js这个问题不大


进入一个文件夹，要在里面
用下面的指令安装hexo

    npm install hexo-cli -g
    npm install hexo --save

然后我的那个文件夹里就有了一些不知什么东西
之后我就往下做

    hexo init
    npm install
    
报错了，我以为正常的，其实错误的原因是文件夹不为空
解决方法：清空这个文件夹或新建一个文件夹
至于原本里面的东西，其实我也不知道什么用

这样之后就可以了

    hexo g
    hexo s
    
就可以访问了，一切正常

再来是github与本地连接，因为我本来就是github用户，这个没什么问题，教程也有很详细的


我们在hexo init的文件夹第可以找到这样一个文件`_config.yml`这就是相当重要的配置文件了，文件末尾有这样的

    deploy:
        type:
        
按照教程指示，改为

    deploy:
        type: git
        repository: https://github.com/[你名字]/[你名字].github.io
        branch: master
        
`[你名字]`就是你之前在github上建立的repository的名字
像`type: git`，`type:` 和 `git`之间一定要有空格
另外，`repository:`可以缩写为`repo`一些教程里用的是缩写，我差点没看懂
网址不要写错，我之前复制来的ssh是`git@github.com:KaizynX/KaizynX.github.io.git`
然后就报错了，中间的:应该改为/，不如自己敲吧

还有很重要的一点是之前建立github的respository的名字一定要和你gihub的用户名一样
比如我的github的用户名是`KaizynX`，respository的名字就是`kaizynx.github.io`，小写，我就是因为这个错误调了好久
在你gethub的respository的settings上会显示` Your site is published at https://kaizynx.github.io/ `
因为之前没有完全一致，会导致显示` Your site is published at https://kaizynx.github.io/kaizyn.github.io`
由此引发一些列不可估量的错误，虽然能访问，但是没法正常显示，如这位兄弟的[情况][6]

再者这篇教程中对于`_config.yml`的配置其实没讲清楚
看[这里][7]大概讲清楚了

    url: http://[你名字].github.io
    root: /
    permalink: :year/:month/:day/:title/
    permalink_defaults:

其中有几行是这样的东西
你需要把第一行改一下

至此网站总算是可以用了
**hexo三部曲**

    hexo clean    #清除缓存
    hexo generate #生成
    hexo deploy   #部署

具体什么意思可以看百度之类，简写就是

    hexo cl
    hexo g -d

**部署之前**应该要安装个git插件

    npm install hexo-deployer-git --save

部署之后可能会跳出一个github的登陆窗口，也可能不会，没什么关系，然后你就可以访问你的网站了，如`https://kaizynx.github.io`
注意是`https`

----------

发布文章
--

站是总算建好了，想起了我最初的目的——写博客

    hexo new 名字

此时博客的目录结构大致如下

    - node_modules
    - public
    - scaffolds
    - source
	    - _posts
    - themes
    
都有什么用可以百度一下
在`_posts`中你就可以看到多了一个`.md`文件
这个就是你新建的博客，`.md`是markdown的格式
竟然是markdown格式，那总该有个IDE吧
[推荐][8] 当然得选一个可以本地编辑有客户端的
编辑完保存，再执行**hexo三部曲**，过一会你就可以看到你的博客更新了，至此似乎已经实现温饱了！

----------
但是
--
怎么和我想的不太一样啊。
这套hexo+github的实质大概是你用`markdown`写一篇文章，hexo帮你生成`html`，部署到`github`上，是个静态的网页，你只能本地修改，本地上传
可我希望像CSDN那样在线发布的呀

**革命尚未成功，同志仍需努力**

----------

更换主题
--
其实我主题是很早就换了，默认的真是太丑了，换的早，坑也出现得早
因为换主题是步入小康社会，温饱还没解决的话就算了
至于教程，原来的那些教程就有，我觉得[这篇][9]比较好的
可以去[官网][10]找找，[这篇][11]推荐的也不错
推荐使用[next][12]主题，据说是人气最高的
为什么呢？我看上的是[Tranquilpeak][13]主题，结果踩了好多坑
一开始我用next，后来换那个，用next倒是啥事也没有，换了之后……唉
毕竟用的人多，bug改的快吧，而且出错了网上容易找参考文献
而我用的这个，几乎没有。好吧，就是没有一个直接的解决方案，有的都是擦边球
说说正文，安装方法，直接`git clone`到`themes`文件夹下就好，详情见教程
然后我们就可以在克隆来的主题文件夹中看到，里面的结构类似我们整个博客文件夹的结构
最重要的也是配置文件了`_config.yml`对，主题有属于它自己的配置
你可以在里面改一些，具体看对应文档吧
通过配置文件你可以改网页图标，背景图片等等之类的
这个就是深入改造了，现在还玩不起，总之不用改也能用

然后就来说说我选的这个主题的坑吧
会报错

    ERROR Script load failed: themes/tranquilpeak/scripts/tags/tabbed_codeblock.js

    Error: Cannot find module 'jsdom'
    
第一句不重要，第二句是关键，先不说怎么解决吧
当时第一次用这个主题，发现不能用，我多绝望了
看来看去就这个看的上眼呐
于是我回到它github的官网上寻求帮助，看看文档之类的
于是我换了一个安装方式(撤回我之前说的话)
这次我从`releases`下载了一个稳定版本，解压到`themes`文件夹
其实两个方法都可以
区别是我选了不同的版本，所以这次没问题了


----------

插件安装
----
[官网][14]
其实我觉得并不需要什么额外插件就已经够用了
关于插件，其实我们之前做的

    npm install hexo-deployer-git --save
    
就是在安装插件，而且我怀疑

    npm install jsdom
    npm install jquery

也是插件，总上所述，安装插件还是一个相对简单的过程，一般也没什么配置
下面还是讲一下我的经历

**后台管理**

[教程][15]
这个插件是`hexo admin`
顾名思义就是你可以以管理员的身份登陆网站，并进行发布文章等操作
我一开始想实现我的伟大报复时，找到了这个
欣喜若狂的我迫不及待地装上了这个，且十分成功
就当我满怀希望觉得我能圆我美梦部署时
发现在github上无法进入admin界面
也就是说你只能在本地`hexo s`后，在本地爽爽
顿时失望透顶，原来也不过将`hexo new ...`换到了在网页上进行罢了
而且这个插件也没有markdown的语法帮助(虽然有预览)
据观察网上应该有界面更绚丽，功能更强大的插件
但由于无法实现我伟大报复，遂另寻其路
然后就有了下文的`TravisCI`大法


----------


继续奋斗——TravisCI自动部署
----

参考一下这两篇文章[①][16] [②][17]
讲的是借助 [TravisCI][18] 实现了只需git push就可以更新
也就是说在不同终端，不需要再装node.js和hexo，只要git (emmm)
虽然和设想差了一些(许多)不过勉强接受吧，毕竟静态网页，没钱买域名买服务器
具体实现讲得挺清楚了，虽然看着玄乎，两者结合着看也是弄下来了

首先要讲源代码存到github上，之前的是把hexo生成的html部署到github上
教程里做法没有说，于是出坑了。
首先到你的博客的文件夹，打开git bash然后`git init`一下
把`remote`加上。这个是git的基本操作，不说了
至于加那个`remote`，就原来那个好了，换个分枝
据说是可以再新建一个responsitory的，但是有点麻烦，也不知道有什么额外开销

    git branch [新分枝名]
    
这个时候报错了

    fatal: Not a valid object name: 'master'.
    
可以参考[这里][19]解决
我是随便`commit`了一次就可以了
然后把所有的东西都发送到这个分枝
**git三部曲**

    git add .
    git commit -m"随便写些什么"
    git push [remote的名字] [新分枝名]
    
再去看TravisCI，过一会就会有动静了，看见它正在帮你进行**hexo三部曲**
说明成功了(呸)

虽然换了个版本在自己电脑上通过了，但在TravisCI上出现了相同的问题

    Error: Cannot find module 'jsdom'

发现我的`.travis.yml`用的是[这里][20]的配置，感觉那两个不好用啊
然后在TravisCI上执行出来的又是没有格式的
我坚信不是我这里的锅了，本地依然完美，于是目光落到了

    # 指定node_js版本
    node_js: 
        - 7.9.0

想想本地都可以的，本地版本`10.8.0`
换上似乎是有一定几率成功了

不过来说**正解**吧，解决问题的关键在于英语水平
错误：找不到模组'jsdom'
姑且这么翻译吧，于是强行安装这个找不到的module
在我们的博客文件夹中打开命令行

    npm install jsdom
    
就可以解决了，不过呢，修补完这个问题后还有一个，这次是

    Error: Cannot find module 'jquery'

同理

    npm install jquery
    
之后**git三部曲**
TravisCI上就能顺利运行了，自动帮你**hexo三部曲**了

*更新于2018-09-09*
终于。。。又出错了
在TravisCI搞出来的也没有格式，哭了
一直也找不到什么原因，曾一度想放弃TravisCI大法过着安稳额hexo三部曲日子
直到我回到学校，试图把博客编辑总站迁移
前面可能说过吧，源代码都在那个文件夹，相当于这就是编辑总部
而不是像CSDN一样可以在网上发布

于是我从git上下载了 源代码 的分枝
在学校电脑上搭建了环境
进行了hexo二部曲，在本地打开服务器一看
竟然也是丢了格式
曾一度认为环境出了问题，转为只能在家工作的想法
不过还是找到问题所在了
原因是主题文件夹下面没有assert文件夹
因为什么头像啊背景啊图标啊之类的都在这个文件夹里面
而罪魁祸首竟然是`.gitignore`
是它把assert给忽略了！
发现了这个让我感觉希望复燃，遂逐渐明白事件的来龙去脉
同理我在最高层文件夹检查了一下`.gitignore`，大致有个了解

    .DS_Store     #不知道，反正我没有
    Thumbs.db     #不知道，反正也没有
    db.json       #应该是和node.js有关的
    *.log         #各种日志
    node_modules/ #node的模组?
    public/       #hexo g弄出的网页源代码
    .deploy*/     #也没有，好像和什么节点有关

需要注意的3-6行吧
第5行估计就是本地能通过而在TravisCI上报错的原因了
在TravisCI上要重新安装node.js和hexo等
你在本地有的插件在那边不一定有
第6行就是网页源代码了吧，你发现git上master分枝的就是这里面的东西
所以TravisCI有一种写法是把public/的push到git的master分枝
而我用的则是在TravisCI上执行hexo d

最后给出我成功的`.travis.yml`代码
```
# 指定语言环境
language: node_js
# 指定需要sudo权限
sudo: required
# 指定node_js版本
node_js: 
  - stable
# 指定缓存模块，可选。缓存可加快编译速度。
#cache:
#  directories:
#    - node_modules

branches:
  only:
    - source

# S: Build Lifecycle
before_install:
  - npm install hexo-cli -g

install:
  - npm install
  - npm install hexo-deployer-git --save
  - npm install jsdom
  - npm install jquery

# 执行清缓存，生成网页操作
script:
  - hexo clean
  - hexo generate

# 设置git提交名，邮箱；替换真实token到_config.yml文件，最后depoy部署
after_script:
  - git config user.name "KaizynX"
  - git config user.email "2291443901@qq.com"
  # 替换同目录下的_config.yml文件中gh_token字符串为travis后台刚才配置的变量，注意此处sed命令用了双引号。单引号无效！
  - sed -i "s/gh_token/${GH_TOKEN}/g" ./_config.yml
  - hexo deploy
# End: Build LifeCycle


# 方法一
#after_script:
# - cd ./public
# - git init
# - git config user.name "KaizynX"
# - git config user.email "2291443901@qq.com"
# - git add .
# - git commit -m "Update Blog"
# - git push --force --quiet "https://${GITHUB_TOKEN}@${GH_REF}" master:master
# E: Build LifeCycle

#env:
# global:
#  - GH_REF: github.com:KaizynX/KaizynX.github.io.git
```
我还遇到过的一个坑是在TravisCI连

    npm install hexo-cli -g
    
都运行不了，报错，这个百度可以解决好像是什么缓存问题
错误代码如下
```

npm ERR! code ERR_TLS_CERT_ALTNAME_INVALID

npm ERR! errno ERR_TLS_CERT_ALTNAME_INVALID

npm ERR! request to https://registry.npmjs.org/hexo-cli failed, reason: Hostname/IP does not match certificate's altnames: Host: registry.npmjs.org. is not in the cert's altnames: DNS:a.sni.fastly.net, DNS:a.sni.global-ssl.fastly.net
```
解决吗我也没实质性解决过吧，你们自己百度吧

----------

网页进阶
----

这部分内容还是参照不同主题的不同文档
在他们github的页面上可以找到
以我为例 [链接][21]


  [1]: https://godweiyang.com/2018/04/13/hexo-blog/
  [2]: https://github.com/
  [3]: https://hexo.io/
  [4]: https://coding.net/
  [5]: http://dugblog.coding.me/Hexo/20180616-build-blog-hexo-next-windows.html
  [6]: https://blog.csdn.net/qq_29347295/article/details/79005154
  [7]: https://blog.csdn.net/xuezhisdc/article/details/53130383
  [8]: https://www.jianshu.com/p/d4e331770e60
  [9]: http://dugblog.coding.me/Hexo/20180616-build-blog-hexo-next-windows.html
  [10]: https://hexo.io/themes/
  [11]: https://www.jianshu.com/p/bcdbe7347c8d
  [12]: https://link.jianshu.com/?t=https://github.com/iissnan/hexo-theme-next
  [13]: https://link.jianshu.com/?t=https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak
  [14]: https://hexo.io/plugins/
  [15]: https://www.cnblogs.com/xingyunblog/p/8681205.html
  [16]: https://www.jianshu.com/p/f4eaff5c2148
  [17]: https://blog.csdn.net/woblog/article/details/51319364
  [18]: travis-ci.org/
  [19]: https://blog.csdn.net/jackie_tsai/article/details/51587994
  [20]: https://blog.csdn.net/Xiong_IT/article/details/78675874
  [21]: https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/DOCUMENTATION.md