# path Anim

```objc


#import "DrawView.h"

@interface DrawView ()

@property (nonatomic, strong) UIBezierPath *path;

@end

@implementation DrawView


- (UIBezierPath *)path
{
    if (_path == nil) {
        _path = [UIBezierPath bezierPath];
    }

    return _path;
}

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    UITouch *touch = [touches anyObject];

    // 获取手指的触摸点
    CGPoint curP = [touch locationInView:self];

    [self.path moveToPoint:curP];

}

- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
    UITouch *touch = [touches anyObject];

    // 获取手指的触摸点
    CGPoint curP = [touch locationInView:self];

    [self.path addLineToPoint:curP];

    [self setNeedsDisplay];
}

- (void)drawRect:(CGRect)rect
{
    [self.path stroke];
}

// 跟随路径而动画
- (void)startAnim
{
    // 图层
    CAShapeLayer *layer = [CAShapeLayer layer];

    layer.path = self.path.CGPath;

    layer.fillColor = [UIColor greenColor].CGColor;

    layer.strokeColor = [UIColor redColor].CGColor;

    layer.strokeEnd = 1;

    [self.layer addSublayer:layer];

    CABasicAnimation *anim = [CABasicAnimation animation];

    anim.keyPath = @"strokeEnd";
    anim.fromValue = @0;
    anim.toValue = @1;
    anim.duration = 5;
    [layer addAnimation:anim forKey:nil];

    // 清空画线
    [self.path removeAllPoints];

    [self setNeedsDisplay];

}

@end


```
