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

```
#####2、可以通过嵌套赋值setValue:forKeyPath,里面可以用.语法

