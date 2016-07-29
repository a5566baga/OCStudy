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

#####页面是否可以滚动
> self.scrollView.scrollEnabled = YES; 

#####设置显示为一页一页的显示(即设置的scrollView的大小为一页)
> self.scrollView.pagingEnabled = NO; 

#####减速，值为0.0-1.0，值越小越好
> self.scrollView.decelerationRate = 0.1; 

#####返回顶部，默认为YES。
######注意：
    当多个scrollView在一个ViewController中的时候，那么通过这个属性可以看出谁是能够返回顶部。

> self.scrollView.scrollsToTop = YES; 

####缩放
#####缩放的最小值
>self.scrollView.minimumZoomScale = 0.01;

#####缩放的最大倍数
>self.scrollView.maximumZoomScale = 30;

#### UIPageControl 的使用
#####属性
######创建
> UIPageControl * pageControl = [[UIPageControl alloc] initWithFrame:CGRectMake(0, 0, 150, 39)]; 

######圆点的个数
> pageControl.numberOfPages = 5; 

######圆点的默认颜色
> pageControl.pageIndicatorTintColor = [UIColor whiteColor]; 

######当前圆点的颜色
> pageControl.currentPageIndicatorTintColor = [UIColor purpleColor]; 

######默认圆点的位置圆点
> pageControl.currentPage = 2; 


---

##UIScrollView的代理
#####遵从协议
> UIScrollViewDelegate 

#####协议中的方法
######已经滚动状态
```
- (void)scrollViewDidScroll:(UIScrollView *)scrollView{
// 打印多的
 NSLog(@"scroll................");
}
```
######将要开始拖拽
```
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView;
```
######将要结束拖拽
```
- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity 
targetContentOffset:(inout CGPoint *)targetContentOffset NS_AVAILABLE_IOS(5_0);
```
######完成拖拽
```
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate;
```
######开始减速
```
- (void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView;
```
######减速结束
```
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView;
```
######动画结束
```
- (void)scrollViewDidEndScrollingAnimation:(UIScrollView *)scrollView;
```
######缩放
```
- (nullable UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView;
```
######将要开始缩放
```
- (void)scrollViewWillBeginZooming:(UIScrollView *)scrollView withView:(nullable UIView *)view NS_AVAILABLE_IOS(3_2); 
// called before the scroll view begins zooming its content
```
######结束缩放
>
---

## UIScrollView 与计时器的结合