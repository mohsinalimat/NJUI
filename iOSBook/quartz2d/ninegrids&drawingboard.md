# nineGrids&drawingBoard

## `1九宫格`

```objc

#import "LockView.h"

@interface LockView ()

@property (nonatomic, strong) NSMutableArray *selectedsBtn;

@property (nonatomic, assign) CGPoint curP;

@end

@implementation LockView

- (NSMutableArray *)selectedsBtn
{
    if (_selectedsBtn == nil) {
        _selectedsBtn = [NSMutableArray array];
    }

    return _selectedsBtn;
}

- (IBAction)pan:(UIPanGestureRecognizer *)pan
{
    // 获取触摸点
    _curP = [pan locationInView:self];

    // 判断触摸点在不在按钮上
    for (UIButton *btn in self.subviews) {
        // 点在不在某个范围内,并且按钮没有被选中
        if (CGRectContainsPoint(btn.frame, _curP) && btn.selected == NO) {
            // 点在按钮上
            btn.selected = YES;

            // 保存到数组中
            [self.selectedsBtn addObject:btn];

        }

    }

    if (pan.state == UIGestureRecognizerStateEnded) {

        // 创建可变字符串
        NSMutableString *strM = [NSMutableString string];
         // 保存输入密码
        for (UIButton *btn in self.selectedsBtn) {

            [strM appendFormat:@"%ld",btn.tag];

        }
        NSLog(@"%@",strM);

        // 还原界面

        // 取消所有按钮的选中
        [self.selectedsBtn makeObjectsPerformSelector:@selector(setSelected:) withObject:NULL];

        // 清除画线,把选中按钮清空
        [self.selectedsBtn removeAllObjects];
    }

    // 重绘
    [self setNeedsDisplay];
}
// 加载完xib的时候调用
- (void)awakeFromNib
{


    // 创建9个按钮
    for ( int i = 0; i < 9; i++) {
        UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];

        // 不允许用户交互，按钮就不能点击，也就不能达到高亮状态
        btn.userInteractionEnabled = NO;

        [btn setImage:[UIImage imageNamed:@"gesture_node_normal"] forState:UIControlStateNormal];

        [btn setImage:[UIImage imageNamed:@"gesture_node_highlighted"] forState:UIControlStateSelected];

        btn.tag = i;

        [self addSubview:btn];
    }
}


// 为什么要在这个方法布局子控件，因为只要一调用这个方法，就表示父控件的尺寸确定
- (void)layoutSubviews
{
    [super layoutSubviews];

    NSUInteger count = self.subviews.count;
    int cols = 3;

    CGFloat x = 0;
    CGFloat y = 0;
    CGFloat w = 74;
    CGFloat h = 74;
    CGFloat margin = (self.bounds.size.width - cols * w) / (cols + 1);

    CGFloat col = 0;
    CGFloat row = 0;
    for (NSUInteger i = 0; i < count; i++) {
        UIButton *btn = self.subviews[i];
        // 获取当前按钮的列数
        col = i % cols;
        row = i / cols;
        x = margin + col * (margin + w);
        y = row * (margin + w);

        btn.frame = CGRectMake(x, y, w, h);

    }

}

// 只要调用这个方法，就会把之前绘制的东西全部清掉，重新绘制
- (void)drawRect:(CGRect)rect
{
    // 没有选中按钮，不需要连线
    if (self.selectedsBtn.count == 0) return;

    // 把所有选中按钮中心点连线
    UIBezierPath *path = [UIBezierPath bezierPath];


    NSUInteger count = self.selectedsBtn.count;
    // 把所有选中按钮之间都连好线
    for (int i = 0; i < count; i++) {
        UIButton *btn = self.selectedsBtn[i];
        if (i == 0) {
            // 设置起点
            [path moveToPoint:btn.center];
        }else{
            [path addLineToPoint:btn.center];
        }

    }

    // 连线到手指的触摸点
    [path addLineToPoint:_curP];


    [[UIColor greenColor] set];
    path.lineWidth = 10;
    path.lineJoinStyle = kCGLineJoinRound;
    [path stroke];
}

@end

```

## `2画板`

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160610_1.png)



```objc

//  ImageHandleView.m
//  08-画板

#import "ImageHandleView.h"

@interface ImageHandleView ()<UIGestureRecognizerDelegate>

@property (nonatomic, weak) UIImageView *imageV;

@end

@implementation ImageHandleView
//- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event{
//        NSLog(@"%s",__func__);
//}
//- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event
//{
//    // 如果点在UIImageView,
//    return self.imageV;
//}

- (UIImageView *)imageV
{
    if (_imageV == nil) {

        UIImageView *imageV = [[UIImageView alloc] initWithFrame:self.bounds];

        imageV.userInteractionEnabled = YES;

        _imageV = imageV;

        // 添加手势
        [self setUpGestureRecognizer];

        [self addSubview:imageV];

    }

    return _imageV;
}

#pragma mark - 拦截拖动手势
- (void)panHandle
{
    NSLog(@"%s",__func__);
}
#pragma mark - 添加手势
- (void)setUpGestureRecognizer
{

    // 添加拖动手势给ImageHandleView
    UIPanGestureRecognizer *panHandle = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(panHandle)];
    [self addGestureRecognizer:panHandle];

    // 平移
    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];
    [_imageV addGestureRecognizer:pan];

    // 旋转
    UIRotationGestureRecognizer *rotation = [[UIRotationGestureRecognizer alloc] initWithTarget:self action:@selector(rotation:)];
    rotation.delegate = self;
    [_imageV addGestureRecognizer:rotation];

    // 缩放
    UIPinchGestureRecognizer *pinch = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(pinch:)];
    pinch.delegate = self;
    [_imageV addGestureRecognizer:pinch];

    // 长按
    UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPress:)];
    [_imageV addGestureRecognizer:longPress];
}

- (void)pan:(UIPanGestureRecognizer *)pan
{
    // 获取手指的偏移量
   CGPoint transP = [pan translationInView:self.imageV];

    // 设置UIImageView的形变
    self.imageV.transform = CGAffineTransformTranslate(self.imageV.transform, transP.x, transP.y);

    // 复位：只要想要相对于上一次就必须复位
    [pan setTranslation:CGPointZero inView:self.imageV];
}
- (void)rotation:(UIRotationGestureRecognizer *)rotation
{
    self.imageV.transform = CGAffineTransformRotate(self.imageV.transform, rotation.rotation);

    rotation.rotation = 0;
}
- (void)pinch:(UIPinchGestureRecognizer *)pinch
{
    self.imageV.transform = CGAffineTransformScale(self.imageV.transform, pinch.scale, pinch.scale);

    pinch.scale = 1;
}
- (void)longPress:(UILongPressGestureRecognizer *)longPress
{
    if (longPress.state == UIGestureRecognizerStateBegan) {
        // 图片处理完毕

        // 高亮的效果
        [UIView animateWithDuration:0.25 animations:^{
            self.imageV.alpha = 0;
        } completion:^(BOOL finished) {

            [UIView animateWithDuration:0.25 animations:^{
                self.imageV.alpha = 1;
            } completion:^(BOOL finished) {
               // 高亮完成的时候

                // 把处理的图片生成一张新的图片，截屏

                // 开启位图上下文
                UIGraphicsBeginImageContextWithOptions(self.bounds.size, NO, 0);

                // 获取位图上下文
                CGContextRef ctx = UIGraphicsGetCurrentContext();

                // 把控件的图层渲染到上下文
                [self.layer renderInContext:ctx];

                // 获取图片
                UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

                // 关闭上下文
                UIGraphicsEndImageContext();

                // 调用Block
                if (_handleCompletionBlock) {
                    _handleCompletionBlock(image);
                }

                // 移除父控件
                [self removeFromSuperview];

            }];

        }];
    }
}

- (void)setImage:(UIImage *)image
{
    _image = image;

    // 展示UIImageView
    self.imageV.image = image;
}

#pragma mark - 手势代理方法
// 是否同时支持多个手势
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
{
    return YES;
}

@end

```
--------

```objc

//
//  ViewController.m
//  08-画板


#import "ViewController.h"
#import "ImageHandleView.h"

#import "DrawView.h"

@interface ViewController ()<UINavigationControllerDelegate,UIImagePickerControllerDelegate>
@property (weak, nonatomic) IBOutlet DrawView *drawView;

@end

@implementation ViewController
#pragma mark - 清屏
- (IBAction)clear:(id)sender {
    [_drawView clear];
}
#pragma mark - 撤销
- (IBAction)undo:(id)sender {
    [_drawView undo];
}
#pragma mark - 橡皮擦
- (IBAction)eraser:(id)sender {
    _drawView.pathColor = [UIColor whiteColor];
}

#pragma mark - 选择照片
- (IBAction)pickerPhoto:(id)sender {
    // 弹出系统的相册
    // 选择控制器（系统相册）
    UIImagePickerController *picekerVc = [[UIImagePickerController alloc] init];

    // 设置选择控制器的来源
    // UIImagePickerControllerSourceTypePhotoLibrary 相册集
    // UIImagePickerControllerSourceTypeSavedPhotosAlbum:照片库
    picekerVc.sourceType = UIImagePickerControllerSourceTypeSavedPhotosAlbum;

    // 设置代理
    picekerVc.delegate = self;

    // modal
    [self presentViewController:picekerVc animated:YES completion:nil];

}

#pragma mark - UIImagePickerControllerDelegate
// 当用户选择一张图片的时候调用
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary *)info
{
    // 获取选中的照片
    UIImage *image = info[UIImagePickerControllerOriginalImage];


    // 创建一个图片处理的View
    ImageHandleView *imageHandleV = [[ImageHandleView alloc] initWithFrame:self.drawView.bounds];
    imageHandleV.handleCompletionBlock = ^(UIImage *image){
        self.drawView.image = image;
    };
    imageHandleV.handleBeginBlock = ^{

        self.drawView.userInteractionEnabled = NO;
    };

    [self.drawView addSubview:imageHandleV];

    // 做图片的处理
    imageHandleV.image = image;

//    // 把选中的照片画到画板上
//    _drawView.image = image;

    // dismiss
    [self dismissViewControllerAnimated:YES completion:nil];
}

#pragma mark - 保存
- (IBAction)save:(id)sender {

    // 截屏
    // 开启上下文
    UIGraphicsBeginImageContextWithOptions(_drawView.bounds.size, NO, 0);

    // 获取上下文
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    // 渲染图层
    [_drawView.layer renderInContext:ctx];

    // 获取上下文中的图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

    // 关闭上下文
    UIGraphicsEndImageContext();


    // 保存画板的内容放入相册
    // image:写入的图片
    // completionTarget图片保存监听者
    // 注意：以后写入相册方法中，想要监听图片有没有保存完成，保存完成的方法不能随意乱写
    UIImageWriteToSavedPhotosAlbum(image, self, @selector(image:didFinishSavingWithError:contextInfo:), nil);

}

// 监听保存完成，必须实现这个方法
- (void)image:(UIImage *)image didFinishSavingWithError:(NSError *)error contextInfo:(void *)contextInfo
{
     NSLog(@"保存图片成功");
}




- (IBAction)colorChange:(UIButton *)sender {

    // 给画板传递颜色
    _drawView.pathColor = sender.backgroundColor;


}
- (IBAction)valueChange:(UISlider *)sender {

    // 给画板传递线宽
    _drawView.lineWidth = sender.value;

}

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end


```

--------

```objc
//
//  DrawView.m
//  08-画板


#import "DrawView.h"


#import "DrawPath.h"
@interface DrawView ()

@property (nonatomic, strong) DrawPath *path;

@property (nonatomic, strong) NSMutableArray *paths;

@end

@implementation DrawView
- (void)setImage:(UIImage *)image
{
    _image = image;

    [self.paths addObject:_image];

    // 重绘
    [self setNeedsDisplay];
}
- (void)clear
{
    [self.paths removeAllObjects];

    [self setNeedsDisplay];
}

- (void)undo
{
    [self.paths removeLastObject];

    [self setNeedsDisplay];
}

- (NSMutableArray *)paths
{
    if (_paths == nil) {
        _paths = [NSMutableArray array];
    }
    return _paths;
}

// 仅仅是加载xib的时候调用
- (void)awakeFromNib
{
    [self setUp];
}

- (instancetype)initWithFrame:(CGRect)frame
{
    if (self = [super initWithFrame:frame]) {
        [self setUp];
    }
    return self;
}

//- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
//{
//    NSLog(@"%s",__func__);
//}

// 初始化设置
- (void)setUp
{
    // 添加pan手势
    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];

    [self addGestureRecognizer:pan];

    _lineWidth = 1;
    _pathColor = [UIColor blackColor];
}


// 当手指拖动的时候调用
- (void)pan:(UIPanGestureRecognizer *)pan
{

    // 获取当前手指触摸点
   CGPoint curP = [pan locationInView:self];

    // 获取开始点
    if (pan.state == UIGestureRecognizerStateBegan) {
        // 创建贝瑟尔路径
      _path = [[DrawPath alloc] init];

        // 设置线宽
        _path.lineWidth = _lineWidth;

        // 给路径设置颜色
        _path.pathColor = _pathColor;

        // 设置路径的起点
        [_path moveToPoint:curP];

        // 保存描述好的路径
        [self.paths addObject:_path];

    }

    // 手指一直在拖动
    // 添加线到当前触摸点
    [_path addLineToPoint:curP];

    // 重绘
    [self setNeedsDisplay];

}

// 绘制图形
// 只要调用drawRect方法就会把之前的内容全部清空
- (void)drawRect:(CGRect)rect
{
    for (DrawPath *path in self.paths) {

        if ([path isKindOfClass:[UIImage class]]) {
            // 绘制图片
            UIImage *image = (UIImage *)path;

            [image drawInRect:rect];
        }else{

            // 画线
            [path.pathColor set];

            [path stroke];
        }

    }
}

@end


```
