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

>是否可编辑

```

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

## UITableViewCell 使用
####创建方式

####属性设置