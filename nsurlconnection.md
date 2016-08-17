# NSURLConnection学习

---

## NSURLConnection的常用类

#### NSURL：请求地址

```
NSURL * url = [NSURL URLWithString:@"https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo_top_ca79a146.png"];
```

#### NSURLRequest：封装一个请求，保存发给服务器的全部数据，包括一个NSURL对象，请求方法、请求头、请求体....

```
NSURLRequest * request = [[NSURLRequest alloc] initWithURL:url];
```

#### NSMutableURLRequest：NSURLRequest的子类

```
NSMutableURLRequest * request = [[NSMutableURLRequest alloc] initWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:1];
```

#### NSURLConnection：负责发送请求，建立客户端和服务器的连接。发送NSURLRequest的数据给服务器，并收集来自服务器的响应数据

```
NSURLConnection * connect = [[NSURLConnection alloc] initWithRequest:request delegate:self startImmediately:NO];
```

---

## NSURLConnection详细使用

### 步骤

> （1）创建一个NSURL对象，设置请求路径（设置请求路径）
> 
> （2）传入NSURL创建一个NSURLRequest对象，设置请求头和请求体（创建请求对象）
> 
> （3）使用NSURLConnection发送NSURLRequest（发送请求）

#### 同步获取数据

```
-(void)syncDownLoad{
    //    1、请求的URL
    NSURL * url = [NSURL URLWithString:@"https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo_top_ca79a146.png"];
    //    2、创建一个请求对象(注：创建对象是就会有请求头中的参数)
    NSURLRequest * request = [[NSURLRequest alloc] initWithURL:url];
    //    3、发送请求
    NSURLResponse * response = nil;
    NSError * err = nil;
    //    注：response会获取响应头中的数据
    NSData * data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&err];

    UIImage * image = [UIImage imageWithData:data];
    self.imageVIew.image = image;
}
```

#### 异步获取数据

```
-(void)asyncDownLoad{
    //    1、请求的URL, 如果是https请求，以前那些请求不可用
    NSURL * url = [NSURL URLWithString:@"https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo_top_ca79a146.png"];
    //    2、创建一个请求对象(注：创建对象是就会有请求头中的参数)
    //    默认是get请求
    //    NSURLRequest * request = [[NSURLRequest alloc] initWithURL:url];

    //    如果规定是post，那么就创建
    //    NSMutableURLRequest * request = [[NSMutableURLRequest alloc] initWithURL:url];
    //    我们可以设置超时的时间,要开plist
    NSMutableURLRequest * request = [[NSMutableURLRequest alloc] initWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:1];
    [request setHTTPMethod:@"GET"];
    //    request.HTTPBody post请求时需要用到
    [request setValue:@"image/png" forHTTPHeaderField:@"Accept-Encoding"];

    //    3、发送请求
    [NSURLConnection sendAsynchronousRequest:request queue:[[NSOperationQueue alloc] init] completionHandler:^(NSURLResponse * _Nullable response, NSData * _Nullable data, NSError * _Nullable connectionError) {
        NSLog(@"%@", [NSThread currentThread]);
        UIImage * image = [UIImage imageWithData:data];
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            self.imageVIew.image = image;
        }];
    }];

    //    [NSThread sleepForTimeInterval:1];
    NSLog(@"---------");
    NSLog(@"--------%@", [NSThread currentThread]);
}
```

#### 使用代理

> 通过类方法或对象初始化方法可以创建代理
> 
> startImmediately:是否start立即执行
> 
> 不是立即执行，我们需要手动启动
> 
> 代理是：NSURLConnectionDataDelegate

```
-(void)connectionDelegate{
    //    创建
    _downData = [[NSMutableData alloc] init];

    //    1、请求的URL
    NSURL * url = [NSURL URLWithString:@"http://4k.znds.com/20140314/8kznds1.jpg"];
    //    2、创建一个请求对象(注：创建对象是就会有请求头中的参数)
    NSURLRequest * request = [[NSURLRequest alloc] initWithURL:url];

    //    [NSURLConnection connectionWithRequest:request delegate:self];

    //    [[NSURLConnection alloc] initWithRequest:request delegate:self startImmediately:YES];

    NSURLConnection * connect = [[NSURLConnection alloc] initWithRequest:request delegate:self startImmediately:NO];
    //    如果不是立即执行。我们必须手动启动它
    [connect start];
    //    取消
//    [connect cancel];

    [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
}
```

```
//接收回应，证明连接有响应，已经下载完响应头，开始下载数据
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response{
    NSLog(@"%s", __func__);
}
```

```
//接收数据，会根据下载数据的大小，进行多次调用
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data{
    NSLog(@"%s", __func__);
    [self.downData appendData:data];
    NSLog(@"%ld", data.length);
}
```

```
//请求结束，加载完成。
- (void)connectionDidFinishLoading:(NSURLConnection *)connection{
    NSLog(@"%s", __func__);
    UIImage * image = [UIImage imageWithData:self.downData];

    self.imageVIew.image = image;
    [UIApplication sharedApplication].networkActivityIndicatorVisible = NO;
}
```

```
//NSURLConnectionDelegate
//请求失败，连接发生错误。原因：断网、网址不对。
- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error{
    NSLog(@"%s", __func__);
}
```

#### 发送请求的步骤：

> 1.设置请求路径
> 2.创建请求对象
> 3.发送请求
>     3.1发送同步请求（一直在等待服务器返回数据，这行代码会卡住，如果服务器，没有返回数据，那么在主线程UI会卡住不能继续执行操作）有返回值
>     3.2发送异步请求：没有返回值
> 
> 说明：任何NSURLRequest默认都是get请求。
> 注意：GET请求中不存在请求体，因为所有的信息都写在URL里面。在IOS里面，请求行和请求头都不用写。



---

## 断点续传



