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

---
##UIView动画
###普通的书写方式
###利用block的书写方式
