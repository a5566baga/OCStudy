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
    只有数字和符号
 UIKeyboardTypeNumbersAndPunctuation, // Numbers and assorted punctuation.
    URL样式（字母和/、数字）
 UIKeyboardTypeURL, // A type optimized for URL entry (shows . / .com prominently).
    全是数字
 UIKeyboardTypeNumberPad, // A number pad (0-9). Suitable for PIN entry.
    拨号类型
 UIKeyboardTypePhonePad, // A phone pad (1-9, *, 0, #, with letters under the numbers).
    带字母的拨号界面
 UIKeyboardTypeNamePhonePad, // A type optimized for entering a person's name or phone number.
    邮箱类型
 UIKeyboardTypeEmailAddress, // A type optimized for multiple email address entry (shows space @ . prominently).
    数字。小数点
 UIKeyboardTypeDecimalPad NS_ENUM_AVAILABLE_IOS(4_1), // A number pad with a decimal point.
    发微博用的
 UIKeyboardTypeTwitter NS_ENUM_AVAILABLE_IOS(5_0), // A type optimized for twitter text entry (easy access to @ #)
    查询
 UIKeyboardTypeWebSearch NS_ENUM_AVAILABLE_IOS(7_0), // A default keyboard type with URL-oriented addition (shows space . prominently).
    与ASCII相同
 UIKeyboardTypeAlphabet = UIKeyboardTypeASCIICapable, // Deprecated
```
#####使用说明代码
> self.textField.keyboardType = UIKeyboardTypeNamePhonePad; 

#### 右下角enter键的风格(默认状态下)
```
 UIReturnKeyDefault,
 UIReturnKeyGo,
 UIReturnKeyGoogle,
 UIReturnKeyJoin,
 UIReturnKeyNext,
 UIReturnKeyRoute,
 UIReturnKeySearch,
 UIReturnKeySend,
 UIReturnKeyYahoo,
 UIReturnKeyDone,
 UIReturnKeyEmergencyCall,
 UIReturnKeyContinue NS_ENUM_AVAILABLE_IOS(9_0),
```
#####使用代码
> self.textField.returnKeyType = UIReturnKeyJoin; 

####设置键盘上方区域的内容
> self.textField.inputAccessoryView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"Action_ReadSun@2x.png"]]; 

####文本框内显示提示文字
> self.textField.placeholder = @"请输入姓名:"; 

####文本框自适应并且设置最小字体
> self.textField.minimumFontSize = 10; 

> self.textField.adjustsFontSizeToFitWidth = YES; 

####对于文本框内的单词自动大写
```
    不纠正
 ITextAutocapitalizationTypeNone,
    纠正单词
 UITextAutocapitalizationTypeWords,
    纠正首单词句子
 UITextAutocapitalizationTypeSentences,
    纠正所有单词
 UITextAutocapitalizationTypeAllCharacters,
```
#####代码演示
>  tf.autocapitalizationType = ITextAutocapitalizationTypeAllCharacters;  

####纠正单词拼写
```
 UITextAutocorrectionTypeDefault,
 UITextAutocorrectionTypeNo,
 UITextAutocorrectionTypeYes,
```
#####代码的展示
> tf.autocorrectionType = UITextAutocorrectionTypeYes; 

####对于键盘进入视图自动弹出
> [self.textField becomeFirstResponder]; 

####对于键盘，点击空白区域，键盘自动消失(重写方法)
```
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
// 键盘消失
 [self.textField resignFirstResponder];
}
```
---
###编辑文本时，响应的7个方法
```
// return NO to disallow editing.
- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField{
 NSLog(@"%s", __func__);
 return YES;
}

// became first responder
- (void)textFieldDidBeginEditing:(UITextField *)textField{
 NSLog(@"%s", __func__);
}

// return YES to allow editing to stop and to resign first responder status. NO to disallow the editing session to end
- (BOOL)textFieldShouldEndEditing:(UITextField *)textField{
 NSLog(@"%s", __func__);

 return YES;
}

// may be called if forced even if shouldEndEditing returns NO (e.g. view removed from window) or endEditing:YES called
- (void)textFieldDidEndEditing:(UITextField *)textField{
 NSLog(@"%s", __func__);
}

 // return NO to not change text
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
 NSLog(@"%s", __func__);
 NSLog(@"%@",string);
 return YES;
}

// called when clear button pressed. return NO to ignore (no notifications)
- (BOOL)textFieldShouldClear:(UITextField *)textField{
 NSLog(@"%s", __func__);
 return YES;
}

// called when 'return' key pressed. return NO to ignore.
- (BOOL)textFieldShouldReturn:(UITextField *)textField{
 NSLog(@"%s", __func__);
 [self.textField resignFirstResponder];
 return YES;
}
```
---

##