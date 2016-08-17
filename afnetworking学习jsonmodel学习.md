# AFNetworking学习\/JSONModel学习

---

## AFNetworking学习

#### 1、监听网络状态

```
    //    检测网络连接方式
    AFNetworkReachabilityManager * manager = [AFNetworkReachabilityManager sharedManager];
    [manager setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
        /*
         AFNetworkReachabilityStatusUnknown          = -1,
         AFNetworkReachabilityStatusNotReachable     = 0,
         AFNetworkReachabilityStatusReachableViaWWAN = 1,
         AFNetworkReachabilityStatusReachableViaWiFi = 2,
         */
        switch (status) {
            case AFNetworkReachabilityStatusUnknown:
                NSLog(@"未知");
                break;
            case AFNetworkReachabilityStatusNotReachable:
                NSLog(@"没有连接");
                break;
            case AFNetworkReachabilityStatusReachableViaWWAN:
                NSLog(@"蜂窝数据(2G/3G/4G/GPRS)");
                break;
            case AFNetworkReachabilityStatusReachableViaWiFi:
                NSLog(@"WiFi数据");
                break;
            default:
                break;
        }
    }];
    //    启动网络监听模式
    [manager startMonitoring];
```

#### 2、get请求一个JSON或是XML数据

```
-(void)getAFNetwork{
    AFHTTPSessionManager * manager = [AFHTTPSessionManager manager];
    /**
     *  总结
     *  1、参数1：是一个请求的URL字符串
     *  2、参数2：参数
     *
     */
    [manager GET:@"http://download.pchome.net/wallpaper/pic-9691-1.jpeg" parameters:nil progress:^(NSProgress * _Nonnull downloadProgress) {
        //        NSLog(@"%.2lf ~ %lld ====== %lld",1.0*downloadProgress.completedUnitCount/downloadProgress.totalUnitCount ,downloadProgress.totalUnitCount, downloadProgress.completedUnitCount);
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            self.progress.progress = 1.0*downloadProgress.completedUnitCount/downloadProgress.totalUnitCount;
        }];
    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {

        //        请求成功
        //        NSLog(@"%@", responseObject);
        UIImage * image = [UIImage imageWithData:responseObject];
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            _imageView.image = image;
        }];

    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
        //        请求失败
        NSLog(@"%@", error);
    }];
}
```

#### 3、post请求一个JSON或是XML数据

```
-(void)postAFNetwork{
    AFHTTPSessionManager * manager = [AFHTTPSessionManager manager];
    NSDictionary * parmerters = @{@"type":@"top",@"key":@"f47260da59b9944239ef813344918073"};
    [manager POST:@"http://v.juhe.cn/toutiao/index" parameters:parmerters progress:^(NSProgress * _Nonnull uploadProgress) {

    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
        NSLog(@"%@", responseObject);
    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {

    }];
}
```

#### 4、下载图片并且显示，带默认图片的那种

```
-(void)AFNetworkImage{
    [self.imageView setImageWithURLRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"http://img-download.pchome.net/download/1k0/bg/4g/o1chb4-1osb.jpg"]] placeholderImage:[UIImage imageNamed:@"QQ20160805-0"] success:^(NSURLRequest * _Nonnull request, NSHTTPURLResponse * _Nullable response, UIImage * _Nonnull image) {
        dispatch_async(dispatch_get_main_queue(), ^{
            self.imageView.image = image;
        });
    } failure:^(NSURLRequest * _Nonnull request, NSHTTPURLResponse * _Nullable response, NSError * _Nonnull error) {

    }];
}
```

#### 5、文件下载，都是要创建task对象，改变路径

```
-(void)downLoadAFNetwork{
    AFHTTPSessionManager * manager = [AFHTTPSessionManager manager];
    NSURL * url = [NSURL URLWithString:@"http://img-download.pchome.net/download/1k0/bg/4g/o1chb4-1l8r.jpg"];
    NSURLRequest * request = [NSURLRequest requestWithURL:url];
    NSURLSessionDownloadTask * task = [manager downloadTaskWithRequest:request progress:^(NSProgress * _Nonnull downloadProgress) {
        self.progress.progress = 1.0*downloadProgress.completedUnitCount/downloadProgress.totalUnitCount;
    } destination:^NSURL * _Nonnull(NSURL * _Nonnull targetPath, NSURLResponse * _Nonnull response) {
//        临时缓存路径
        NSLog(@"%@", targetPath);
//        下载的文件路径
        NSString * docmentPath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;

        return [NSURL fileURLWithPath:[docmentPath stringByAppendingPathComponent:@"image"]];
    } completionHandler:^(NSURLResponse * _Nonnull response, NSURL * _Nullable filePath, NSError * _Nullable error) {
        [[NSOperationQueue mainQueue] addOperationWithBlock:^{
            self.imageView.image = [UIImage imageWithData:[NSData dataWithContentsOfURL:filePath]];
        }];
        NSLog(@"%@", filePath);
    }];

    [task resume];
}
```

#### 6、上传数据（基本类型）

```
-(void)uploadAFNerwork01{
    AFHTTPSessionManager * manage = [AFHTTPSessionManager manager];
    NSString * uploadStr = @"shanchuangwangzhi";
//    上传文件
    NSDictionary * dic = @{@"userName":@"ZZQ"};
    [manage POST:uploadStr parameters:dic constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
        UIImage * image = [UIImage imageNamed:@"QQ20160805-0"];
        NSData * data = UIImagePNGRepresentation(image);
        [formData appendPartWithFileData:data name:@"file" fileName:@"QQ20160805-0" mimeType:@"image/png"];

    } progress:^(NSProgress * _Nonnull uploadProgress) {
//        上传文件的进度


    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
//        成功请求

    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
//        失败请求

    }];
}
```

#### 7、上传文件

```
-(void)uploadAFNerwork02{
    AFHTTPSessionManager * manage = [AFHTTPSessionManager manager];
    NSString * uploadStr = @"shanchuangwangzhi";
    //    上传文件
    NSDictionary * dic = @{@"userName":@"ZZQ"};
    [manage POST:uploadStr parameters:dic constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
        [formData appendPartWithFileURL:@"上传文件URL" name:@"" fileName:@"" mimeType:@"image/jpg" error:nil];

    } progress:^(NSProgress * _Nonnull uploadProgress) {

    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {

    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {

    }];
}
```

### 官方说明：

> https:\/\/github.com\/AFNetworking\/AFNetworking



---

## JSONModel学习

### 说明：

> #### 1、这个主要是在创建model的时候不同，有了很强大的功能
> 
> #### 2、使用第三方库，注意看文档

### 官方说明：

> https:\/\/github.com\/jsonmodel\/jsonmodel

![](/assets/JSONModel.md)

