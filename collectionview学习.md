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

###4、代理
######代理delegate
     self.collectionView.dataSource = self; 
     self.collectionView.delegate = self;
######代理所需要的协议
    UICollectionViewDelegate
    UICollectionViewDataSource
    UICollectionViewDelegateFlowLayout

###5、头视图和尾视图的注册
```
[self.collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"headersID"];
```
```
[self.collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footersID"];
```
###6、头尾视图的回调方法
```
//设置头尾视图尺寸
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForHeaderInSection:(NSInteger)section{
//    宽度不管用
    return CGSizeMake(200, 100);
}
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForFooterInSection:(NSInteger)section{
    return CGSizeMake(200, 100);
}
//item的大小
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath{
//    设置重写设置item大小的方法，那么设置的属性就不管用了，无效
    return CGSizeMake(100, 100);
}
//内部边距
- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section{
//    设置内容与边界的距离
    return UIEdgeInsetsMake(110, 20, 30, 20);
}

//返回headerview和footerview
-(UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath{
    if ([kind isEqualToString: UICollectionElementKindSectionHeader]) {
//        UICollectionReusableView * headerView = [collectionView dequeueReusableCellWithReuseIdentifier:@"headersID" forIndexPath:indexPath];
        
        UICollectionReusableView * headerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"headersID" forIndexPath:indexPath];
        
        headerView.backgroundColor = [UIColor colorWithRed:0.0 green:1.0 blue:1.0 alpha:1.0];
        return headerView;
    }else{
//        UICollectionReusableView * foorterView = [collectionView dequeueReusableCellWithReuseIdentifier:@"footersID" forIndexPath:indexPath];
        
        UICollectionReusableView * footerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footersID" forIndexPath:indexPath];
        footerView.backgroundColor = [UIColor colorWithRed:1.0 green:0.0 blue:1.0 alpha:1.0];
        return footerView;
    }
}
```
###7、设置cell
```
//返回每个section的row的数量
-(NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section{
    return self.dataSource.count;
}
//返回section的大小
-(NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView{
    return 1;
}
//返回的是cell
-(UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath{
    if (indexPath.row %2 == 0) {
#if 1
        UICollectionViewCell * cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"collectionID" forIndexPath:indexPath];
        
        cell.backgroundColor = [UIColor redColor];
        
        UIView * selectView = [[UIView alloc] init];
        selectView.backgroundColor = [UIColor grayColor];
        cell.selectedBackgroundView = selectView;
        
        UILabel * label = [[UILabel alloc] init];
        label.text = [NSString stringWithFormat:@"%ld", indexPath.row];
        label.textAlignment = NSTextAlignmentCenter;
        label.backgroundColor = [UIColor redColor];
        label.alpha = 0.5;
        cell.backgroundView = label;
//        cell.selectedBackgroundView = label;
        
//        UIImageView * imgView = [[UIImageView alloc] initWithImage:self.dataSource[indexPath.row]];
//        cell.backgroundView = imgView;
#endif
        return cell;
    }else if (indexPath.row%3 == 0){
        CollectionViewCell * myCell = [collectionView dequeueReusableCellWithReuseIdentifier:@"collectionCellID" forIndexPath:indexPath];
        myCell.imageView.image = self.dataSource[indexPath.row];
        return myCell;
    }else{
        MyCollectionCell * xibCell = [collectionView dequeueReusableCellWithReuseIdentifier:@"myConllectionCellID" forIndexPath:indexPath ];
        xibCell.imageView.image = self.dataSource[indexPath.row];
        
        return xibCell;
    }
}

//是否能选择,设置item是否可选
- (BOOL)collectionView:(UICollectionView *)collectionView shouldSelectItemAtIndexPath:(NSIndexPath *)indexPath{
    if (indexPath.row%2 == 0) {
        return YES;
    }else
        return  NO;
}
//选择是调用
- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath{
    NSLog(@"section %ld,  item: %ld",indexPath.section, indexPath.row);
}
```
![](/assets/collectionCell表现.png)

---
##自定义CollectionCell实现瀑布流效果
#####明确目的
######需要写两个类，一个是继承于UICollectionViewCell的类；一个是继承UICollectionViewLayout的类

####步骤
#####1、声明属性（放在cell上的内容）
```
@interface CollectionViewCell : UICollectionViewCell

@property(nonatomic, strong)UILabel * numberLabel;

@property(nonatomic, strong)UIImageView * imageView;

@end
```
#####2、实现初始化方法和layoutSubviews方法
```
- (instancetype)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        _numberLabel = [[UILabel alloc] init];
        _numberLabel.font = [UIFont systemFontOfSize:30];
        _numberLabel.textAlignment = NSTextAlignmentCenter;
        [self.contentView addSubview:_numberLabel];
        
        _imageView = [[UIImageView alloc] init];
        [self.contentView addSubview:_imageView];
    }
    return self;
}

-(void)layoutSubviews{
    self.numberLabel.frame = CGRectMake(0, 0, CGRectGetWidth(self.frame), CGRectGetHeight(self.frame));
    
    self.imageView.frame = CGRectMake(0, 0, CGRectGetWidth(self.frame), CGRectGetHeight(self.frame));
}
```
#####3、
