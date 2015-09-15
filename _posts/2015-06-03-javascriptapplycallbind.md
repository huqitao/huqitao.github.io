---
layout: post
title: "javascript的call、apply、bind"
categories:
- STUDY
tags:
- STUDY


---

**相同点**

1，都是Function对象原型链的扩展方法，都可以改变把Function对象中的this的指向

**不同点**

1, call(thisObj[, arg1][, arg2]...)

    var foo = {
        name : 'foo1',
        say : function (word1, word2) {
            console.log(this.name, word1, word2)
        }
    }
    var moo = {
        name : 'foo2'
    }
    foo.say.call(moo); // foo2 undefined undefined
    foo.say.call(moo, 'hell', 'wu') // foo2, hell, wu
    
    call把当前对象的this指向第一个参数，后面参数为可选分别为调用方法的参数

2，apply(thiobj[, [arg1[,arg2][,arg3]]]);

    var foo = {
        name : 'foo1',
        say : function (word1, word2) {
            console.log(this.name, word1, word2)
        }
    }
    var moo = {
        name : 'foo2'
    }
    foo.say.apply(moo); // foo2 undefined undefined
    foo.say.apply(moo, ['hell', 'wu']) // foo2, hell, wu
    
call与apply方法用法基本相同，除了传参形式。apply第二个参数为数组，分别魏颖调用发方法的参数。

此外，apply经常把类数组的对象，转成数组：

    var foos = document.getElementByClassName('foo');
    Array.prototype.slice.apply(foos); //foos 已变成数组


3, fun.bind(thisAry[, arg1[, arg2]]);

调用bind的方法会产生一个新函数，指向thisary。与apply和call的区别是bind产生的新方法，可以被变量引用保存到内存中，一直使用。

    var foo = {
        name : 'foo1',
        say : function (word1, word2) {
            console.log(this.name, word1, word2)
        }
    }
    var moo = {
        name : 'foo2'
    }
    foo.say.bind(moo)() // foo2
    foo.say.bind(moo)('hell', 'hah') // foo2, hell ,hah
    var newfoo = foo.say.bind(moo)();
    newfoo('hell', 'hah') // foo2 hell haha
    
当然，bind是ESCM5后的方法，所以IE8不支持：

    if (!Function.prototype.bind) { //若浏览器不支持bind方法
        Function.prototype.bind = function (oThis) { //oThis 是bind所传参数，包括this指示对象以及绑定方法所需参数
            if (typeof this !== 'function') { //this指的是bind所绑定的方法，若不是函数则抛出错误
                throw new TypeError('绑定方法不是一个函数');
            }
            
            var aArgs = Arrayprototype.slice.call(arguments,1),
            //通过slice方法截取bind参数索引为1开始一直到最后的参数，即除了this后面参数。arguments 指 bind所传参数
            fTobind = this, //绑定的函数
            fNOP = function (){}, //空的函数，
            fBound = function () {
                return fToBind.apply(this instanceof fNOP && oThis ? this : oThis || window, aArgs.concat(Array.prototype.slice.call(arguments)));//this 是绑定的函数， fNOP作为空的函数，this instanceof fNOP会返回false,所以this instanceof fNOP && oThis ? this : oThis || window只会返回 oThis ，若oThis为空的时候，返回window。aArgs.concat(Array.prototype.slice.call(arguments)))返回 bind 绑定返回的方法运行时候所传的参数。 这样 整个fBound 返回oThis的第一个对象直接调用方法。
            }
            fNOp.prototype = this.prototype; //复制要绑定的函数到fNoP函数的原型链
            fBound.prototype = new fNOP(); 
            return fBound;
        }
    }
