---
layout: post
title: "karma与jasmine学习记录"
categories:
- STUDY
tags:
- STUDY

---

关于karma与jasmine自动化单元测试的文章已经很多,比如<code>http://blog.fens.me/nodejs-karma-jasmine/</code与<code>http://blog.fens.me/nodejs-jasmine-bdd/</code>.本文也主要学习了这两篇文章. 这里主要记录自己学习使用自动化测试的一些东西.

#####Jasmine

Jasmine是BDD模式,即行为驱动开发,这里相对于TDD,即测试驱动开发.

Jasmine使用bower安装失败(可能因为网络原因),便从jasmine在github上项目地址下载的的项目文件. 在READ.md中installation部分有关于引用jasmine的说明.把下载的类库解压,并新建项目文件,把解压的文件中的<code>jasmine-standalone-2.0.0.zip</code>拿到项目根目录并解压.引用如下:

<pre>
<\link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-2.0.0/jasmine_favicon.png">
<\link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-2.0.0/jasmin.css">
<\link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-2.0.0/jasmine.js">
<\link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-2.0.0/jasmine_html.js">
<\link rel="shortcut icon" type="image/png" href="jasmine/lib/jasmine-2.0.0/jasmine_boot.js">

</pre>


网上一些教程会格外写个启动文件,2.0以后则不需要单独写启动文件,只需要引入jasmine_boot.js即可.

一个简单的例子是:
<pre>
describe('A suite of explame',function(){)
it('test',function(){)
expect(true).toEqual(true)
});
})
</pre>
describe可以嵌套

测试结果在页面直接显示.


#####karma

karma可以完成自动化测试.karma-coverage可以进行覆盖率的测试.

karma的安装直接使用<code>npm intsall -g karma</code>.但是win7的系统下使用npm安装完成后,并不能使用karma,并报错"'karma'不是内部或外部命令，也不是可运行的程序或批处理文件".参考<code>http://camnpr.com/software-wiki/1611.html</code>的办法,在cmd安装命令版中查看npm包安装地址(我的地址是:<code>C:\Users\Administrator\AppData\Roaming\npm</code>),打开这个文件夹新建<code>karma.cmd</code>,把以下内容复制到<code>karma.cmd</code>中重新使用npm包安装即可.
<pre>
@IF EXIST "%~dp0\node.exe" (
"%~dp0\node.exe" "%~dp0\node_modules\karma\bin\karma" %*
) ELSE (
node "%~dp0\node_modules\karma\bin\karma" %*
)
</pre>

安装好,使用<code>karma init</code>命令进行配置文件初始化进行参数配置后,重新启用<code>karma start</code>即可.


实验项目文件夹已上传到github.地址是:<code>https://github.com/7demo/karmaAndJasmine.git</code>
