---
layout: post
title: "Javascript生成字符画（工具篇）"
categories:
- STUDY
tags:
- STUDY


---

> 之前逛网站的时候，会偶尔打开别人家网站的控制台，看下代码，然后会发现在控制台输出一些比较好玩儿的图形代码——字符画，比如大家都知道的——知乎。

于是乎，自己尝试下。由于很简单，所以就无脑罗列了。

 1. 下载字符画生成工具——ASCII Generator（当然其他的也ok）。然后把要变成字符串的图片拖到图片区域。
   
    ![zifu][1]
 2. 可以根据自己的需要进行大小、字符样式进行调整，然后复制出来。
    
 3. 复制到Sublime中，然后选择第行第一列把换行换成“\n”（若直接丢到代码中，会因为代码中不能有换行使代码报错）。
   
    ![此处输入图片的描述][2]
    ![此处输入图片的描述][3]
 4. 剩下的丢到console中，然后浏览器查看。
   
    ![此处输入图片的描述][4]
    ![此处输入图片的描述][5]

**The End.**

PS:太简单了有没有，没有挑战有没有（其实也是参考了别人的文章，奈何现在找不到原地址了，没得贴出来，sorry，以后碰到补上），那等下次补个<code>canvas</code>版的。哈哈！又自我挖坑！


  [1]: https://raw.githubusercontent.com/7demo/7demo.github.io/master/images/zifu1.jpg
  [2]: https://raw.githubusercontent.com/7demo/7demo.github.io/master/images/zifu2.jpg
  [3]: https://raw.githubusercontent.com/7demo/7demo.github.io/master/images/zifu3.jpg
  [4]: https://raw.githubusercontent.com/7demo/7demo.github.io/master/images/zifu4.jpg
  [5]: https://raw.githubusercontent.com/7demo/7demo.github.io/master/images/zifu5.jpg