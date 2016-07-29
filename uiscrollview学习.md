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
```
- (void)scrollViewDidEndZooming:(UIScrollView *)scrollView withView:(nullable UIView *)view atScale:(CGFloat)scale; 
// scale between minimum and maximum. called after any 'bounce' animations
```
######视图是否返回到顶部
```
- (BOOL)scrollViewShouldScrollToTop:(UIScrollView *)scrollView; 
// return a yes if you want to scroll to the top. if not defined, assumes YES
```
######视图已经返回到顶部
```
- (void)scrollViewDidScrollToTop:(UIScrollView *)scrollView; // called when scrolling animation finished. may be called immediately if already at top
```
---

## UIScrollView 与计时器的结合
######注意：
    1、定时器在控制器的视图转换下是不会停止的
    2、要计时器停止(声明周期结束要用) 
        [self.timer invalidate]; 

#####实现图片自动轮播的效果
######代码如下:
```
#import "ViewController.h"

#define SCREEN_WIDTH CGRectGetWidth(self.view.frame)
#define PICTURE_COUNT 10

@interface ViewController ()<UIScrollViewDelegate>
@property(nonatomic, strong)UIScrollView * scrollView;
@property(nonatomic, strong)UIPageControl * pageControl;
@property(nonatomic, strong)NSTimer * timer;
//显示图片信息
@property(nonatomic, strong)UILabel * picInfo;
@property(nonatomic, strong)NSArray * infoArray;
@end

@implementation ViewController

- (void)viewDidLoad {
 [super viewDidLoad];
 // Do any additional setup after loading the view, typically from a nib.

 // self.view.backgroundColor = [UIColor blueColor];

 [self createForView];
 [self scrollViewAddPic];
 [self createPageControl];
 [self createTimer];
 [self createInformation];
}

-(void)createPageControl{

 _pageControl = [[UIPageControl alloc] initWithFrame:CGRectMake(SCREEN_WIDTH/5*3, 535, 100, 39)];
 _pageControl.numberOfPages = PICTURE_COUNT;
 _pageControl.pageIndicatorTintColor = [UIColor whiteColor];
 _pageControl.currentPageIndicatorTintColor = [UIColor blackColor];

 [self.view addSubview:_pageControl];
}

-(void)createForView{
 _scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 65, CGRectGetWidth(self.view.frame), 500)];
 _scrollView.backgroundColor = [UIColor redColor];
 [self.view addSubview:self.scrollView];

}

-(void)scrollViewAddPic{

 NSMutableArray<UIImage *> * imgArray = [NSMutableArray array];
 for (NSInteger i = 0; i < PICTURE_COUNT; i++) {
 NSString * path = [[NSBundle mainBundle] pathForResource:[NSString stringWithFormat:@"41_%ld", i] ofType:@"jpg"];
 UIImage * image = [UIImage imageWithContentsOfFile:path];
 [imgArray addObject:image];
 }
 [imgArray insertObject:[imgArray lastObject] atIndex:0];
 [imgArray addObject:imgArray[1]];
 // float SCREEN_WIDTH = CGRectGetWidth(self.view.frame);
 self.scrollView.contentSize = CGSizeMake(SCREEN_WIDTH*imgArray.count, 500);
 self.scrollView.pagingEnabled = YES;

 for (NSInteger j = 0; j < imgArray.count; j++) {
 UIImageView * imgView = [[UIImageView alloc] initWithFrame:CGRectMake(SCREEN_WIDTH*j, 0, SCREEN_WIDTH, 500)];
 imgView.image = imgArray[j];
 [self.scrollView addSubview:imgView];
 }

 [_scrollView setContentOffset:CGPointMake(SCREEN_WIDTH, 0)];

 self.scrollView.delegate = self;

}
#pragma mark
#pragma mark =========== UIScrollViewDelegate
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView{
 if (scrollView.contentOffset.x/SCREEN_WIDTH == 0) {
 scrollView.contentOffset = CGPointMake(PICTURE_COUNT*SCREEN_WIDTH, 0);
 }else if(scrollView.contentOffset.x/SCREEN_WIDTH == 11){
 scrollView.contentOffset = CGPointMake(SCREEN_WIDTH, 0);
 }
 NSInteger index = (_scrollView.contentOffset.x/SCREEN_WIDTH)-1;
 self.pageControl.currentPage = index;
 self.picInfo.text = self.infoArray[index];

 NSLog(@"%@", NSStringFromCGSize(scrollView.contentSize));

}

-(void)scrollViewDidEndScrollingAnimation:(UIScrollView *)scrollView{
 if (scrollView.contentOffset.x/SCREEN_WIDTH == 0) {
 scrollView.contentOffset = CGPointMake(PICTURE_COUNT*SCREEN_WIDTH, 0);
 }else if(scrollView.contentOffset.x/SCREEN_WIDTH == 11){
 scrollView.contentOffset = CGPointMake(SCREEN_WIDTH, 0);
 }
 NSInteger index = (_scrollView.contentOffset.x/SCREEN_WIDTH)-1;
 self.pageControl.currentPage = index;
 self.picInfo.text = self.infoArray[index];
}
-(void)createInformation{
 _infoArray = @[@"美女1", @"美女2", @"美女3", @"美女4", @"美女5", @"美女6", @"美女7", @"美女8", @"美女9", @"美女10"];
 _picInfo = [[UILabel alloc] initWithFrame:CGRectMake(30, 500, 200, 50)];
 _picInfo.font = [UIFont systemFontOfSize:30];

 [self.view addSubview:self.picInfo];
 _picInfo.text = _infoArray[0];
}

#pragma mark
#pragma mark ========== timer
-(void)createTimer{
 _timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(changePic) userInfo:nil repeats:YES];
}

-(void)changePic{
 float _X = self.scrollView.contentOffset.x + SCREEN_WIDTH;
 [self.scrollView setContentOffset:CGPointMake(_X, 0) animated:YES];
}

-(void)scrollViewWillBeginDragging:(UIScrollView *)scrollView{
 self.timer.fireDate = [NSDate distantFuture];
}

-(void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate{
 self.timer.fireDate = [NSDate distantPast];
}

- (void)didReceiveMemoryWarning {
 [super didReceiveMemoryWarning];
 // Dispose of any resources that can be recreated.
}

@end
```