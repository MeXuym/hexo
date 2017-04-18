---
title: 快速Xcode缺少真机调包
date: 2017-04-18 14:59:53
categories: "Xcode"
tags: ["Xcode","问题记录"]
---

前些天有一些问题需要真机调试，因为一些原因只有一台测试机。它是iOS10.2.1，我的是Xcode8.1,没有这一个的真机调包。我们知道这时候可以升级下Xcode就可以真机调试新的iOS真机。有没有不升级Xcode又可以快速真机调试的办法？

### 1.找对应的真机调试包
比如当Xcode提示：This iPhone 6s is running iOS 10.2.1(14D27), which may not be supported by this version of Xcode. 的时候。根据提示网上找 iOS 10.2.1(14D27) 的真机调试包，然后解压放到这个目录下面：/Applications/Xcode8.1.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport 。重启一下Xcode,等待它一下，这时候一般都可以了。
<!--more-->
### 2.找不到真机调试包（不想升级Xcode）
有时候会有网上中不到对应真机测试包的情况，比如我找不到 iOS 10.2.1(14D27) 的真机调试包，也还是有办法能够真机调试。
![XYMScan](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMScan/XYMScan.gif)
如图来到这个目录原本我只有IPhoneOS10.1.sdk,我们复制一份然后改名IPhoneOS10.2.sdk。

进入到IPhoneOS10.2.sdk，看到里面有一个SDKSettings.plist文件。
![XYMScan](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMScan/XYMScan.gif)

修改里面所有包含版本的值为iOS 10.2（原本都是iOS 10.1的）
![XYMScan](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMScan/XYMScan.gif)

然后来到DeviceSupport里面，复制一份原本存在的 10.1(14B72) 改名为 10.2.1(14D27) 即前面Xcode提示信息中的缺少的真机调试包名。
![XYMScan](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/XYMScan/XYMScan.gif)

然后重启Xcode,连接真机，等待Xcode，然后运行。这个做法可能会引发一些未知的bug,毕竟不是真的对应的真机调试包，最好还是升级Xcode。

