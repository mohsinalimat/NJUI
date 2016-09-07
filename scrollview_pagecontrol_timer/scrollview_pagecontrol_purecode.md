# scrollView pageControl pureCode

# `.h`

```objc


#import <UIKit/UIKit.h>

@interface LMJPageView : UIView
/** 图片名字 */
@property (nonatomic, strong) NSArray<UIImage *> *images;

/** 创建控件仅仅有控件没有图片 */
+ (instancetype)pageView;

/** 创建scrollView的时候同时初始化图片 */
+ (instancetype)pageViewWithImageNames:(NSArray<UIImage *> *)images;

@end


```
#### `**在`.m`中可以用懒加载的方式开启定时器,自己写的时候没有考虑到**`

![](../LibrarypPictures/Snip20160518_2.png)
---

# `.m`

```objc

#import "LMJPageView.h"
#import "Masonry/Masonry.h"

@interface LMJPageView () <UIScrollViewDelegate>
/** View里边的ScrollView */
@property (weak, nonatomic) UIScrollView *scrollView;

/** pageControl */
@property (weak, nonatomic) UIPageControl *pageControl;

/** 定时器 */
@property (nonatomic, strong) NSTimer *timer;

@end

@implementation LMJPageView

+ (instancetype)pageView
{
    return [[self alloc] init];
}

+ (instancetype)pageViewWithImageNames:(NSArray<UIImage *> *)images
{
    LMJPageView *pageView = [self pageView];
    pageView.images = images;
    return pageView;
}
/** 创建2个控件添加到View */
- (instancetype)initWithFrame:(CGRect)frame
{
    if(self = [super initWithFrame:frame])
    {
        // 自己的测试背景
        self.backgroundColor = [UIColor yellowColor];
        /** 创建scrollView */
        UIScrollView *scrollView = [[UIScrollView alloc] init];
        // 测试背景
        scrollView.backgroundColor = [UIColor greenColor]; // 测试背景
        // 不显示滚动条
        scrollView.showsHorizontalScrollIndicator = NO;
        scrollView.showsVerticalScrollIndicator = NO;
        // scrollView 的自动截页功能
        scrollView.pagingEnabled = YES;
        // scollView 的代理设置
        scrollView.delegate = self;
        // 添加到View里边
        [self addSubview:scrollView];
        self.scrollView = scrollView;

        /** 创建pageControl */
        UIPageControl *pageControl = [[UIPageControl alloc] init];
        // 测试背景
        pageControl.backgroundColor = [UIColor blueColor]; // 测试背景
        // 小圆点颜色
        pageControl.pageIndicatorTintColor = [UIColor grayColor];
        pageControl.currentPageIndicatorTintColor = [UIColor redColor];

        //添加到View里边
        [self addSubview:pageControl];
        self.pageControl = pageControl;

        // 开启定时滚动
        [self startTimer];

    }
    return self;
}

- (void)layoutSubviews
{
    [super layoutSubviews];
    // scrollView的设置frame
    self.scrollView.frame = self.bounds;

    // scrollView的宽高
    CGFloat w = self.scrollView.frame.size.width;
    CGFloat h = self.scrollView.frame.size.height;

    [self.scrollView.subviews enumerateObjectsUsingBlock:^(__kindof UIImageView * _Nonnull imageView, NSUInteger idx, BOOL * _Nonnull stop) {
        CGFloat x = idx * self.scrollView.frame.size.width;

        imageView.frame = CGRectMake(x, 0, w, h);
    }];

    /** 约束pageControl的位置autolayout */
    [self.pageControl makeConstraints:^(MASConstraintMaker *make) {
        make.right.equalTo(self.right);
        make.bottom.equalTo(self.bottom);
    }];


    //scrollView的滚动范围
    self.scrollView.scrollEnabled = YES;
    // 宽度设置
    self.scrollView.contentSize = CGSizeMake(CGRectGetMaxX(self.scrollView.subviews.lastObject.frame), 0);

}

- (void)setImages:(NSArray<UIImage *> *)images
{
    _images = images;

    // 重新设置图片的时候重新创建imageView
    [self.scrollView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];
    // 创建对应的imageView
    for (int i = 0; i < images.count; i++)
    {
        UIImageView *imageView = [[UIImageView alloc] init];
        // 测试背景
        imageView.backgroundColor = [UIColor redColor];
        imageView.image = self.images[i];
        // 添加到scrollView
        [self.scrollView addSubview:imageView];
    }
    // 设置pageControl的页码数
    self.pageControl.numberOfPages =  self.images.count;

}

/**   pageControl的当前页码显示 */
- (void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    self.pageControl.currentPage = self.scrollView.contentOffset.x / self.scrollView.frame.size.width + 0.5;
}

/** 定时器设置 */
#pragma mark - 定时器和触摸停止

/** 开启自动翻页定时 */
- (void)startTimer
{
    self.timer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(nextPage) userInfo:nil repeats:YES];
    // 多线程
    [[NSRunLoop mainRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
}
/** 结束定时器 */
- (void)endTimer
{
    [self.timer invalidate];
    self.timer = nil;
}

/** 下一页 */
- (void)nextPage
{
    NSUInteger pageNum =  self.pageControl.currentPage;
    pageNum++;

    if(pageNum == self.pageControl.numberOfPages)
    {
        pageNum = 0;
    }

    // 下一页的内容偏移量
    CGFloat contentX = self.scrollView.frame.size.width * pageNum;

    CGPoint offset = CGPointMake(contentX, 0);
    // 动画设置
    [self.scrollView setContentOffset:offset animated:YES];
}
// 开始拖拽的时候开始定时器
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    [self endTimer];
}

// 结束拖拽的
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    //    NSLog(@"scrollViewDidEndDragging");
    [self startTimer];
}

@end

```
