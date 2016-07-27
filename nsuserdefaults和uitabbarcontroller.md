# NSUserDefaults 和UITabbarController

---

## NSUserDefaults

> 注意事项：
> 
> > 1、是一个单例
> > 
> > 2、存储的内容实在一个plist文件中
> > 
> > 通过`NSHomeDirectory()`方法就能够打印出当前工程的路径。
> > 
> > ![](/assets/NSUerDefault存储的数据路径.png)
>>
>>3、内容删除一次之后，如果没有再存，那么就没有了

#####代码实现
```
// NSUserDefaults是个单例
 // 可以持久化数据
 NSUserDefaults * userDefaults = [NSUserDefaults standardUserDefaults];
 // NSLog(@"%@", userDefaults);
 // 把一个数据存储起来
 NSLog(@"%@", [userDefaults objectForKey:@"array"]);
 NSLog(@"%ld", [userDefaults integerForKey:@"value"]);

 // 删除一次就没了。通过key删除内容
 [userDefaults removeObjectForKey:@"array"];
```
#####利用代理传值
######ViewController.m
```
- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view, typically from a nib.

 self.view.backgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];

// 补充：
 AppDelegate * appD = [UIApplication sharedApplication].delegate;
 appD.name = @"zhang";
 NSLog(@"%@", appD.name);

}

-(void)viewWillAppear:(BOOL)animated{
 AppDelegate * appD = [UIApplication sharedApplication].delegate;
 appD.name = @"zhang";
 NSLog(@"viewWillAppear %@", appD.name);
}

//-(void)viewWillDisappear:(BOOL)animated{
// AppDelegate * appD = [UIApplication sharedApplication].delegate;
// appD.name = @"zhang";
// NSLog(@"viewWillDisappear %@", appD.name);
//}


-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
 AViewController * avc = [[AViewController alloc] init];
 [self presentViewController:avc animated:YES completion:nil];
}

-(void)test{
 // NSUserDefaults是个单例
 // 可以持久化数据
 NSUserDefaults * userDefaults = [NSUserDefaults standardUserDefaults];
 // NSLog(@"%@", userDefaults);
 // 把一个数据存储起来
 NSLog(@"%@", [userDefaults objectForKey:@"array"]);
 NSLog(@"%ld", [userDefaults integerForKey:@"value"]);

 // 删除一次就没了。通过key删除内容
 [userDefaults removeObjectForKey:@"array"];

}

- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}
```
######AViewController.m
```
- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view.
 self.view.backgroundColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5];

 AppDelegate * appD = [UIApplication sharedApplication].delegate;

 NSLog(@"AViewController %@", appD.name);

 appD.name = @"wang";
}


-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
 [self dismissViewControllerAnimated:YES completion:nil];
}

- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}
```
######注意：
>在传值的时候，要注意值输出和修改的位置。

---

## UITabbarController

### 功能
    UITabbarController创建的是一个底部的工具栏

### 创建
>创建对象
>> UITabBarController * tabBarController = [[UITabBarController alloc] init]; 
 self.window.rootViewController = tabBarController;
>创建导航控制栏
>> ViewController * vc = [[ViewController alloc] init]; 
 UINavigationController * nvc = [[UINavigationController alloc] initWithRootViewController:vc];


### 页面添加

### 创建联系

### 修改属性

### 自定义

