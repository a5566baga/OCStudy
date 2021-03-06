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

### 思路：

> #### 要明确在在哪里操作，我们下载的东西存在沙盒的tmp目录下的Cache中。所以我们要在代理的方法中进行读写操作，拼接NSData。

#### 1、请求数据

```
-(void)startRequest{
    _urlStr = @"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg";
    //    NSString * string = @"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg";

//    创建文件
    _fm = [NSFileManager defaultManager];
    if (![_fm fileExistsAtPath:[self memoryPath]]) {
        [_fm createFileAtPath:[self memoryPath] contents:nil attributes:nil];
    }
    _handle = [NSFileHandle fileHandleForUpdatingAtPath:[self memoryPath]];
    [self.picData setData:[_handle readDataToEndOfFile]];
    [_handle seekToEndOfFile];

    NSMutableURLRequest * request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:_urlStr] cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:10];

    NSString * rangStr = [NSString stringWithFormat:@"bytes=%ld-", [self.picData length]];
    [request setValue:rangStr forHTTPHeaderField:@"Range"];

    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        _connect = [[NSURLConnection alloc] initWithRequest:request delegate:self startImmediately:YES];
//        self.runloop = [NSRunLoop currentRunLoop];
        CFRunLoopRun();
    });
}
```

#### 2、创建本地存储下载数据的目录结构

```
-(NSString *)memoryPath{
    NSString * document = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;
    NSString * nameMD5 = [self.urlStr stringFromMD5];
    NSString * path = [document stringByAppendingPathComponent:nameMD5];
    return path;
}
```

#### 3、开始下载的代理方法

> 注意：这里要处理再次下载的时候，要计算现在下载存储了数据的大小，为了能在后面继续拼接，不出现错误

```
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response{
//    开始下载
    [_connect start];
    self.fileTotle = [NSNumber numberWithInteger: response.expectedContentLength + [self.picData length]];
}
```

#### 4、数据下载多次的调用的代理方法（在这里写数据的拼接）

```
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data{
//    下载数据，多次
    [self.picData appendData:data];
//     加上data去文件后面
    [self.handle writeData:data];
    NSLog(@"%ld", data.length);
    float precent = 1.0*[self.picData length] / [self.fileTotle integerValue];
    self.progress.progress = precent;
}
```

#### 5、数据下载完毕

```
- (void)connectionDidFinishLoading:(NSURLConnection *)connection{
//    结束下载
    UIImage * image = [UIImage imageWithData:self.picData];
    self.imageView.image = image;
    NSLog(@"==========");

    float precent = 1.0*[self.picData length] / [self.fileTotle integerValue];
    self.progress.progress = precent;

    //    关闭文件
    [self.handle closeFile];
//    [self.handle synchronizeFile];
}
```



---



### 完整代码示例

```
#import "ViewController.h"
#import "NSString+MD5Addition.h"

@interface ViewController ()<NSURLConnectionDataDelegate>

@property (weak, nonatomic) IBOutlet UIImageView *imageView;
@property (weak, nonatomic) IBOutlet UIProgressView *progress;
//全局的发送请求
@property (nonatomic, strong)NSURLConnection * connect;

@property (nonatomic, strong)NSMutableData * picData;

//总的下载数据打下
@property(nonatomic, strong)NSNumber * fileTotle;
//URL
@property(nonatomic, copy)NSString * urlStr;
//文件的读取
@property(nonatomic, strong) NSFileHandle * handle;
//文件操作
@property(nonatomic, strong)NSFileManager * fm;

- (IBAction)startDownLoad:(id)sender;
- (IBAction)stopDownLoad:(id)sender;

@end

@implementation ViewController

-(NSMutableData *)picData{
    if (_picData == nil) {
        _picData = [[NSMutableData alloc] init];
    }
    return _picData;
}

-(void)startRequest{
    _urlStr = @"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg";
    //    NSString * string = @"http://img.zcool.cn/community/033ef53557121b80000004383e36a19.jpg";
    
//    创建文件
    _fm = [NSFileManager defaultManager];
    if (![_fm fileExistsAtPath:[self memoryPath]]) {
        [_fm createFileAtPath:[self memoryPath] contents:nil attributes:nil];
    }
    _handle = [NSFileHandle fileHandleForUpdatingAtPath:[self memoryPath]];
    [self.picData setData:[_handle readDataToEndOfFile]];
    [_handle seekToEndOfFile];
    
    NSMutableURLRequest * request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:_urlStr] cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:10];
    
    NSString * rangStr = [NSString stringWithFormat:@"bytes=%ld-", [self.picData length]];
    [request setValue:rangStr forHTTPHeaderField:@"Range"];
    
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        _connect = [[NSURLConnection alloc] initWithRequest:request delegate:self startImmediately:YES];
//        self.runloop = [NSRunLoop currentRunLoop];
        CFRunLoopRun();
    });
}

-(NSString *)memoryPath{
    NSString * document = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;
    NSString * nameMD5 = [self.urlStr stringFromMD5];
    NSString * path = [document stringByAppendingPathComponent:nameMD5];
    return path;
}

- (IBAction)startDownLoad:(id)sender {
//    [_connect start];
    [self startRequest];
}

- (IBAction)stopDownLoad:(id)sender {
    NSLog(@"]]]]]]]]]]]]]]]]]]]");
    [_connect cancel];
}

#pragma mark
#pragma mark ========== delegate
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response{
//    开始下载
    [_connect start];
    self.fileTotle = [NSNumber numberWithInteger: response.expectedContentLength + [self.picData length]];
}
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data{
//    下载数据，多次
    [self.picData appendData:data];
//     加上data去文件后面
    [self.handle writeData:data];
    NSLog(@"%ld", data.length);
    float precent = 1.0*[self.picData length] / [self.fileTotle integerValue];
    self.progress.progress = precent;
}
- (void)connectionDidFinishLoading:(NSURLConnection *)connection{
//    结束下载
    UIImage * image = [UIImage imageWithData:self.picData];
    self.imageView.image = image;
    NSLog(@"==========");
    
    float precent = 1.0*[self.picData length] / [self.fileTotle integerValue];
    self.progress.progress = precent;
    
    //    关闭文件
    [self.handle closeFile];
//    [self.handle synchronizeFile];
}


- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
//    [self startRequest];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```

#### 效果图

![](/assets/connect断点续传.png)

