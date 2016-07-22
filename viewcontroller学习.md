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


---
##导航控制器