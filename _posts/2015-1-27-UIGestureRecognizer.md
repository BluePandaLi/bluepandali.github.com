---
layout: post
category: IOS
title: Gesture In IOS
tagline: by BluePandaLi
tags: [手势,Gesture]
---

IOS中的手势

<!--more-->

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