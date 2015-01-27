---
layout: post
category: IOS
title: IOS Animation
tagline: by BluePandaLi
tags: [animation]
---

该文章学习自 objc.io 英文：[Animations Explained](http://www.objc.io/issue-12/animations-explained.html) 中文：[动画解释](http://objccn.io/issue-12-1/)

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
opacity

上面的图片中小火箭 改变的 x.position 这个属性，用以下代码可以实现该属性。

	CABasicAnimation *animation = [CABasicAnimation animation];
	animation.keyPath = @"position.x";
	animation.fromValue = @77;
	animation.toValue = @455;
	animation.duration = 1;

	[rocket.layer addAnimation:animation forKey:@"basic"];
	


