---
title: pushAnimation
date: 2016-12-01 23:47:44
categories: "iOS"
tags: "动画"
---
在之前的项目中，应产品的要求，需要实现自定义的push和pop动画。最近比较有空，于是把这一部分抽出来

![pushAnimation](https://raw.githubusercontent.com/MeXuym/hexoSource/master/_posts/pushAnimation/push.gif)

## 要做的事情
1.我们会自定义一个自己的动画类
2.自定义自己的一个NavigationController（要使用自定义的push动画，就继承这个类）
<!--more-->

## 自定义动画类

我们先实现一个push自定义动画类姑且叫做XYMAnimation，它实现UIViewControllerAnimatedTransitioning协议，用来定义一个非交互动画（就是动画过程中没交互）

### 遵守协议

动画类.h文件遵守协议。还有两个属性分别表示当前的navigationController，和动画的类型（push或者pop)。


```
#define ScreenWidth [UIScreen mainScreen].bounds.size.width
#define ScreenHeight [UIScreen mainScreen].bounds.size.height

#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>

@interface XYMAnimation : NSObject<UIViewControllerAnimatedTransitioning>

@property(nonatomic,assign)UINavigationControllerOperation  navigationOperation;
@property(nonatomic,weak)UINavigationController * navigationController;

@end
```
### .m文件中实现两个协议方法

动画持续的时间。
```
- (NSTimeInterval)transitionDuration:(id<UIViewControllerContextTransitioning>)transitionContext
{
  return 3.0;
}
```

动画的内容，动画怎么做完全取决于你，下面是我写的平移动画，你还可以缩放和旋转动画。

```
- (void)animateTransition:(id<UIViewControllerContextTransitioning>)transitionContext
{
  //目的ViewController
  UIViewController *toViewController = [transitionContext viewControllerForKey:UITransitionContextToViewControllerKey];

  //起始ViewController
  UIViewController *fromViewController = [transitionContext viewControllerForKey:UITransitionContextFromViewControllerKey];

  //添加toView到上下文
  [[transitionContext containerView] insertSubview:toViewController.view belowSubview:fromViewController.view];

  //自定义动画
  toViewController.view.transform = CGAffineTransformMakeTranslation(320, 568);
  [UIView animateWithDuration:[self transitionDuration:transitionContext] animations:^{

    fromViewController.view.transform = CGAffineTransformMakeTranslation(-320, -568);
    toViewController.view.transform = CGAffineTransformIdentity;

  } completion:^(BOOL finished) {

    fromViewController.view.transform = CGAffineTransformIdentity;
    // 声明过渡结束时调用 completeTransition: 这个方法
   [transitionContext completeTransition:![transitionContext transitionWasCancelled]];
  }];
}
```

## 自定义NavigationController

继承自UINavigationController类，遵守 UINavigationControllerDelegate 协议。
XYMPushAnmation就是前面自定义的动画类，这里要用到
```
@interface RootViewController ()<UINavigationControllerDelegate>

@property (nonatomic,strong)XYMPushAnmation *xymPushAnmation;

@end
```

### .m文件中创建自定义动画实例

```
- (XYMPushAnmation *)xymPushAnmation
{
  if (_xymPushAnmation == nil) {
     _xymPushAnmation = [[XYMPushAnmation alloc]init];
  }
  return _xymPushAnmation;
}

- (void)viewDidLoad {

  self.delegate = self;
  _xymPushAnmation = [[XYMPushAnmation alloc] init];
}
```

### 关键点：实现这一个代理方法，使用我们自定义的动画

```
//这个代理方法里面使用我们的自定义的动画
- (nullable id <UIViewControllerAnimatedTransitioning>)navigationController:(UINavigationController *)navigationController
animationControllerForOperation:(UINavigationControllerOperation)operation
fromViewController:(UIViewController *)fromVC
toViewController:(UIViewController *)toVC  NS_AVAILABLE_IOS(7_0)
{
  self.xymPushAnmation.navigationController = self;      //当前这个navigationController使用这个动画
  self.xymPushAnmation.navigationOperation = operation;  //operation可以知道是在做push还是pop
  return self.xymPushAnmation;                           //使用我们的自定义动画
}
```

## Demo

Demo：[github地址](https://github.com/MeXuym/PushPopAnimation)
