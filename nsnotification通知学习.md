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

####4、注意事项