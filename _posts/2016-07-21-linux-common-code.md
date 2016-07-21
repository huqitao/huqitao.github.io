---
layout: post
title:  "linux 常用命令"
categories: Linux
---

* content
{:toc}

## 基本命令

 - ls 查看文件
 - ls -l 查看文件列表
 - rm -rf ### 删除某个文件/文件夹
 - chmod 给文件/文件夹权限    文件夹内全部文件 -R
 - mkdir ## 创建文件/文件夹

---

## 查看空间

 - df -h 查看当前系统空间
 - df -i 查看系统inodes使用情况 
 - du -h 查看当前目录使用情况 （做文件轮询）
 - du -sh 查看当前目录使用情况（只看结果，不做文件轮询）

 ---
 
## 搜索进程

###ps -aux | grep python 查询指定应用进程


	root@ubuntu:/# ps -aux | grep python
	root      7911  0.0  2.8 362524 115888 ?       Sl   03:17   0:02 /usr/bin/python /usr/bin/ubuntu-kylin-software-center-daemon
	huqitao   7940  0.0  1.1 628456 45124 ?        Sl   03:17   0:00 python indicator-china-weather.py
	root      9644  0.0  0.0  15988   988 pts/4    S+   04:53   0:00 grep --color=auto python


###netstat -anp | grep 4000 查看制定端口


       	root@ubuntu:/# netstat -anp | grep 4000
        tcp        0      0 127.0.0.1:4000          0.0.0.0:*               LISTEN      9728/ruby       

---

## 结束进程

 - kill 4000 结束端口为4000的进程
 - kill -s 9 4000 强制结束端口为4000的进程
 - killall php-cgi 结束全部 php-cgi的程序
 - pkill -9 php-cgi 强制结束 php-cgi 的程序
