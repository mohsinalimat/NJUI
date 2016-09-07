# pureCode EqualHeight Cell Package
- cell的高度实际上由tableViewController里边设置rowHeight决定,
#`frame` 方式 -- `吐了`

# `LMJDealCellTableViewCell.m`

```obj-c

#import "LMJDealCellTableViewCell.h"
#import "LMJDeal.h"
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
        // 在这里边创建cell子控件,并设置一些属性
        UIImageView *iconImageView = [[UIImageView alloc] init];
        // 注意是给cell里边的contentView添加子控件
        [self.contentView addSubview:iconImageView];
        self.iconImageView = iconImageView;

        UILabel *titleLabel = [[UILabel alloc] init];
        titleLabel.textAlignment = NSTextAlignmentLeft;
        [self.contentView addSubview:titleLabel];
        self.titleLabel = titleLabel;

        UILabel *priceLabel = [[UILabel alloc] init];
        priceLabel.textColor = [UIColor orangeColor];
        priceLabel.textAlignment = NSTextAlignmentLeft;
        [self.contentView addSubview:priceLabel];
        self.priceLabel = priceLabel;


        UILabel *buyCountLabel = [[UILabel alloc] init];
        buyCountLabel.font = [UIFont systemFontOfSize:14];
        buyCountLabel.textColor = [UIColor lightGrayColor];
        buyCountLabel.textAlignment = NSTextAlignmentRight;
        [self.contentView addSubview:buyCountLabel];
        self.buyCountLabel = buyCountLabel;
    }
    return self;
}


- (void)layoutSubviews
{
    [super layoutSubviews];

    CGFloat contentH = self.contentView.frame.size.height;
    CGFloat contentW = self.contentView.frame.size.width;
    CGFloat margin = 10;

    CGFloat iconX = margin;
    CGFloat iconY = margin;
    CGFloat iconW = 100;
    CGFloat iconH = contentH - 2 * iconY;
    self.iconImageView.frame = CGRectMake(iconX, iconY, iconW, iconH);

    // titleLabel
    CGFloat titleX = CGRectGetMaxX(self.iconImageView.frame) + margin;
    CGFloat titleY = iconY;
    CGFloat titleW = contentW - titleX - margin;
    CGFloat titleH = 21;
    self.titleLabel.frame = CGRectMake(titleX, titleY, titleW, titleH);
    //    CGRectMake(titleX, titleY, titleW, titleH);

    // priceLabel
    CGFloat priceX = titleX;
    CGFloat priceH = 21;
    CGFloat priceY = contentH - margin - priceH;
    CGFloat priceW = 70;
    self.priceLabel.frame = CGRectMake(priceX, priceY, priceW, priceH);

    // buyCountLabel
    CGFloat buyCountH = priceH;
    CGFloat buyCountY = priceY;
    CGFloat buyCountX = CGRectGetMaxX(self.priceLabel.frame) + margin;
    CGFloat buyCountW = contentW - buyCountX - margin;
    self.buyCountLabel.frame = CGRectMake(buyCountX, buyCountY, buyCountW, buyCountH);
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

@end

```

---

# `LMJDealsTableViewController.m`

```obj-c

#import "LMJDealsTableViewController.h"
#import "LMJDeal.h"
#import "LMJDealCellTableViewCell.h"
@interface LMJDealsTableViewController ()
/** deals */
@property (nonatomic, strong) NSArray<LMJDeal *> *deals;
@end

@implementation LMJDealsTableViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    //1. 确保外边的ID和内部的一样
    static NSString *ID = @"deal";

    //2.通过注册,来让系统自动创建cell, 系统只会调用- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier这个方法,并且style == UITableViewCellStyleDefault
    [self.tableView registerClass:[LMJDealCellTableViewCell class] forCellReuseIdentifier:ID];
}


- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.deals.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{

    LMJDealCellTableViewCell *cell = [LMJDealCellTableViewCell dealCellWithTableView:tableView];

    cell.deal = self.deals[indexPath.row];


    return cell;

}


- (NSArray<LMJDeal *> *)deals
{
    if (_deals == nil) {
        NSString *path = [[NSBundle mainBundle] pathForResource:@"deals" ofType:@"plist"];
        NSArray *dictArr = [NSArray arrayWithContentsOfFile:path];

        NSMutableArray *arrM = [NSMutableArray array];

        [dictArr enumerateObjectsUsingBlock:^(NSDictionary * _Nonnull dict, NSUInteger idx, BOOL * _Nonnull stop) {
            LMJDeal *deal = [LMJDeal dealWithDict:dict];
            [arrM addObject:deal];
        }];

        _deals = arrM.copy;
    }
    return _deals;
}
// 隐藏电池状态条
- (BOOL)prefersStatusBarHidden
{
    return YES;
}
@end

```
