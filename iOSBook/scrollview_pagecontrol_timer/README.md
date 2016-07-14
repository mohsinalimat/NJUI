# scrollView pageControl timer

- pageControl的页码数字从`0-(numOfPages-1)`,`self.pageControl.currentPage` 只能在这个范围之内取值

    ```objc

        NSUInteger pageNum =  self.pageControl.currentPage;
    pageNum++;

    if(pageNum == self.pageControl.numberOfPages)
    {
        pageNum = 0;
    }

    ```

# `.h`

````objc

#import <UIKit/UIKit.h>

@interface NJPageView : UIView
/** 图片的数据数组 */
@property (nonatomic, strong) NSArray *imageNames;
+ (instancetype)pageView;

/** 设置pageControl tint color */
@property (nonatomic, strong) UIColor *pageViewPageControlPageIndicatorTintColor;

/** 设置pageControl current color */
@property (nonatomic, strong) UIColor *pageViewPageControlcurrentPageIndicatorTintColor;
@end

````

# `.m`


````objc

#import "NJPageView.h"

@interface NJPageView () <UIScrollViewDelegate>

@property (weak, nonatomic) IBOutlet UIScrollView *scrollView;
@property (weak, nonatomic) IBOutlet UIPageControl *pageControl;

@property (nonatomic, strong) NSTimer *timer;

@end

@implementation NJPageView
+ (instancetype)pageView
{
    return [[[NSBundle mainBundle] loadNibNamed:NSStringFromClass(self) owner:nil options:nil] lastObject];
}

- (void)layoutSubviews
{
    /** 记得调用 */
    [super layoutSubviews];

    // 设置scrollView 的frame
    self.scrollView.frame = self.bounds;

    // 获得父控件的高和宽
    CGFloat viewW = self.scrollView.frame.size.width;
    CGFloat viewH = self.scrollView.frame.size.height;

    // 设置pageControl 的frame
    self.pageControl.frame = CGRectMake(viewW - self.pageControl.frame.size.width, viewH - self.pageControl.frame.size.height, self.pageControl.frame.size.width, self.pageControl.frame.size.height);

    // 设置imageView 所有的frame
    [self.scrollView.subviews enumerateObjectsUsingBlock:^(__kindof UIImageView * _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        obj.frame = CGRectMake(idx * viewW, 0, viewW, viewH);
    }];

    // 设置内容的尺寸
    self.scrollView.contentSize = CGSizeMake(viewW * self.scrollView.subviews.count, 0);
}

- (void)setImageNames:(NSArray *)imageNames
{
    _imageNames = imageNames;

    // 重新设置数据的时候要清空以前的数据
    [self.scrollView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];

    // 创建对应数据的imageView, 并且添加到scrollView
    [self.imageNames enumerateObjectsUsingBlock:^(NSString *  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        UIImageView *imageView = [[UIImageView alloc] init];
        imageView.image = [UIImage imageNamed:self.imageNames[idx]];
        [self.scrollView addSubview:imageView];
    }];

    // 设置pageControl的总页数
    self.pageControl.numberOfPages = self.imageNames.count;
}

// 修改pageControl的当前页码数, 监听滚动,设置页码数
- (void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    self.pageControl.currentPage = (self.scrollView.contentOffset.x / self.scrollView.frame.size.width) + 0.5;
}

#pragma mark - 设置定时器
// 初始化方法的标准写法
- (void)setup
{
    [self startTimer];
}
// xib 里边的控件苏醒的时候会调用
- (void)awakeFromNib
{
    [self setup];
}

- (void)startTimer
{
    // 设置定时器的同时,启动定时器
    self.timer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(nextImageView) userInfo:nil repeats:YES];
    // 设置多线程
    [[NSRunLoop mainRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
}

- (void)endTimer
{
    // invalidate 使作废
    [self.timer invalidate];
    self.timer = nil;
}

- (void)nextImageView
{
    // 下一页是当前页加111
    NSUInteger index = self.pageControl.currentPage + 1;
    // 判断+1后是不是跟    总页数(是从1开始), 相同, 实际上页码(是从 0 开始)
    if(index == self.pageControl.numberOfPages)
        index = 0;

    // 动画设置setContentOffset
    [self.scrollView setContentOffset:CGPointMake(index * self.scrollView.frame.size.width, 0) animated:YES];
}

#pragma mark - 拖拽的时候停止定时器
// 开始拖拽的时候停止定时器
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    [self endTimer];
}
// 停止拖拽的时候开始定时器
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    [self startTimer];
}

#pragma mark - pageControl 的颜色设置
// 设置其他页的颜色
- (void)setPageViewPageControlPageIndicatorTintColor:(UIColor *)pageViewPageControlPageIndicatorTintColor
{
    _pageViewPageControlPageIndicatorTintColor = pageViewPageControlPageIndicatorTintColor;
    self.pageControl.pageIndicatorTintColor = pageViewPageControlPageIndicatorTintColor;
}

// 设置当前页的颜色
- (void)setPageViewPageControlcurrentPageIndicatorTintColor:(UIColor *)pageViewPageControlcurrentPageIndicatorTintColor
{
    _pageViewPageControlcurrentPageIndicatorTintColor = pageViewPageControlcurrentPageIndicatorTintColor;
    self.pageControl.currentPageIndicatorTintColor = pageViewPageControlcurrentPageIndicatorTintColor;
}
@end

````
