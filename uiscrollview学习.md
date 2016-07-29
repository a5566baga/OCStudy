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