---
title: 博客魔改
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-03-18 20:00:00
categories:
  - 技术
tags:
  - Blog
description: 一场翻车经历的记录
photos: 
---
心血来潮给差不多一年半前建的博客换波主题，然后遇到了史上难题
<!--more-->

## 主题

这次换的主题是 [Sakura](https://github.com/mashirozx/Sakura/) 这也太好看了吧

## 步骤

### 下载主题

git clone 到 theme 文件夹

### 准备环境

去官网安装 node.js

安装 hexo

`npm i hexo-cli -g`

### 修改配置文件，修改主题

在 `_config.yml` 文件中把 `theme:` 选项修改

### 初步测试

`hexo g`

报错

```
INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 147, Column 2]
  unknown block tag: note
    at Object._prettifyError (D:\Blog\node_modules\nunjucks\src\lib.js:36:11)
    at Template.render (D:\Blog\node_modules\nunjucks\src\environment.js:524:21)
    at Environment.renderString (D:\Blog\node_modules\nunjucks\src\environment.js:362:17)
    at D:\Blog\node_modules\hexo\lib\extend\tag.js:66:9
    at Promise._execute (D:\Blog\node_modules\bluebird\js\release\debuggability.js:303:9)
    at Promise._resolveFromExecutor (D:\Blog\node_modules\bluebird\js\release\promise.js:483:18)
    at new Promise (D:\Blog\node_modules\bluebird\js\release\promise.js:79:10)
    at Tag.render (D:\Blog\node_modules\hexo\lib\extend\tag.js:64:10)
    at Object.tagFilter [as onRenderEnd] (D:\Blog\node_modules\hexo\lib\hexo\post.js:230:16)
    at D:\Blog\node_modules\hexo\lib\hexo\render.js:65:19
    at tryCatcher (D:\Blog\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (D:\Blog\node_modules\bluebird\js\release\promise.js:512:31)
    at Promise._settlePromise (D:\Blog\node_modules\bluebird\js\release\promise.js:569:18)
    at Promise._settlePromise0 (D:\Blog\node_modules\bluebird\js\release\promise.js:614:10)
    at Promise._settlePromises (D:\Blog\node_modules\bluebird\js\release\promise.js:693:18)
    at Async._drainQueue (D:\Blog\node_modules\bluebird\js\release\async.js:133:16)
    at Async._drainQueues (D:\Blog\node_modules\bluebird\js\release\async.js:143:10)
    at Immediate.Async.drainQueues (D:\Blog\node_modules\bluebird\js\release\async.js:17:14)
    at processImmediate (internal/timers.js:456:21)

```

我换回了原来的主题 `next` 还是报错一样的

可以看出问题出在 `\node_modules` 也就是刚装的 `node.js` ??

然后，对，没错，我就这么死在了第一步。 全文终

### 重新测试

嗯，没错，我在原来的拷贝上是可以 `hexo g`

那么就是新的 `node_modules` 出了问题，可是为什么呢

然后我换了新主题又该死的报一样的错，我。。。

是的我又自闭了

### 傻瓜包

于是我尝试[傻瓜一键式安装包](https://github.com/honjun/hexo-theme-sakura)

```
npm install hexo-cli -g --save
npm install
hexo g
```

呜呜，好像成功了呜呜呜

于是本次不是博客整改升级了，而是 `博客迁徙`

### 文章迁徙

最重要的一部分内容

然后，呜呜呜，又是同样的错误，大胆猜测自己之前的文章格式和新主题不符

嗯，，好像确实。。。

要勇敢舍弃，才能更好得前行(雾)

### 开始配置

替换 `_config.yml` 文件

然后又错? `WARN  No layout: index.html`

所以还是一项一项小心翼翼改

按照[文档](https://docs.hojun.cn/sakura/docs/#/home)里面的改

## 末了

总之这个主题是可以用了？

之后就是配置这个主题了呜呼

之前下载的主题问题应该是，那个不是 `hexo` 的主题，是 `wordpress` 的主题

## 找到解决方法

第二天进行博客迁徙

通过一篇一篇复制，终于找到了问题所在

错误信息
```
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
Nunjucks Error:  [Line 108, Column 4] unknown block tag: note
```

错误原因是某篇 `.md` 中用了一下语法

```md
{% note default %}

K が奇数の時、 a,b,c を K で割ったあまりはすべて 0 である必要があります。 K が偶数の時、 a,b,c をK で割ったあまりはすべて 0 であるか、あるいはすべて K/2 である必要があります。このような組の個数は、 N 以下で K で割って 0 あまるものの個数と K/2 あまるものの個数から求めることができるので、この問題を解くことができました。

{% endnote %}
```

出错的就是 `note`

原来这个语法是 `Next` 主题的特性，该死

删掉即可