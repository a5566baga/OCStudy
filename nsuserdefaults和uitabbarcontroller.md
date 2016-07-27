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
> > 
> > 3、内容删除一次之后，如果没有再存，那么就没有了

##### 代码实现

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

##### 利用代理传值

###### ViewController.m

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

###### AViewController.m

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

###### 注意：

> 在传值的时候，要注意值输出和修改的位置。

---

## UITabbarController

### 功能

```
UITabbarController创建的是一个底部的工具栏
```

### 创建

> 创建对象
> 
> > UITabBarController * tabBarController = [[UITabBarController alloc] init]; 
> >  self.window.rootViewController = tabBarController;
> 
> 创建导航控制栏
> 
> >  ViewController * vc = [[ViewController alloc] init]; 

### 页面添加

> 创建多个继承于 UIViewController 的视图

```
 AViewController * avc = [[AViewController alloc] init]; 
 // [tabBarController addChildViewController:avc];
 avc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_me_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 avc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_me_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 avc.title = @"好友";
 BViewController * bvc = [[BViewController alloc] init];
 // [tabBarController addChildViewController:bvc];
 bvc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_news_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 bvc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_news_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 bvc.title = @"新闻";
 CViewController * cvc = [[CViewController alloc]init];
 cvc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_media_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 cvc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_media_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 cvc.title = @"视频";
 DViewController * dvc = [[DViewController alloc]init];
 dvc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_reader_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 dvc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_reader_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 dvc.title = @"阅读";
 EViewController * evc = [[EViewController alloc]init];
 evc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_me_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 evc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_me_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 evc.title = @"联系人";
```

### 创建联系
>创建数组
>> NSArray * viewControllersArray = @[nvc, avc,bvc,cvc,dvc,evc]; 

>添加view视图
>> tabBarController.viewControllers = viewControllersArray; 

### 修改属性
######TabBar的属性
>背景颜色
>> [UITabBar appearance].barTintColor = [UIColor orangeColor]; 

>设置默认背景图片
>> nvc.tabBarItem.image = [[UIImage imageNamed:@"tabbar_icon_found_normal@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]; 

>设置选中状态下的图片背景
>> nvc.tabBarItem.selectedImage = [[UIImage imageNamed:@"tabbar_icon_found_highlight@2x"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]; 

>设置tabBar的title
>> nvc.title = @"开灯"; 

>设置背景图片与边界的距离
>> vc.tabBarItem.imageInsets = UIEdgeInsetsMake(0, 0, -10, -20); 

>显示消息内容
>> nvc.tabBarItem.badgeValue = @"New"; 

###### UINavigationController 属性
>返回按钮处的字体颜色
>>``` [UINavigationBar appearance].tintColor = [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5]; ```

>标题颜色设置
>>``` [UINavigationBar appearance].titleTextAttributes = @{NSFontAttributeName:[UIFont systemFontOfSize:30], NSForegroundColorAttributeName:[UIColor whiteColor]}; ```

>背景色
>>``` [UINavigationBar appearance].barTintColor = [UIColor blackColor]; ```

###利用数据持久化，让每次打开tabbar的时候，下面按钮的位置与上次自定义过后的位置相同
#####1、先存储内容
```

```
#####2、拿出来，遍历排序
### 自定义

