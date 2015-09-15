---
layout: post
title: "javascript数组去重"
categories:
- STUDY
tags:
- STUDY


---
---



######写在前

之前一直说要做基本功练习的，然而自己却各种懒惰，也没有养成按时写的习惯。前几天做yousi的工具函数库时候，用到数组去重，便去参考了网上一些方法，今天做下整理。

第一个方法是最简单的，也是自己可以完全写出来的。
<pre>
    Array.prototype.unique = function()
    {
        var newArray = [],flag={}; //flag 作为对象，存储元素是否重复的标志----字典查询更快
        for(var i=0;i<this.length;i++)
        {
            if(!flag[this[i]])
            {
                newArray[newArray.length] = this[i];
                flag[this[i]] = true;
            };
        };
        return newArray;
    };
</pre>
使用字典查询速度比遍历数组速快来提升性能，是不是很机智。额，其实是参考别人所写。假如是自己的估计应该是这样的：
<pre>
    Array.prototype.unique = function()
    {
        var newArray = []; 
        for(var i=0;i<this.length;i++)
        {   
            var isHas = false;
            for(var k=0;k<newArray.length;k++)
            {
                if(this[i]==newArray[k])
                {
                    isHas = true;
                    break;
                }
            }
            if(!isHas)
            {
                newArray[this.length] = this[i];
            }
        };
        return newArray;
    };
</pre>
这个就是用和新数组中的所有元素进行比较，如果有就不往新数组里放，性能极差。其实新数组中之前的元素必定不会重复，所以可以改为如下：
<pre>
    Array.prototype.unique = function()
    {
        var newArray = this.concat(); 
        for(var i=0;i<newArray.length;i++)
        {   
            for(var k=i+1;k<newArray.length;k++)
            {
                if(newArray[i]==newArray[k])
                {
                    newArray.splice(i,1);
                }
            }
        };
        return newArray;
    };
</pre>
还是很耗性能。当然如果用indexOf,也可以改成这还样：
<pre>
    Array.prototype.unique = function()
    {
        var newArray = []; 
        for(var i=0;i<this.length;i++)
        {   
            if(newArray.indexOf(this[i])==-1)
            {
                newArray[newArray.length] = this[i];
            }
        };
        return newArray;
    };
</pre>
但是<code>indexOf</code>在ie有兼容性问题。

然后据网上一些高手推荐的方法，先让数组排序，然后比较相邻的元素，会性能有大大的提高：
<pre>
    Array.prototype.unique = function()
    {
        this.sort();
        var newArray=[this[0]];
        for(var i=1;i<this.length;i++)
        {
            if(this[i]!=newArray[newArray.length-1])
            {
                newArray[newArray.length] = this[i];
            }
        };
    };
</pre>

其实还是第一种性能最好，综合来看，最后一种适合用。

参考：

[http://blog.csdn.net/chengxuyuan20100425/article/details/8497277](http://blog.csdn.net/chengxuyuan20100425/article/details/8497277);
[http://www.cnblogs.com/duanhuajian/p/3499993.html](http://www.cnblogs.com/duanhuajian/p/3499993.html);
