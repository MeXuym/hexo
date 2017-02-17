---
title: 实现如masonry的链式语法
date: 2016-11-24 23:47:44
categories: "iOS"
tags: "轮子"
---

之前在项目中有用到过masonry,它让我们很方便的用代码实现来写约束。
它写约束的时候语法如下：

```
[view mas_makeConstraints:^(MASConstraintMaker *make) {
make.edges.equalTo(superview).with.insets(padding);
}];
```
那时候就觉得它这种一路点下去的链式语法好方便。看过它的源码过后，它其实运用了block的一种用法，block充当返回值。

<!--more-->

### block充当返回值

作为方法返回值，内部不能调用，只能外界调用，相当于代替了方法！
我们写一个Human类作为例子，定义和实现一个run方法，返回值是一个block

```
- (void (^)(int))run
{
  return ^(int value){
    NSLog(@"跑了%d米",value);
  };
}
```

那么这个方法我们可以在viewController中这样调用

```
humanObject.run(1); //输出 “跑了1米”
```
好！我们已经实现了链式语法了，已经可以“.run(1)”了。
噗！。。。没用啊！这只能“.run”一次啊，我们要的是“.run(1).run(2).run(3).run(4)…”
好下面我们就看下一直“.run”下去怎么实现。

### block作为返回值实现链式语法

想一下我们在前面写了什么东西的时候可以“.run”（我们前面是创建了一个Human对象，然后 humanObject.run(1)）
当humanObject.run(1)完后我们也能得到一个humanObject的话不就可以继续”.run“了
于是对前面实现的方法做一些改动：

```
//.h文件也要做相应改动
- (Human *(^)(int))run
{
  return ^ (int value){
    NSLog(@"跑了%d米",value); //要执行的操作
    return self;             //这一句是关键
  };
}
```

### 结果

接下来我们就可以一路”.run()“下去了。

```
Human *man = [[Human alloc]init];
man.run(1).run(2).run(3).run(4).run(5);
```

### 输出结果：

```
ChainSyntax[10915:496122] 跑了1米
ChainSyntax[10915:496122] 跑了2米
ChainSyntax[10915:496122] 跑了3米
ChainSyntax[10915:496122] 跑了4米
ChainSyntax[10915:496122] 跑了5米
```

### Demo
github地址：[实现如masonry的链式语法](https://github.com/MeXuym/ChainSyntax)
