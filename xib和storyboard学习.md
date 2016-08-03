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

#####创建步骤
######1、创建
![](/assets/storyBoard创建.png)
######2、拖控件与之前xib类似
######3、添加navigationController和tabBar
        1、选中一个ViewController
        2、点击【Editor】->【Embed In】
        3、选择要用的，一般要是都用的话，只创建一个tabBar，多个view创建多个navigationController
![](/assets/navcationbar创建.png)
######4、注册使用
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    _window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor redColor];
//    ViewController * Vc = [[ViewController alloc] init];
//    获取storyBoard文件
    UIStoryboard * storyBoard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
//    获取
//    UIViewController * vc = [storyBoard instantiateInitialViewController];
    
//    UIViewController * vc = [storyBoard instantiateViewControllerWithIdentifier:@"BlueViewController"];
    
    ViewController * vc = [storyBoard instantiateInitialViewController];
    self.window.rootViewController = vc;
    
    [self.window makeKeyAndVisible];
    
    return YES;
}
```
######5、每个ViewController都要对应一个文件，在文件中进行相关修改