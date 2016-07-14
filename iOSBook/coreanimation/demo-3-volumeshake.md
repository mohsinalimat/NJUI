# Demo-3-volumeShake `CAReplicatorLayer`

# 音量振动条
### 如果实现？
创建3个layer，按顺序播放y轴缩放动画

### 利用CAReplicatorLayer实现
1、什么是CAReplicatorLayer？

一种可以复制自己子层的layer,并且复制出来的layer和原生子层有同样的属性，位置，形变，动画。

2、CAReplicatorLayer属性

- `instanceCount`: 子层总数（包括原生子层）
- `instanceDelay`: 复制子层动画延迟时长
- `instanceTransform`: 复制子层形变(不包括原生子层)，每个复制子层都是相对上一个。
- `instanceColor`: 子层颜色，会和原生子层背景色冲突，因此二者选其一设置。
- `instanceRedOffset、instanceGreenOffset、instanceBlueOffset、instanceAlphaOffset`: 颜色通道偏移量，每个复制子层都是相对上一个的偏移量。

### 如果利用CAReplicatorLayer实现
1.首先创建复制layer,音乐振动条layer添加到复制layer上，然后复制子层就好了。

```objc
CAReplicatorLayer *layer = [CAReplicatorLayer layer];

    layer.frame = CGRectMake(50, 50, 200, 200);

    layer.backgroundColor = [UIColor lightGrayColor].CGColor;

    [self.view.layer addSublayer:layer];
```

2.先创建一个音量振动条，并且设置好动画,动画是绕着底部缩放，设置锚点

```objc
	CALayer *bar = [CALayer layer];

    bar.backgroundColor = [UIColor redColor].CGColor;

    bar.bounds = CGRectMake(0, 0, 30, 100);

    bar.position = CGPointMake(15, 200);

    bar.anchorPoint = CGPointMake(0.5, 1);

    [layer addSublayer:bar];

    CABasicAnimation *anim = [CABasicAnimation animation];

    anim.keyPath = @"transform.scale.y";

    anim.toValue = @(0.1);

    anim.autoreverses = YES;

    anim.repeatCount = MAXFLOAT;

    [bar addAnimation:anim forKey:nil];
```
3.复制子层

```objc
	// 设置4个子层，3个复制层
    layer.instanceCount = 4;

    // 设置复制子层的相对位置，每个x轴相差40
    layer.instanceTransform = CATransform3DMakeTranslation(40, 0, 0);

    // 设置复制子层的延迟动画时长
    layer.instanceDelay = 0.3;
```

# `demo3`

```objc
#import "ViewController.h"

@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIView *lightView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    // CAReplicatorLayer复制图层，可以把图层里面所有子层复制
    // 创建复制图层
    CAReplicatorLayer *repL = [CAReplicatorLayer layer];

    repL.frame = _lightView.bounds;

    [_lightView.layer addSublayer:repL];

    CALayer *layer = [CALayer layer];

    layer.anchorPoint = CGPointMake(0.5, 1);
    layer.position = CGPointMake(15, _lightView.bounds.size.height);
    layer.bounds = CGRectMake(0, 0, 30, 150);

    layer.backgroundColor = [UIColor whiteColor].CGColor;

    [repL addSublayer:layer];

    CABasicAnimation *anim = [CABasicAnimation animation];

    anim.keyPath = @"transform.scale.y";

    anim.toValue = @0.1;

    anim.duration = 0.5;

    anim.repeatCount = MAXFLOAT;

    // 设置动画反转
    anim.autoreverses = YES;


    [layer addAnimation:anim forKey:nil];


    // 复制层中子层总数
    // instanceCount：表示复制层里面有多少个子层，包括原始层
    repL.instanceCount = 3;

    // 设置复制子层偏移量，不包括原始层,相对于原始层x偏移
    repL.instanceTransform = CATransform3DMakeTranslation(45, 0, 0);

    // 设置复制层动画延迟时间
    repL.instanceDelay = 0.1;

    // 如果设置了原始层背景色，就不需要设置这个属性
    repL.instanceColor = [UIColor greenColor].CGColor;

    repL.instanceGreenOffset = -0.3;


}

```





