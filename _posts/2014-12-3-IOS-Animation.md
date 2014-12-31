---
layout: post
category: IOS
title: IOS Animation
tagline: by BluePandaLi
tags: [animation]
---

在IOS中实现动画的方式有 **Core Animation**，IOS7之后的 **UIKit Dynamics** 物理模拟框架，以及FaceBook开源的 **Pop**[动画引擎](https://github.com/facebook/pop)。

<!--more-->

#UIView 和 CALayer的关系

首先我们在试验项目中写下 UIView , Command + 鼠标左键 查看其定义  

![](http://bluepanda-blog.qiniudn.com/blogCoreAnimationDemo_UIView.png)   

查看CALayer的定义
  
![](http://bluepanda-blog.qiniudn.com/blogCoreAnimationDemo_CALayer.png)  
 
根据以上定义我们能得出如下结论：
 
*	UIView属于UIKit，CALayer属于QuarterCores。说明UIView侧重于界面逻辑，CALayer侧重于界面渲染。 
*	UIView继承自UIResponder，CALayer继承自NSObject。说明只有UIView才能处理事件。
*   UIView包含了CALayer属性，在注释中我们看到这样一句注释UIView是UILayer的代理。可以推测UIView是一个处理事件的、能够呈现CALayer的特殊实现。

#View-Layer协作

###逻辑关系
* 在 iOS 中，所有的 view 都是由一个底层的 layer 来驱动的
* 在 iOS 中也有一些单独的 layer，比如 AVCaptureVideoPreviewLayer 和 CAShapeLayer.

###与动画的关系
* 改变一个单独的 layer 的任何属性的时候，都会触发一个从旧的值过渡到新值的简单动画.
* 改变的是 view 中 layer 的同一个属性，它只会从这一帧直接跳变到下一帧.
* UIView 默认情况下禁止了 layer 动画，但是在 animation block 中又重新启用了它们. 
* 无论何时一个可动画的 layer 属性改变时，layer 都会寻找并运行合适的 'action' 来实行这个改变。在 Core Animation 的专业术语中就把这样的动画统称为动作 (action，或者 CAAction)。

###动画调用顺序

layer 将像[文档]()中所写的的那样去寻找动作，整个过程分为五个步骤。  

1. If the layer has a delegate that implements the actionForLayer:forKey: method, the layer calls that method. The delegate must do one of the following:	a. Return the action object for the given key.
	b. Return the NSNull object if it does not handle the action.
	
2. The layer looks in the layer’s actions dictionary for a matching key/action pair.
3. The layer looks in the style dictionary for an actions dictionary for a matching key/action pair.
4. The layer calls the defaultActionForKey: class method to look for any class-defined actions.
5. If any of the above steps returns an instance of NSNull, it is converted to nil before continuing.

When an action object is invoked it receives three parameters: the name of the event, the object on which the event happened (the layer), and a dictionary of named arguments specific to each event kind.

>而让这一切变得有趣的是，当 layer 在背后支持一个 view 的时候，view 就是它的 delegate；在 iOS 中，如果 layer 与一个 UIView 对象关联时，这个属性必须被设置为持有这个 layer 的那个 view。

>对于 view 中的 layer 来说，对动作的搜索只会到第一步为止（至少我没有见过 view 返回一个 nil 然后导致继续搜索动作的情况）。对于单独的 layer 来说，剩余的四个步骤可以在 CALayer 的 actionForKey: 文档中找到。


#
CoreAnimation

UIView的Layer树形在系统内部具有三份拷贝  

* 模型树(Model Layer Tree) UIView属性层   
* 呈现树(Presentation layer Tree) Layer动画属性层  
* 渲染树(Rendering Layer Tree) 渲染树 渲染树是是私有的，用户不可见，应该在其他线程或进程实现，进行动画时不会阻塞主线程。  
* CoreAnimation负责维护两个平行layer层次结构：model layer tree(模型层树)和presentation layer tree (表示层树)。前者反映我们直接能看到的layers的状态，而后者的layers则是动画正在表现的近似值

~~UIView委托强大的 Core Graphics 对他们的layer进行渲染。~~