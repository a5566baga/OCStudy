# 自动布局

---

## AutoreSizing自动布局

### 介绍：

> #### AutoreSizing就是一种停靠方式，停靠在哪一边就与哪一边固定。
> 
> #### 设定时要注意先关掉AutoLayout

![](/assets/关闭AutoLayout.png)

#### 停靠方式

```
     UIViewAutoresizingNone                 = 0,
     UIViewAutoresizingFlexibleLeftMargin   = 1 << 0, 左边伸缩
     UIViewAutoresizingFlexibleWidth        = 1 << 1, 宽度伸缩
     UIViewAutoresizingFlexibleRightMargin  = 1 << 2, 右边伸缩
     UIViewAutoresizingFlexibleTopMargin    = 1 << 3, 上边伸缩
     UIViewAutoresizingFlexibleHeight       = 1 << 4, 高度伸缩
     UIViewAutoresizingFlexibleBottomMargin = 1 << 5 下面伸缩
```

![](/assets/停靠模式.png)

#### 代码使用

```
    _blueView = [[UIView alloc] initWithFrame:CGRectMake(10, 30, 300, 50)];
    _blueView.backgroundColor = [UIColor blueColor];
    _blueView.autoresizingMask = UIViewAutoresizingFlexibleBottomMargin | UIViewAutoresizingFlexibleRightMargin | UIViewAutoresizingFlexibleWidth;
    [self.view addSubview:_blueView];
```

---

## AutoLayout自动布局

### 介绍：

> #### 比停靠方式更加强大的布局方式
> 
> #### 要用代码关闭AutoreSizing
> 
> #### 约束条件不要重复，要都约束完整
> 
> #### AutoLayout添加约束，一定要明确地让系统知道控件的大小和位置

#### 视图中布局：

![](/assets/StoryBoard中设置布局.png)

#### 代码布局：

> #### 1、声明view
> 
> #### 2、禁用autoresizing
> 
> #### 3、添加到当前view中
> 
> #### 4、设置NSLayoutConstraint
> 
> #### 5、将约束条件添加到view中

![](/assets/代码自动布局.png)

#### 代码实现：

> #### 注意对应，和约束条件要完整，不同视图之间的约束要加在他们共同的父视图上

```
 //蓝色View
    UIView * blueView = [[UIView alloc]init];
    blueView.backgroundColor = [UIColor blueColor];
    blueView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:blueView];

    //高
    NSLayoutConstraint * bHeightConstraint = [NSLayoutConstraint constraintWithItem:blueView attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:0.0 constant:60];
    [blueView addConstraint:bHeightConstraint];

    //左
    NSLayoutConstraint * bLeftConstraint = [NSLayoutConstraint constraintWithItem:blueView attribute:NSLayoutAttributeLeft relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeLeft multiplier:1.0 constant:20];

    //顶
    NSLayoutConstraint * bTopConstraint = [NSLayoutConstraint constraintWithItem:blueView attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTop multiplier:1.0 constant:64];

    //右
    NSLayoutConstraint * bRightConstraint = [NSLayoutConstraint constraintWithItem:blueView attribute:NSLayoutAttributeRight relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeRight multiplier:1.0 constant:-20];

    [self.view addConstraints:@[bLeftConstraint,bTopConstraint,bRightConstraint]];


    //----------------设置红色View

    //红色View
    UIView * redView = [[UIView alloc]init];
    redView.backgroundColor = [UIColor redColor];
    redView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:redView];

    //上
    NSLayoutConstraint * rTopConstrain = [NSLayoutConstraint constraintWithItem:redView attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:blueView attribute:NSLayoutAttributeBottom multiplier:1.0 constant:20];


    //左
    NSLayoutConstraint * rleftConstraint = [NSLayoutConstraint constraintWithItem:redView attribute:NSLayoutAttributeLeft relatedBy:NSLayoutRelationEqual toItem:blueView attribute:NSLayoutAttributeCenterX multiplier:1.0 constant:0];

    //右
    NSLayoutConstraint * rRightConstraint = [NSLayoutConstraint constraintWithItem:redView attribute:NSLayoutAttributeTrailing relatedBy:NSLayoutRelationEqual toItem:blueView attribute:NSLayoutAttributeTrailing multiplier:1.0 constant:0];

    //高度
    NSLayoutConstraint * rHeightConstraint = [NSLayoutConstraint constraintWithItem:redView attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationEqual toItem:blueView attribute:NSLayoutAttributeHeight multiplier:1.0 constant:0];

    [self.view addConstraints:@[rleftConstraint,rTopConstrain,rRightConstraint,rHeightConstraint]];
```

##### 约束条件

```
    NSLayoutAttributeLeft = 1,
    NSLayoutAttributeRight,
    NSLayoutAttributeTop,
    NSLayoutAttributeBottom,
    NSLayoutAttributeLeading,
    NSLayoutAttributeTrailing,
    NSLayoutAttributeWidth,
    NSLayoutAttributeHeight,
    NSLayoutAttributeCenterX,
    NSLayoutAttributeCenterY,
    NSLayoutAttributeBaseline,
    NSLayoutAttributeLastBaseline = NSLayoutAttributeBaseline,
    NSLayoutAttributeFirstBaseline NS_ENUM_AVAILABLE_IOS(8_0),


    NSLayoutAttributeLeftMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeRightMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeTopMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeBottomMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeLeadingMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeTrailingMargin NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeCenterXWithinMargins NS_ENUM_AVAILABLE_IOS(8_0),
    NSLayoutAttributeCenterYWithinMargins NS_ENUM_AVAILABLE_IOS(8_0),

    NSLayoutAttributeNotAnAttribute = 0
```

##### 大小条件

```
    NSLayoutRelationLessThanOrEqual = -1,
    NSLayoutRelationEqual = 0,
    NSLayoutRelationGreaterThanOrEqual = 1,
```

### VFL语言

> #### 利用可视化对视图进行布局
> 
> #### 需要设置字典，字符串，view

#### 例子一：

```
-(void)testVFL1{
    //    写一个蓝色的view，离左右边界为20，顶部top为64，高度是60
    UIView * blueView = [[UIView alloc] init];
    blueView.backgroundColor = [UIColor blueColor];
    blueView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:blueView];
    
    NSString * vfl = @"H:|-20-[abc]-20-|";
    NSDictionary * views = @{@"abc":blueView};
    //    VFL字符创语句    约束操作    VFL中的具体数值    控件
    NSArray<NSLayoutConstraint *> * constraint = [NSLayoutConstraint constraintsWithVisualFormat:vfl options:kNilOptions metrics:nil views:views];
    
    [self.view addConstraints:constraint];
    
    NSString * vfl2 = @"V:|-20-[abc(50)]";
    NSDictionary * views2 = @{@"abc":blueView};
    NSArray<NSLayoutConstraint *> * constraints2= [NSLayoutConstraint constraintsWithVisualFormat:vfl2 options:kNilOptions metrics:nil views:views2];
    [self.view addConstraints:constraints2];
}
```

#### 例子二：

```
-(void)testTwo{
    UIView * topView = [[UIView alloc] init];
    topView.backgroundColor = [UIColor blueColor];
    topView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:topView];
    
    UIView * downView = [[UIView alloc] init];
    downView.backgroundColor = [UIColor orangeColor];
    downView.translatesAutoresizingMaskIntoConstraints = NO;
    [self.view addSubview:downView];
    
    //    水平约束
    NSNumber * margin = @20;
    NSString * topVFL = @"H:|-margin-[topView]-margin-|";
    //    会自动将这个view转换成一个字典dictionary
    NSDictionary * topViewDic = NSDictionaryOfVariableBindings(topView);
    //    NSDictionary * topMerics = @{@"margin": margin};
    NSDictionary * topMerics = NSDictionaryOfVariableBindings(margin);
    NSArray<NSLayoutConstraint *> * topConstraints = [NSLayoutConstraint constraintsWithVisualFormat:topVFL options:kNilOptions metrics:topMerics views:topViewDic];
    [self.view addConstraints:topConstraints];
    
    //    垂直约束
    NSNumber * topHeight = @80;
    NSString * topVFLV = @"V:|-margin-[topView(topHeight)]";
    NSDictionary * topViewDicV = NSDictionaryOfVariableBindings(topView);
    NSDictionary * topMericsV = NSDictionaryOfVariableBindings(margin ,topHeight);
    NSArray<NSLayoutConstraint *> * topConstrainsV = [NSLayoutConstraint constraintsWithVisualFormat:topVFLV options:kNilOptions metrics:topMericsV views:topViewDicV];
    [self.view addConstraints:topConstrainsV];
    
    //    下面高度
    NSString * downVFL = @"V:[topView]-margin-[downView(==topView)]";
    NSDictionary * downViewDic = NSDictionaryOfVariableBindings(topView, downView);
    NSDictionary * downMerics = NSDictionaryOfVariableBindings(margin);
    NSArray<NSLayoutConstraint *> * downConstrains = [NSLayoutConstraint constraintsWithVisualFormat:downVFL options:kNilOptions metrics:downMerics views:downViewDic];
    [self.view addConstraints:downConstrains];
    
    //    下面宽度
    NSString * downVFLH = @"H:|-margin-[downView]-margin-|";
    NSDictionary * downViewDicH = NSDictionaryOfVariableBindings(downView);
    NSDictionary * downMericsH = NSDictionaryOfVariableBindings(margin);
    NSArray<NSLayoutConstraint *> * downConstrainsH = [NSLayoutConstraint constraintsWithVisualFormat:downVFLH options:kNilOptions metrics:downMericsH views:downViewDicH];
    [self.view addConstraints:downConstrainsH];
}
```





---

## 第三方——Masonry进行布局

> 

