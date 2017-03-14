---
title: mac下修改mysql的root的密码
date: 2017-01-11 11:41:28
categories: "mysql"
tags: ["mysql"]
---

最近因为项目需要，想在自己mac上用mysql测试下脚本。结果发现吧root的密码忘记（太久之前装的mysql,取得也不是常用密码）。
怎么办？改密码呗。

### Mac OS X - 重置 MySQL Root 密码

1.停止 mysql server.  通常是在 '系统偏好设置' > MySQL > 'Stop MySQL Server'
  或者通过命令停止 sudo /usr/local/mysql/support-files/mysql.server stop

2.终端输入：sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
<!--more-->
3.终端新建窗口：
  sudo /usr/local/mysql/bin/mysql -u root
  然后输入重设密码的命令：
  UPDATE mysql.user SET authentication_string=PASSWORD('新密码') WHERE User='root';
  FLUSH PRIVILEGES;
  \q

4.重启MySQL
  '系统偏好设置' > MySQL > 'Star MySQL Server'
  或者 sudo /usr/local/mysql/support-files/mysql.server start

5.我的mysql版本是5.7.17，旧版的mysql的UPDATE可能要这样写
  UPDATE mysql.user SET Password=PASSWORD('新密码') WHERE User='root';
