---
layout: post
title: "学习深入理解JavaScript系列（1）"
categories:
- STUDY
tags:
- STUDY


---
---


**可维护的代码：**
> 可读的；
> 一致的；
> 可预测的；
> 看上去一个人写的；
> 可记录；

其实可以理解为：

>符合统一规范；
>有注释与说明文档；

**变量：**

>全局变量是在任何函外面声明的或者是未声明可以直接简单使用的
><code>this</code>指该环境的全局对象。在浏览器中，全局对象有<code>window</code>属性，但是<code>window</code>指全局对象本身

**减少全局变量：**

>实用<code>var</code>声明；
>命名空间模式
>自执行函数

若在js环境内不加<code>var</code>使用局部变量，则会隐式转成全局变量，特别注意的是实用任务链进行声明时：
`function foo () {`
`var a = b = 0;`
`};`
此时，由于是从右到左进行赋值的，也就是先<code>b = 0</code>，但由于b未声明，则隐式的转变为全局变量（隐式全局变量），最好如下：
`function foo () {`
`var a,b;`
`a = b = 0`
`};`

**隐私全局变量与明确定义全局变量的区别：**
>通过<code>var</code>创建的全局变量是不能被<code>delete</code>删除的；
>隐式全局变量是可以被<code>delete</code>删除的；
;
说明隐式全局变量并不是真正全局变量，它们是全局对象的属性。而属性是可以通过<code>delete</code>删除的，而变量是不能的。具体到执行上：

`var global_var = 1;`
`global_novar = 2`;

执行<code>delete</code>时，返回值也不同，删除真正全局变量返回<code>false</code>，隐式全局变量则返回<code>true</code>。

`delete global_var;  //false`
`delete global_novar; //true`

若此时获取全局变量与隐式全局变量的值，则有所不同。全局变量仍然为<code>1</code>，隐式全局则是<code>undefined</code>。

`typeof global_var; //'number'`
`typeof global_novar; //'undefined'`

<code>'use sctrict'</code>时，使用隐式变量直接报错。

**单<code>var</code>形式与散布<code>var</code>:**

>单一<code>var</code>一般位于代码顶部，易于查询与维护，防止未声明就使用，减少逻辑错误；还可以减少代码
><code>var</code>散布的话，容易因为声明提前（无论在代码哪个地方声明都会被提前至顶部，但赋值仍然在远处）造成问题。

**<code>for</code>循环：**

`for ( var i = 0, max = array.length; i<max; i++ ){`

`}`

`for( var i = 0; i < array.length; i++ ){`

`}`

以上两种，当<code>array</code>为<code>HTMLcollection</code>时，使用第一张更快，因为长度被缓存后，不必每次都去访问。另外<code>i++</code>是不符合JSLint的。建议使用<code>i = i + 1</code>或者<code>i += 1</code>。

因为和0作比较要比和数组长度（非0）更快，所以可以如下:
`var i, array = [];`
`for (i = array.length; i++) {`

`}`
或者
`var array = [], `
    `i = array.length;`
`while (i--){`
    
`}`

**<code>for in</code>循环:**

><code>for in</code>称为“枚举”，用在对象的遍历上。数组用<code>for</code>循环。
>使用<code>for in</code>时候可以使用<code>hasOwnProperty</code>过滤原型链上的属性(只会输出对象的属性值，会过滤掉原型链上的属性)。

`var man = {`
`   hands : 2,`
`   legs : 2,`
`   heads : 1`
`};`

`if (typeof Object.prototype.clone === "undefined") {`
    `Object.prototype.clone = function () {};`
`};`

`for (var i in man) {`
    `if (man.hasOwnPrototype(i)) {`
        `console.log(i,":",man[i]);`
    `};`
`};`
将输出：
`hands : 2`
`legs : 2`
`heads : 1`

或者写成：

`for (var i in man) {`
    `if (Object.prototype.hasOwnPrototype.call(man,i)) {`
        `console.log(i, ":", man[i]);`
    `}`
`};`

或者使用局部变量进行缓存：

`var i, hasOwn = Object.prototype.hasOwnPrototype;`
`for (i in man) {`
    `if (hasOwn.call(man, i)) {`
        `console.log(i, ":", man[i]);`
    `};`
`};`

*<code>for</code>与<code>for in</code>循环与<code>if</code>语句的连写：*
`//警告：通不过jslint的检测`
`var i, hasOwn = Object.prototype.hasOwnProperty;`
`for (var i in man) if (hasOwn.call(man, i)) {`
    `console.log(i, ":", man[i])`
`}`

**不扩展内置原型链:**

>降低维护性，使用<code>for in</code>时易造成混乱

不过当满足两条件时候可以添加:
*未来版本JS添加，现在是提前定义使用；
*文档说明

`if (typeof Object.prototype.myMethod !== "function") {`
`   Object.prototype.myMethod = function () {`
`       //...`
`   }；`
`};`


**<code>switch</code>**

><code>case</code>与<code>switch</code>对齐,<code>case</code>代码缩进；
>以<code>default</code>结尾，即使无匹配
><code>case</code>以<code>break</code>结束,若贯穿，则说明bian

**避免使用隐式类型转换，使用严格模式**

**避免使用<code>eval()</code>：**

><code>eval</code>接受任意字符串。当事先知道代码时候可以使用:

`var property = 'name';`
`alert(obj[property]);`

代替：

`var property = 'name';`
`alert(eval('obj.' + protety));`

>使用<code>eval</code>有安全隐患。特别是处理<code>JSON</code>时，可以使用内置的<code>JSON.parse()</code>，若不支持，则使用<code>JSON.org</code>

同样要避免给<code>setInteral()</code>、<code>setTimeout()</code>、<code>Function()</code>传递字符串。

`//反面例子`
`setTimeout("myFunc()",1000);`
`setTimeout("myFunc(1,2)",1000);`

`//正面例子`
`setTimeout(myFunc,1000);`
`setTimeout(function () {`
`   myFunc(1,2);`()
`},1000);`

<code>Function</code>构造类似<code>eval</code>。如果必须使用<code>eval</code>可以使用<code>new Function</code>。因为，<code>Function</code>实在局部作用域中运行代码，避免影响全局：
`console.log(typeof un);    //undefined`
`console.log(typeof deux);  //undefined`
`console.log(typeof trois); //undefined`

`var jsstring = 'var un = 1; console.log(un);'`
`eval(jsstring); //log:1`

`var jsstring = 'var deux = 2; console.log(deux);'`
`new Function(jsstring)();    //log:2`

`var jsstring = 'var trois = 3; console.log(trois);'`
`(function () {`
`   eval(jsstring);`
`}()};  //log:3`

`console.log(typeof un);    //number`
`console.log(typeof deux);  //undefined`
`console.log(typeof trois); //undefined`

<code>eval</code>可以干扰作用域链。<code>Function()</code>则不(<code>Function()</code>与<code>new Function()</code>是相同的)。

`(function () {`
`   var local = 1;`
`   eval('local = 3; console.log(local)');  //log 3`
`   console.log(local); //log 3`
`}());`

`(function () {`
`   var local = 1;`
`   Function('console.log(typeof local);')(); //log undefined`
`}());`

**<code>parseInt</code>**

>从字符串中获取数值，应该两个参数，第一个为字符串，第二个为数字的进制。特别是ECMA3标准中，0开头的字符串数字被处理成八进制。ECMA5已改变。

`parseInt('08',10);`

**缩进与空格:**

>jslint默认4个空格为缩进。
>标点符号(逗号，分号，属性冒号 操作符)后跟空格（非结尾）;
><code>for</code>循环: <code>for (var i = 0, max = 10; i < max; i+=1) {}/code>
>函数声明：<code>function myfunc() {]</code>;
>匿名函数： <code>var myFunc = function () {}</code>

**命名**

>类型驼峰，方法名小驼峰，常量全部大写
>尾下划线表私有：<code>name_</code>,一个下划线前缀表保护属性，两个下划线属性表私有属性
>前后各有两个下划线表内置<code>__proto__</code>



