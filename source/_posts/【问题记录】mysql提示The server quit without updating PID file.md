---
title: 【问题记录】mysql提示The server quit without updating PID
date: 2017-01-16 11:41:28
categories: "mysql"
tags: ["mysql","问题记录"]
---

今天想在自己mac下的mysql建个表测试下python脚本，发现我的mysql启动不了。
不是和之前一样start然后提示success,而是Starting MySQL...。
一段时间后就提示：The server quit without updating PID file
根据它的提示，我修改了这个文件的读写权限，然后start,报错依旧。
google娘说问题可能有很多种：
<!--more-->

### 1.可能是/usr/local/mysql/data/xxx.pid文件没有写的权限
只是我一开始尝试办法，但是没有效果。可以尝试执行
```
chown -R mysql:mysql /usr/local/mysql/data
chmod -R 755 /usr/local/mysql/data
```
然后重新启动


### 2.可能已经存在mysql进程
我的问题是这个，已经有mysql进程（该不会是我前几天start了mysql一直没关吧！）
ps -ef|grep mysqld
查看是否有mysqld进程，有的话kill掉
kill -9 进程号


### 3.可能是二次安装mysql。
可能是第二次在机器上安装mysql，有残余数据影响了服务的启动。
解决方法：去mysql的数据目录/data看看，如果存在mysql-bin.index，删除掉。


### 4.mysql在启动时没有指定配置文件
mysql在启动时没有指定配置文件时会使用/etc/my.cnf配置文件，请打开这个文件查看在[mysqld]节下有没有指定数据目录(datadir)。
解决方法：请在[mysqld]下设置这一行：datadir = /usr/local/mysql/data


### 5.skip-federated字段问题
解决方法：检查一下/etc/my.cnf文件中有没有没被注释掉的skip-federated字段，如果有就立即注释掉吧。


### 6.错误日志目录不存在
解决方法：使用“chown” “chmod”命令赋予mysql所有者及权限

参考链接：[mysql提示The server quit without updating PID file](http://blog.rekfan.com/articles/186.html)
