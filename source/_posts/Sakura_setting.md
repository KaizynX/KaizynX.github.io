---
title: Hexo-Sakura主题配置
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-03-19 10:00:00
categories:
  - 技术
tags:
  - Blog
description: Hexo-Sakura 主题在此博客上的配置历程
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Sakura_setting/sakura.png 
---

## 主题介绍

[github](https://github.com/honjun/hexo-theme-sakura)

直接整个下载,作为博客的根目录,在此基础上配置

## 初级配置

[官方文档](https://docs.hojun.cn/sakura/docs/#/)

参考文档可以完成大部分主流配置

## cdn

百度搜索例如 `jsDelivr+github搭建免费的cdn` 的关键字

```
$ https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径
# 栗子：
$ https://cdn.jsdelivr.net/gh/Fofade/cnblogsThemes@1.0/live2dw/assets/z16.model.json # 加载1.0版本

# 注意： 如果不加  @发布的版本号  默认加载最新版本
# 栗子：
$ https://cdn.jsdelivr.net/gh/Fofade/cnblogsThemes/live2dw/assets/z16.model.json # 加载最新版本
```

## 进阶配置

主要参考 [博客链接](https://yremp.live/hexo-sakura/)

这篇也不错的 [博客链接](https://blog.csdn.net/u011759205/article/details/104839472)

作者的 [bilibili](https://www.bilibili.com/video/av43856893)

## 切换背景

第一次尝试上面博客提供的切换背景方法失败

于是百度到如下方法 [另一篇博客](https://blog.csdn.net/cungudafa/article/details/104295156)

修改文件  `\themes\Sakura\layout\_partial\footer.ejs`

在 `</footer>` 前添加如下代码

```
  <!--壁纸切换-->
  <div class="skin-menu no-select" id="mainskin"  style="position: fixed">
    <div class="theme-controls row-container">
        <ul class="menu-list">
            <li id="white-bg"> <i class="fa fa-television" aria-hidden="true"></i></li>
            <li id="sakura-bg"> <i class="iconfont icon-sakura"></i></li>
            <li id="gribs-bg"> <i class="fa fa-slack" aria-hidden="true"></i></li>
            <li id="KAdots-bg"> <i class="iconfont icon-dots"></i></li>
            <li id="totem-bg"> <i class="fa fa-optin-monster" aria-hidden="true"></i></li>
            <li id="pixiv-bg"> <i class="iconfont icon-pixiv"></i></li>
            <li id="bing-bg"> <i class="iconfont icon-bing"></i></li>
            <li id="dark-bg"> <i class="fa fa-moon-o" aria-hidden="true"></i></li>
        </ul>
    </div>
    <div class="font-family-controls row-container">
        <button type="button" class="control-btn-serif selected" data-mode="serif" onclick="mashiro_global.font_control.change_font()">Serif</button>
        <button type="button" class="control-btn-sans-serif" data-mode="sans-serif" onclick="mashiro_global.font_control.change_font()">Sans Serif</button>
    </div>
  </div>   
  <canvas id="night-mode-cover"></canvas> 
    <div class="changeSkin-gear no-select">
      <div class="keys" id="setbtn"> <span id="open-skinMenu"> 切换壁纸 | SCHEME TOOL 
        &nbsp;<i class="iconfont icon-gear inline-block rotating"></i> </span></div>
  </div><!--skin.end-->
```

但是令人震惊的是，之前博客的方法突然又起效了。

## 背景动画

[参考博客](https://blog.csdn.net/cungudafa/article/details/104295156)

花瓣飘落、文字，爱心，连线，飘动的彩带

修改文件  `\themes\Sakura\layout\_partial\footer.ejs`

```
	<!--浏览器搞笑标题-->
	<script src="https://cdn.jsdelivr.net/gh/wallleap/cdn@latest/js/hititle.js"></script>
	<!-- 🌸飘落 -->
	<script src="https://cdn.jsdelivr.net/gh/wallleap/cdn@latest/js/sakura.js"></script>
	<!-- 雪花飘落 -->
	<!-- <script src="https://cdn.jsdelivr.net/gh/Yafine/cdn@2.5/source/js/snow1.js"></script> -->
	<!-- // 文字 -->
	<script src="https://cdn.jsdelivr.net/gh/wallleap/cdn/js/shehuizhuyi.js"></script>
	<!-- // 烟花 -->
	<canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas> 
	<script type="text/javascript" src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script> 
	<script src="https://cdn.jsdelivr.net/gh/Yafine/cdn@2.5/source/js/fireworks.js"></script>
	<!-- // 星星跟随坠落 -->
	<script src="https://cdn.jsdelivr.net/gh/cungudafa/cdn@2.1.2/js/cursor.js"></script>
	<!-- // 爱心 -->
	<script src="https://cdn.jsdelivr.net/gh/wallleap/cdn/js/love.js"></script>
	<!-- // 连线 -->
	<!-- <script src="https://cdn.jsdelivr.net/gh/wallleap/cdn/js/canvas-nest.min.js"></script>
	<script type="text/javascript" src="https://cdn.bootcss.com/canvas-nest.js/1.0.1/canvas-nest.min.js"></script> -->
	<!-- 样式二（飘动的彩带） -->
	<script src="https://cdn.jsdelivr.net/gh/wallleap/cdn@latest/js/piao.js" type="text/javascript"></script>
	 <!--背景切换-->
	 <script src="//instant.page/3.0.0" type="module" defer integrity="sha384-OeDn4XE77tdHo8pGtE1apMPmAipjoxUQ++eeJa6EtJCfHlvijigWiJpD7VDPWXV1"></script>
```

## 添加页面底部图片

修改文件  `\themes\Sakura\layout\_partial\footer.ejs`

在一堆 `<script type="",url="">` 之间(其实我不清楚插哪不可以反正这里可以)插入 

```
<center>
  <img src="">
</center>
```

## 代码渲染问题

根目录下的 `_config.yml` 文件

```yml
highlight: 
  enable: false
  line_number: false
  auto_detect: false
  tab_replace:
```

上面三个选项之前全写了 `true` 导致渲染出错

## 置顶 和 加密

[参考博客](https://blog.csdn.net/cungudafa/article/details/104346521)

按照博客里的一步一步做

关于置顶标签的问题，也确实是找不到放哪里哭了

不过勇于尝试多次之后就找到一个较为合适的位置了。。略

## 修改字体

*2020-03-20*

[巨佬博客](https://m1314.cn/302.html)

一个不太大众的博客主题最糟心的事情估计是技术博客不好找吧

也可能是之前用的 Next 主题太火了，把我惯坏了

不过这个大佬是用 `Wordpress` 搭建的，和 `Hexo` 有很大的出入

不过还是可以借鉴模仿的(不会网页语言的我感到自己好厉害)

在这个文件 `\themes\Sakura\source\css\style.css`

找到里面的关键词 `font-family:`

类似 `font-family:Arial,Helvetica,sans-serif;`

这后面就是字体,虽然有很多个,但经过我的盲目分析,显示的应该是第一个

然后你改一下第一个就好...

不过文件中有好多,应该是不同地方用不同字体,至于具体什么地方,看括号外名字猜测吧

暂时不知道外部字体的导入方法,你可以改成一些常用字体或系统自带的字体吧

## 修改 关于-我 中的对话内容

*2020-03-21*

修改文件 `\themes\Sakura\source\js\botui.js`

## 图片放大

*2020-03-21*

在 `.md` 插入图片时使用如下代码

```
<img src="" data-action="zoom">
```

效果演示

<img src="https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/Sakura_setting/sakura.png" data-action="zoom">

## 内容折叠

*2020-03-22*

[参考博客](https://www.faker.top/2020/02/14/Hexo/h6-折叠文字/)

找这个教程也是把我气的，一搜索全是 next 的教程

<details>
  <summary>HTML语法测试</summary>

代码渲染失败

```cpp
int main() {
    return 0;
}
```

</details>

{% spoiler "插件效果测试" %}
[插件](https://github.com/fletchto99/hexo-sliding-spoiler)

代码能渲染
```cpp
int main() {
    return 0;
}
```
{% endspoiler %}

## 最后

从昨晚一直折腾到今天下午三点半，总算是初步建立好了，累die

总的来说，想折腾一个好看的博客，可能并不是需要你学多少专业的知识，会多少专业的技能

而是看你有没有耐心，一颗坚持不懈的狂热的心

你需要的是借鉴模仿，以及如何从百度等搜索引擎找到解决问题方法的能力