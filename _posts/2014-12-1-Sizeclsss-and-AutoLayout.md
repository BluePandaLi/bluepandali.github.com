---
layout: post
category: IOS
title: SizeClass and AutoLayout
tagline: by BluePandaLi
tags: [sizeclass,autolayout]
---
在IOS8之后使用XIB开发将是大势所趋，[@叶孤城___](http://www.jianshu.com/users/b82d2721ba07/latest_articles) 博客中有很好的三篇教程很适合入门。

[SizeClass和AutoLayout教程1](http://www.jianshu.com/p/bd071f9a558d) 简单添加AutoLayout约束   
[SizeClass和AutoLayout教程2](http://www.jianshu.com/p/a4cf3db81c0b) 多个View添加AutoLayout约束  
[SizeClass和AutoLayout教程3](http://www.jianshu.com/p/3d6b2341fd83) AutoLayout与SizeClass的协作  
[SizeClass和AutoLayout教程4](http://www.jianshu.com/p/e72e957497b3) SizeClass与FontSize、ImageSet控制  
[Beginning Adaptive Layout Tutorial](http://www.raywenderlich.com/83276/beginning-adaptive-layout-tutorial) 原英文教程  
总结：

*	AutoLayout本质是使用各种条件告诉你View的位置和大小。
*	建立约束时IB结构面板出现黄色剪头表示添加约束后控件大小与IB中显示不符，此时使用 Resolve Auto Layout Issues ->  All Views\Update Frames 刷新界面。
*	当约束不足是IB结构面板会出现红色剪头，点击红色剪头IB会告诉你缺少那些不足的标记。
*	constraints to margins IOS新特性，当你添加左右上下约束的时候系统会在你添加的数值上加16px（有特殊情况不遵循原博说明）。
*	添加两个View之间的约束，可以直接右键选中拖动链接，并可以在Size Inspector重新设置主从关系。
*	Size Inspector不能删除约束，只能卸载。在结构控制器中可以删除约束。


