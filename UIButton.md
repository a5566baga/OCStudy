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