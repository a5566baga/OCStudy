# UIViewController学习

---

## 视图控制器

### 创建一个控制器

```
 self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
 self.window.backgroundColor = [UIColor grayColor];
// 创建一个控制器
 ViewController * VC = [[ViewController alloc] init];
// 把视图控制器与window联系起来
 self.window.rootViewController = VC;

 [self.window makeKeyAndVisible];
```

#### 注意：主控制器只能创建一个，并且不能够被删除。对于控制器编写的代码在AppDelegate.m里面。

### 视图控制器的声明周期

> -\(void\)viewDidLoad
> 
> > 在创建的时候就调用一次
> 
> -\(void\)viewDidUnload
> 
> > 不加载（现在已经弃用了）
> 
> -\(void\)viewWillAppear:\(BOOL\)animated
> 
> > 视图将要出现
> 
> -\(void\)viewDidAppear:\(BOOL\)animated
> 
> > 视图已经出现
> 
> -\(void\)viewWillDisappear:\(BOOL\)animated
> 
> > 视图将要消失
> 
> -\(void \)viewDidDisappear:\(BOOL\)animated
> 
> > 视图已经消失

#### 在写东西的时候，一般都在viewDidLoad中写。

### UIApplication 介绍

```
 UIApplication 是真正意义上的单例。
 UIApplication * application = [UIApplication sharedApplication]; 设置一些应用级的程序。
 application.networkActivityIndicatorVisible = YES; 使网速的图标一直转
 application.statusBarStyle = UIStatusBarStyleLightContent; 状态栏样式。
 application.statusBarHidden = YES; 隐藏状态栏
 如果要使statusBarStyle文件起作用，要先改plist文件， View controller-based status bar appearance 显示YES 。
```

#### 注意： 写视图布局，添加控件一般都写在viewDidLoad方法中。

### 创建视图

#### 在viewDidLoad中直接设置背景颜色等属性即可，添加的控件也写在里面。

```
- (void)viewDidLoad {
 [super viewDidLoad];
 self.view.backgroundColor = [UIColor yellowColor];
 // Do any additional setup after loading the view.
 [self createButton];
 [self createTextField];
}
```

#### 视图控制器的跳转

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

### 视图控制器之间使用代理传值

###### AppDelegate.h

```
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;

@end
```

###### AppDelegate.m

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

###### FirstViewController.h

```
#import <UIKit/UIKit.h>
#import "SecondViewController.h"

@interface FirstViewController : UIViewController<SecondViewControllerDelegate>

@property(nonatomic, copy)NSString * labelTitle;

@end
```

###### FirstViewController.m

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

###### SecondViewController.h

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

###### SecondViewController.m

```
#import "SecondViewController.h"
#import "FirstViewController.h"

@interface SecondViewController ()

@property(nonatomic, strong)UITextField * textField;

@end

@implementation SecondViewController

//只会被加载一次
- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view.

 self.view.backgroundColor = [UIColor brownColor];

 [self createButton];
 [self createTextField];
}


-(void)createTextField{
 self.textField = [[UITextField alloc] initWithFrame:CGRectMake(100, 60, 200, 50)];
 self.textField.backgroundColor = [UIColor blueColor];
 self.textField.textAlignment = NSTextAlignmentCenter;
 [self.view addSubview:self.textField];
}

-(void)onClick:(UIButton*)btn{
 NSLog(@"%s", __func__);
// FirstViewController * fVC = (FirstViewController *)[self presentedViewController];
// [self.textField resignFirstResponder];
// fVC.labelTitle = self.textField.text;

// 回调
// if (self.delegate != nil && [self.delegate respondsToSelector:@selector(senderString:formViewController:)]) {
// [self.delegate senderString:self.textField.text formViewController:self];
// }

// _strPublic = self.textField.text;

 if (self.delegate != nil && [self.delegate respondsToSelector:@selector(senderString:formViewController:)]) {
 [self.delegate senderString:self.textField.text formViewController:self];
 }
 [self dismissViewControllerAnimated:YES completion:^{
 // 防止产生循环引用
 // __weak typeof (self) weakSelf = self;
 // weakSelf.senderBlcok(self.textField.text);
 }];
}

-(void)createButton{
 UIButton * btn = [[UIButton alloc] initWithFrame:CGRectMake(20, 40, 20, 20)];
 btn.backgroundColor = [UIColor blueColor];
 [btn setTitle:@"P" forState:UIControlStateNormal];
 [self.view addSubview:btn];
 [btn addTarget:self action:@selector(onClick:) forControlEvents:UIControlEventTouchUpInside];
}

- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}

@end
```

---

## 导航控制器

### 创建导航控制器

##### 创建UINavigationBar对象，让它作为控制器

```
 self.window = [[UIWindow alloc] init];
 self.window.frame = [UIScreen mainScreen].bounds;
 self.window.backgroundColor = [UIColor grayColor];

 RootViewController * rVC = [[RootViewController alloc] init];
 UINavigationController * nvc = [[UINavigationController alloc] initWithRootViewController:rVC];

 [[UINavigationBar appearance] setBackgroundColor:[UIColor orangeColor]];

 [[UINavigationBar appearance] setTintColor:[UIColor redColor]];

 self.window.rootViewController = nvc;

// 总结：
// 导航栏高44，如果不变背景，如果背景图片大于44，那么渲染上面20的状态栏
// 小于也会渲染。只有等于44的时候才不渲染状态栏
 [[UINavigationBar appearance]setBackgroundImage:[UIImage imageNamed:@"Nav_Bg2"] forBarPosition:0 barMetrics:0];

 [self.window makeKeyAndVisible];
```

### 创建Button

```
 UIButton * btn = [[UIButton alloc] initWithFrame:CGRectMake(30, 80, 100, 40)];
 [btn setTitle:@"切下个页面" forState:UIControlStateNormal];
 [btn setTitleColor:[UIColor redColor] forState:UIControlStateHighlighted];
 [btn addTarget:self action:@selector(onClick:) forControlEvents:UIControlEventTouchUpInside];
 btn.backgroundColor = [UIColor blueColor];
 btn.tag = 100;
 [self.view addSubview:btn];

 UIButton * btn2 = [[UIButton alloc] initWithFrame:CGRectMake(150, 80, 100, 40)];
 [btn2 setTitle:@"返回前页面" forState:UIControlStateNormal];
 [btn2 setTitleColor:[UIColor redColor] forState:UIControlStateHighlighted];
 [btn2 addTarget:self action:@selector(onClick:) forControlEvents:UIControlEventTouchUpInside];
 btn2.backgroundColor = [UIColor blueColor];
 btn2.tag = 101;
 [self.view addSubview:btn2];
```

### 创建多个ViewController视图

### 通过Button的事件跳转视图
---

## 导航工具栏
###创建工具按钮
#####设置工具栏的背景
>设置背景颜色
>> [[UINavigationBar appearance] setBackgroundColor:[UIColor orangeColor]]; 

>设置填充颜色
>>[[UINavigationBar appearance] setTintColor:[UIColor redColor]];

>设置背景图片
>> [[UINavigationBar appearance]setBackgroundImage:[UIImage imageNamed:@"Nav_Bg2"] forBarPosition:0 barMetrics:0]; 

>注意：
>> 导航栏高44，如果不变背景，如果背景图片大于44，那么渲染上面20的状态栏; 小于也会渲染。只有等于44的时候才不渲染状态栏。

###创建 UIBarButtonItem 
#####左边的BarButton
```
UIBarButtonItem * leftBarbtn = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemCompose target:self action:@selector(action)];
self.navigationItem.leftBarButtonItems = @[leftBarbtn, leftBarbtn2];
```
#####右边的BarButton
```
UIBarButtonItem * rightBarbtn = [[UIBarButtonItem alloc] initWithTitle:@"WO" style:UIBarButtonItemStylePlain target:self action:@selector(action)];
self.navigationItem.rightBarButtonItem = rightBarbtn;
self.navigationItem.rightBarButtonItems = @[rightBarbtn, rightBarbtn2];
```
######系统自带的barbutton属性
>样式
>>```UIBarButtonSystemItemDone, 
 UIBarButtonSystemItemCancel,
 UIBarButtonSystemItemEdit,
 UIBarButtonSystemItemSave,
 UIBarButtonSystemItemAdd,
 UIBarButtonSystemItemFlexibleSpace,
 UIBarButtonSystemItemFixedSpace,
 UIBarButtonSystemItemCompose,
 UIBarButtonSystemItemReply,
 UIBarButtonSystemItemAction,
 UIBarButtonSystemItemOrganize,
 UIBarButtonSystemItemBookmarks,
 UIBarButtonSystemItemSearch,
 UIBarButtonSystemItemRefresh,
 UIBarButtonSystemItemStop,
 UIBarButtonSystemItemCamera,
 UIBarButtonSystemItemTrash,
 UIBarButtonSystemItemPlay,
 UIBarButtonSystemItemPause,
 UIBarButtonSystemItemRewind,
 UIBarButtonSystemItemFastForward,
 UIBarButtonSystemItemUndo NS_ENUM_AVAILABLE_IOS(3_0),
 UIBarButtonSystemItemRedo NS_ENUM_AVAILABLE_IOS(3_0),
 UIBarButtonSystemItemPageCurl NS_ENUM_AVAILABLE_IOS(4_0),```

>风格
>>```UIBarButtonItemStylePlain, 
 UIBarButtonItemStyleBordered NS_ENUM_DEPRECATED_IOS(2_0, 8_0, "Use UIBarButtonItemStylePlain when minimum deployment target is iOS7 or later"),
 UIBarButtonItemStyleDone,```

#####自定义的BarButton
```
UIButton * btn = [[UIButton alloc] initWithFrame:CGRectMake(0, 0, 30, 20)];
 btn.backgroundColor = [UIColor orangeColor];
UIBarButtonItem * btnItem = [[UIBarButtonItem alloc] initWithCustomView:btn];
self.navigationItem.rightBarButtonItem = btnItem;
```
#####自定义toolbar
######默认toolbar位置
```
 UIBarPositionAny = 0,
 UIBarPositionBottom = 1, // The bar is at the bottom of its local context, and directional decoration draws accordingly (e.g., shadow above the bar).
 UIBarPositionTop = 2, // The bar is at the top of its local context, and directional decoration draws accordingly (e.g., shadow below the bar)
 UIBarPositionTopAttached = 3, // The bar is at the top of the screen (as well as its local context), and its background extends upward—currently only enough for the status bar.
```
######默认toolbar样式
```
 UIBarMetricsDefault,
 UIBarMetricsCompact,
 UIBarMetricsDefaultPrompt = 101, // Applicable only in bars with the prompt property, such as UINavigationBar and UISearchBar
 UIBarMetricsCompactPrompt,

 UIBarMetricsLandscapePhone NS_ENUM_DEPRECATED_IOS(5_0, 8_0, "Use UIBarMetricsCompact instead") = UIBarMetricsCompact,
 UIBarMetricsLandscapePhonePrompt NS_ENUM_DEPRECATED_IOS(7_0, 8_0, "Use UIBarMetricsCompactPrompt") = UIBarMetricsCompactPrompt,
```
######第一种方式定义
```
[self.navigationController setToolbarHidden:NO];

 [self.navigationController.toolbar setBackgroundImage:[[UIImage imageNamed:@"header_bg64"] stretchableImageWithLeftCapWidth:10 topCapHeight:10] forToolbarPosition:UIBarPositionBottom barMetrics:0];

 self.toolbarItems = @[rightBarbtn2, rightBarbtn];
```

######第二种方式定义
```
 UIToolbar * toolbar = [[UIToolbar alloc] initWithFrame:CGRectMake(0, CGRectGetHeight([UIScreen mainScreen].bounds)/1.17, CGRectGetWidth(self.view.frame), 44)];
 toolbar.items = @[leftBarbtn, btnItem];
 toolbar.backgroundColor = [UIColor blackColor];
 [self.view addSubview:toolbar];
```

######自定义导航头的大小，颜色，位置
```
 UILabel * label = [[UILabel alloc] init];
 label.backgroundColor = [UIColor redColor];
 label.frame = CGRectMake(0, 40, CGRectGetWidth([UIScreen mainScreen].bounds), 40);
 label.text = @"MY ROOT";
self.navigationItem.titleView = label;
```

