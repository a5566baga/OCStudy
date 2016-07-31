#UITableView学习

---
###注意点：
    注意：如果是导航控制器，tableView的(0, 0)会向下移动64(针对这个属性是YES)；如果这个属性为NO，那么不会向下移动64. 
    self.automaticallyAdjustsScrollViewInsets = YES;

##创建
>创建语句
>>```UITableView * tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, CGRectGetWidth([UIScreen mainScreen].bounds), CGRectGetHeight(self.view.frame)) style:UITableViewStyleGrouped]; ```

>风格，tableView的风格
>>``` UITableViewStylePlain, // regular table view 
      UITableViewStyleGrouped // preferences style table view```


##常用属性
>设置headerView

######注意： header如果不设置tableHeaderView的高度，由视图本身大小决定。 
``` 
UIView * headerView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, CGRectGetWidth(self.view.frame), 200)]; 
 headerView.backgroundColor = [UIColor grayColor];
 tableView.tableHeaderView = headerView;
```

>设置footerView

```
 UIView * footerView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, CGRectGetWidth(self.view.frame), 100)];
 footerView.backgroundColor = [UIColor brownColor];
 tableView.tableFooterView = footerView;
```

>设置分割线的颜色

```
tableView.separatorColor = [UIColor redColor];
```

> 设置分割线的样式 

```
 UITableViewCellSeparatorStyleNone,
 UITableViewCellSeparatorStyleSingleLine,
 UITableViewCellSeparatorStyleSingleLineEtched
```
```
tableView.separatorStyle = UITableViewCellSeparatorStyleSingleLine;
```

>row的高度

```
tableView.rowHeight = 20;
```

>是否可以被选中

```
 tableView.allowsSelection = NO;
```


##代理
######遵循的协议
>UITableViewDelegate

>UITableViewDataSource

必须要实现的方法：
```
 - (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section; 
```
```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
```

## UITableViewCell 使用
####创建方式
######要写的代理方法
```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
```
######复用池的使用
```
static NSString * identifer = @"cellId";
UITableViewCell * cell = [tableView dequeueReusableCellWithIdentifier:identifer];
if (nil == cell) {
 cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:@"cellId"];
}
```
######cell的样式
```
 UITableViewCellStyleDefault, // Simple cell with text label and optional image view (behavior of UITableViewCell in iPhoneOS 2.x)
 imageView textLabel detailTextLabel

 UITableViewCellStyleValue1, // Left aligned label on left and right aligned label on right with blue text (Used in Settings)
 textLabel detailTextLabel

 UITableViewCellStyleValue2, // Right aligned label on left with blue text and left aligned label on right (Used in Phone/Contacts)
 image textLabel detailTextLabel(在textLabel下面)

 UITableViewCellStyleSubtitle // Left aligned label on top and left aligned label on bottom with gray text (Used in iPod).
```

####属性设置
>右侧属性样式

```
UITableViewCellAccessoryNone, // don't show any accessory view
 UITableViewCellAccessoryDisclosureIndicator, // regular chevron. doesn't track

 UITableViewCellAccessoryDetailDisclosureButton __TVOS_PROHIBITED, // info button w/ chevron. tracks

 UITableViewCellAccessoryCheckmark, // checkmark. doesn't track

 UITableViewCellAccessoryDetailButton NS_ENUM_AVAILABLE_IOS(7_0) __TVOS_PROHIBITED // info button. tracks
```
