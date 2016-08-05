#NSNotification通知学习

---

##NSNotificationCenter
####1、创建,注册通知
```
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(revice:) name:@"notification" object:nil];
```
####2、使用
######选择器中的方法是显示传入的信息
```
-(void)revice:(NSNotification *)notification{
//notification.userInfo是发送的数据
    NSLog(@"%@", notification.userInfo);
    self.firstLabel.text = notification.userInfo[@"key"];
}
```
######post传值，提交发送的数据
```
[[NSNotificationCenter defaultCenter] postNotificationName:@"notification" object:nil userInfo:@{@"key":@"value"}];
```
######删除
>删除self对象上的全部通知
```
[[NSNotificationCenter defaultCenter] removeObserver:self];
```
>删除某个通知
```
[[NSNotificationCenter defaultCenter] removeObserver:self name:@"notification" object:nil];
```

####3、作用
    1、用作逆向传值使用
    2、可以解耦合    
####4、注意事项
    如果@selecter中的方法有参数，那么参数类型是NSNotification; 
        name是通知的名字,可以区分是哪一个通知；
        obj如果存放了一对象，那么你发送通知时也必须有一个相同的对象。否则通知发送不成功。
    发送通知，通过name判断，name名字一样就发送消息。