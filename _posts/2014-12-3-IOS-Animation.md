---
layout: post
category: IOS
title: IOS Animation
tagline: by BluePandaLi
tags: [animation]
---

该篇学习笔记学习自 objc.io 英文：[Animations Explained](http://www.objc.io/issue-12/animations-explained.html) 中文：[动画解释](http://objccn.io/issue-12-1/)

<!--more-->

###动画原理

我们可以使用UIKit的很多API来完成动画，但Core Animation将会让你更好的理解正在发生什么，它以更明确的方式来描述动画，UIKit的一些高级封装正是使用了这些方式。

我们所使用的UIView,以及 layer-backed 的 NSView，在呈现时动画时是修改它们的 layer 来委托强大的 Core Graphics 来进行渲染的。

但添加一个动画到 layer 时，是不用直接修改它们的属性的。 原因是 Core Graphics 维护了三个平行的层次结构： model layer tree && presentation layer tree && rendering tree

* model layer tree （模型层树）我们能直接看到的 layers 的状态
* presentation layer tree （表示层树）动画正在表现值的近似
* rendering tree （渲染树）

虽然可能不会直接去修改 presentation layer tree ，但是可以用来输出用以保证动画的正确性，另外我们会经常使用它的值创建新的动画。

	-[CALayer presentationLayer]
	-[CALayer modelLayer]
使用上面两个方法可以很轻松的获取两个 layer


###动画种类

*	CABasicAnimation 	 	  	  	从一个值改变到另一个值的动画
*	CAKeyframeAnimation	 			路径动画		
*	CAAnimationGroup 				多个动画组合

###基本动画

最常见的情况是View的一个属性改变为另外一个属性。

![image](http://img.objccn.io/issue-12/rocket-linear.gif)


上面的图片中小火箭 改变的 x.position 这个属性，用以下代码可以实现该属性。

	CABasicAnimation *animation = [CABasicAnimation animation];
	animation.keyPath = @"position.x";
	animation.fromValue = @77;
	animation.toValue = @455;
	animation.duration = 1;

	[rocket.layer addAnimation:animation forKey:@"basic"];

* position.x 是储存在 position 属性中的结构体成员。Core Animation 支持的路径键见此[列表](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Key-ValueCodingExtensions/Key-ValueCodingExtensions.html)
* 该段代码完成之后，火箭将会出现在初始位置。因为在默认情况下，动画不会在超出其持续时间后还修改 presentation layer，并且在结束时候会彻底移除，当移除动画时presentation layer 的值将会回到modle layer 的值。
* 如果想要保持动画结束时的状态有以下两种方法

		//方法1 更改 modle layer的值
		rocket.layer.position = CGPointMake(455, 61);
		
		//方法2 禁止自动移除 并设置fillMode, 不建议使用该方法，动画一直存在会消耗渲染器的性能。
		animation.fillMode = kCAFillModeForward;
		animation.removedOnCompletion = NO;
* 我们创建的动画是可以复用的，animation 在加载到 layer 的时候已经创建了一份拷贝，所以同一个动画实例可以加到不同的 layer之上。


		CABasicAnimation *animation = [CABasicAnimation animation];
		animation.keyPath = @"position.x";
		animation.byValue = @378;
		animation.duration = 1;

		[rocket1.layer addAnimation:animation forKey:@"basic"];
		rocket1.layer.position = CGPointMake(455, 61);

		animation.beginTime = CACurrentMediaTime() + 0.5;

		[rocket2.layer addAnimation:animation forKey:@"basic"];
		rocket2.layer.position = CGPointMake(455, 111);
* byValue 上面代码使用了byValue 其作用是 使用 presentation layer 当前值作为初始值，加上byValue的值后结束。


