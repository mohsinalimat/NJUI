# CAKeyframeAnimation

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160612_23.png)


```objc

#import "DrawView.h"

@interface DrawView ()

@property (nonatomic, strong) UIBezierPath *path;

@end

@implementation DrawView

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    // touch
    UITouch *touch = [touches anyObject];

    // 获取手指的触摸点
    CGPoint curP = [touch locationInView:self];

    // 创建路径
    UIBezierPath *path = [UIBezierPath bezierPath];
    _path = path;

    // 设置起点
    [path moveToPoint:curP];

}

- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
    // touch
    UITouch *touch = [touches anyObject];

    // 获取手指的触摸点
    CGPoint curP = [touch locationInView:self];

    [_path addLineToPoint:curP];

    [self setNeedsDisplay];
}

- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event
{
    // 给imageView添加核心动画
    // 添加核心动画

    CAKeyframeAnimation *anim = [CAKeyframeAnimation animation];


    anim.keyPath = @"position";

    //    anim.values = @[@(angle2Radion(-10)),@(angle2Radion(10)),@(angle2Radion(-10))];

    anim.path = _path.CGPath;

    anim.duration = 1;

    anim.repeatCount = MAXFLOAT;

    [[[self.subviews firstObject] layer] addAnimation:anim forKey:nil];
}

- (void)drawRect:(CGRect)rect
{
    [_path stroke];
}


```
