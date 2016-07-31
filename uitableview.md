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

>设置footerView

>设置分割线的颜色

>是否可编辑

>row的高度

>是否可以被选中


##代理

## UITableViewCell 使用
####创建方式

####属性设置