# NSUserDefaults 和UITabbarController

---

## NSUserDefaults

> 注意事项：
> 
> > 1、是一个单例
> > 
> > 2、存储的内容实在一个plist文件中
> > 
> > 通过`NSHomeDirectory()`方法就能够打印出当前工程的路径。
> > 
> > ![](/assets/NSUerDefault存储的数据路径.png)
>>
>>3、内容删除一次之后，如果没有再存，那么就没有了

#####代码实现
```
// NSUserDefaults是个单例
 // 可以持久化数据
 NSUserDefaults * userDefaults = [NSUserDefaults standardUserDefaults];
 // NSLog(@"%@", userDefaults);
 // 把一个数据存储起来
 NSLog(@"%@", [userDefaults objectForKey:@"array"]);
 NSLog(@"%ld", [userDefaults integerForKey:@"value"]);

 // 删除一次就没了。通过key删除内容
 [userDefaults removeObjectForKey:@"array"];
```

```

```
---

## UITabbarController

### 功能

### 创建

### 页面添加

### 创建联系

### 修改属性

### 自定义

