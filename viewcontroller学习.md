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
---
##导航控制器