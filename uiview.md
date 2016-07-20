# UIView
---
##父视图与子视图的关系
####一个子视图只有一个父视图
####一个父视图可以有多个子视图
##UIView的创建方式
```
UIView * myView = [[UIView alloc] initWithFrame:CGRectMake(60, 100, 300, 300)];
    
myView.backgroundColor = [UIColor orangeColor];
myView.bounds = CGRectMake(50, 0, 300, 300);

[self.window addSubview:myView];
```
##
