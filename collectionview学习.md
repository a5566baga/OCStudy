#CollectionView学习

---

###创建数据源
```
-(void)initForDataSource{
    for (NSInteger i = 0; i  < 51; i++) {
        NSString * path = [[NSBundle mainBundle] pathForResource:[NSString stringWithFormat:@"%ld", i] ofType:@"JPG"];
        UIImage * img = [UIImage imageWithContentsOfFile:path];
//        _dataSource 为了防止出现循环引用，在懒加载的情况下使用；一般还是用self.dataSource吧
        [self.dataSource addObject:img];
    }
}
```

###创建collectionView
#####1、布局文件
######在写collectionView之前要先写布局文件
#####2、注册cell
#####3、基本属性

###代理
