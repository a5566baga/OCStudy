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

### 设置曲线：要中间添加一个点

```
    CGContextMoveToPoint(contextRef, 20, 100);
    //    绘制曲线
    CGContextAddQuadCurveToPoint(contextRef, 150, 100, 80, 150);
    CGContextStrokePath(contextRef);
```

### 三角形创建

> #### 填充方式
> 
> > #### CGContextClosePath\(contextRef\); 自动闭合
> > 
> > #### CGContextFillPath\(contextRef\); 填充并且闭合
> > 
> > #### CGContextDrawPath\(contextRef, kCGPathFillStroke\); 可以设置填充方式
> 
> #### 可选模式
> 
> > kCGPathFill,
> >      kCGPathEOFill,
> >      kCGPathStroke,
> >      kCGPathFillStroke,
> >      kCGPathEOFillStroke

```
    CGContextMoveToPoint(contextRef, 30, 100);
    CGContextAddLineToPoint(contextRef, 100, 200);
    CGContextAddLineToPoint(contextRef, 222, 100);
//    CGContextAddLineToPoint(contextRef, 30, 100);

//    设置线或是边界线的颜色
    [[UIColor blackColor] setStroke];
//    填充色
    [[UIColor redColor] setFill];
//    写填充，会自动闭合
    CGContextClosePath(contextRef);
//    填充颜色
//    CGContextSetFillColorWithColor(contextRef, [UIColor redColor].CGColor);
//    填充
//    CGContextFillPath(contextRef);

//    设置图型或路静连线
//    CGContextStrokePath(contextRef);
//    绘制既有边界又有内容
    CGContextDrawPath(contextRef, kCGPathFillStroke);
```

### 绘制一个矩形

```
    CGContextAddRect(contextRef, CGRectMake(80, 80, 180, 180));
    CGContextSetLineWidth(contextRef, 15);
    CGContextDrawPath(contextRef, kCGPathStroke);
    
    UIBezierPath * path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(80, 300, 100, 100) cornerRadius:50];
    [[UIColor redColor] setStroke];
    [path stroke];
```

![](/assets/绘制矩形.png)

### 绘制圆形

```
    CGContextAddEllipseInRect(contextRef, CGRectMake(80, 80, 200, 200));
    [[UIColor whiteColor] setStroke];
    CGContextSetFillColorWithColor(contextRef, [UIColor blackColor].CGColor);
    CGContextSetLineWidth(contextRef, 10);
//    CGContextStrokePath(contextRef);
    CGContextDrawPath(contextRef, kCGPathFillStroke);
    
//    YES顺时针，NO逆时针
    UIBezierPath * path = [UIBezierPath bezierPathWithArcCenter:CGPointMake(200, 400) radius:100 startAngle:0 endAngle:M_PI*2 clockwise:YES];
    [path fill];
```

![](/assets/绘制圆形.png)

### 绘制一个弧（根据圆心设置不同，填充方式也不同）

```
//    最后一个参数，为0是是顺时针，否则是逆时针
    CGContextAddArc(contextRef, 100, 100, 80, 0, M_PI/2, 0);
    CGContextAddLineToPoint(contextRef, 100, 100);
    CGContextFillPath(contextRef);
    
    CGPoint center = CGPointMake(100, 300);
    UIBezierPath * path = [UIBezierPath bezierPathWithArcCenter:center radius:80 startAngle:0 endAngle:M_PI_2 clockwise:NO];
    [path fill];
```

![](/assets/弧（填充的）.png)

### 饼状图

```
    NSArray * number = @[@20, @30, @50,@22, @90, @120];
    
    //    总数
    float totle = 0;
    for (NSNumber * num in number) {
        totle += num.floatValue;
    }
    CGPoint point = CGPointMake(self.bounds.size.width/2, self.bounds.size.height/2);
    
    //    画第一部分扇形
    float startAngle = 0;
    float endAngle = 0;
    float radius = MIN(CGRectGetWidth(self.bounds), CGRectGetHeight(self.bounds))/2.0-5;
    for (NSNumber * num in number) {
        startAngle = endAngle;
        endAngle = [num floatValue]/totle*M_PI*2 + startAngle;
        UIBezierPath * path = [UIBezierPath bezierPathWithArcCenter:point radius:radius startAngle:startAngle endAngle:endAngle clockwise:YES];
        [[self runColor] setFill];
        [path addLineToPoint:point];
        [path fill];
    }
```

![](/assets/饼状图.png)

### 绘制文本

```
    NSString * string = @"绘制的字符串abcdefg";
    NSDictionary * dic = @{NSFontAttributeName:[UIFont systemFontOfSize:30], NSForegroundColorAttributeName:[UIColor whiteColor]};
    [string drawInRect:CGRectMake(50, 50, 200, 200) withAttributes:dic];
```

![](/assets/绘制文本.png)

### 绘制图片

```
    UIImage * img = [UIImage imageNamed:@"head.jpg"];
//    [img size];获取图片大小
//    [img drawInRect:CGRectMake(40, 40, 150, 150)];
//    在指定点为起点绘制图片
//    [img drawAtPoint:CGPointMake(150, 150)];
//    平铺图片，并且显示不下的会被割掉
    [img drawAsPatternInRect:CGRectMake(50, 50, 200, 200)];
```

### 切割图片

```
    UIImage * img = [UIImage imageNamed:@"default"];
    CGContextAddEllipseInRect(contextRef, CGRectMake(40, 40, 100, 100));
    CGContextClip(contextRef);
    CGContextFillPath(contextRef);
    [img drawInRect:CGRectMake(40, 40, 100, 100)];
```

![](/assets/剪裁图片.png)

### 平移、缩放（注意位置：原点即\(0,0\)点）

```
//    原点平移的位置，x平移了140，y值平移了60。可以简单理解为重新设置原点
//    CGContextTranslateCTM(contextRef, 140, 140);
//    旋转，按照原点进行旋转多少度。在旋转之后的坐标下平移，位置是根据旋转之后的计算
    CGContextRotateCTM(contextRef, M_PI_4);
    CGContextRotateCTM(contextRef, -M_PI_4);
//    缩放,使原来的坐标缩放了一定的比例
    CGContextScaleCTM(contextRef, 2, 1);
    
    UIImage * img = [UIImage imageNamed:@"default"];
    [img drawAtPoint:CGPointMake(20, 120)];
```

