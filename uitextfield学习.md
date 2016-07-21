# UITextField 学习

---

## UITextField 介绍

```
文本输入框,继承于 UIControl 间接继承 UIView 
```

---

## UITextField基本功能介绍

### 文本框的创建

```
self.textField = [[UITextField alloc] init];
self.textField.frame = CGRectMake(100, 50, 250, 50);
self.textField.backgroundColor = [UIColor clearColor];
[self.window addSubview:self.textField];
```

### 文本框的外观

#### 边框自带的方法

> 1、UITextBorderStyleNone  
> 2、UITextBorderStyleLine 
> 3、UITextBorderStyleBezel 
> 4、UITextBorderStyleRoundedRect
> 
> 注意： 如果边框设置成了UITextBorderStyleRoundedRect，那么背景图片不起作用

#####示例 （通过stretchableImageWithLeftCapWidth设置背景图片）
>self.textField.borderStyle = UITextBorderStyleNone; 

>self.textField.background = [[UIImage imageNamed:@"qb_tenpay_dialog_input_bg@2x"]stretchableImageWithLeftCapWidth:10 topCapHeight:5]; 

####设置文本框内容样式
```
self.textField.textColor = [UIColor redColor];
self.textField.font = [UIFont systemFontOfSize:30];
self.textField.textAlignment = NSTextAlignmentCenter;
```
###文本框内部内容
####系统自带的删除内容按钮
```
 UITextFieldViewModeNever,
 编辑时显示清除按钮
 UITextFieldViewModeWhileEditing,
 非编辑状态下清除按钮
 UITextFieldViewModeUnlessEditing,
 总是有清除按钮
 UITextFieldViewModeAlways
```
#####使用
> self.textField.clearButtonMode = UITextFieldViewModeAlways; 

####安全输入（输入全为*）
> self.textField.secureTextEntry = YES; 

