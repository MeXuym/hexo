---
title: python定时任务
date: 2017-03-29 15:36:48
categories: "Python"
tags: ["Python","轮子"]
---

最近都在用python做各种各样的事情帮运营的妹子减轻工作量。
微信运营群的管理和数据统计，操作excel的各种小脚本。
最近则是希望能定时每天往微信运营群里发运营消息，然后就有了这个小脚本。
<!--more-->

### 逻辑结构
大概的思路是用while来当做一个runloop，然后里面的每一次循环都间隔一段时间（now到所定时间的时间差）达到定时的目的。
```
def func():
<!--more-->
    while True:
        # 每一次的循环操作开始前计算sleep时间
        # 代码 #

        time.sleep(delta.seconds)
       
        # 要定时执行的操作
        # 代码 #
```

### 每日定时计算
使用python的datetime.timedelta来得到明天要定时的时间，得到的时间与当前时间相减就是sleep的时间。
计算sleep时间
```
# 假定明天开始，每天9点执行任务
tomorrow = datetime.datetime.replace(datetime.datetime.now() + datetime.timedelta(days=1), 
hour=9, minute=0, second=0)
delta = tomorrow - datetime.datetime.now()
```

### 子线程上执行这个操作

```
import threading
t = threading.Thread(target=func)
t.start()
```
