---
title: Windows vscode cmd 中文乱码
author: Kaizyn
avatar: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/custom/avatar.jpg
authorLink: https://kaizynx.github.io/
date: 2020-03-22 13:00:00
categories:
  - 技术
tags:
description: 解决vscode cmd下乱码问题
photos: https://cdn.jsdelivr.net/gh/KaizynX/cdn/img/posts/cmd_chinese/cover.jpg 
---

[主要参考博客](https://blog.csdn.net/qq_35038153/article/details/78430359)

[次要参考博客](https://www.cnblogs.com/chouxianyu/p/11249810.html)

查看命令行属性发现 当前代码页编码是 `936 GBK`

网上有一种方法是在cmd输入 `chcp 65001`, 并在注册表中让该语句自动执行

但这种方法并不能解决 vscode 中命令行的乱码问题,于是采用如下方法

1. 在运行中通过`regedit`进入注册表

2. 找到`HKEY_CURRENT_USER\Console\%SystemRoot%_system32_cmd.exe`

3. 新建一个 `DWORD（32位值）`,命名为`CodePage`，值设为`65001` 
  
   已有`CodePage`的话，修改它，改为`十进制`，`65001`