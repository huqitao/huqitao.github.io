---
layout: post
title: "学习深入理解JavaScript系列（2）-函数声明与函数表达式"
categories:
- STUDY
tags:
- STUDY


---
---

**函数声明与函数表达式：**

> 二者都是用于创建函数
> 函数声明必须带有标识符，即函数名称；函数表达式则不必带函数名称。
> 判断是否是函数声明和函数表达式，通过上下文：若<code>function foo () {}</code>作为赋值表达式一部分则为函数表达式，若包含在函数体内或者程序最顶部，则为函数声明。

<pre>
    function () {} //函数声明
    var bar = function foo () {}; //表达式，赋值表达式的一部分
    new function bar () ｛｝　//new表达式的一部门
    (function () {
        function () {} //声明，函数体的一部分
    })();
    因为<code>()</code>作为分组操作符，所以(function(){})是函数表达式
</pre>

函数声明会在任何表达式被解析前，会先被解析和求值。由于标准不一，建议使用表达式。

<pre>
    if (true) {
        function foo () {}
    } else {
        function foo () {};
    }
    foo (); //由于标准不一，有可能会返回第一或者第二
</pre>

建议如下：

<pre>
    var foo;
    if (true) {
        foo = function (){};
    } else {
        foo = function () {};
    }
    foo();
</pre>

函数声明只能出现在程序或者函数体内。不能出现在条件语句中。

函数表达式的标识符只在函数内部起作用，函数外不起作用。

<pre>
    var f = function foo () {
        return typeof foo; //此作用域内部有效
    }
    typeof foo //foo此作用域无效，ie8可能有效
</pre>

//函数表达式起名字为了调试方便。方便堆栈调用信息。

**JScript**

>JScript是ie8的js标准。

>函数表达式的标识符泄露到外部作用域
<pre>
    var f = function g(){};
    typeof g; //function  应该是undefined
</pre>
>函数声明不受条件代码块的影响，但是表达式求值不会被影响如
<code>
    var f = function g() {
        return 1;
    }
    if (false) {
        f = function g() {
            return 2;
        }
    }
    g(); //2 
</code>

> 不引用函数标识符。并且在函数内需要显示的对隐式函数进行回收。

<pre>
    var f = (function (){
        var f, g;
        if (true) {
            f = function g(){};
        } else {
            f = function g(){};
        }
        g = null;
        return f;
    })
</pre>
或者

<pre>
    var f = (function () {
        function func(){};
        return func;
    })();
</pre>

>一些低版本的浏览器，函数声明的上一级作用域是继承于Object.prototpe，依次才是上衣作用域。

<pre>
    Object.prototype.x = 'out';
    (function () {
        var x = 'inner';
        (function (){
            alert(x) //outer
        })()
    })();
</pre>
