# Demo-1

#`转盘`

```objc
#import "LMJWheelButton.h"

@implementation LMJWheelButton


- (void)layoutSubviews
{
    [super layoutSubviews];
    UIImage *image = [UIImage imageNamed:@"LuckyAstrology"];

    CGFloat w = image.size.width / 12.0;
    CGFloat h = image.size.height;
    CGFloat x = (self.bounds.size.width - w) / 2.0;
    CGFloat y = (self.bounds.size.height - h) / 5.0;

    self.imageView.frame = CGRectMake(x, y, w, h);
}

- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event
{
    // 设置一个可以点击的范围
    CGFloat w = self.bounds.size.width;
    CGFloat h = self.bounds.size.height / 2.0;

    // 设置按钮的上班部分可以点击
    CGRect rect = CGRectMake(0, 0, w, h);

    if(CGRectContainsPoint(rect, point))
    {
        return [super hitTest:point withEvent:event];
    }
    else
    {
        return nil;
    }
}
- (void)setHighlighted:(BOOL)highlighted
{

}
@end

```

----

```objc
#import "LMJWheelView.h"
#import "LMJWheelButton.h"
#define keyPath(obj, key) @(((void)obj.key, #key))
#define angle2Radion(angle) ((angle) / 180.0 * M_PI)

@interface LMJWheelView ()
@property (weak, nonatomic) IBOutlet UIImageView *centerImageView;
/** <#digest#> */
@property (weak, nonatomic) UIButton *selectedBtn;

/** <#digest#> */
@property (nonatomic, strong) CADisplayLink *link;
@end

@implementation LMJWheelView

- (CADisplayLink *)link
{
    if(_link == nil)
    {
        _link = [CADisplayLink displayLinkWithTarget:self selector:@selector(angleChange)];
        [_link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];
    }
    return _link;
}
// 背景图片
- (void)drawRect:(CGRect)rect
{
    UIImage *image = [UIImage imageNamed:@"LuckyBaseBackground"];
    [image drawInRect:rect];
}

// 添加按钮, 设置按钮的属性
- (void)awakeFromNib
{
    [super awakeFromNib];
    NSInteger count = 12;

    for (NSInteger i = 0; i < count; i++)
    {
        LMJWheelButton *btn = [LMJWheelButton buttonWithType:UIButtonTypeCustom];
        [self.centerImageView addSubview:btn];
        btn.tag = i;
        btn.layer.anchorPoint = CGPointMake(0.5, 1);
        btn.transform = CGAffineTransformMakeRotation(angle2Radion(360.0/12.0 * i));
        [btn addTarget:self action:@selector(btnClick:) forControlEvents:UIControlEventTouchUpInside];
        [self setupBtnAllImages:btn];

        if(i == 0) [self btnClick:btn];
    }
}
- (void)btnClick:(UIButton *)btn
{
    self.selectedBtn.selected = NO;
    btn.selected = YES;
    self.selectedBtn = btn;
}
- (void)setupBtnAllImages:(UIButton *)btn
{
    // 设置选中时候的背景图片
    [btn setBackgroundImage:[UIImage imageNamed:@"LuckyRototeSelected"] forState:UIControlStateSelected];

    // 加载大图片
    UIImage *bigNormalImage = [UIImage imageNamed:@"LuckyAstrology"];
    UIImage *bigSelectedImage = [UIImage imageNamed:@"LuckyAstrologyPressed"];

    // 因为裁剪的时候是根据像素进行裁剪的, 所以求出裁剪的像素宽高
    CGFloat scale = [UIScreen mainScreen].scale;
    CGFloat clipImageW = bigNormalImage.size.width / 12.0 * scale;
    CGFloat clipImageH = bigNormalImage.size.height * scale;

    // 裁剪的frame
    CGRect clipRect = CGRectMake(btn.tag * clipImageW, 0, clipImageW, clipImageH);
    CGImageRef normalImage = CGImageCreateWithImageInRect(bigNormalImage.CGImage, clipRect);
    CGImageRef selectedImage = CGImageCreateWithImageInRect(bigSelectedImage.CGImage, clipRect);

    [btn setImage:[UIImage imageWithCGImage:normalImage] forState:UIControlStateNormal];
    [btn setImage:[UIImage imageWithCGImage:selectedImage] forState:UIControlStateSelected];

}

// 设置按钮的位置和尺寸
- (void)layoutSubviews
{
    [super layoutSubviews];
    UIImage *image = [UIImage imageNamed:@"LuckyBaseBackground"];
    self.bounds = CGRectMake(0, 0, image.size.width, image.size.height);

    UIImage *btnImage = [UIImage imageNamed:@"LuckyRototeSelected"];

    CGFloat btnW = btnImage.size.width;
    CGFloat btnH = btnImage.size.height;

    for (LMJWheelButton *btn in self.centerImageView.subviews) {
        btn.bounds = CGRectMake(0, 0, btnW, btnH);
        btn.layer.position = self.centerImageView.center;

    }
}

- (void)pause
{
    self.link.paused = YES;
}

- (void)start
{
    self.link.paused = NO;
}
- (void)angleChange
{
    // 1秒钟, 调用了60 次, 只转动30度
    CGFloat angle = angle2Radion(30.0/60.0);

    self.centerImageView.transform = CGAffineTransformRotate(self.centerImageView.transform, angle);
}
// 加载XIB
+ (instancetype)wheelView
{
    return [[NSBundle mainBundle] loadNibNamed:NSStringFromClass(self) owner:nil options:nil].lastObject;
}

- (IBAction)startPicker:(UIButton *)sender
{
    // 1创建一个动画, 并且centerView不需要和用户交互
    CABasicAnimation *anim = [CABasicAnimation animation];
    anim.duration = arc4random_uniform(2)+1;
    anim.keyPath = @"transform.rotation";
    anim.toValue = @(angle2Radion(360.0 * (arc4random_uniform(3)+1)));

    anim.delegate = self;
    [self.centerImageView.layer addAnimation:anim forKey:nil];
}
- (void)animationDidStart:(CAAnimation *)anim
{
    self.centerImageView.userInteractionEnabled = NO;
    self.link.paused = YES;
}

- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag
{
    CGFloat angle = atan2(self.selectedBtn.transform.b, self.selectedBtn.transform.a);

    self.centerImageView.transform = CGAffineTransformMakeRotation(-angle);

    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        self.link.paused = NO;
        self.centerImageView.userInteractionEnabled = YES;
    });
}
@end

```
