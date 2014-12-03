---
layout: post
category: IOS
title: IOS Animation
tagline: by BluePandaLi
tags: [animation]
---
#IOS中实现动画的框架

*	Core Animation
*	IOS7之后出现了 UIKit Dynamics 物理模拟框架
*  以及FaceBook开源的 [Pop动画引擎](https://github.com/facebook/pop)

#UIView 和 CALayer

首先我们在试验项目中写下 **UIView** , Command + 鼠标左键 查看其定义；  
>![](http://bluepanda-blog.qiniudn.com/blogCoreAnimationDemo_UIView.png)  
>在UIView源码中发现:  
>*	`UIView`属于`UIKit`  
>*	`UIView`继承自`UIResponder`能够处理事件  
>*  类中包含了`CALayer`的get属性并在注释中 我们看到这样一句话 `view是layer的代理`  
>
>官方文档:  
>* The **UIResponder** class defines an interface for objects that respond to and handle events. It is the superclass of UIApplication, UIView and its subclasses (which include UIWindow). Instances of these classes are sometimes referred to as responder objects or, simply, responders.  
>
>* The **UIView** class defines a rectangular area on the screen and the interfaces for managing the content in that area. At runtime, a view object handles the rendering of any content in its area and also handles any interactions with that content. The UIView class itself provides basic behavior for filling its rectangular area with a background color. More sophisticated content can be presented by subclassing UIView and implementing the necessary drawing and event-handling code yourself. The UIKit framework also includes a set of standard subclasses that range from simple buttons to complex tables and can be used as-is. For example, a UILabel object draws a text string and a UIImageView object draws an image.  
>* The **UIKit** framework (UIKit.framework) provides the crucial infrastructure needed to construct and manage iOS apps. This framework provides the window and view architecture needed to manage an app’s user interface, the event handling infrastructure needed to respond to user input, and the app model needed to drive the main run loop and interact with the system.  


查看**CALayer**的定义；  
>![](http://bluepanda-blog.qiniudn.com/blogCoreAnimationDemo_CALayer.png)  
>在CALayer源码中发现： 
>* `CALayer`属于`QuarterCore`  
>* `CALayer`继承自`NSObject`  
>
> 官方文档:  
> 
> The **CALayer** class manages image-based content and allows you to perform animations on that content. **Layers are often used to provide the backing store for views but can also be used without a view to display content.** A layer’s main job is to manage the visual content that you provide but the layer itself has visual attributes that can be set, such as a background color, border, and shadow. In addition to managing visual content, the layer also maintains information about the geometry of its content (such as its position, size, and transform) that is used to present that content onscreen. Modifying the properties of the layer is how you initiate animations on the layer’s content or geometry. A layer object encapsulates the duration and pacing of a layer and its animations by adopting the CAMediaTiming protocol, which defines the layer’s timing information.  

> **If the layer object was created by a view, the view typically assigns itself as the layer’s delegate automatically, and you should not change that relationship.**For layers you create yourself, you can assign a delegate object and use that object to provide the contents of the layer dynamically and perform other tasks. A layer may also have a layout manager object (assigned to the layoutManager property) to manage the layout of subviews separately.  


###综上结论：

*	`CALayer`只关心绘制
*  可以认为`UIView`是一个处理事件的、CALayer的、特殊实现。

#CoreAnimation

UIView的Layer树形在系统内部具有三份拷贝  
* 模型树(Model Layer Tree) UIView属性层   
* 呈现树(Presentation layer Tree) Layer动画属性层  
* 渲染树(Rendering Layer Tree) 渲染树 渲染树是是私有的，用户不可见，应该在其他线程或进程实现，进行动画时不会阻塞主线程。  
* CoreAnimation负责维护两个平行layer层次结构：model layer tree(模型层树)和presentation layer tree (表示层树)。前者反映我们直接能看到的layers的状态，而后者的layers则是动画正在表现的近似值

~~UIView委托强大的 Core Graphics 对他们的layer进行渲染。~~