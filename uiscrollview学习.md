# UIScrollView 

---
## UIScrollView 的基本使用
####创建
>将UIScrollView写成属性
```
-(UIScrollView *)scrollView{
 if (nil == _scrollView) {
 _scrollView = [[UIScrollView alloc] initWithFrame:self.view.frame];
 _scrollView.backgroundColor = [UIColor blueColor];
 }
 return _scrollView;
}
```

####基本属性
######基本显示的大小（即可以拖动的尺寸）
> self.scrollView.contentSize = imgSize; 

#####偏移的位置
> [self.scrollView setContentOffset:CGPointMake(200, 100)]; 

#####内容与边框的距离(现用现测，起点都是根据周围出发)
> self.scrollView.contentInset = UIEdgeInsetsMake(<#CGFloat top#>, <#CGFloat left#>, <#CGFloat bottom#>, <#CGFloat right#>); 

#####是否启用拉出边界后的弹簧效果
> self.scrollView.bounces = YES; 

#####滚动条样式
> self.scrollView.indicatorStyle = UIScrollViewIndicatorStyleBlack; 
```
 UIScrollViewIndicatorStyleDefault, // black with white border. good against any background
 UIScrollViewIndicatorStyleBlack, // black only. smaller. good against a white background
 UIScrollViewIndicatorStyleWhite // white only. smaller. good against a black background 
```

#####横向的滚动条是否显示
> self.scrollView.showsHorizontalScrollIndicator = YES; 

#####竖向的滚动条是否演示
> self.scrollView.showsVerticalScrollIndicator = NO; 

#####滚动条距离边界的位置(边界是起点)
> self.scrollView.scrollIndicatorInsets = UIEdgeInsetsMake(100, 100, 100, 100); 

#####
>

#####
>

#####
>

#####
>

#####
>

#####
>

####缩放

#### UIPageControl 的使用
#####属性
---

## UIScrollView 与计时器的结合