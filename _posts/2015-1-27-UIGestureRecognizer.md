---
layout: post
category: IOS
title: Touch event and Gesture In IOS
tagline: by BluePandaLi
tags: [Touch,Gesture]
---

IOS中的触摸时间和手势

<!--more-->

###触摸事件
####UITouch

在IOS中，代表触摸对象的类是UITouch。当用户触摸屏幕后，就会产生相应的事件。UITouch包括以下几种重要的属性。

*	Window		触摸产生窗口
*	View		触摸产生视图
*	tapCount	短时间内轻击屏幕的次数	
*	timestamp 	事件产生或者变化的时间，单位是秒
*	phase UITouchPhase 枚举类型 

UIView中包括两个重要的成员函数用来获取事件在View中得触发位置，当参数为nil时获取的是在Window中得位置。

	 - (CGPoint)locationInView:(UIView *)view：
	 - (CGPoint)previousLocationInView:(UIView *)view

####UITouch 发生过程

当手指接触到屏幕时事件开始，直到手指离开屏幕事件结束。在此期间的所有UITouch对象都被包含在 UIEvent 事件对象中，由程序分发给处理者。分发过程如下。

 UIApplication -> AcviveWindow -> FirstResponder OR Responder chain -> UIWindow -> 丢弃
 
事件产生 经过UIApplication 从 AcvtiveWindow分发寻找第一响应者（即事件产生者）如果第一响应者不处理该事件，则沿着该视图的 SuperView 一直向上传递，该条传递链称为响应链，最后将会回到UIWindow寻求处理，如果响应链中没有处理者，事件将会被丢弃。 

所有继承了 UIResponder 的类都可以作为处理者。

####控制事件的触发和分发

*	局部的事件分发和接收可以使用 userInteractionEnabled
*	hidden 属性为 true 也不会触发和接收事件
*	UIApplication 中 beginIngnoringInteractionEvents endIngnoringInteractionEvents 可以在全局控制事件触发和分发
*	多点触控的开关为 multipleTouchEnabled


###创建手势

  
	UIView* view = [[UIView alloc]init];  
	view.userInteractionEnabled = YES;
	
	//点击
    UITapGestureRecognizer* singleRecognizer;
    singleRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(singleTap:)];
    //点击的次数
    singleRecognizer.numberOfTapsRequired = 1; // 单击
    //点击的手指数
    singleRecognizer.numberOfTouchesRequired = 1;
    [view addGestureRecognizer:singleRecognizer];
    
    //拖拽
    UIPanGestureRecognizer *panGestureRecognizer = [[UIPanGestureRecognizer alloc]initWithTarget:self                                               action:@selector(handlePan:)];
    [view addGestureRecognizer:panGestureRecognizer];
    
    //缩放
    UIPinchGestureRecognizer *pinchGestureRecognizer = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(handlePinch:)];
    [view addGestureRecognizer:pinchGestureRecognizer];
    
    //旋转
    UIRotationGestureRecognizer *rotateRecognizer = [[UIRotationGestureRecognizer alloc] initWithTarget:self action:@selector(handleRotate:)];
    [view addGestureRecognizer:rotateRecognizer];
    
    //滑动
    UISwipeGestureRecognizer *swipeRecognizer = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(handleSwipe:)];
    //设置滑动方向
    [swipeRecognizer setDirection:UISwipeGestureRecognizerDirectionDown];
    [view addGestureRecognizer:swipeRecognizer];


###响应事件

	- (void)singleTap:(UITapGestureRecognizer*)recognizer
	{
    	NSLog(@"单击");
	}
	
	- (void)handlePan:(UIPanGestureRecognizer*) recognizer
	{
    	NSLog(@"拖拽");
    	CGPoint translation = [recognizer translationInView:self.view];
    	recognizer.view.center = CGPointMake(recognizer.view.center.x + 		translation.x, recognizer.view.center.y + translation.y);
    	[recognizer setTranslation:CGPointZero inView:self.view];
	}

	- (void) handlePinch:(UIPinchGestureRecognizer*) recognizer
	{
    	NSLog(@"缩放, %f", recognizer.scale);
    	recognizer.view.transform = 	CGAffineTransformScale(recognizer.view.transform, recognizer.scale, recognizer.scale);
	    recognizer.scale = 1;
	}

	- (void) handleRotate:(UIRotationGestureRecognizer*) recognizer
	{
    	NSLog(@"旋转, %f", recognizer.rotation);
    	recognizer.view.transform = CGAffineTransformRotate(recognizer.view.transform, recognizer.rotation);
	    recognizer.rotation = 0;
	}

	- (void)handleSwipe:(UISwipeGestureRecognizer*)recognizer
	{
    	NSLog(@"滑动");
    	CGPoint translation = [recognizer locationInView:self.view];
    	NSLog(@"Swipe - start location: %f,%f", translation.x, translation.y);
    	recognizer.view.transform = CGAffineTransformMakeTranslation(translation.x, translation.y);
	}