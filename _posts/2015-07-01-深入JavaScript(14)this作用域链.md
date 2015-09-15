---
layout: post
title: "深入理解JavaScript系列（14）作用域链"
categories:
- STUDY
tags:
- STUDY


---

每个上下文拥有自己的变量对象：对于全局上下文，它是全局对象自身；对于函数，它是活动对象。

函数上下文的作用域链在函数调用时创建的，包含活动对象与函数内部[[scope]]属性。

    Scope = AO + [[scope]];//活动对象是作用域数组的第一个对象
    
上下文中的局部变量相对父作用域变量拥有较高的优先级。另外函数的[[scope]]创建后是持续存在直至销毁。

    var x = 10;
    function foo () { //foo的[[scope]]持续存在，包含有x=10的属性，因此无论如何被引用，不会改变。
        console.log(x);
    }
    (function () {
        var x = 20;
        foo ();  //所以为10
    })();
    
**构建函数的[[Scope]]**

函数构造函数的[[Scope]]为全局对象：

    var x = 10;
    function foo () {
        var y = 10;
        var bar = Function ('alert(x); alert(y)');
        bar(); // x 10; y undefined
    }
    
**二维作用域链查找**

对象中属性的查找，如对象没有直接找到，那么将会查找原型链，这就是二维查找：1，作用域链；2，每个作用域链--深入到原型链环节（即先查作用域链再查原型）。

    function foo () {
        console.log(x);
    }
    
    Object.prototype.x = 10;
    
    foo(); // 10
    
    var x = 10;
    Object.prototype.x = 20;
    function foo () {console.log(x)}; // 10;
    
**eval上下文中的作用域链**

代码eval的上下文与当前的调用上下文（calling context）拥有相同的作用域链。

**代码执行阶段作用域链的影响（with/catch）**

    var x = 10;
    with ({x :20}) {
        console.log(x); // 20
    }
    console.log(x); // 10;
    
1,x = 10;
2,对象{x:20}添加作用域前端；
3，如果有var的声明，进行解析；
4，with声明完成后，它的特定对象从作用域链中移除（x:20）。回到以前的状态

    var x = 10, y = 10;
    with ({x :20}) {
        var x = 30; var y  = 30;
        console.log(x); // 30
    }
    console.log(x); // 10;
    console.log(y); // 10


同样，catch语句的异常参数变得可以访问，创建只有一个属性的新对象，异常参数名。语句运行完成后，作用域链恢复到以前状态。

    var e = 10;
    try{
    
    } catch (e) {
        ///
    }