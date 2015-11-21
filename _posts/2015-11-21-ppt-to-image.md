---
layout:  post
title:   "PPT转图片"
date:    2015-09-26 02:27:44
categories: Tools
---

### PPT转图片解决办法

---
使用环境：unbutu

首先将 ppt 转换为 pdf，然后将 pdf 转换为 图片，具体步骤如下：
1，首先安装 uncoconv：sudo apt-get install unoconv
2，然后安装 imagemagick:sudo apt-get install imagemagick
3，准备 ppt 文件
4，进入 ppt 所在目录 默认文件为 1234.ppt
5，运行命令 unoconv 1234.ppt 
>(根据服务器配置不同)大概等3~10秒 会在当前目录生成 1234.pdf 文件

6，运行命令 convert 1234.pdf cc.jpg 
>会在当前文件夹下生成图片 cc为自己取的名字

---