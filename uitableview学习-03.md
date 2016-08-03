#UITableView的自定义

---
##MVC模式下的自定义
    1、创建model层(放创建的模型层)
    2、创建controller层(控制层)
    3、创建view层(显示层)
    4、创建source层(放图片、plist文件)
    5、Supporting Files (系统自带目录，放一些其它的内容)


###model层的设置
######1、通过对要解析的内容设置属性值
######2、-(instancetype)initWithDictionary:(NSDictionary *)dic;这个方法，把对应的值附到属性上。通过[self setValuesForKeysWithDictionary:dic];这个方法完成赋值，然后再把对象传到数组中去。
######3、如果要是找不到对应的key，需要用系统的方法进行来判断
```
//找不到则执行这个方法
-(void)setValue:(id)value forUndefinedKey:(NSString *)key{
// 可以对对象找不到的key进行一定的处理
}

```
```
//对找的到的属性的key进行处理，如果是空实现，那么所有的属性也无法赋值
-(void)setValue:(id)value forKey:(NSString *)key{
    如果是数组的话就在这里赋值
}
```

