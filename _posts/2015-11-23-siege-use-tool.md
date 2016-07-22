---
layout:  post
title:   "压力测试工具siege的用法"
date:    2015-11-23 19:40:00
categories: Tools
---
* content
{:toc}

Siege是linux下的一个web系统的压力测试工具，支持多链接，支持get和post请求，可以对web系统进行多并发下持续请求的压力测试。

## 安装 Siege

    apt-get install siege 
    
## Siege命令常用参数

-c 200 指定并发数200

-r 5 指定测试的次数5

-f urls.txt 制定url的文件

-i internet系统，随机发送url

-b 请求无需等待 delay=0

-t 5 持续测试5分钟

-r和-t一般不同时使用

##常用的siege命令举例

200个并发对http://www.google.com发送请求100次

    siege -c 200 -r 100 http://www.google.com

在urls.txt中列出所有的网址

    siege -c 200 -r 100 -f urls.txt

随机选取urls.txt中列出所有的网址

    siege -c 200 -r 100 -f urls.txt -i

delay=0，更准确的压力测试，而不是功能测试

    siege -c 200 -r 100 -f urls.txt -i -b

指定http请求头 文档类型

    siege -H &quot;Content-Type:application/json&quot; -c 200 -r 100 -f urls.txt -i -b

## Siege输出结果说明

Transactions: 总共测试次数

Availability: 成功次数百分比

Elapsed time: 总共耗时多少秒

Data transferred: 总共数据传输

Response time: 等到响应耗时

Transaction rate: 平均每秒处理请求数

Throughput: 吞吐率

Concurrency: 最高并发

Successful transactions: 成功的请求数

Failed transactions: 失败的请求数

## Siege使用的一些总结

1，发送post请求时，url格式为：http://www.xxxx.com/ POST p1=v1&p2=v2

2，如果url中含有空格和中文，要先进行url编码，否则siege发送的请求url不准确

**引用自:**[不争.倔强的活着](http://buzheng.org/blog/usage-of-siege.html)