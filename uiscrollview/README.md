# `UIScrollView`

-  在右边属性里边可以设置`左右滚动条`的是否显示
-  可以设只翻页的`分页功能`

### `contentSize` 属性
````objc
// contentSize 设置内容的尺寸后才能滚动
// 如果想禁止某个方向的滚动就设置宽或高是0
self.scrollView.contentSize = CGSizeMake(weigh, height);

// 获取最后一个子控件,
//  `确保`是最后一个`子控`件,而不是滚动条`
CGFloat contentH = CGRectGetMaxY(lastGird.frame);

````

### `contentOffset` 属性
````objc
// 指的是内容左上角,和scrollView左上角的,差值, 支架
// 偏移量,偏移量,偏移量
self.scrollView.contentOffset = CGPointMake(x, y);
````

### `contentInset` 属性
````objc
// 能在内容的四周增加额外的滚动区域
    UIEdgeInsetsMake(<#CGFloat top#>, <#CGFloat left#>, <#CGFloat bottom#>, <#CGFloat right#>);
self.scrollView.contentInset = UIEdgeInsetsMake(...);

````

### 缩放

````objc
#import "ViewController.h"

@interface ViewController () <UIScrollViewDelegate>
@property (weak, nonatomic) IBOutlet UIScrollView *scrollview;
@property (weak, nonatomic) UIImageView *imageView;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    UIImageView *imageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"minion"]];
    [self.scrollview addSubview:imageView];
    self.imageView = imageView;

    self.scrollview.backgroundColor = [UIColor redColor];
    self.scrollview.contentSize = imageView.image.size;

    // 设置代理
    self.scrollview.delegate = self;
    // 设置缩放比例
    self.scrollview.maximumZoomScale = 2.0;
    self.scrollview.minimumZoomScale = 0.2;
}

#pragma mark - <UIScrollViewDelegate>
/**
 这个方法的返回值决定了要缩放的内容(返回值只能是UIScrollView的子控件)
 */
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView
{
    return self.imageView;
}

// 在进行缩放的时候调用这个方法
- (void)scrollViewDidZoom:(UIScrollView *)scrollView
{
    NSLog(@"缩放ing-----%f", scrollView.zoomScale);
}

````

# `常用的代理方法`

```objc
// 只要在滚动就会调用
- (void)scrollViewDidScroll:(UIScrollView *)scrollView

// 刚开始拖拽的时候调用
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView

// 结束拖拽的时候调用
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate

// 要缩放scrollView里边的什么控件要返回
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView

// 减速完毕后,流滑完毕后回掉用
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView

// 刚开始调用的时候就回调用
- (void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView

// 即将拖拽停止的时候调用,和scrollViewDidEndDragging同时调用的
// 但是scrollViewWillEndDragging同时调用的
- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity targetContentOffset:(inout CGPoint *)targetContentOffset


```




























