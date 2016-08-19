# **Quartz **2D

---

## 基本概念

> Quartz是Mac OS X的Darwin核心之上的绘图层，有时候也认为是CoreGraphics\(制图\)。
> 
> 共有两种部分组成Quartz：
> 
> Quartz Compositor，合成视窗系统，管理和合成幕后视窗影像来建立Mac OS X使用者接口。（了解，即幕后工作）
> 
> Quartz 2D，是iOS和Mac OS X环境下的二维绘图引擎。（熟练，幕前工作，由我们来操作的）
> 
> 涉及内容包括：基于路径的绘图，透明度绘图，遮盖，阴影，透明层，颜色管理，防锯齿渲染，生成PDF，以及PDF元数据相关处理
> 
> quartz 2D能完成的工作：绘制图形（线条、三角形、矩形、圆等），绘制（文字，图片、PDF,截图），自定义UI控件，给报表。

## 绘制图形使用

### 步骤：（CoreGraphics）

> #### 1、获取上下文
> 
> #### 2、创建一个路径
> 
> #### 3、 添加路径
> 
> #### 4、渲染

### 步骤：（ UIBezierPath ）

> #### 1、创建 UIBezierPath 对象
> 
> #### 2、创建路径
> 
> #### 3、修饰
> 
> #### 4、渲染

### 基本

```
    //    1、获取上下文
    CGContextRef contextRef = UIGraphicsGetCurrentContext();
    //    2、创建一个路径
    CGMutablePathRef path = CGPathCreateMutable();
    //    路径的起始点
    CGPathMoveToPoint(path, nil, 0, 0);
    //    添加一个路径点
    CGPathAddLineToPoint(path, nil, 100, 100);
    //    3、添加路径
    CGContextAddPath(contextRef, path);
    //    4、渲染
    CGContextStrokePath(contextRef);
```

### 设置多条线段

```
    CGContextMoveToPoint(contextRef, 40, 100);
    //    终点位置
    CGContextAddLineToPoint(contextRef, 150, 100);
    CGContextAddLineToPoint(contextRef, 150, 300);

    CGContextMoveToPoint(contextRef, 200, 200);
    CGContextAddLineToPoint(contextRef, 300, 500);

    //    设置颜色
    CGContextSetStrokeColorWithColor(contextRef, [UIColor colorWithRed:arc4random()%255/255.0 green:arc4random()%255/255.0 blue:arc4random()%255/255.0 alpha:0.5].CGColor);
    //    [[UIColor blueColor] setStroke];
    //    设置宽度
    CGContextSetLineWidth(contextRef, 30);
    /*
     kCGLineJoinMiter,
     kCGLineJoinRound,
     kCGLineJoinBevel
     */
    //    连接处角的样子
    CGContextSetLineJoin(contextRef, kCGLineJoinRound);
    /*
     kCGLineCapButt,
     kCGLineCapRound,
     kCGLineCapSquare
     */
    //    设置线起始位置样式
    CGContextSetLineCap(contextRef, kCGLineCapRound);

    CGContextSetAlpha(contextRef, 0.5);

    CGContextStrokePath(contextRef);
```

![](/assets/多条线段绘制.png)

### 贝塞尔曲线:单独的线段

```
    UIBezierPath * path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(30, 40)];
    [path addLineToPoint:CGPointMake(100, 100)];
    [[UIColor redColor] setStroke];
    
    UIBezierPath * path2 = [UIBezierPath bezierPath];
    [path2 moveToPoint:CGPointMake(120, 120)];
    [path2 addLineToPoint:CGPointMake(200, 200)];
    [[UIColor purpleColor] setStroke];
    
    [path stroke];
    [path2 stroke];
```



