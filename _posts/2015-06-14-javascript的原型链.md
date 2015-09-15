---
layout: post
title: "JavaScript原型与原型链"
categories:
- STUDY
tags:
- STUDY


---

**constructor**

<code>constructor</code>的值是一个函数。除了<code>null</code>、<code>undefined</code>以外的值、数组、函数、对象都有<code>constructor</code>属性，<code>constructor</code>的值是这个值、数组、函数或者对象的构造函数。

调用构造函数，必须使用<code>new</code>。

    var a = 12;    a.constructor//Number(),不能直接12.constructor
    'str'.constructor //String()
    false.constructor //Boolean()
    var a = {name : 'li'}; a.constructor // Object()
    var a = [1, 'd']; a,constructor // Array()
    var a = function () {}; a.constructor //Function()
    var A = function () {};
    var a = new A();
    a.constructor //A();
    
但是所有的构造函数的constructor都是Function
    
**prototype**

<code>prototype</code>是函数的一个属性。通过构造函数构造出来的函数是没有此属性的。一个函数的默认<code>prototype</code>的属性值是一个与函数同名的空对象。匿名函数的<code>prototype</code>的值为<code>object</code>:

    var a = function () {this.name='3'};
    a.prototype // a {}

    var A = function () {};
    A.prototype.show = function () {};
    
    var B = function () {};
    B.prototype = A.prototype;
    
    var test = new B();
    test.constructor // A
    
因为<code>B.prototype = A.prototype;</code>把B.prototype的指向A的原型链。即A与B共同继承同一个原型链（此时，B与A共享一个原型链，但是B的实例不能访问A中声明的属性值。比如test不能访问name属性）。若只用一般的<code>B.prototype.constructor = B;</code>修正也不行（因为此时改变了B也就行改变了A）：

    var a = function () {};
    a.prototype // a {}

    var A = function () {};
    A.prototype.show = function () {};
    
    var B = function () {};
    B.prototype = A.prototype;
    B.prototype.constructor = B;
    
    var test = new B();
    test.constructor // B
    
    var testa = new A();
    testa.constructor // B 此时因为 A与B共同的原型链的构造函数 被指向了B

所以一个方法B继承A的方法为：

    var a = function () {};
    a.prototype // a {}

    var A = function () {};
    A.prototype.show = function () {};
    
    var B = function () {};
    B.prototype = new A();
    B.prototype.constructor = B;
    
    var test = new B();
    test.constructor // B
    
    var testa = new A();
    testa.constructor // B
    

<code>new A()<code>是A的实例，这叫做a。即为：B.prototype = a，而a.__proto__指向A的原型链（A.prototype）。a作为一个对象，只继承原型链不会修改。 prototype的值是一个对象，它的构造函数也就是它的constructor属性的值就是自己函数。

    A.prototype.constroctor === A //true

即 构造函数的prototype的特殊属性constructor的初始默认值为该函数。

不过往prototype添加属性与修改prototype是不一样的：

    var A = function () {};
    A.prototype.method1 = function () {};
    var a = new A();
    
    a.construtor === A //true
    
    var B = function () {};
    B.prototype = {
        method1 : function (){}
    }
    var b = new B();
    
    b.constructor === B //false

这是因为覆盖prototype时，相当于如下：

    B.prototype = new Object({
        method1:function (){};
    })
    
此时由于constructor一直指向自身的构造函数：

    B.prototype.constructor === Object

所以需要指向自己进行修正。

//再次解读：
B.prototype = {
        method1 : function (){}
}相当于new object(method1 : function (){}),这里称作obj。obj的contractor为Object。肯定与{method1 : function (){}}不等。

**__proto__**

<code>__proto__</code>属于对象的内部原型。<code>prototype</code>构造器的原型。

所有构造器的__proto__指向Function.prototype, 所有对象的__proto__都指向构造器的prototype。

    Number.__proto__ === Function.prototype;
    Boolean.__proto__ === Function.prototype;
    String.__proto__ === Function.prototype;
    Object.__proto__ === Function.prototype;
    Function.__proto__ === Function.prototype;
    Array.__proto__ === Function.prototype;
    RegExp.__proto__ === Function.prototype;
    Error.__proto__ === Function.prototype;
    Date.__proto__ === Function.prototype;
    var Person = function (){};
    Person.__proto__ === Function.prototype;
    
只有Function.prototype的typeof Function.prototype为function,其他的typeof **.prototype 为Object。

Function.prototype 为 'function Empty()'

Function.prototype.__proto__ === Object.prototype。而Object.prototype.__proto__ === null


**总结**

每一对象都有属性，使用__proto__来标记，原型是一个对象的引用或null(Object.prototype.__proto__ === null)

对象的原型来自构造函数的原型属性。

另外需要总结下，Object与Function的关系及操作。
    