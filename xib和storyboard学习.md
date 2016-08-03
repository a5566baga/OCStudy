#xib和storyboard学习

---
##xib学习
#####步骤
######1、创建
    【command】+【n】创建，选中XIB选项
![](/assets/xib创建.png)

     创建之后的内容
![](/assets/xib创建的内容.png)
######2、关联属性
    拖动的三种方式
    1、【control】+左键拖到关联上的.h或.m的声明中(右键直接拖也可)
    2、右键控件，然后拉到目的地
    3、先写声明，然后关联
![](/assets/XIB控件拖动.png)
######3、运行
     要加载xib内容的声明
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    _window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
//    ViewController * vc = [[ViewController alloc] init];
//    这个会去加载xib的内容
    ViewController * xibVc = [[ViewController alloc] initWithNibName:@"ViewController" bundle:nil];
    self.window.rootViewController = xibVc;
    [self.window makeKeyAndVisible];
    
    return YES;
}
```

---

##stroyboard

