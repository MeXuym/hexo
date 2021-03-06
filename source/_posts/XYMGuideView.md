---
title: XYMGuideView
date: 2016-11-14 23:47:44
categories: "iOS"
tags: "轮子"
---
首次启动打开某个页面的时候弹出的新手引导蒙版，再次进来这个页面后不会再提示。

![XYMGuideView](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMGuideView/XYMGuideView.gif)

## 使用
这其实是一个UIView，在你需要新手引导或者新功能引导的页面导入头文件
```
#import "XYMGuideView.h"
```
<!--more-->

### 创建对象
```
//直接创建不用给它frame(默认就是覆盖整个屏幕)
XYMGuideView *guideView = [[XYMGuideView alloc]init];
```
### 设置标识
设置当前这个引导层的名字标识（一个项目中可能在不同的页面，会用到多个引导层，用这个标识区分开）
```
guideView.guideName = @"hello";
```

### 设置需要焦点区域和引导信息View
设置引导用户关注的区域，提供两种类型，根据需要选择。 一种是圆形区域，调用对象方法 setCircularFocus ，传入GCPoint类型参数为圆形位置，radius为半径。
```
[guideView setCircularFocus:GCPoint radius:int];
```
提示引导信息的View 调用对象方法 addTipsView:传入一个你自己的自定义UIView(通常是一个UIImageView或者一个UILabel)
```
[guideView addTipsView:UIView];
```
### 实现代理方法

如果想要在点击完引导层的时候时候做一些事情，这里提供一个代理方法，得到当前点击的引导层的名字标识

#### 遵守协议
```
@interface RootViewController ()<XYMGuideViewDelegate>
@end
```

#### 设置代理
```
guideView.delegate = self;
@end
```

#### 实现代理方法
拿到当前点击的引导层的标识
```
-(void)XYMGuideClick:(NSString*)guideName{
   NSLog(@"%@",guideName);
}
```

github地址：[XYMGuideView](https://github.com/MeXuym/XYMGuideView)
