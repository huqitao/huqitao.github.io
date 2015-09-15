---
layout: post
title: "css3动画与变形"
categories:
- STUDY
tags:
- STUDY


---
---



####准备

transform-origin设定元素中心，值为(x,y)，也可以为bottom/center/top/left/right；增加了z轴后，(x,y,z)。

transform-style有两个属性值：flat与preserve-3d。flat为默认值，所有变换为2D效果。preserve-3d为元素在3D空间呈现。没有这个属性，3D变换没有效果，需要设置在父级元素上。

perspective,设置查看者位置，可理解为视距。值越小，越近，元素越大，3D效果越明显。当然值无限大的时候与none一样。perspective-origin设定源点角度。

backface-visibility,值为visible与hidden。决定元素旋转后背面是否可见。默认旋转后背面可见。

####transform与translate

transform是变形，包括旋转(rotate)、扭曲(skew),缩放(scale)、移动(translate)（矩阵不说）。translate是transform的一个属性，是移动。并且skew(x,y)可以写成skewX(x)、skewY(y),translate(x,y)可以写成translateX(x)、transflateY(y)，scale(x,y)可以写成scaleX(x)、scaleY(y).要注意的是，缩放是整个图形的缩放包括边界,当缩放的值为0.5时对应的边界只在firefox上只显示一条，小于0.5的时候则不显示边界（也可能是边界问题）。此处变化都是2D转变。

transform的兼容性：IE10,firefox、opera、chrome、safari支持transform。但是<code>-ms-transform</code>支持ie9的2D转换。其他浏览器则不加浏览器前缀即可。（其实只测了firefox与chrome浏览器）

以上只是2D转换。transfrom还有属性支持3D转换。3D缩放scale3d(x,y,z)与scaleZ(z)，z轴缩放，单独使用没有效果，如3D缩放必须与perspective一起使用才有效果。（transform-style是否设置则对结果没有影响）。没有三维的倾斜。

3D变换的兼容性：IE10没能很好支持。自己用的chrome与firefox，不需要写私有前缀。但是为了保险，建议都3D与2D都写。
<pre>
  -webkit-transform:
  -moz-transform:
  -o-transform:
  -ms-transform:
  transform:
</pre>

####transition设定元素其他样式一定时间内的平滑过渡。[属性，间隔时间，变化速率，延迟时间]。i兼容性上，ie9不支持。

####animation动画。需要先使用keyframes定义关键帧。兼容性问题，只有chrome支持，并且需要加前缀。

此处为[demo](https://github.com/7demo/transformAndAnimation)
