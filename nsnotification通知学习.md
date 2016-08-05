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

---

##自定义通知
####思路：
#####1、创建一个单例
```
static MyNotificationCenter * notification = nil;
+(instancetype)defaultCenter{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        notification = [[MyNotificationCenter alloc] init];
    });
    return notification;
}
```
#####2、仿照系统创建方法
> -(void)addObserver:(id)observer selector:(SEL)aSelector name:(nullable NSString *)aName object:(nullable id)anObject;

```
-(void)addObserver:(id)observer selector:(SEL)aSelector name:(NSString *)aName object:(id)anObject{
    
    Model * model = [[Model alloc] initWithObserver:observer selecto:aSelector name:aName object:anObject];
    [self.modelArray addObject:model];
    
}
```

>-(void)postNotificationName:(NSString *)aName object:(nullable id)anObject userInfo:(nullable NSDictionary *)aUserInfo;

```
-(void)postNotificationName:(NSString *)aName object:(id)anObject userInfo:(NSDictionary *)aUserInfo{
    for (Model * model in self.modelArray) {
        if ([model.aName isEqualToString:aName]) {
            [model.observer performSelector:model.aSelector withObject:aUserInfo];
        }
    }
}
```

>-(void)removeObserver:(id)observer;

```
-(void)removeObserver:(id)observer{
    for (NSInteger i = self.modelArray.count-1; i >= 0; i--) {
        if (self.modelArray[i].observer == observer) {
            [self.modelArray removeObjectAtIndex:i];
        }
    }
}
```

######之后的使用方式如同系统使用一样
#######全部代码如下
- MyNotificationCenter.h
```
#import <Foundation/Foundation.h>
#import "Model.h"

@interface MyNotificationCenter : NSObject

+(instancetype)defaultCenter;

- (void)addObserver:(id)observer selector:(SEL)aSelector name:(nullable NSString *)aName object:(nullable id)anObject;

- (void)postNotificationName:(NSString *)aName object:(nullable id)anObject userInfo:(nullable NSDictionary *)aUserInfo;

- (void)removeObserver:(id)observer;

@end
```
