---
title: python监控exe小脚本
date: 2017-04-06 16:07:04
categories: "Python"
tags: ["Python","轮子"]
---

运营在使用微信的机器人程序来帮助运营微信群，机器人程序是windows下的exe程序。机器人程序不是很稳定，是不是会崩溃挂掉，然后，就没有然后了。就想着要不自己写个小脚本来监控exe程序，如果挂掉了就把它起起来。

### 大概思路
每隔一段时间拿到tasklist然后遍历它，没有匹配到，就说明这个exe挂掉了，然后用os.system()把它起起来。
<!--more-->
### 检查某个exe
取得是'.exe'前面的名字字符串传给这个函数检查
```
def checkkey(data):
    #tasklist也可换成linux下打印所有进程的命令 ps aux
    for line in os.popen('tasklist').readlines():
        #判断用正则更准确，也可以使用find或者index判断
        pattern = re.compile(r'%s.*'% data, re.I)
        match = pattern.match(line)
        if(match):
            return True
    return False
```

### 监控多个exe
我自己弄得一个测试的字典 dic{exe名 : exe路径, exe名 : exe路径}，然后遍历这个字典来监控里面的每一个exe。
```
def checkExe():
    for key in dic:
        if(checkkey(key) == False):
            print '%s : error' % key
            os.system(dic[key])
        else:
            print '%s : running' % key
            flag = 0
            break
```

### 定时检查一下

```
def runloop():
    while True:
        print u'开始监控'
        time.sleep(150)
        checkExe()
        if(flag==1):
           # email或者短信提醒
           print u'有exe挂掉了'
        time.sleep(150)
```

### 开线程跑函数
```
import threading
t = threading.Thread(target = runloop)
t.start()
```
### 上代码
下面是一份py测试代码，配置一下前面的测试字典dic就好，希望对你有用（逃
```
#coding=utf-8
import re
import os
import time
import urllib2
import urllib
import subprocess

flag = 1

# 组装自己用的测试字典（路径中夹杂中文有坑）
cStr = u'测试'
path = 'C:/Users/admin/Desktop/' + cStr + '/qunxiaoxi1.exe'
path = path.encode('gbk')
jiqiren = 'C:/Users/admin/Desktop/jiqiren/jiqiren2.1.94/jiqiren2.1.94.exe'
dic = {'qunxiaoxi1':path, 'jiqiren2.1.94':jiqiren}

# 看有没在运行
def checkkey(data):
    #tasklist也可换成linux下打印所有进程的命令 ps aux
    for line in os.popen('tasklist').readlines():
        #判断用正则更准确，也可以使用find或者index判断
        pattern = re.compile(r'%s.*'% data, re.I)
        match = pattern.match(line)
        if(match):
            return True
    return False

# 遍历要监控的字典
def checkExe():
    for key in dic:
        if(checkkey(key) == False):
            print '%s : error' % key
            os.system(dic[key])
        else:
            print '%s : running' % key
            flag = 0
            break

# 隔一段时间检查一下
def runloop():
    while True:
        print u'开始监控'
        time.sleep(150)
        checkExe()
        if(flag==1):
           # email或者短信提醒
           print u'有exe挂掉了'
        time.sleep(150)

# 开线程来执行
import threading
t = threading.Thread(target = runloop)
t.start()
```

