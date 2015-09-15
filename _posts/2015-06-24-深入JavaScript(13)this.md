---
layout: post
title: "深入理解JavaScript系列（13）this"
categories:
- STUDY
tags:
- STUDY


---

this是执行上下文的一个属性。在进入上下文时确定，并在上下文运行期间保持不变。

**全局代码中的this**

在全局代码中，this就是全局对象本身。

**函数代码中的this**

在函数中this虽然是在进入上下文确定，但是每一次调用就不同。影响this的值：

 - 函数的上下文
 - 不同的调用方式。如：

    function foo () {console.log(this)};
    foo() //global
    foo.prottotype.constructor() // foo.prototype
    //<strong>需要注意</strong>
    
    /**
    * 著名例子
    **/
    var foo = {
        bar : function (){console.log(this)};
    }
    foo.bar() // bar
    var newfoo = foo.bar;
    newfoo() //global
    
***引用类型（Reference type）**

引用类型的值是可以表示为拥有两个属性的对象----base（即拥有属性的对象）:

    var foo  = 10;
    var fooReference = {
        base : 'global',
        propertyName : 'foo'
    }
    即为global
    
在函数上下文中红，如果调用函数的括号的左边是引用类型的值，那么this将为引用类型的值，否则为null,此时将隐式的转为 全局对象。但是在ＥＣＭＡ５中，不再隐式的转，而为undefined。

    function foo () {
        return this;
    }
    foo() //global 其实是一个引用类型，括号左边的foo是一个标识符。base也为全局对象。
    
    var foo = {
        bar : function () {
            return this;
        }
    }
    
    foo.bar(); //foo,. bar是foo的属性，那么base就是foo
    
    var test = foo.bar;
    test(); //global test生成新的标识符。
    
    <strong style='color:red'>以后解决地方：</strong>
    以下伪代码获得引用对象的值：
    
    function GetValue (value) {
        if (TYpe(value) != Reference) {
            return value;
        }
        var base = GetBase(value);
        if (base === null) {
            throw new ReferenceError
        }
        
        return base.[[Get]](GetPropertyName(value));//[Get]](GetPropertyName(value)   返回对象属性真正的值-
    } 
    
    自己的理解为，当执行运算时候（非引用于括号运算），则自动运行GetValue方法，此时会返回函数而不是引用的对象。那么则为全局对象（或不准确），如：
    
    var foo = {
        bar : function (){return this};
    }
    (foo.bar = foo.bar)(); //global  运算符
    (false || foo.bar)(); //非运算 global
    (foo.bar, foo.bar)(); //逗号运算 global
    
**非引用类型与函数调用**

此时设定为ull, 在ecma之前为全局对象。

    (function (){
        console.log(this); //null->global
    })
    
更多例子：

    var foo = {
        bar : function () {console.log(this)};
    }
    foo.bar(); // foo
    (foo.bar)(); //foo  组运算符不适用。

特殊情况：当调用表达式限定call括号左边的值，this虽未null但是会被隐式转为global:

    function foo (){
        function bar () {
            return this;
        }
        bar();
    }
    foo(); // global

结合上诉例子，理解应该是：foo作为活动对象返回null，这个即被当做this（活动对象总是返回this）,因此相当于null.bar(),隐式转为global.bar();

但是例外的是with语句：
如果with对象包含函数名属性，在with语句调用函数时，with语句添加到该对象作用域的最前面（活动对象的前面）。base对象不再是活动对象，而是with语句的对象。（理解困难，但是例子简单)

    var x = 10;
    with({
        foo : function (){
            console.log(this.x)
        },
        x : 20
    }) {
        foo(); //20
    }

还有catch语句：

    try {
        throw function () {
            return this
        }
    } catch (e) {
        e();  //ES3 的标准是 _catchObject  ES5 是global
    }
    
递归函数也是(递归调用中，base是父对象，之后才是特定对象)：

    (function foo (bar) {
        console.log(this); // global而不是foo
        !bar && foo(1)
    })();
    
构造函数的this：

function A () {
    this.x = 10;
}
var a = new A();
a.x // 10;  this指向新创建对象

此外还有apply/call/bind 的this。这之前的文章已经写过。
    