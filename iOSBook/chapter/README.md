
# `autoresizing`
- Xcode6 以前的方法



````objc
/*
 UIViewAutoresizingNone                 = 0,

 UIViewAutoresizingFlexibleLeftMargin   = 1 << 0, 左边的间距是否可变, 默认固定
 UIViewAutoresizingFlexibleRightMargin  = 1 << 2, // 右边的间距是否可变, 默认固定
 UIViewAutoresizingFlexibleTopMargin    = 1 << 3, // 顶部的间距是否可变, 默认固定
 UIViewAutoresizingFlexibleBottomMargin = 1 << 5 // 底部的间距是否可变, 默认固定

  UIViewAutoresizingFlexibleHeight       = 1 << 4, // 高度是否可变, 默认不可以
  UIViewAutoresizingFlexibleWidth        = 1 << 1, // 宽度是否可变, 默认不可以
 */
@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    UIView *blueView = [[UIView alloc] init];
    blueView.backgroundColor = [UIColor redColor];
    CGFloat wh = 100;
    CGFloat x = self.view.frame.size.width - wh;
    CGFloat y = self.view.frame.size.height - wh;
    blueView.frame = CGRectMake(x, y, wh, wh);

    blueView.autoresizingMask = UIViewAutoresizingFlexibleTopMargin | UIViewAutoresizingFlexibleLeftMargin;

    [self.view addSubview:blueView];

}

````
