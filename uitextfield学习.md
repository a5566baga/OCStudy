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
---
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
---
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

####文本框左边和右边添加图标
#####自带显示图标属性
```
 UITextFieldViewModeNever, 
 UITextFieldViewModeWhileEditing,
 UITextFieldViewModeUnlessEditing,
 UITextFieldViewModeAlways
```
#####代码展示
> self.textField.leftView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"Action_Alias@2x.png"]]; 

> self.textField.leftViewMode = UITextFieldViewModeAlways; 

---
###键盘的设计
#### UIKeyboardAppearance 键盘样式
```
UIKeyboardAppearanceDefault, // Default apperance for the current input method.
UIKeyboardAppearanceDark NS_ENUM_AVAILABLE_IOS(7_0),
UIKeyboardAppearanceLight NS_ENUM_AVAILABLE_IOS(7_0),
UIKeyboardAppearanceAlert = UIKeyboardAppearanceDark, // Deprecated
```
#####使用说明代码
> self.textField.keyboardAppearance = UIKeyboardAppearanceDark; 

#### UIKeyboardType 键盘样式
```
    默认样式
 UIKeyboardTypeDefault, // Default type for the current input method.
    ASCII码样式
 UIKeyboardTypeASCIICapable, // Displays a keyboard which can enter ASCII characters, non-ASCII keyboards remain active
    
 UIKeyboardTypeNumbersAndPunctuation, // Numbers and assorted punctuation.
 UIKeyboardTypeURL, // A type optimized for URL entry (shows . / .com prominently).
 UIKeyboardTypeNumberPad, // A number pad (0-9). Suitable for PIN entry.
 UIKeyboardTypePhonePad, // A phone pad (1-9, *, 0, #, with letters under the numbers).
 UIKeyboardTypeNamePhonePad, // A type optimized for entering a person's name or phone number.
 UIKeyboardTypeEmailAddress, // A type optimized for multiple email address entry (shows space @ . prominently).
 UIKeyboardTypeDecimalPad NS_ENUM_AVAILABLE_IOS(4_1), // A number pad with a decimal point.
 UIKeyboardTypeTwitter NS_ENUM_AVAILABLE_IOS(5_0), // A type optimized for twitter text entry (easy access to @ #)
 UIKeyboardTypeWebSearch NS_ENUM_AVAILABLE_IOS(7_0), // A default keyboard type with URL-oriented addition (shows space . prominently).
 UIKeyboardTypeAlphabet = UIKeyboardTypeASCIICapable, // Deprecated
```
