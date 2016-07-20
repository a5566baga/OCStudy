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
##当子视图超出父视图部分的操作
>减去子视图超出的部分
>>myView.clipsToBounds = YES;  

>透明度会对子视图产生影响
>>myView.alpha = 0.1;  透明度会对子视图产生影响

>隐藏设置也会对子视图产生影响
>>myView.hidden = YES; 

>父视图内子视图的不会响应。若为YES的时候，子视图超出父视图部分也不会响应。注：UILabel和UIImageView的事件响应开关，是view.userInteractionEnabled = NO。
>>myView.userInteractionEnabled = NO;  

>获得视图的方法
>>```
[myView viewWithTag:100];
UIView * redView = [self.window viewWithTag:100];
UIView * yellow = [self.window viewWithTag:1002];
UIView * green = [self.window viewWithTag:1000];
```

>获取所有的子视图
>>NSArray * array = [self.window subviews];

>获得父视图的方法
>>[greenView superview];

---
##视图的操作
###位置的创建
####center
    center的位置在平移过会不会变
####bounds
    bounds的位置大部分时间是(0,0)。如果定义，那么是对子视图的限制
####frame
    frame大小改变是会改变的
#####代码展示
```

```
