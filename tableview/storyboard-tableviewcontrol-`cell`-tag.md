# storyboard->tableViewControl->`Cell`-`tag`

- 直接在storyboard中`自定义等高cell` 拖进去`cell.contentView`
- 设置cell的ID,不用设置cell的`CustomClass`
- 设置里边的子控件的tag
- 在代码里边直接根据tag拿到子控件设置数据<br>

![](../LibrarypPictures/Snip20160518_23.png)

- `要设置cell的高度,在tableView里边设置rowHeight`<br>

    ![](../LibrarypPictures/Snip20160518_21.png)

    - `或者直接拖宽`

```objc

#import "LMJDealsTableViewController.h"
#import "LMJDeal.h"
@interface LMJDealsTableViewController ()
/** deals */
@property (nonatomic, strong) NSArray<LMJDeal *> *deals;
@end

@implementation LMJDealsTableViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}


- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.deals.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 拿到ID
    static NSString *ID = @"deal";
    // 获得或者创建cell
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    //1.拿到对应的模型
    LMJDeal *deal = self.deals[indexPath.row];
    // 设置cell里边控件的数据
    UIImageView *iconImageView = (UIImageView *)[cell viewWithTag:10];
    iconImageView.image = [UIImage imageNamed:deal.icon];

    UILabel *titleLabel = (UILabel *)[cell viewWithTag:20];
    titleLabel.text = deal.title;

    UILabel *priceLabel = (UILabel *)[cell viewWithTag:30];
    priceLabel.text = [NSString stringWithFormat:@"￥%@", deal.price];

    UILabel *buyCountLabel = (UILabel *)[cell viewWithTag:40];
    buyCountLabel.text = [NSString stringWithFormat:@"一共有%@人购买", deal.buyCount];

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
