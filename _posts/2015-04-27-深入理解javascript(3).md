---
layout: post
title: "学习深入理解JavaScript系列（3）-module模式"
categories:
- STUDY
tags:
- STUDY


---
---

**module模式**

> 模块化->重用，松耦合。

<pre>
    var Cal = function () {
        return {
            add : function (x) {
                //
            }
        }
    }
</pre>

调用：<code>var cal = new Cal()</code>。但是每一次都会复制一份实例。如果只复制一份，需要自执行。

<pre>
    var Cal = (function () {
        return {
            add : function (x) {
                //
            }
        }
    }());
</pre>

可以把外部变量（或全局）传递到匿名函数中。

<pre>
    var Cal = (function ($, w) {
        return {
            add : function (x) {
                //
            }
        }
    }(jQuery, window));
</pre>

*扩展*
<pre>
    var addModule = (function (m) {
        m.add = function () {

        };
        return m;
    }(myModule))
</pre>

若避免引用的时候不存在,需要在引用为空的时候为<code>{}</code>
<pre>
    var addModule = (function (m) {
        return m;
    }(myModule || {}))
</pre>

modlue分割到多个文件后，如果私有方法可以共享，则如下：

<pre>
    var myModule = (function (my) {
        var _private = my._private = my._private || {},
            _seal = my._seal = my._seal || function () {
                delete my._private;
                delete my._seal;
                delete my._unseal;
            },
            _unseal = my._unseal = my._unseal || function () {
                my._private = _private;
                my._seal = _seal;
                my._unseal = _unseal;
            }
    }(mymodule || {}))
</pre>