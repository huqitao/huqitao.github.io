---
layout: post
title: "jqueryMobile与fis-amd"
categories:
- STUDY
tags:
- STUDY


---
 1. <code>mobileinit</code>无效解决办法。
    由于mobileinit事件是需要再jquery-mobile.js载入前进行绑定，而是用fis-amd后，requirejs对文件中<code>require</code>进行了提前解析，所以
    <code>requre('/lib/jquery.mobile.js')</code>
    与
    `$(document).on(mobileinit, function () { 
        //code
    })`
    不可在同一个启动页，否则则无效。要解决的话，需要把<code>mobileinit</code>的绑定单独写成一个模块，在需要的地方进行引用：

    //无效，即使使用settimeout 包require('/lib/jquery.mobile')
    `define(function (require){
        //引入jquery与jquerymobile
        var $ = require('jquery');
        $(document).on('mobileinit', function () {
            console.log('折腾')
        });
        require('/lib/components/jquerymobile/jquery.mobile-1.4.5.min.js');
    })`
    
    //以下为解决办法：init.js为绑定mobileinit的模块
    //init.js
    define(function (require){
        var $ = require('jquery');
        $(document).on('mobileinit', function () {
            console.log('折腾')
        });
    })
    //app.js
    //引入jquery与jquerymobile
    var $ = require('jquery');
    require('/lib/js/web/init.js');
    require('/lib/components/jquerymobile/jquery.mobile-1.4.5.min.js');