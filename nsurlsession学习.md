# NSURLSession学习

---

## 基本用法

#### 步骤：

> 1、创建请求
> 
> 2、创建一个session
> 
> 3、明确一个任务
> 
> 4、启用任务

##### 示例

```
//    1、创建一个请求
    NSURL * url = [NSURL URLWithString:@"http://v.juhe.cn/toutiao/index?type=%E7%A7%91%E6%8A%80&key=f47260da59b9944239ef813344918073"];
    NSURLRequest * request = [NSURLRequest requestWithURL:url];
    //    2、创建一个session(会话控制)
    NSURLSession * urlSession = [NSURLSession sharedSession];
    //    明确一个任务
    NSURLSessionDataTask * dataTask = [urlSession dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        NSDictionary * dic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableLeaves error:nil];
        NSLog(@"%@", dic);
    }];
    //    启用任务
    [dataTask resume];
```

##### post方法提交信息

> #### 注意：
> 
> ##### 可能出现字符串编码问题
> 
> ```
> [@"" stringByAddingPercentEncodingWithAllowedCharacters:<#(nonnull NSCharacterSet *)#>]
> ```
> 
> ```
> [@"" stringByAddingPercentEscapesUsingEncoding:<#(NSStringEncoding)#>]
> ```

```
NSURL * url = [NSURL URLWithString:@"http://v.juhe.cn/toutiao/index"];
NSMutableURLRequest * request = [NSMutableURLRequest requestWithURL:url];
NSString * parm = @"type=科技&key=f47260da59b9944239ef813344918073";
request.HTTPMethod = @"POST";
request.HTTPBody = [parm dataUsingEncoding:NSUTF8StringEncoding];
NSURLSession * urlSession = [NSURLSession sharedSession];
NSURLSessionDataTask * dataTask = [urlSession dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        NSDictionary * dic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableLeaves error:nil];
        NSLog(@"%@", dic);
    }];
[dataTask resume];
```

upload上传数据

```
NSURL * url = [NSURL URLWithString:@"http://v.juhe.cn/toutiao/index?type=%E7%A7%91%E6%8A%80&key=f47260da59b9944239ef813344918073"];
NSURLRequest * request = [NSURLRequest requestWithURL:url];
NSURLSession * urlSession = [NSURLSession sharedSession];
//创建一个上传数据（虚拟的）
NSString * uploadData = @"abcdefghijklmn";
NSURLSessionUploadTask * uploadTask = [urlSession uploadTaskWithRequest:request fromData:[uploadData dataUsingEncoding:NSUTF8StringEncoding] completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        NSString * content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
        NSLog(@"%@", content);
    }];
[uploadTask resume];
```

---

## 数据下载相关

