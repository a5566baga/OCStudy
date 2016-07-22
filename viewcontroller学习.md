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
####注意：主控制器只能创建一个，并且不能够被删除

---
##导航控制器