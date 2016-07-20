# UIButton学习
---
##UIButton
####UIbutton 也是视图类,继承于 UIControl, 间接继承于 UIView,是一个按钮类.
---
##继承关系
####UIButton——>UIControl——->UIView
---
##基本属性
####背景色
>button.backgroundColor = [UIColor grayColor];

####Button状态
>UIControlStateNormal       正常状态
>
>UIControlStateHighlighted  高亮状态
>  
>UIControlStateDisabled 禁用状态
>
>UIControlStateSelected     选中状态

####正常状态下的按钮
>[button setTitle:@" Follow" forState:UIControlStateNormal];

####高亮状态的按钮
>[button setTitle:@"What?" forState:UIControlStateHighlighted];

####禁用状态按钮
>[button setTitle:@"禁用" forState: UIControlStateDisabled];

####选中状态的按钮
>[button setTitle:@"选中" forState: UIControlStateSelected];

####设置正常标题的颜色
>[button setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];

####设置高亮标题的颜色
>[button setTitleColor:[UIColor yellowColor] forState:UIControlStateHighlighted];

####改变button的字体
>button.titleLabel.font = [UIFont fontWithName:@"Zapfino" size:30.0];

####创建图片对象
>UIImage *image = [UIImage imageNamed:@"bgSelected"];

####设置背景图片正常
####设置背景图片高亮
####设置前置图片正常
####设置前置图片高亮