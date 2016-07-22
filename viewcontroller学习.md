#UIViewController学习

---
##视图控制器
###创建一个控制器
```
 self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
 self.window.backgroundColor = [UIColor grayColor];
// 创建一个控制器
 ViewController * VC = [[ViewController alloc] init];
// 把视图控制器与window联系起来
 self.window.rootViewController = VC;

 [self.window makeKeyAndVisible];
```
####注意：主控制器只能创建一个，并且不能够被删除。对于控制器编写的代码在AppDelegate.m里面。
###视图控制器的声明周期
> -(void)viewDidLoad 
>>在创建的时候就调用一次

> -(void)viewDidUnload 
>>不加载（现在已经弃用了）

> -(void)viewWillAppear:(BOOL)animated 
>>视图将要出现

> -(void)viewDidAppear:(BOOL)animated 
>>视图已经出现

> -(void)viewWillDisappear:(BOOL)animated 
>>视图将要消失

> -(void )viewDidDisappear:(BOOL)animated 
>>视图已经消失

####在写东西的时候，一般都在viewDidLoad中写。
### UIApplication 介绍
     UIApplication 是真正意义上的单例。
     UIApplication * application = [UIApplication sharedApplication]; 设置一些应用级的程序。
     application.networkActivityIndicatorVisible = YES; 使网速的图标一直转
     application.statusBarStyle = UIStatusBarStyleLightContent; 隐藏状态栏。
     如果要使statusBarStyle文件起作用，要先改plist文件， View controller-based status bar appearance 显示YES 。
####注意： 写视图布局，添加控件一般都写在viewDidLoad方法中。 

###创建视图
####在viewDidLoad中直接设置背景颜色等属性即可，添加的控件也写在里面。
```
- (void)viewDidLoad {
 [super viewDidLoad];
 self.view.backgroundColor = [UIColor yellowColor];
 // Do any additional setup after loading the view.
 [self createButton];
 [self createTextField];
}
```
####视图控制器的跳转
```
-(void)onClick:(UIButton*)btn{
 NSLog(@"%s", __func__);
// sVC是指定的跳转的视图控制器
// animated：是是否有转场动画
// complete：完成跳转后执行的block
 [self.delegate showNewText:self.textField.text];
// 回到上一层视图控制器。根视图不能够销毁
 [self dismissViewControllerAnimated:YES completion:nil];
}
```
###视图控制器之间使用代理传值
######AppDelegate.h
```
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;

@end
```
######AppDelegate.m
```
#import "AppDelegate.h"
#import "FirstViewController.h"

@interface AppDelegate ()

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
 // Override point for customization after application launch.

 self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
 self.window.backgroundColor = [UIColor grayColor];

 // 创建一个控制器
 FirstViewController * fVC = [[FirstViewController alloc] init];
 // 把视图控制器与window联系起来
 self.window.rootViewController = fVC;
 fVC.labelTitle = @"I am good boy";
 [self.window makeKeyAndVisible];

 return YES;
}
```
######FirstViewController.h
```
#import <UIKit/UIKit.h>
#import "SecondViewController.h"

@interface FirstViewController : UIViewController<SecondViewControllerDelegate>

@property(nonatomic, copy)NSString * labelTitle;

@end
```
######FirstViewController.m
```
#import "FirstViewController.h"
#import "SecondViewController.h"

@interface FirstViewController ()

@property(nonatomic, strong)UILabel * label;

@end

@implementation FirstViewController

- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view.
 self.view.backgroundColor = [UIColor orangeColor];
 [self createLabel];
 [self createButton];
// _strPublic = @"xiaoming";
}

-(void)createButton{
 UIButton * btn = [[UIButton alloc] initWithFrame:CGRectMake(20, 40, 20, 20)];
 btn.backgroundColor = [UIColor blueColor];
 [self.view addSubview:btn];
 [btn addTarget:self action:@selector(onClick:) forControlEvents:UIControlEventTouchUpInside];
}

-(void)onClick:(UIButton*)btn{
 NSLog(@"%s", __func__);
 SecondViewController * sVC = [[SecondViewController alloc] init];

 sVC.modalTransitionStyle = UIModalTransitionStyleFlipHorizontal;
// 代理实现
 sVC.delegate = self;
 [sVC setSenderBlcok:^(NSString * string) {
 self.label.text = string;
 }];

 [self presentViewController:sVC animated:YES completion:^{

 }];

}

#pragma mark ------------- SecondViewControllerDelegate
-(void)senderString:(NSString *)string formViewController:(SecondViewController *)secondViewController{
 self.label.text = string;
}

-(void)viewWillAppear:(BOOL)animated{
 [super viewWillAppear:animated];
// self.label.text = self.labelTitle;
}


-(void)createLabel{
 self.label = [[UILabel alloc] initWithFrame:CGRectMake(100, 40, 200, 50)];
 self.label.backgroundColor = [UIColor brownColor];
 self.label.text = self.labelTitle;
 self.label.textAlignment = NSTextAlignmentCenter;
 [self.view addSubview:self.label];
}

- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}
@end
```
######SecondViewController.h
```
#import <UIKit/UIKit.h>

@class SecondViewController;
@protocol SecondViewControllerDelegate <NSObject>

-(void)senderString:(NSString *)string formViewController:(SecondViewController *)secondViewController;

@end

@interface SecondViewController : UIViewController

@property(nonatomic, copy)NSString * lableTitle;

@property(nonatomic, assign)id<SecondViewControllerDelegate> delegate;

@property(nonatomic, copy) void (^senderBlcok)(NSString * string);

@end
```

---
##导航控制器