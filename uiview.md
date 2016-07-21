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

---
##停靠方式
###具体的停靠方式
     UIViewAutoresizingNone                 = 0,
     右边不变，左边自适应
     UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
     两边不变，中间自适应
     UIViewAutoresizingFlexibleWidth        = 1 << 1,
     左边不变。右边自适应
     UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
     下边不变，上边自适应
     UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
     上下不变，中间自适应
     UIViewAutoresizingFlexibleHeight       = 1 << 4,
     上边不变，下面自适应
     UIViewAutoresizingFlexibleBottomMargin = 1 << 5
####使用方式
    yellowView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
####注意：
#####要添加button的target
    [btn addTarget:self action:@selector(transformShape:) forControlEvents:UIControlEventTouchUpInside];
---
##自定义UIView
###根据不同的需求来继承UIView类，重写方法
####1、定义要写的控件属性
```
#import <UIKit/UIKit.h>

@interface InforView : UIView

@property(nonatomic, strong)UILabel * titleLabel;
@property(nonatomic, strong)UILabel * detailLabel;

-(void)setImage:(UIImage *)image;

@end
```
####2、不需要外界访问的属性要写在匿名类中
```
#import "InforView.h"

@interface InforView()

@property(nonatomic, strong)UIImageView * imageView;

@end

@implementation InforView

-(void)setImage:(UIImage *)image{
    if (self.imageView != nil) {
        self.imageView.image = image;
    }
}

- (instancetype)initWithFrame:(CGRect)frame
{
//    设置成员对象的frame值时最好写在layoutSubviews方法中
    self = [super initWithFrame:frame];
    if (self) {
        _imageView = [[UIImageView alloc] init];
        _imageView.backgroundColor = [UIColor grayColor];
        [self addSubview:_imageView];
        
        _titleLabel = [[UILabel alloc] init];
        _titleLabel.backgroundColor = [UIColor blueColor];
        
        _detailLabel = [[UILabel alloc] init];
         _detailLabel.backgroundColor = [UIColor blueColor];
        
        [self addSubview:self.titleLabel];
        [self addSubview:self.detailLabel];
    }
    return self;
}

//这个方法，是当所有对于对象所修饰的方法结束后调用此方法。
-(void)layoutSubviews{
    float margin = 8;
    float leftmargin = 2;
    _imageView.frame = CGRectMake(leftmargin, margin, CGRectGetWidth(self.frame)/3.0, CGRectGetHeight(self.frame)-2*margin);
    
    float height = (CGRectGetHeight(self.frame)-3*margin)/2;
    float width = CGRectGetWidth(self.frame)/3.0*2 - 3*leftmargin ;
    float x = CGRectGetWidth(self.frame)/3.0 + 2*leftmargin;
    _titleLabel.frame = CGRectMake(x, margin, width, height);
    
    _detailLabel.frame = CGRectMake(x, 2*margin+height, width, height);

}

@end
```
####注意：
>在用init初始化view的时候，如果没有用intiWithFrame方法，只用init的话,那么也会调用intiWithFrame方法，只不过没有给大小赋值。
>
>frame的值是一定要赋予的，只有这样才能创建出来可用的控件
>
>对于属性中的控件要重写-(void)layoutSubviews;方法，在里面进行创建。因为在外面创建当前对象后，在没有对当前对象进行修饰的方法时，会自动调用layoutSubviews方法。

---
##CABasicAnimation动画
####简介
在iOS中，你能看得见摸得着的东西基本上都是UIView，比如一个按钮、一个文本标签、一个文本输入框、一个图标等等，这些都是UIView。
其实UIView之所以能显示在屏幕上，完全是因为它内部的一个图层，在创建UIView对象时，UIView内部会自动创建一个图层(即CALayer对象)，通过UIView的layer属性可以访问这个层
@property(nonatomic,readonly,retain) CALayer *layer; 
当UIView需要显示到屏幕上时，会调用drawRect:方法进行绘图，并且会将所有内容绘制在自己的图层上，绘图完毕后，系统会将图层拷贝到屏幕上，于是就完成了UIView的显示
换句话说，UIView本身不具备显示的功能，拥有显示功能的是它内部的图层。
####创建对象
>CABasicAnimation * animation = [CABasicAnimation animationWithKeyPath:@"transform.scale.y"];

####设置动画持续时间(秒)
>animation.duration = 5;

####设置动画开始的初始x
>animation.fromValue = @1;

####设置动画结束的终点值
>animation.toValue = @3;

####设置延迟时间
>animation.beginTime = CACurrentMediaTime()+2;

####设置代理
>animation.delegate = self;

####设置路径返回
>animation.autoreverses = YES;

####设置重复次数
>animation.repeatCount = 3;

####设置自动删除动画
>animation.removedOnCompletion = YES;

####添加动画的key
>[animation setValue:@"yAnimation" forKey:@"myView"];

>[myView.layer addAnimation:animation forKey:@"myView"];

####通过key获取动画对象
>UIView * myView = [self.window viewWithTag:100];

>[myView.layer animationForKey:@"myView"];

####移除一个动画
>[myView.layer removeAnimationForKey:@"myView"]

####具体代码实现——
```
#import "AppDelegate.h"

@interface AppDelegate ()
@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    self.window.rootViewController = [[UIViewController alloc] init];
    [self.window makeKeyAndVisible];
    
    [self createView];
    return YES;
}

-(void)createView{
    UIView * myView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 200, 200)];
    myView.center = self.window.center;
    myView.backgroundColor = [UIColor blackColor];
    
    myView.tag = 100;
    
    [self.window addSubview:myView];
    
    CABasicAnimation * animation = [CABasicAnimation animationWithKeyPath:@"transform.scale.y"];
//    设置动画持续时间
    animation.duration = 5;
//    x的初始值
    animation.fromValue = @1;
//    x的动画终点值
    animation.toValue = @3;
//    设置延迟时间+2
    animation.beginTime = CACurrentMediaTime()+2;
//    设置代理
    animation.delegate = self;
//    添加key
//    [animation setValue:@"yAnimation" forKey:@"myView"];
    
//    设置为自动回来，动画按原路径返回
    animation.autoreverses = YES;
    animation.repeatCount = 3;
//    如果设置此属性为YES那么当动画结束时会自动删除这个动画
    animation.removedOnCompletion = YES;
    
    [myView.layer addAnimation:animation forKey:@"myView"];
    
}

-(void)animationDidStart:(CAAnimation *)anim{
    NSLog(@"%s", __func__);
//    NSLog(@"%@", [anim valueForKey:@"myView"]);
    
    UIView * myView = [self.window viewWithTag:100];
//    通过key获取动画对象
    if ([myView.layer animationForKey:@"myView"] == anim) {
        NSLog(@"anim相同");
//        移除一个动画
//        [myView.layer removeAnimationForKey:@"myView"];
    }
    
}

-(void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag{
    UIView * myView = [self.window viewWithTag:100];
    NSLog(@"%s", __func__);
//    如果自动删除动画的时候，这个就不会打印
    if ([myView.layer animationForKey:@"myView"] == anim) {
        NSLog(@"————————anim相同");
    }
}
```
---
##UIView动画
###普通的书写方式
####添加动画
>[UIView beginAnimations:@"black" context:(__bridge void*)blackView ];

####手动添加监听器
>[UIView setAnimationWillStartSelector:@selector(stratrssAnimation)];
>
>[UIView setAnimationDidStopSelector:@selector(stopAnimation)];

####设置速度
>[UIView setAnimationDuration:0.24];

####动画重复次数
>[UIView setAnimationRepeatCount:100];

####自动回复
>[UIView setAnimationRepeatAutoreverses:YES];

####延迟时间
>[UIView setAnimationDelay:2];

####设置动画曲线
>[UIView setAnimationCurve:UIViewAnimationCurveLinear];

>动画展示的几种方式
     >>在开头和结尾的时候慢
     UIViewAnimationCurveEaseInOut,         // slow at beginning and end
     在开始的时候慢
     UIViewAnimationCurveEaseIn,            // slow at beginning
     在结尾的时候慢
     UIViewAnimationCurveEaseOut,           // slow at end
     线性运动，匀速
     UIViewAnimationCurveLinear

####设置代理
>[UIView setAnimationDelegate:self];

####提交动画
>[UIView commitAnimations];

####注意事项：
    1、可以自己定义监听方法，此时创建的代理对象会调用自己写的方法。
    2、如果没有自己添加方法，那么系统会调用代理对象的-(void)animationWillStart:(NSString *)animationID context:(void *)context;和-(void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag;
    3、如果两者皆存在，那么自己定义的方法会覆盖掉系统的方法。

###利用block的书写方式
####动画特效的类型
```
UIViewAnimationOptionLayoutSubviews            = 1 <<  0,
UIViewAnimationOptionAllowUserInteraction      = 1 <<  1, // turn on user interaction while animating
UIViewAnimationOptionBeginFromCurrentState     = 1 <<  2, // start all views from current value, not initial value
UIViewAnimationOptionRepeat                    = 1 <<  3, // repeat animation indefinitely
UIViewAnimationOptionAutoreverse               = 1 <<  4, // if repeat, run animation back and forth
UIViewAnimationOptionOverrideInheritedDuration = 1 <<  5, // ignore nested duration
UIViewAnimationOptionOverrideInheritedCurve    = 1 <<  6, // ignore nested curve
UIViewAnimationOptionAllowAnimatedContent      = 1 <<  7, // animate contents (applies to transitions only)
UIViewAnimationOptionShowHideTransitionViews   = 1 <<  8, // flip to/from hidden state instead of adding/removing
UIViewAnimationOptionOverrideInheritedOptions  = 1 <<  9, // do not inherit any options or animation type

UIViewAnimationOptionCurveEaseInOut            = 0 << 16, // default
UIViewAnimationOptionCurveEaseIn               = 1 << 16,
UIViewAnimationOptionCurveEaseOut              = 2 << 16,
UIViewAnimationOptionCurveLinear               = 3 << 16,

UIViewAnimationOptionTransitionNone            = 0 << 20, // default
UIViewAnimationOptionTransitionFlipFromLeft    = 1 << 20,
UIViewAnimationOptionTransitionFlipFromRight   = 2 << 20,
UIViewAnimationOptionTransitionCurlUp          = 3 << 20,
UIViewAnimationOptionTransitionCurlDown        = 4 << 20,
UIViewAnimationOptionTransitionCrossDissolve   = 5 << 20,
UIViewAnimationOptionTransitionFlipFromTop     = 6 << 20,
UIViewAnimationOptionTransitionFlipFromBottom  = 7 << 20,
```
####动画创建Demo1
#####Duration：是动画的时间；animations 变化。
```
[UIView animateWithDuration:3 animations:^{
            blackView.center = self.window.center;
            blackView.alpha = 0.1;
        }];
```
####动画创建Demo2
#####Duration：是动画的时间；animations 变化；completion 动画结束内容。
```
[UIView animateWithDuration:3 animations:^{
        blackView.transform = CGAffineTransformScale(blackView.transform, 2, 2);
    } completion:^(BOOL finished) {
        //        blackView.transform = CGAffineTransformIdentity;
        //        blackView.alpha = 1;
        [UIView animateWithDuration:3 animations:^{
            blackView.transform = CGAffineTransformIdentity;
            blackView.alpha = 1;
        }];
    }];
```
####动画创建Demo3
#####Duration 动画时间 ；delay 延迟时间 ; Damping 反弹（0—1）值越小，弹性越大; Velocity;options 可以设定动画的曲线，设置一些特效等等。
```
[UIView animateWithDuration:3 delay:2 usingSpringWithDamping:0.2 initialSpringVelocity:3 options: UIViewAnimationOptionCurveEaseInOut animations:^{
        blackView.frame = CGRectMake(120, 300, 200, 200);
        blackView.alpha = 1;
        
    } completion:^(BOOL finished) {
        [UIView animateWithDuration:3 animations:^{
                        blackView.transform = CGAffineTransformIdentity;
                        blackView.alpha = 1;
                    }];
    }];
```
---
##UIImageView的使用
###创建
```
self.imageView = [[UIImageView alloc] initWithFrame:[UIScreen mainScreen].bounds];
self.imageView.image = [UIImage imageNamed:@"eat_00.jpg"];
```
###添加一组图片的方式：可以通过数组接收图片的名字，然后执行
```
for (NSInteger i = 0; i < count; i++) {
  UIImage * image = [UIImage imageNamed:[NSString stringWithFormat:@"%@_%02ld.jpg", name, i]];
  [array addObject:image];
}
```
###动画添加的图片集
```
self.imageView.animationImages = array;
```
###动画播放的时间设置
```
self.imageView.animationDuration = 0.08*array.count;
```
###动画重复次数
```
self.imageView.animationRepeatCount = 1;
```
###动画开始
```
 [self.imageView startAnimating];
```
###清除缓存
```
[self.imageView performSelector:@selector(setAnimationImages:) withObject:nil afterDelay:self.imageView.animationDuration+0.5];
```
####注意
    1、要注意针对图片大小选择不同处理图片的方式。如果图片较小，则将图片放在缓存中，提高效率
    >UIImage * image = [UIImage imageNamed:[NSString stringWithFormat:@"%@_%02ld.jpg", name, i]];
    如果图片较大，则就不能使用这种方法，应用：
    >NSString * path = [[NSBundle mainBundle] pathForResource:[NSString stringWithFormat:@"%@_%02ld", name, i ] ofType:@"jpg"];
     [UIImage imageWithContentsOfFile:path];
     UIImage * image = [UIImage imageWithContentsOfFile:path];
    2、注意缓存的清理
    
####全部代码展示：
```
#import "AppDelegate.h"

@interface AppDelegate ()

@property(nonatomic, strong)UIImageView * imageView;

@end

@implementation AppDelegate


- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    self.window.rootViewController = [[UIViewController alloc] init];
    [self.window makeKeyAndVisible];

    [self createUIImageView];
    return YES;
}

-(void)createUIImageView{
//    创建UIImageView
    self.imageView = [[UIImageView alloc] initWithFrame:[UIScreen mainScreen].bounds];
//    self.imageView.backgroundColor = [UIColor orangeColor];
    self.imageView.image = [UIImage imageNamed:@"eat_00.jpg"];
    
    //    创建fartButton
    UIButton * fartButton = [[UIButton alloc] initWithFrame:CGRectMake(10, 535, 40, 40)];
    [fartButton setBackgroundImage:[UIImage imageNamed:@"fart"] forState:UIControlStateNormal];
    [fartButton addTarget:self action:@selector(fartAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
    //    创建pieButton
    UIButton * pieButton = [[UIButton alloc] initWithFrame:CGRectMake(10, 500, 40, 40)];
    [pieButton setBackgroundImage:[UIImage imageNamed:@"pie"] forState:UIControlStateNormal];
    [pieButton addTarget:self action:@selector(pieAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
//    创建 eatbutton
    UIButton * eatButton = [[UIButton alloc] initWithFrame:CGRectMake(10, 570, 40, 40)];
    [eatButton setBackgroundImage:[UIImage imageNamed: @"eat"] forState:UIControlStateNormal];
    [eatButton addTarget:self action:@selector(eatAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
    //    创建drinkButton
    UIButton * drinkButton = [[UIButton alloc] initWithFrame:CGRectMake(10, 605, 40, 40)];
    [drinkButton setBackgroundImage:[UIImage imageNamed: @"drink"] forState:UIControlStateNormal];
    [drinkButton addTarget:self action:@selector(drinkAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
//    创建scratchButton
    UIButton * scratchButton = [[UIButton alloc] initWithFrame:CGRectMake(10, 640, 40, 40)];
    [scratchButton setBackgroundImage:[UIImage imageNamed:@"scratch"] forState:UIControlStateNormal];
    [scratchButton addTarget:self action:@selector(scratchAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
//    创建cymbalButton
    UIButton * cymbalButton = [[UIButton alloc]initWithFrame:CGRectMake(10, 675, 40, 40)];
    [cymbalButton setBackgroundImage:[UIImage imageNamed:@"cymbal"] forState:UIControlStateNormal];
    [cymbalButton addTarget:self action:@selector(cymbalAnamation:) forControlEvents:UIControlEventTouchUpInside];
    
//    knockoutButton
    UIButton * knockoutButton = [[UIButton alloc] initWithFrame:CGRectMake(180, 130, 50, 50)];
    knockoutButton.backgroundColor = [UIColor clearColor];
    [knockoutButton addTarget:self action:@selector(knockoutAnamation) forControlEvents:UIControlEventTouchUpInside];
    
//    footrightButton
    UIButton * footrightButton = [[UIButton alloc] initWithFrame:CGRectMake(150, 665, 50, 50)];
    footrightButton.backgroundColor = [UIColor clearColor];
    [footrightButton addTarget:self action:@selector(footRightAnamation) forControlEvents:UIControlEventTouchUpInside];
    
//    footleftButton
    UIButton * footrileftButton = [[UIButton alloc] initWithFrame:CGRectMake(210, 665, 50, 50)];
    footrileftButton.backgroundColor = [UIColor clearColor];
    [footrileftButton addTarget:self action:@selector(footLetfAnamation) forControlEvents:UIControlEventTouchUpInside];
//    angryButton
    UIButton * angryButton1 = [[UIButton alloc] initWithFrame:CGRectMake(80, 130, 55, 60)];
    angryButton1.backgroundColor = [UIColor clearColor];
    [angryButton1 addTarget:self action:@selector(angryAnamation) forControlEvents:UIControlEventTouchUpInside];
    
    UIButton * angryButton2 = [[UIButton alloc] initWithFrame:CGRectMake(280, 130, 55, 60)];
    angryButton2.backgroundColor = [UIColor clearColor];
    [angryButton2 addTarget:self action:@selector(angryAnamation) forControlEvents:UIControlEventTouchUpInside];
    
    
    [self.window addSubview: self.imageView];
    [self.window addSubview:eatButton];
    [self.window addSubview:drinkButton];
    [self.window addSubview:scratchButton];
    [self.window addSubview:cymbalButton];
    [self.window addSubview:fartButton];
    [self.window addSubview:pieButton];
    [self.window addSubview:knockoutButton];
    [self.window addSubview:footrightButton];
    [self.window addSubview:footrileftButton];
    [self.window addSubview:angryButton1];
    [self.window addSubview:angryButton2];
}

-(void)footLetfAnamation{
    [self animationName:@"footLeft" count:30];
}

-(void)footRightAnamation{
    [self animationName:@"footRight" count:29];
}

-(void)angryAnamation{
    [self animationName:@"angry" count:26];
}

-(void)knockoutAnamation{
    [self animationName:@"knockout" count:81];
}

-(void)pieAnamation:(UIButton *)btn{
    [self animationName:@"pie" count:24];
}

-(void)fartAnamation:(UIButton *)btn{
    [self animationName:@"fart" count:28];
}

-(void)cymbalAnamation:(UIButton *)btn{
    [self animationName:@"cymbal" count:13];
}
-(void)eatAnamation:(UIButton *)btn{
    [self animationName:@"eat" count:40];
}


-(void)drinkAnamation:(UIButton *)btn{
    [self animationName:@"drink" count:81];
}

-(void)scratchAnamation:(UIButton *)btn{
    [self animationName:@"scratch" count:56];
}

-(void)animationName:(NSString *)name count:(NSUInteger)count{
    
    NSMutableArray * array = [NSMutableArray array];
    for (NSInteger i = 0; i < count; i++) {
        
        NSString * path = [[NSBundle mainBundle] pathForResource:[NSString stringWithFormat:@"%@_%02ld", name, i ] ofType:@"jpg"];
        [UIImage imageWithContentsOfFile:path];
        
//        直接使用你name的方式加载图片是会有缓存的。程序员还无法手动控制。
//        适合存放较小的图片，
//        UIImage * image = [UIImage imageNamed:[NSString stringWithFormat:@"%@_%02ld.jpg", name, i]];
//        [array addObject:image];
//        利用路径的方法是不会有缓存的。执行效率不如有缓存的方法高。适合存放较大的图片资源。
        
        UIImage * image = [UIImage imageWithContentsOfFile:path];
        [array addObject:image];
    }
    self.imageView.animationImages = array;
    self.imageView.animationDuration = 0.08*array.count;
    self.imageView.animationRepeatCount = 1;
    
    [self.imageView startAnimating];
    
//    [self performSelector:@selector(clear) withObject:nil afterDelay:0.08*array.count+1];
    
    [self.imageView performSelector:@selector(setAnimationImages:) withObject:nil afterDelay:self.imageView.animationDuration+0.5];
}

//-(void)clear{
//    self.imageView.animationImages = nil;
//    NSLog(@"%s", __func__);
//}




- (void)applicationWillResignActive:(UIApplication *)application {
    // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
    // Use this method to pause ongoing tasks, disable timers, and throttle down OpenGL ES frame rates. Games should use this method to pause the game.
}

- (void)applicationDidEnterBackground:(UIApplication *)application {
    // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
    // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
}

- (void)applicationWillEnterForeground:(UIApplication *)application {
    // Called as part of the transition from the background to the inactive state; here you can undo many of the changes made on entering the background.
}

- (void)applicationDidBecomeActive:(UIApplication *)application {
    // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
}

- (void)applicationWillTerminate:(UIApplication *)application {
    // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
}

@end
```
---

####附录：动画的方式介绍

    常规动画属性设置（可以同时选择多个进行设置）
    UIViewAnimationOptionLayoutSubviews：动画过程中保证子视图跟随运动。
    UIViewAnimationOptionAllowUserInteraction：动画过程中允许用户交互。
    UIViewAnimationOptionBeginFromCurrentState：所有视图从当前状态开始运行。
    UIViewAnimationOptionRepeat：重复运行动画。
    UIViewAnimationOptionAutoreverse ：动画运行到结束点后仍然以动画方式回到初始点。
    UIViewAnimationOptionOverrideInheritedDuration：忽略嵌套动画时间设置。
    UIViewAnimationOptionOverrideInheritedCurve：忽略嵌套动画速度设置。
    UIViewAnimationOptionAllowAnimatedContent：动画过程中重绘视图（注意仅仅适用于转场动画）。  
    UIViewAnimationOptionShowHideTransitionViews：视图切换时直接隐藏旧视图、显示新视图，而不是将旧视图从父视图移除（仅仅适用于转场动画）
    UIViewAnimationOptionOverrideInheritedOptions ：不继承父动画设置或动画类型。
    2.动画速度控制（可从其中选择一个设置）
    UIViewAnimationOptionCurveEaseInOut：动画先缓慢，然后逐渐加速。
    UIViewAnimationOptionCurveEaseIn ：动画逐渐变慢。
    UIViewAnimationOptionCurveEaseOut：动画逐渐加速。
    UIViewAnimationOptionCurveLinear ：动画匀速执行，默认值。
    3.转场类型（仅适用于转场动画设置，可以从中选择一个进行设置，基本动画、关键帧动画不需要设置）
    UIViewAnimationOptionTransitionNone：没有转场动画效果。
    UIViewAnimationOptionTransitionFlipFromLeft ：从左侧翻转效果。
    UIViewAnimationOptionTransitionFlipFromRight：从右侧翻转效果。
    UIViewAnimationOptionTransitionCurlUp：向后翻页的动画过渡效果。    
    UIViewAnimationOptionTransitionCurlDown ：向前翻页的动画过渡效果。    
    UIViewAnimationOptionTransitionCrossDissolve：旧视图溶解消失显示下一个新视图的效果。    
    UIViewAnimationOptionTransitionFlipFromTop ：从上方翻转效果。    
    UIViewAnimationOptionTransitionFlipFromBottom：从底部翻转效果。


