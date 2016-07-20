# UIView
---
##父视图与子视图的关系
####一个子视图只有一个父视图
####一个父视图可以有多个子视图
>注：
*      如果A视图添加到B视图上，那么就称B视图是A视图的父视图，A是B的子视图。
*      一个视图只能有一个父视图，但一个视图可以有多个子视图。
*      一个视图可以直接加到另一个视图上。

##UIView的创建方式
```
UIView * myView = [[UIView alloc] initWithFrame:CGRectMake(60, 100, 300, 300)];
    
myView.backgroundColor = [UIColor orangeColor];
myView.bounds = CGRectMake(50, 0, 300, 300);

[self.window addSubview:myView];
```
##当子视图超出父视图部分的操作
>减去子视图超出的部分
>>myView.clipsToBounds = YES;  

>透明度会对子视图产生影响
>>myView.alpha = 0.1;  透明度会对子视图产生影响

>隐藏设置也会对子视图产生影响
>>myView.hidden = YES; 

>父视图内子视图的不会响应。若为YES的时候，子视图超出父视图部分也不会响应。注：UILabel和UIImageView的事件响应开关，是view.userInteractionEnabled = NO。
>>myView.userInteractionEnabled = NO;  

>获得视图的方法
>>```
[myView viewWithTag:100];
UIView * redView = [self.window viewWithTag:100];
UIView * yellow = [self.window viewWithTag:1002];
UIView * green = [self.window viewWithTag:1000];
```

>获取所有的子视图
>>NSArray * array = [self.window subviews];

>获得父视图的方法
>>[greenView superview];

---
##视图的操作
###位置的创建
####center
    center的位置在平移过会不会变
####bounds
    bounds的位置大部分时间是(0,0)。如果定义，那么是对子视图的限制
####frame
    frame大小改变是会改变的
#####代码展示
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.rootViewController = [[UIViewController alloc] init];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    [self createView];
    return YES;
}

-(void)createView{
    UIView * myView = [[UIView alloc] init];
//    frame的point坐标是相对是父视图的位置。
    myView.frame = CGRectMake(60, 100, 300, 300);
    myView.backgroundColor = [UIColor orangeColor];
    [self.window addSubview:myView];
//    改变center值。frame会变吗？会改变。
//    bounds值会改变吗？一个view在没设置bounds值前，bounds的(x, y)大部分都是(0, 0)
    myView.center = self.window.center;
    myView.tag = 100;
//    对bounds的修改不影响当前对象。但会修改当前对象的大小
//    注：bounds的(x, y)是设置的view左上角起始位置是多少。
    myView.bounds = CGRectMake(-50, -50, 300, 300);
    NSLog(@"myView.center  \n%@", NSStringFromCGPoint(myView.center));
    NSLog(@"myView.frame  \n%@", NSStringFromCGRect( myView.frame));
    NSLog(@"myView.bounds  \n%@", NSStringFromCGRect( myView.bounds));
//
//    UIView * secondView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 100, 100)];
//    secondView.backgroundColor = [UIColor purpleColor];
//    [myView addSubview:secondView];

//    视图的形变 CGAffineTransform。平移不会改变视图的bounds还有和center的值。只会改变frame的值。左上角是基本点。
//    myView.transform = CGAffineTransformMakeTranslation(100, 100);
    
    UIButton * btn = [[UIButton alloc] initWithFrame:CGRectMake(100, 40, 80, 30)];
    [btn setTitle:@"变型" forState:UIControlStateNormal];
    btn.backgroundColor = [UIColor blueColor];
    [btn addTarget:self action:@selector(shapeTransform) forControlEvents:UIControlEventTouchUpInside];
    [self.window addSubview:btn];
    
    
}

-(void)shapeTransform{
    UIView * myView = (UIView *)[self.window viewWithTag:100 ];
//    myView.transform = CGAffineTransformMakeTranslation(-70, 200);
//    放大或是缩小时，是不会在一侧发生的。只针对原视图只实现一次。
//    myView.transform = CGAffineTransformMakeScale(0.4, 0.5);
//    旋转，center和bounds值不变，frame值改变。整数顺时针转的。
//    myView.transform  = CGAffineTransformMakeRotation(M_PI_4);
//    一直旋转
//    myView.transform = CGAffineTransformRotate(myView.transform, M_PI_4/2.0);
    NSLog(@"=========================");
    NSLog(@"myView.center  \n%@", NSStringFromCGPoint(myView.center));
    NSLog(@"myView.frame  \n%@", NSStringFromCGRect( myView.frame));
    NSLog(@"myView.bounds  \n%@", NSStringFromCGRect( myView.bounds));

//    利用公式完成缩放、平移、旋转
    myView.transform = CGAffineTransformMake(0.5, 0, 33, 22, 2, 3);
    
    static BOOL flg = NO;
    if (flg) {
//        会让形变恢复
        myView.transform = CGAffineTransformIdentity;
    }
    flg = !flg;
    
}
```
>注意：
>>放大或是缩小时，是不会在一侧发生的。只针对原视图只实现一次
>>>myView.transform = CGAffineTransformMakeTranslation(-70, 200);

>>旋转，center和bounds值不变，frame值改变。整数顺时针转的
>>>myView.transform  = CGAffineTransformMakeRotation(M_PI_4);

>>一直旋转
>>>myView.transform = CGAffineTransformRotate(myView.transform, M_PI_4/2.0);

>>利用公式完成缩放、平移、旋转
>>>myView.transform = CGAffineTransformMake(0.5, 0, 33, 22, 2, 3);

>>形变恢复
>>>myView.transform = CGAffineTransformIdentity;

---
##改变视图位置关系
####插入视图进入某个位置，可以改变父子视图关系
>[redView exchangeSubviewAtIndex:0 withSubviewAtIndex:2];

>[yellow insertSubview:green atIndex:0];
    
####前置视图
>[redView bringSubviewToFront:green];

####后置视图
>[redView sendSubviewToBack:yellow];

####移除视图
>[yellow removeFromSuperview];
>
>[button removeFromSuperview];

####插入视图，在某视图的上方
>[redView insertSubview:yellow aboveSubview:green];
