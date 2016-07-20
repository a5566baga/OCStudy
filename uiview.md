# UIView
---
##父视图与子视图的关系
####一个子视图只有一个父视图
####一个父视图可以有多个子视图
>注：
*      如果A视图添加到B视图上，那么就称B视图是A视图的父视图，A是B的子视图。
*      一个视图只能有一个父视图，但一个视图可以有多个子视图。
*      一个视图可以直接加到另一个视图上。

##UIView的创建方式
```
UIView * myView = [[UIView alloc] initWithFrame:CGRectMake(60, 100, 300, 300)];
    
myView.backgroundColor = [UIColor orangeColor];
myView.bounds = CGRectMake(50, 0, 300, 300);

[self.window addSubview:myView];
```
##
