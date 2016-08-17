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



