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

##### upload上传数据

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

##### 下载数据\(要掌握对数据的存储位置的变更\)

```
//    1 创建url
    NSURL * url = [NSURL URLWithString:@"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg"];
//    2 设置请求
    NSURLRequest * request = [NSURLRequest requestWithURL:url];
//    3 创建会话,默认是有缓存，有cookies。。。。
    NSURLSession * session = [NSURLSession sharedSession];
    NSURLSessionDownloadTask * downLoadTask = [session downloadTaskWithRequest:request completionHandler:^(NSURL * _Nullable location, NSURLResponse * _Nullable response, NSError * _Nullable error) {
//        沙盒中的temp目录。会在其中创建一个临时文件。下载完成后删除
//        file:///Users/zhangzengqiang/Library/Developer/CoreSimulator/Devices/0C8E5CB1-7E70-403F-A366-01FC6F1B6DCB/data/Containers/Data/Application/8E2C5086-6FB4-42DB-8933-7B24A3F37F8E/tmp/CFNetworkDownload_8OaGGg.tmp
        NSLog(@"%@", location);

//        让文件转移到目录
        NSString * documentPath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;
        NSString * fullDocPath = [documentPath stringByAppendingPathComponent:[location lastPathComponent]];

        NSURL * fullDocURL = [NSURL fileURLWithPath:fullDocPath];

        [[NSFileManager defaultManager] moveItemAtURL:location toURL:fullDocURL error:nil];

        NSLog(@"%@",[NSThread currentThread]);

        dispatch_async(dispatch_get_main_queue(), ^{
            _imageView.image = [UIImage imageWithData:[NSData dataWithContentsOfURL:fullDocURL]];
        });

    }];

    [downLoadTask resume];
```

---

## 代理

### 遵从的协议&lt; NSURLSessionDelegate, NSURLSessionDataDelegate &gt;

### 声明格式：

#### 1、实现代理

#### 2、实现代理中的方法

### 例子：

#### 实现代理

```
    NSURLSession * session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:self delegateQueue:[[NSOperationQueue alloc] init]];
    NSURLSessionDataTask * dataTask = [session dataTaskWithURL:[NSURL URLWithString:@"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg"]];
    [dataTask resume];
```

#### 代理方法——

##### 1、服务器响应

```
- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask
didReceiveResponse:(NSURLResponse *)response
 completionHandler:(void (^)(NSURLSessionResponseDisposition disposition))completionHandler{
    NSLog(@"%s", __func__);
//    允许处理服务器的响应
    completionHandler(NSURLSessionResponseAllow);
}
```

##### 2、多次调用数据

```
- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask
    didReceiveData:(NSData *)data{
    NSLog(@"| %s", __func__);
}
```

##### 3、无论请求成功还是失败的都会调用的方法

```
-(void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task didCompleteWithError:(NSError *)error{
    NSLog(@"%s ++", __func__);
}
```

