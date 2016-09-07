# Image:Quartz2D

# `Bitmap Graphics Context`


## `1图片加水印后生成新的图片`

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.

    // 加载图片
    UIImage *image = [UIImage imageNamed:@"小黄人"];

    // 0.获取上下文，之前的上下文都是在view的drawRect方法中获取（跟View相关联的上下文layer上下文）
    // 目前我们需要绘制图片到新的图片上，因此需要用到位图上下文

    // 怎么获取位图上下文,注意位图上下文的获取方式跟layer上下文不一样。位图上下文需要我们手动创建。

    // 开启一个位图上下文，注意位图上下文跟view无关联，所以不需要在drawRect.
    // size:位图上下文的尺寸（新图片的尺寸）
    // opaque: 不透明度 YES：不透明 NO:透明，通常我们一般都弄透明的上下文
    // scale:通常不需要缩放上下文，取值为0，表示不缩放
    UIGraphicsBeginImageContextWithOptions(image.size, NO, 0);

    /*第三种方式
    // 1.获取上下文(位图上下文)
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 2.描述路径
    CGContextMoveToPoint(ctx, 50, 50);

    CGContextAddLineToPoint(ctx, 200, 200);

    [[UIColor redColor] set];

    // 3.渲染上下文
    CGContextStrokePath(ctx);
    */


    /*第二种方式
//    UIBezierPath *path =[UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, 300, 300)];
//
//    [[UIColor redColor] set];
//    [path stroke];
    */


    /*第三种方式
//    // 1.绘制原生的图片
//    [image drawAtPoint:CGPointZero];
//
//    // 2.给原生的图片添加文字
//    NSString *str = @"小码哥";
//
//    // 创建字典属性
//    NSMutableDictionary *dict = [NSMutableDictionary dictionary];
//    dict[NSForegroundColorAttributeName] = [UIColor redColor];
//    dict[NSFontAttributeName] = [UIFont systemFontOfSize:20];
//
//    [str drawAtPoint:CGPointMake(200, 528) withAttributes:dict];
    */
    // 3.生成一张图片给我们,从上下文中获取图片
    UIImage *imageWater = UIGraphicsGetImageFromCurrentImageContext();

    // 4.关闭上下文
//    UIGraphicsEndImageContext();

    _imageView.image = imageWater;



}

```



## `2图片的裁剪`

```objc
+ (UIImage *)imageWithClipImage:(UIImage *)image borderWidth:(CGFloat)borderWidth borderColor:(UIColor *)color
{
    // 图片的宽度和高度
    CGFloat imageWH = image.size.width;

    // 设置圆环的宽度
    CGFloat border = borderWidth;

    // 圆形的宽度和高度
    CGFloat ovalWH = imageWH + 2 * border;

    // 1.开启上下文
    UIGraphicsBeginImageContextWithOptions(CGSizeMake(ovalWH, ovalWH), NO, 0);

    // 2.画大圆
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, ovalWH, ovalWH)];

    [color set];

    [path fill];

    // 3.设置裁剪区域
    UIBezierPath *clipPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(border, border, imageWH, imageWH)];
    [clipPath addClip];

    // 4.绘制图片
    [image drawAtPoint:CGPointMake(border, border)];

    // 5.获取图片
    UIImage *clipImage = UIGraphicsGetImageFromCurrentImageContext();

    // 6.关闭上下文
    UIGraphicsEndImageContext();

    return clipImage;

}

```


## `3屏幕截屏`

```objc

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.

    // 生成一张新的图片

  UIImage *newImage =  [UIImage imageWithCaputureView:self.view];

    // image转data
    // compressionQuality： 图片质量 1:最高质量

    //    NSData *data = UIImageJPEGRepresentation(newImage, 1);
    NSData *data = UIImagePNGRepresentation(newImage);

    [data writeToFile:@"/Users/xiaomage/Desktop/view.png" atomically:YES];

}

+ (UIImage *)imageWithCaputureView:(UIView *)view
{
    // 开启位图上下文
    UIGraphicsBeginImageContextWithOptions(view.bounds.size, NO, 0);

    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 把控件上的图层渲染到上下文,layer只能渲染
    [view.layer renderInContext:ctx];

    // 生成一张图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

    // 关闭上下文
    UIGraphicsEndImageContext();

    return image;
}

```

## `4图片截取`

```objc

#import "ViewController.h"

@interface ViewController ()

@property (nonatomic, assign) CGPoint startP;

@property (weak, nonatomic) IBOutlet UIImageView *imageV;
@property (nonatomic, weak) UIView *clipView;

@end

@implementation ViewController

- (UIView *)clipView{
    if (_clipView == nil) {
        UIView *view = [[UIView alloc] init];
        _clipView = view;

        view.backgroundColor = [UIColor blackColor];
        view.alpha = 0.5;

        [self.view addSubview:view];
    }

    return _clipView;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.

    // 给控制器的view添加一个pan手势
    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];

    [self.view addGestureRecognizer:pan];
}

- (void)pan:(UIPanGestureRecognizer *)pan
{
    CGPoint endA = CGPointZero;

    if (pan.state == UIGestureRecognizerStateBegan) { // 一开始拖动的时候

        // 获取一开始触摸点
      _startP = [pan locationInView:self.view];

    }else if(pan.state == UIGestureRecognizerStateChanged){ // 一直拖动
        // 获取结束点
         endA = [pan locationInView:self.view];

        CGFloat w = endA.x - _startP.x;
        CGFloat h = endA.y - _startP.y;

        // 获取截取范围
        CGRect clipRect = CGRectMake(_startP.x, _startP.y, w, h);


        // 生成截屏的view
        self.clipView.frame = clipRect;



    }else if (pan.state == UIGestureRecognizerStateEnded){


        // 图片裁剪，生成一张新的图片

        // 开启上下文
        // 如果不透明，默认超出裁剪区域会变成黑色，通常都是透明
        UIGraphicsBeginImageContextWithOptions(_imageV.bounds.size, NO, 0);

        // 2设置裁剪的范围
        //        UIBezierPath *bezier = [UIBezierPath bezierPathWithRect:self.clipView.frame];
        //        [bezier addClip];

        UIRectClip(self.clipView.frame);

        // 获取上下文
        CGContextRef ctx = UIGraphicsGetCurrentContext();

        // 把控件上的内容渲染到上下文
        [_imageV.layer renderInContext:ctx];


        // 生成一张新的图片
        _imageV.image = UIGraphicsGetImageFromCurrentImageContext();


        // 关闭上下文
        UIGraphicsEndImageContext();


        // 先移除
        [_clipView removeFromSuperview];
        // 截取的view设置为nil
        _clipView = nil;

    }

}

```


## `5图片擦除`

```objc

@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.

    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];

    [self.view addGestureRecognizer:pan];
}


- (void)pan:(UIPanGestureRecognizer *)pan
{
    // 获取当前点
    CGPoint curP = [pan locationInView:self.view];

    // 获取擦除的矩形范围
    CGFloat wh = 100;
    CGFloat x = curP.x - wh * 0.5;
    CGFloat y = curP.y - wh * 0.5;

    CGRect rect = CGRectMake(x, y, wh, wh);

    // 开启上下文
    UIGraphicsBeginImageContextWithOptions(self.view.bounds.size, NO, 0);

    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 控件的layer渲染上去
    [_imageView.layer renderInContext:ctx];

    // 擦除图片
    CGContextClearRect(ctx, rect);

    // 生成一张图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

    _imageView.image = image;

    // 关闭上下文
    UIGraphicsEndImageContext();
}


```





