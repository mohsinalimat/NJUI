# pureCode EqualHeight Cell Package `Autolayout`

- cell的高度实际上由tableViewController里边设置rowHeight决定

# `LMJDealCellTableViewCell.m`

```obj-c

#import "LMJDealCellTableViewCell.h"
#import "LMJDeal.h"
#import "Masonry/Masonry.h"

@interface LMJDealCellTableViewCell ()

@property (weak, nonatomic)  UIImageView *iconImageView;
@property (weak, nonatomic)  UILabel *titleLabel;
@property (weak, nonatomic)  UILabel *buyCountLabel;
@property (weak, nonatomic)  UILabel *priceLabel;

@end

@implementation LMJDealCellTableViewCell

+ (instancetype)dealCellWithTableView:(UITableView *)tableView
{
    //1. 确保外边的ID和内部的一样
    static NSString *ID = @"deal";

    //2. 现根据传进来的tableView到它的缓存池子里边找, ID cell
    LMJDealCellTableViewCell *dealCell = [tableView dequeueReusableCellWithIdentifier:ID];

    //3. 第一种方式: 手动创建, 如果没有,我们来创建dealCell
    // 如果tableViewController里边根据cell的ID,注册LMJDealCellTableViewCell类型(第二种方式),这一步就不会调用
    // 但是这一步也可以写,以防外边不注册, 并且封装完整
    if(dealCell == nil)
    {
        dealCell  = [[self alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:ID];
        NSLog(@"test");

    }
    return dealCell;
}
// 通过注册,来让系统自动创建cell, 系统只会调用-initWithStyle这个方法,
// 不会调用-initWithFrame
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if(self = [super initWithStyle:style reuseIdentifier:reuseIdentifier])
    {
        CGFloat margin = 10;
        // 在这里边创建cell子控件,并设置一些属性
        UIImageView *iconImageView = [[UIImageView alloc] init];
        // 注意是给cell里边的contentView添加子控件
        [self.contentView addSubview:iconImageView];
        self.iconImageView = iconImageView;
        [iconImageView makeConstraints:^(MASConstraintMaker *make) {
            make.top.and.left.equalTo(iconImageView.superview).offset(margin);
            make.bottom.equalTo(iconImageView.superview).offset(-margin);
            make.width.equalTo(iconImageView.height).multipliedBy(1.67);
        }];

        UILabel *titleLabel = [[UILabel alloc] init];
        titleLabel.textAlignment = NSTextAlignmentLeft;
        [self.contentView addSubview:titleLabel];
        self.titleLabel = titleLabel;
        [titleLabel makeConstraints:^(MASConstraintMaker *make) {
            make.top.equalTo(iconImageView.top);
            make.left.equalTo(iconImageView.right).offset(margin);
            make.right.equalTo(titleLabel.superview.right).offset(-margin);
        }];

        UILabel *priceLabel = [[UILabel alloc] init];
        priceLabel.textColor = [UIColor orangeColor];
        priceLabel.textAlignment = NSTextAlignmentLeft;
        [self.contentView addSubview:priceLabel];
        self.priceLabel = priceLabel;
        [priceLabel makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(titleLabel.left);
            make.bottom.equalTo(iconImageView.bottom);
            make.width.equalTo(70);
        }];


        UILabel *buyCountLabel = [[UILabel alloc] init];
        buyCountLabel.font = [UIFont systemFontOfSize:14];
        buyCountLabel.textColor = [UIColor lightGrayColor];
        buyCountLabel.textAlignment = NSTextAlignmentRight;
        [self.contentView addSubview:buyCountLabel];
        self.buyCountLabel = buyCountLabel;
        [buyCountLabel makeConstraints:^(MASConstraintMaker *make) {
            make.right.equalTo(titleLabel.right);
            make.bottom.equalTo(priceLabel.bottom);
            make.left.equalTo(priceLabel.right).offset(margin);
        }];
    }
    return self;
}

- (void)setDeal:(LMJDeal *)deal
{
    _deal = deal;
    // 设置cell里边控件的数据

    self.iconImageView.image = [UIImage imageNamed:deal.icon];


    self.titleLabel.text = deal.title;


    self.priceLabel.text = [NSString stringWithFormat:@"￥%@", deal.price];


    self.buyCountLabel.text = [NSString stringWithFormat:@"一共有%@人购买", deal.buyCount];
}

```

