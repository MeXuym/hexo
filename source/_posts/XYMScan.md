---
title: XYMScan
date: 2016-11-07 23:47:44
categories: "iOS"
tags: "轮子"
---
二维码和条形码扫描，依赖原生AVFoundation框架实现，简单封装

![XYMScan](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMScan/XYMScan.gif)

## 使用
<!--more-->
### 导入头文件
```
#import "XYMScanView.h"
```
### 遵守协议
```
@interface XYMScanViewController ()<XYMScanViewDelegate>
```

### 创建对象

在你要用到的控制器里面创建View（宽高设置为屏幕宽高
```
XYMScanView *scanV = [[XYMScanView alloc]initWithFrame:CGRectMake(0, 0, ScreenWidth, ScreenHeight)];
scanV.delegate = self;
[self.view addSubview:scanV];
```

### 设置扫码框的宽度

设置扫码框的宽度，不设置默认是250（正方形）
```
scanV.scanW = 250;
```

### 实现代理方法

拿到扫码的数据scanDataString
```
-(void)getScanDataString:(NSString*)scanDataString{
}
```

### 对象方法

提供两个对象方法（开始扫码，结束扫码）
```
- (void)startRunning;
- (void)stopRunning;

```

github地址：[XYMScan](https://github.com/MeXuym/XYMScan)
