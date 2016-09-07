# package View

- 提供类方法快速创建
- 设置模型属性
- 方法中记得重写- (instancetype) initWithFrame
- 方法中记得重写 - (void)layoutSubviews;
- 还有设置数据

# `.h`
````objc

#import <UIKit/UIKit.h>

@class LMJShop;
@interface LMJShopView : UIView

@property (nonatomic, strong) LMJShop *shop;

+ (instancetype)shopView;

+ (instancetype)shopViewWithShop:(LMJShop *)shop;

- (instancetype)initWithShop:(LMJShop *)shop;
@end


````


# `.m`

````objc

#import "LMJShopView.h"
#import "LMJShop.h"

@interface LMJShopView ()
@property (nonatomic, strong) UIImageView *iconView;
@property (nonatomic, strong) UILabel *nameLabel;
@end

@implementation LMJShopView
// init会自动调用initWithFrame
+ (instancetype)shopView
{
    return [[self alloc] init];
}
- (instancetype)initWithShop:(LMJShop *)shop
{
    if(self = [super init]) // 也会调用自己的initWithFrame
    {
        self.shop = shop;
    }
    return self;
}
/** 提供类方法快速创建,并设置数据 */
+ (instancetype)shopViewWithShop:(LMJShop *)shop
{
    return [[self alloc] initWithShop:shop];
}

/** 添加子控件,设置子控件的属性 */
- (instancetype)initWithFrame:(CGRect)frame
{
    if(self = [super initWithFrame:frame])
    {
        self.backgroundColor = [UIColor blueColor]; // 测试

        // 添加图片
        self.iconView = [[UIImageView alloc] init];
        self.iconView.backgroundColor = [UIColor redColor]; // 测试
        [self addSubview:self.iconView];

        // 添加文字
        self.nameLabel = [[UILabel alloc] init];
        self.nameLabel.backgroundColor = [UIColor greenColor]; // 测试
        self.nameLabel.textAlignment = NSTextAlignmentCenter;
        self.nameLabel.textColor = [UIColor blackColor];
        self.nameLabel.font = [UIFont systemFontOfSize:11];
        [self addSubview:self.nameLabel];
    }
    return self;
}

/** 重新布局子控件 */
- (void)layoutSubviews
{
    /*
     再次记得要调用
     */
    [super layoutSubviews]; // 重点 重点

    CGFloat shopViewW = self.frame.size.width;
    CGFloat shopViewH = self.frame.size.height;

    self.iconView.frame = CGRectMake(0, 0, shopViewW, shopViewW);
    self.nameLabel.frame = CGRectMake(0, shopViewW, shopViewW, shopViewH - shopViewW);
}

/** 设置数据 */
- (void)setShop:(LMJShop *)shop
{
    _shop = shop;
    self.nameLabel.text = shop.name;
    self.iconView.image = [UIImage imageNamed:shop.icon];
}
@end


````

### 懒加载子控件  和initWithFrame 添加子控件的对比

````objc

/** 添加子控件,设置子控件的属性 */
- (instancetype)initWithFrame:(CGRect)frame
{
    if(self = [super initWithFrame:frame])
    {
        self.backgroundColor = [UIColor blueColor]; // 测试

//        // 添加图片
//        self.iconView = [[UIImageView alloc] init];
//        self.iconView.backgroundColor = [UIColor redColor]; // 测试
//        [self addSubview:self.iconView];
//
//        // 添加文字
//        self.nameLabel = [[UILabel alloc] init];
//        self.nameLabel.backgroundColor = [UIColor greenColor]; // 测试
//        self.nameLabel.textAlignment = NSTextAlignmentCenter;
//        self.nameLabel.textColor = [UIColor blackColor];
//        self.nameLabel.font = [UIFont systemFontOfSize:11];
//        [self addSubview:self.nameLabel];
    }
    return self;
}

- (UILabel *)nameLabel
{
    if(_nameLabel == nil)
    {
        _nameLabel = [[UILabel alloc] init];
        _nameLabel.backgroundColor = [UIColor greenColor]; // 测试
        _nameLabel.textAlignment = NSTextAlignmentCenter;
        _nameLabel.textColor = [UIColor blackColor];
        _nameLabel.font = [UIFont systemFontOfSize:11];
        [self addSubview:_nameLabel];
    }
    return _nameLabel;
}
- (UIImageView *)iconView
{
    if(_iconView == nil)
    {
        _iconView = [[UIImageView alloc] init];
        _iconView.backgroundColor = [UIColor redColor]; // 测试
        [self addSubview:_iconView];
    }
    return _iconView;
}
````
