# drawerUSE

```objc

#import "LMJDrawerViewController.h"

#define MaxOffSetY 80.0
#define screenW [UIScreen mainScreen].bounds.size.width
#define screenH [UIScreen mainScreen].bounds.size.height
#define yxScale MaxOffSetY/screenW
#define MaxOffsetX 310.0
#define MinOffsetX -250.0

@interface LMJDrawerViewController ()
@property (weak, nonatomic, readonly) UIView *leftView;
@property (weak, nonatomic, readonly) UIView *rightView;
@property (weak, nonatomic, readonly) UIView *mainView;
@end

@implementation LMJDrawerViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupViews];
    UIPanGestureRecognizer *panGR = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];
    [self.view addGestureRecognizer:panGR];
    UITapGestureRecognizer *tapGR = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tap:)];
    [self.view addGestureRecognizer:tapGR];
}

- (void)tap:(UITapGestureRecognizer *)tapGR
{
    if(self.mainView.frame.origin.x != 0)
    {
        [UIView animateWithDuration:0.25 animations:^{
            self.mainView.frame = self.view.bounds;
        }];
    }
}
- (void)pan:(UIPanGestureRecognizer *)panGR
{
    CGPoint transP = [panGR translationInView:self.view];

    // 根据偏移点算出新的frame
    self.mainView.frame = [self frameWithOffSetX:transP.x];
    self.rightView.hidden = (self.mainView.frame.origin.x > 0);

    [panGR setTranslation:CGPointZero inView:self.view];

    if(panGR.state == UIGestureRecognizerStateEnded)
    {
        CGFloat offSetX;
        if(self.mainView.frame.origin.x > screenW/2)
        {
            offSetX = MaxOffsetX - self.mainView.frame.origin.x;
        }
        else if (CGRectGetMaxX(self.mainView.frame) < screenW/2)
        {
            offSetX = MinOffsetX - self.mainView.frame.origin.x;
        }
        else
        {
            offSetX = 0 - self.mainView.frame.origin.x;;
        }
        [UIView animateWithDuration:0.25 animations:^{
            self.mainView.frame = [self frameWithOffSetX:offSetX];
        }];
    }
}

- (CGRect)frameWithOffSetX:(CGFloat)offSetX
{
    CGRect frame = self.mainView.frame;

    CGFloat preX = frame.origin.x;
    CGFloat preW = frame.size.width;
    CGFloat preH = frame.size.height;

    CGFloat nowX = preX + offSetX;
    CGFloat nowY = yxScale * (nowX > 0 ? nowX : (-nowX));

    CGFloat nowH = screenH - 2.0 * nowY;
    CGFloat hScale = nowH / preH;
    CGFloat nowW = hScale * preW;

    frame.size.width = nowW;
    frame.size.height = nowH;
    frame.origin.x = nowX;
    frame.origin.y = nowY;

    return frame;
}
- (void)setupViews
{
    UIView *leftView = [[UIView alloc] initWithFrame:self.view.bounds];
    leftView.backgroundColor = [UIColor greenColor];
    [self.view addSubview:leftView];
    _leftView = leftView;


    UIView *rightView = [[UIView alloc] initWithFrame:self.view.bounds];
    rightView.backgroundColor = [UIColor blueColor];
    [self.view addSubview:rightView];
    _rightView = rightView;


    UIView *mainView = [[UIView alloc] initWithFrame:self.view.bounds];
    mainView.backgroundColor = [UIColor redColor];
    [self.view addSubview:mainView];
    _mainView = mainView;

}
@end


```

---

![](../LibrarypPictures/Snip20160605_1.png)
