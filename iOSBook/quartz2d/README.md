# Quartz2D
- `Quartz 2D` 是二位绘图引擎

# `Layer Graphics Context`


## `1基础`

```objc


// 绘图的步骤： 1.获取上下文 2.创建路径（描述路径） 3.把路径添加到上下文 4.渲染上下文
// 通常在这个方法里面绘制图形
// 为什么要再drawRect里面绘图，只有在这个方法里面才能获取到跟View的layer相关联的图形上下文
// 什么时候调用:当这个View要显示的时候才会调用drawRect绘制图形，
// 注意：rect是当前控件的bounds

#pragma mark - 最原始的绘图方式
- (void)drawLine
{
    // 1.获取图形上下文
    // 目前我们所用的上下文都是以UIGraphics
    // CGContextRef Ref：引用 CG:目前使用到的类型和函数 一般都是CG开头 CoreGraphics
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 2.描述路径
    // 创建路径
    CGMutablePathRef path = CGPathCreateMutable();

    // 设置起点
    // path：给哪个路径设置起点
    CGPathMoveToPoint(path, NULL, 50, 50);

    // 添加一根线到某个点
    CGPathAddLineToPoint(path, NULL, 200, 200);

    // 3.把路径添加到上下文
    CGContextAddPath(ctx, path);

    // 4.渲染上下文
    CGContextStrokePath(ctx);

}


#pragma mark - 绘图第二种方式
- (void)drawLine1
{
    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 描述路径
    // 设置起点
    CGContextMoveToPoint(ctx, 50, 50);

    CGContextAddLineToPoint(ctx, 200, 200);

    // 渲染上下文
    CGContextStrokePath(ctx);

}


#pragma mark - 绘图第三种方式
- (void)drawLine2
{
    // UIKit已经封装了一些绘图的功能

    // 贝瑟尔路径
    // 创建路径
    UIBezierPath *path = [UIBezierPath bezierPath];

    // 设置起点
    [path moveToPoint:CGPointMake(50, 50)];

    // 添加一根线到某个点
    [path addLineToPoint:CGPointMake(200, 200)];

    // 绘制路径
    [path stroke];

    //    NSLog(@"%@",NSStringFromCGRect(rect));

}


- (void)drawCtxState
{
    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 描述路径
    //起点
    CGContextMoveToPoint(ctx, 50, 50);

    CGContextAddLineToPoint(ctx, 100, 50);

    // 设置起点
    CGContextMoveToPoint(ctx, 80, 60);

    // 默认下一根线的起点就是上一根线终点
    CGContextAddLineToPoint(ctx, 100, 200);

    // 设置绘图状态,一定要在渲染之前
    // 颜色
    [[UIColor redColor] setStroke];

    // 线宽
    CGContextSetLineWidth(ctx, 5);

    // 设置连接样式
    CGContextSetLineJoin(ctx, kCGLineJoinBevel);

    // 设置顶角样式
    CGContextSetLineCap(ctx, kCGLineCapRound);


    // 渲染上下文
    CGContextStrokePath(ctx);
}


- (void)drawUIBezierPathState
{
    UIBezierPath *path = [UIBezierPath bezierPath];

    [path moveToPoint:CGPointMake(50, 50)];

    [path addLineToPoint:CGPointMake(200, 200)];


    path.lineWidth = 10;
    [[UIColor redColor] set];

    [path stroke];

    UIBezierPath *path1 = [UIBezierPath bezierPath];

    [path1 moveToPoint:CGPointMake(0, 0)];

    [path1 addLineToPoint:CGPointMake(30, 60)];
    [[UIColor greenColor] set];

    path1.lineWidth = 3;

    [path1 stroke];
}


- (void)drawRect:(CGRect)rect {
    // Drawing code

    // 如何绘制曲线

    // 原生绘制方法

    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 描述路径
    // 设置起点
    CGContextMoveToPoint(ctx, 50, 50);

    // cpx:控制点的x
    CGContextAddQuadCurveToPoint(ctx, 150, 20, 250, 50);


    // 渲染上下文
    CGContextStrokePath(ctx);
}

```

## `2圆角矩形和圆弧`

```objc

- (void)drawRect:(CGRect)rect {
    // Drawing code

    // 圆角矩形
//   UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:
CGRectMake(20, 20, 200, 200) cornerRadius:100];


    // 圆弧
    // Center：圆心
    // startAngle:弧度
    // clockwise:YES:顺时针 NO：逆时针

    // 扇形
    CGPoint center = CGPointMake(125, 125);
     UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:100
     startAngle:0 endAngle:M_PI_2 clockwise:YES];

    // 添加一根线到圆心
    [path addLineToPoint:center];

    // 封闭路径，关闭路径：从路径的终点到起点
//    [path closePath];


//    [path stroke];

    // 填充：必须是一个完整的封闭路径,默认就会自动关闭路径
    [path fill];

}

```


## `3下载进度`

```objc

- (void)setProgress:(CGFloat)progress
{
    _progress = progress;


    // 重新绘制圆弧
//    [self drawRect:self.bounds];

    // 重绘，系统会先创建与view相关联的上下文，然后再调用drawRect
    [self setNeedsDisplay];
}


// 注意：drawRect不能手动调用，因为图形上下文我们自己创建不了，只能由系统帮我们创建，并且传递给我们

- (void)drawRect:(CGRect)rect {
    // Drawing code

    // 创建贝瑟尔路径
    CGFloat radius = rect.size.width * 0.5;
    CGPoint center = CGPointMake(radius, radius);


    CGFloat endA = -M_PI_2 + _progress * M_PI * 2;

    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:radius - 2 startAngle:-M_PI_2 endAngle:endA clockwise:YES];


    [path stroke];


}

```


## `4画饼图`

```objc

- (NSArray *)arrRandom
{
    int total = 100;
    int temp = 0;
    NSMutableArray *arrM = [NSMutableArray array];
    for (int i = 0; i < arc4random_uniform(10)+1; i++) {
        temp = arc4random_uniform(total);

        [arrM addObject:@(temp)];

        total -= temp;
    }

    if(total) [arrM addObject:@(total)];
    return arrM;

}
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code

    NSArray *arr = [self arrRandom];
    CGFloat radius = rect.size.width * 0.5;
    CGPoint center = CGPointMake(radius, radius);


    CGFloat startA = 0;
    CGFloat angle = 0;
    CGFloat endA = 0;


    for (int i = 0; i < arr.count; i++) {
        startA = endA;
        angle = [arr[i] integerValue] / 100.0 * M_PI * 2;
        endA = startA + angle;
        UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:startA endAngle:endA clockwise:YES];

        [path addLineToPoint:center];

        [[self colorRandom] set];

        [path fill];
    }


}
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [self setNeedsDisplay];
}
- (UIColor *)colorRandom
{
    // 0 ~ 255 / 255
    // OC:0 ~ 1
    CGFloat r = arc4random_uniform(256) / 255.0;
    CGFloat g = arc4random_uniform(256) / 255.0;
    CGFloat b = arc4random_uniform(256) / 255.0;
    return [UIColor colorWithRed:r green:g blue:b alpha:1];
}

- (void)draw
{
    CGFloat radius = self.bounds.size.width * 0.5;
    CGPoint center = CGPointMake(radius, radius);


    CGFloat startA = 0;
    CGFloat angle = 0;
    CGFloat endA = 0;


    // 第一个扇形
    angle = 25 / 100.0 * M_PI * 2;
    endA = startA + angle;
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:startA endAngle:endA clockwise:YES];

    // 添加一根线到圆心
    [path addLineToPoint:center];

    // 描边和填充通用
    [[UIColor redColor] set];

    [path fill];

    // 第二个扇形
    startA = endA;
    angle = 25 / 100.0 * M_PI * 2;
    endA = startA + angle;
    UIBezierPath *path1 = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:startA endAngle:endA clockwise:YES];

    // 添加一根线到圆心
    [path1 addLineToPoint:center];

    // 描边和填充通用
    [[UIColor greenColor] set];

    [path1 fill];

    // 第二个扇形
    startA = endA;
    angle = 50 / 100.0 * M_PI * 2;
    endA = startA + angle;
    UIBezierPath *path2 = [UIBezierPath bezierPathWithArcCenter:center radius:radius startAngle:startA endAngle:endA clockwise:YES];

    // 添加一根线到圆心
    [path2 addLineToPoint:center];

    // 描边和填充通用
    [[UIColor blueColor] set];

    [path2 fill];

}

```


## `5柱状图`

```objc
- (void)drawRect:(CGRect)rect {
    // Drawing code

    NSArray *arr = [self arrRandom];


    CGFloat x = 0;
    CGFloat y = 0;
    CGFloat w = 0;
    CGFloat h = 0;

    for (int i = 0; i < arr.count; i++) {

        w = rect.size.width / (2 * arr.count - 1);
        x = 2 * w * i;
        h = [arr[i] floatValue] / 100.0 * rect.size.height;
        y = rect.size.height - h;


        UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(x, y, w, h)];


        [[self colorRandom] set];

        [path fill];


    }

}

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    [self setNeedsDisplay];
}
- (NSArray *)arrRandom
{
    int total = 100;
    int temp = 0;
    NSMutableArray *arrM = [NSMutableArray array];
    for (int i = 0; i < arc4random_uniform(10)+1; i++) {
        temp = arc4random_uniform(total);

        [arrM addObject:@(temp)];

        total -= temp;
    }

    if(total) [arrM addObject:@(total)];
    return arrM;

}

- (UIColor *)colorRandom
{
    // 0 ~ 255 / 255
    // OC:0 ~ 1
    CGFloat r = arc4random_uniform(256) / 255.0;
    CGFloat g = arc4random_uniform(256) / 255.0;
    CGFloat b = arc4random_uniform(256) / 255.0;
    return [UIColor colorWithRed:r green:g blue:b alpha:1];

}

```


## `6绘制文字`

```objc

- (void)drawText
{
    // 绘制文字

    NSString *str = @"asfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdfasfdsfsdf";
    // 不会换行
    //    [str drawAtPoint:CGPointZero withAttributes:nil];

    [str drawInRect:self.bounds withAttributes:nil];

}
- (void)attrText
{
    // 绘制文字

    NSString *str = @"asfdsfsdf";

    // 文字的起点
    // Attributes：文本属性

    NSMutableDictionary *textDict = [NSMutableDictionary dictionary];

    // 设置文字颜色
    textDict[NSForegroundColorAttributeName] = [UIColor redColor];

    // 设置文字字体
    textDict[NSFontAttributeName] = [UIFont systemFontOfSize:30];

    // 设置文字的空心颜色和宽度

    textDict[NSStrokeWidthAttributeName] = @3;

    textDict[NSStrokeColorAttributeName] = [UIColor yellowColor];

    // 创建阴影对象
    NSShadow *shadow = [[NSShadow alloc] init];
    shadow.shadowColor = [UIColor greenColor];
    shadow.shadowOffset = CGSizeMake(4, 4);
    shadow.shadowBlurRadius = 3;
    textDict[NSShadowAttributeName] = shadow;

    // 富文本:给普通的文字添加颜色，字体大小
    [str drawAtPoint:CGPointZero withAttributes:textDict];
}

```


## `7绘制图片`


```objc

- (void)drawRect:(CGRect)rect {
    // Drawing code

    // 超出裁剪区域的内容全部裁剪掉
    // 注意：裁剪必须放在绘制之前
    UIRectClip(CGRectMake(0, 0, 50, 50));

    UIImage *image = [UIImage imageNamed:@"001"];

    // 默认绘制的内容尺寸跟图片尺寸一样大
//    [image drawAtPoint:CGPointZero];

//    [image drawInRect:rect];
    // 绘图
    [image drawAsPatternInRect:rect];

}

- (void)awakeFromNib{
//    UIImage *image = [UIImage imageNamed:@"001"];
//
//    // 默认绘制的内容尺寸跟图片尺寸一样大
//    //    [image drawAtPoint:CGPointZero];
//
//
//    //    [image drawInRect:rect];
//    // 绘图
//    [image drawAsPatternInRect:self.bounds];
}

```
---

```objc
- (instancetype)initWithImage:(UIImage *)image
{
    // 默认跟图片尺寸一样大
    if (self = [super initWithFrame:CGRectMake(0, 0, image.size.width, image.size.height)]) {

        _image = image;
    }

    return self;
}

- (void)setImage:(UIImage *)image
{
    _image = image;

    [self setNeedsDisplay];
}

- (void)drawRect:(CGRect)rect {


    [_image drawInRect:rect];
}

```

## `8雪花定时器`

```objc
- (void)drawRect:(CGRect)rect {
    // Drawing code


    // 如果以后想绘制东西到view上面，必须在drawRect方法里面，不管有没有手动获取到上下文

    // 修改雪花y值
    UIImage *image =  [UIImage imageNamed:@"雪花"];

    [image drawAtPoint:CGPointMake(50, _snowY)];

    _snowY += 10;

    if (_snowY > rect.size.height) {
        _snowY = 0;
    }

}

// 如果在绘图的时候需要用到定时器，通常

// NSTimer很少用于绘图，因为调度优先级比较低，并不会准时调用
- (void)awakeFromNib
{
    // 创建定时器
//    [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(timeChange) userInfo:nil repeats:YES];
    CADisplayLink *link = [CADisplayLink displayLinkWithTarget:self selector:@selector(timeChange)];

    // 添加主运行循环
    [link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];
}

// CADisplayLink:每次屏幕刷新的时候就会调用，屏幕一般一秒刷新60次

// 1秒 2次
//static int count = 0;
- (void)timeChange
{
//    count++;
//    if (count % 30) {// 一秒钟调用2次
//
//    }


    // 注意：这个方法并不会马上调用drawRect,其实这个方法只是给当前控件添加刷新的标记，等下一次屏幕刷新的时候才会调用drawRect
    [self setNeedsDisplay];
}

```

## `9贝瑟尔上下文状态用最原始的方法处理`

```objc

- (void)drawRect:(CGRect)rect {
    // Drawing code

    // 1.获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 2.描述路径
    // 第一根
    UIBezierPath *path = [UIBezierPath bezierPath];

    [path moveToPoint:CGPointMake(10, 125)];

    [path addLineToPoint:CGPointMake(240, 125)];

    // 把路径添加到上下文
    // .CGPath 可以UIkit的路径转换成CoreGraphics路径
    CGContextAddPath(ctx, path.CGPath);

    // 保存一份上下文的状态
    CGContextSaveGState(ctx);



    // 设置上下文状态
    CGContextSetLineWidth(ctx, 10);

    [[UIColor redColor] set];

    // 渲染上下文
    CGContextStrokePath(ctx);

    // 第二根



    // 2.描述路径
    // 第一根
    path = [UIBezierPath bezierPath];

    [path moveToPoint:CGPointMake(125, 10)];

    [path addLineToPoint:CGPointMake(125, 240)];

    // 把路径添加到上下文
    // .CGPath 可以UIkit的路径转换成CoreGraphics路径
    CGContextAddPath(ctx, path.CGPath);

    // 还原状态
    CGContextRestoreGState(ctx);
//    // 设置上下文状态
//    CGContextSetLineWidth(ctx, 1);
//
//    [[UIColor blackColor] set];

    // 渲染上下文
    CGContextStrokePath(ctx);

}

```

## `10平缩放旋转`

```objc

- (void)drawRect:(CGRect)rect {

    // 1.获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();


    // 2.描述路径
   UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(-100, -50, 200, 100)];

    [[UIColor redColor] set];

    // 上下文矩阵操作
    // 注意:矩阵操作必须要在添加路径之前

    //  平移
    CGContextTranslateCTM(ctx, 100, 50);

    // 缩放
    CGContextScaleCTM(ctx, 0.5, 0.5);

    // 旋转

    CGContextRotateCTM(ctx, M_PI_4);

    // 3.把路径添加上下文
    CGContextAddPath(ctx, path.CGPath);



    [[UIColor redColor] set];


    // 4.渲染上下文
    CGContextFillPath(ctx);

}

```
