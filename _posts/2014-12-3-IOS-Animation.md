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

#CoreAnimation

UIView的Layer树形在系统内部具有三份拷贝  

* 模型树(Model Layer Tree) UIView属性层   
* 呈现树(Presentation layer Tree) Layer动画属性层  
* 渲染树(Rendering Layer Tree) 渲染树 渲染树是是私有的，用户不可见，应该在其他线程或进程实现，进行动画时不会阻塞主线程。  
* CoreAnimation负责维护两个平行layer层次结构：model layer tree(模型层树)和presentation layer tree (表示层树)。前者反映我们直接能看到的layers的状态，而后者的layers则是动画正在表现的近似值

~~UIView委托强大的 Core Graphics 对他们的layer进行渲染。~~