---
layout: post
title: "impress.js学习笔记"
categories:
- STUDY
tags:
- STUDY


---
---


<small>偶然间发现impress.js这款做ppt神器，大一时候看到炫酷技术又澎湃的心情又燃起。于是乎，学习下。</small>

<small>impress.js貌似没有什么文档，只有在demo页面中的注释哩写满了how rto use。所以这次翻译便是翻译部分index.html吧。</small>

<small>impress版本：[0.5.3](https://github.com/bartaz/impress.js/archive/0.5.3.zip)</small>

引入impress.js在页面底部。然后执行<code>impress().init();</code>

你想要知道怎么样使用<code>impress.js？</code>
第一步，也是很重要的一步，你需要阅读源码。它说明了<code>impress.js</code>是怎么样使用html与css构建特效的。
你必须具备一定得html与css基础才能够有效的使用impress.js。更加重要的是你必须能够设计出页面。impress.js没有任何样式。一切都需要你自己动手设计。

impress.js不依赖任何额外的样式。js增加所有展示样式。可在基础之上自定义样式

impress.js用body标签去设置一些有用的样式名字。用来设置浏览器不支持css3效果时候的样式。
第一个有用的样式名字：<code>imress-not-supported</code>。当然浏览器不支持css3特性时候，将会采用这个样式。如果impress.js知道浏览器支持css3，那么将会移除<code>imress-not-supported</code>

核心元素是<code>id="impress"</code>。这个元素不必是个div，所有效果将在ipress内部执行。所有配置都在元素上进行。每一张ppt都需要加样式名字：<code>step</code>。所有样式变化都以为视点中心为中心。

样式step元素的id名字不是必须的，它用来去定义url，如果缺少id，则你热保温ustep-N，url为<code>#/step-22</code>。但是id="step-2"虽然可以用但是不推荐。但是如果一个场景定义id,当播放这个场景时候，则body的样式则为class="impress-on-id"。比如第一个场景的id为"first"，则当播放第一个ppt时候body的样式为class='impress-on-first'。

每个场景都是正常状态，添加的旋转、缩放都是指ppt切换时候的动画。所以将会有三个状态：过去(past)、现在(present)、将来(future)。只有一个元素是当前样式<code>present</code>

<code>data-transition-duration="2000"</code>鸟事动画执行时间为2s。
<code>data-perspective=="500"</code>用来设置perspective。
<code>data-scale="4"</code>表示元素放大四倍。
<code>data-rotate="90"</code>表示元素将会顺时针旋转90度。
<code>data-x/y/z</code>表示x/y/z的偏移。

<code>impress()</code>提供了所有api,比如<code>impress().init()</code>,<code>impress().next()</code>,<code>impress().prev()</code>,<code>impress().goto(idx|id|element,[动画时间])</code>（每次调用impress()不会初始化，更适合的办法是保存impress()到一个变量中，通过变量进行调用。）