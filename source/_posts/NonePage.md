---
title: 访问博客空无一物
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2021-04-06 22:40:00
categories:
  - 技术
tags:
description: 记一次博客崩溃
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/NonePage/cover.jpg
---
# 问题发生

因为某些不义之徒妄想窥视我的加密博客,眼不见心不烦,打算给博客加个隐藏文章的新功能

找到了这么一个[插件](https://github.com/printempw/hexo-hide-posts)

增加一个新功能,必然要测试后才能上线

因为我换了新电脑,之前配置的环境都丢了,今天重新配了一下

首先是 *Node.js* 去官网看看版本已经到 *14.16.0 LTS*

再看我之前用的版本是 *12.16.1* 想了三秒钟决定更新

嗯,然后本地测试了一下,成功了,我开开心心地 *push* 上去了

还有点不放心,特意上 *travis-ci* 看了一下,绿色通过

然后上博客一看,怎么什么都没有,就是字面意思的什么都没有,页面空白

我按 *F12* 查看了一下,还是几乎什么都没有,怎么回事

# 问题锁定

然后我去 *github* 上看看,似乎看出什么不对劲了

就是生成的 *index.html* 文件都是空的, 0KB

我本地也生成了一下,发现如出一辙,可为什么本地端口可以显示

百度 [关于hexo搭建博客——生成index.html页面为空](https://blog.csdn.net/qq_41507243/article/details/104433464)

里面提到的原因 **第二种：缺失相应配置文件**

因此我注意到 `hexo -g` 时确实有警告 (程序员才不看警告)

```
(node:16448) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:16448) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:16448) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
```

因为本地和 *travis-ci* 是一样的情况,因此合理怀疑是 *Node.js* 版本更新而舍弃了一些配置

# 问题解决

懒得折腾,改回 *12.16.1* 版本

这次崩溃验证了一个编程真理, 代码能运行就不要改了