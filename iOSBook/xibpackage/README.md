
- xib package


````objc

#import <UIKit/UIKit.h>

@class LMJShop;
@interface LMJShopView : UIView

@property (nonatomic, strong) LMJShop *shop;

+ (instancetype)shopView;

+ (instancetype)shopViewWithShop:(LMJShop *)shop;


@end

````

````objc

#import "LMJShopView.h"
#import "LMJShop.h"

@interface LMJShopView ()
@property (nonatomic, strong) IBOutlet UIImageView *iconView;
@property (nonatomic, strong) IBOutlet UILabel *nameLabel;
@end

@implementation LMJShopView
// init会自动调用initWithFrame
+ (instancetype)shopView
{
    return [[[NSBundle mainBundle] loadNibNamed:NSStringFromClass(self) owner:nil options:nil] lastObject];
}

/** 提供类方法快速创建,并设置数据 */
+ (instancetype)shopViewWithShop:(LMJShop *)shop
{
    LMJShopView *shopView = [self shopView];
    shopView.shop = shop;
    return shopView;
}


/** 重新布局子控件 */
- (void)layoutSubviews
{
    /*
     再次记得  要调用
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



````

一个控件有2种创建方式
通过代码创建
初始化时一定会调用initWithFrame:方法

通过xib\storyboard创建
初始化时不会调用initWithFrame:方法，只会调用initWithCoder:方法
初始化完毕后会调用`awakeFromNib`方法

有时候希望在控件初始化时做一些初始化操作，比如添加子控件、设置基本属性
这时需要根据控件的创建方式，来选择在`initWithFrame:`、initWithCoder:、`awakeFromNib`的哪个方法中操作

````
