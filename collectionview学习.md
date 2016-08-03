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
######在写collectionView之前要先写布局文件UICollectionViewFlowLayout
```
UICollectionViewFlowLayout * flowLayout = [[UICollectionViewFlowLayout alloc] init];
//    滑动方向不同，距离设置不一样
//    设置行间距(横)
    flowLayout.minimumLineSpacing = 20;
//    设置item的间距(竖向),如果值过小不起作用
    flowLayout.minimumInteritemSpacing = 20;
//    设置每个元素的大小
    flowLayout.itemSize = CGSizeMake(200, 100);
//    设置与元素之间的间距
    flowLayout.sectionInset = UIEdgeInsetsMake(10, 10, 10, 10);
    /*
        纵向
     UICollectionViewScrollDirectionVertical,
        横向
     UICollectionViewScrollDirectionHorizontal
     */
    flowLayout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
```
#####2、创建collectionView
```
_collectionView = [[UICollectionView alloc] initWithFrame:self.view.frame collectionViewLayout:flowLayout];
 [self.view addSubview:_collectionView];
```
#####3、注册cell
######系统自带的cell注册
```
//    sellconView不注册会崩溃
    [self.collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@"collectionID"];
```
######自定义一部分的cell的注册
```
    [self.collectionView registerClass:[CollectionViewCell class] forCellWithReuseIdentifier:@"collectionCellID"];
```
######通过xib创建的cell注册
``` 
    [self.collectionView registerNib:[UINib nibWithNibName:@"MyCollectionCell" bundle:nil] forCellWithReuseIdentifier:@"myConllectionCellID"];
```

###代理
######代理delegate
     self.collectionView.dataSource = self; 
     self.collectionView.delegate = self;
######代理所需要的协议
    UICollectionViewDelegate
    UICollectionViewDataSource
    UICollectionViewDelegateFlowLayout

###头视图和尾视图的注册
```

```


