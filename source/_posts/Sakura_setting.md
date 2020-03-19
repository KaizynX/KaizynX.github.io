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