#KVC&KVO

---

##KVC
    NSKeyValueCoding 的简称 是通过字符串key来访问属性的方式
####使用方式
#####1、用setValue:forKey的方法为属性赋值
######创建对象，里有banner、speed属性
```
@property(nonatomic,  copy)NSString * banner;
@property(nonatomic, strong)NSNumber * speed;
```
######在外面可以通过KVC方式进行赋值
```
[car setValue:@"BMW" forKey:@"banner"];
[car setValue:@120 forKey:@"speed"];
```
######通过字典进行赋值
```
NSDictionary * dic = @{@"banner":@"AUTO", @"speed":@300};
[car setValuesForKeysWithDictionary:dic];
```
######注意：
>1、当通过此方法添加不存在的key的时候，要重写此方法

```
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{
    
}
```
>2、当如果传递数组的时候，不会赋值，这时候要重写这个方法

```
-(void)setValue:(id)value forKey:(NSString *)key{
    这里判断数组，并且给数组赋值
}
```
#####2、可以通过嵌套赋值setValue:forKeyPath,里面可以用.语法
######为Car类中添加一个Engine属性
```
@property(nonatomic, strong)Engine * engine;
```
#####为Engine定义一个属性
```
@property(nonatomic, strong)NSNumber * power;
```
######通过path传递，能把engine对象中的属性赋值
```
[car setValue:@300 forKeyPath:@"engine.power"];
```

#####自定义
```
//自定义的setValue forKey 的简单实现原理
-(void)mySetValue:(id)value forKey:(NSString *)key{
    NSString * selStr = [NSString stringWithFormat:@"set%@:", [key capitalizedString]];
    SEL sel = NSSelectorFromString(selStr);
    if ([self respondsToSelector:sel]) {
        [self performSelector:sel withObject:value];
    }else{
        [self mySetValue:value forUnderFindKey:key];
    }
}

-(void)mySetValue:(id)value forUnderFindKey:(NSString *)key{
    NSLog(@"%@", value);
}
```
---

#KVO
    KVO (NSKeyValueObserving)
