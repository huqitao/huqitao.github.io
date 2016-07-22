---
layout: post
title:  "git 安装及使用"
categories: Git
---

* content
{:toc}

## Git 安装

### Liunx - ubuntu

 1. 安装 apt-get install git 
 2. 设置用户 git config --global user.name '####'
 3. 设置邮箱 git config --global user.email '###@###.com'
 4. 生成ssh-key ssh-keygen -C ###@##.com -t rsa 然后敲回车，默认密码就为空

### windows

 1. 安装git
 2. 从程序目录打开 “Git Bash”
 3. 设置用户 git config --global user.name '####'
 4. 设置邮箱 git config --global user.email '###@###.com'
 5. 生成ssh-key ssh-keygen -C ###@##.com -t rsa 然后敲回车，默认密码就为空
 6. 进入Administrator/.ssh
 7. 用记事本打开id_rsa.pub文件，复制内容，在github.com的网站上到ssh密钥管理页面，添加新公钥，随便取个名字，内容粘贴刚

## Git 使用

### 常用命令

#### 设置本地目录
git remote add origin ###/##.git

#### 提交代码
git add .   添加全部要提交的文件
git commit -m '提交的描述'   提交代码到暂存区
git push origin master   提交到远程目录

#### 拉取代码
git pull origin master

#### 还原版本
git log 查看提交版本
git reset --hard ####(版本号，commit: 后面的字符串，或者复制github的commit:字符串)
