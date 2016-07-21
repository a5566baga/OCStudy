# UITextField 学习

---

## UITextField 介绍

```
文本输入框,继承于 UIControl 间接继承 UIView 
```
---
##UITextField基本功能介绍
### 文本框的创建

```
self.textField = [[UITextField alloc] init];
self.textField.frame = CGRectMake(100, 50, 250, 50);
self.textField.backgroundColor = [UIColor clearColor];
[self.window addSubview:self.textField];
```
###文本框的外观


