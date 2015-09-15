---
layout: post
title: "学习深入理解JavaScript系列（4）-立即调用函数表达式"
categories:
- STUDY
tags:
- STUDY


---
---

**立即调用函数表达式**

任何`function`在执行的时候都会创建执行上下文。相当于自己的私有变量：

<pre>
    function makeCounter () {
        var i = 0;
        return function () {
            console.log(i++);
        }
    }

    var counter1 = makeCounter();
    counter1(); //log : 1;
    counter1(); //log:2

    var counter2 = makeCounter();
    counter2(); //log:1
    counter2(); //log:2

</pre>

counter1与counter2是两个不同的实例 (应该是单例模式)

以下每个也为实例，<strong style='color:red'>静态方法与原型链方法区别</strong>

<pre>
    var aa = function () {
        this.i = 0;
        this.k = 0;
        this.count = function () {
            console.log(this.k++)
        }
    }
    aa.prototype = {
        log : function () {
            console.log(this.i++)
        }
    }
    var a1 = new aa();
    a1.count() // log 0
    a1.count() //log 1
    a1.log(); // log 0
    a1.log();// log 1
    var a2 = new aa1;
    a2.count() // log 0
    a2.count()// log 1
    a2.log(); // log 0
    a2.log();// log 1
</pre>

一般函数表达式需要在哎后面加一个<code>()</code>即可实现自执行。

<pre>
    var foo = function () {}();
</pre>

但是下面函数声明中由于括号只是一个分组操作符，没有包含函数表达式。

<pre>
    function foo(){}()
</pre>

但是如果括号包含有表达式，虽然不报错，但是：

<pre>
    function foo () {}(1)
</pre>

等价于

<pre>
    function foo () {};
    (1)
</pre>

但是当括号包括整个语句即可。可以把函数声明变为表达式

<pre>
    (function (){}()) //推荐
    (function () {})() 
</pre>

自执行还可以：

<pre>
    new function (){} //必须是小写function，不能为Function。此时返回object
    new function (data){console.log(data)}(3) //传参
</pre>

**闭包保存状态**

由于闭包传参的时候，可以lock参数

<pre>
    var eles = document.getElementsByTagName('a');
    for (var i = 0; i < eles.length; i++) {
        eles[i].addEventListener('click', function () {
            e.preventDefault();
            console.log('i is ' + i);
        }, 'false')
    }
</pre>

由于变量i没有进行lock，所以循环以后，点击的时候i才获得值，也就是最后一个。此时需要闭包进行lock。虽然循环完毕，i的值进行变化，但是由于闭包自执行使得i的值lock.

<pre>
    
    var eles = document.getElementsByTagName('a');
    for (var i = 0; i < eles.length; i++) {
        (function (k) {
            eles[k].addEventListener('click', function () {
                e.preventDefault();
                console.log('i is ' + k);
            }, 'false')
        })(i)
        
    }

</pre>

或者

<pre>

    var eles = document.getElementsByTagName('a');
    for (var i = 0; i < eles.length; i++) {
        eles[k].addEventListener('click', (function (k) {
            return function () {
                e.preventDefault();
                console.log('i is ' + k);
            }
        })(i), 'false')
        
    }


</pre>

**自执行匿名函数与匿立即执行的函数表达式**

<pre>
    
    function foo() { foo() } //自己执行自己，自执行函数

    但

    var foo = function () { foo() } //可能是自执行匿名函数 仅仅是 foo 引用自身

    var foo = function () { arguments.callee()} //自己调用自己 但是没有标记名称 是自执行匿名函数


    （ function () {} ()） //立即执行

   

    (function foo () {foo()} ()) // 立即调用自身的函数也可以自执行
    (function () { arguments.callee ()}() ) // 立即调用自身的函数也可以自执行


</pre>

自执行一个应用:Module 模式：

<pre>
    
    var counter = (function () {

        var i = 0;

        rerurn {
            get : function () {
                return i;
            }
        }

    }())

</pre>

