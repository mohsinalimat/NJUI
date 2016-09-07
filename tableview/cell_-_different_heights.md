# Cell`Custom` - `different Heights`

- 因为` [cell layoutIfNeeded]`自动布局在计算一个label尺寸的时候会出现误差,所以要给Label,设置属性`preferredMaxLayoutWidth`, 告诉`layoutIfNeeded`label的`真实宽度`
    - 一般不做label的宽度约束, 而是通过屏幕宽度,来设置label的宽度最大属性

```objc
- (void)awakeFromNib
{
    // 获得屏幕的尺寸
    // 设置label每一行文字的最大宽度
    // 为了保证计算出来的数值 跟 真正显示出来的效果一致
    self.contentLabel.preferredMaxLayoutWidth = [UIScreen mainScreen].bounds.size.width - 20;
}
```


- 当cell显示在tableView上的时候,下边的方法`[tableView cellForRowAtIndexPath:indexPath>]`才能拿到`显示`在tableView里边的cell
- 在`(UITableViewCell *)tableView...`方法中虽然返回了cell,但是并`没有显示`在tableView里边

```objc

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatusCell *cell = [XMGStatusCell cellWithTableView:tableView];
    cell.status = self.statuses[indexPath.row];

    return cell;
}

#pragma mark - 代理方法
/**
 *  返回每一行的高度,在这个方法[tableView cellForRowAtIndexPath:indexPath>]中只能拿到显示在tableView里边的cell, 否则拿到的是空空
 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatusCell *cell = [tableView cellForRowAtIndexPath:indexPath>] // cell == nil,,,
    return cell.height;
}

```



# `拿到cell的不同高度的方法`

 ## 方法一.把高度存在控制器的里一个`可变字典属性`当中<br>

- 1.先在自定义cell里边写一个获得cell高度的方法

   ```objc

   - (CGFloat)height
{
    if (self.pictureView.hidden) { // 没有配图
        return CGRectGetMaxY(self.contentLabel.frame) + 10;
    } else { // 有配图
        return CGRectGetMaxY(self.pictureView.frame) + 10;
    }
}

   ```

- 2.在`控制器`里边定义个`可变字典的属性`,并懒加载字典

```objc

@interface XMGStatusesViewController ()
@property (strong, nonatomic) NSArray *statuses;
/** 存放所有cell的高度 */
@property (strong, nonatomic) NSMutableDictionary *heights;
@end
@implementation XMGStatusesViewController
- (NSMutableDictionary *)heights
{
    if (!_heights) {
        _heights = [NSMutableDictionary dictionary];
    }
    return _heights;
}

```

- 3.在`(UITableViewCell *)tableView:cellForRowAtIndexPath:`方法中把拿到的高度添加进词典,
    - 注意点,在获得cell的高度的时候:设置数据后, `先把cell强制布局`,再去获得高度

```objc

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatusCell *cell = [XMGStatusCell cellWithTableView:tableView];
    cell.status = self.statuses[indexPath.row];
    // 强制布局
    [cell layoutIfNeeded];
    // 存储高度
    self.heights[@(indexPath.row)] = @(cell.height);
    return cell;
}

```

- 4.在`tableView:heightForRowAtIndexPath:`里边把词典里边的高度根据索引的key拿出来,返回去给对应索引的cell
    - 注意点在调用`tableView:heightForRowAtIndexPath:`之前要做`性能优化`, 调用`tableView:estimatedHeightForRowAtIndexPath:`,保证调用的顺序
    - 否则`系统会先调用tableView:heightForRowAtIndexPath:`, `一下子调N次`, `想拿到所有cell的高度`,导致词典里边是空的;
    - 做`性能优化`即 调用`tableView:estimatedHeightForRowAtIndexPath:`后,`系统会先调用(UITableViewCell *)tableView:cellForRowAtIndexPath:`,再去调用`tableView:heightForRowAtIndexPath:`, `一次一次调用`

```objc
/**
 *  返回每一行的高度
 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return [self.heights[@(indexPath.row)] doubleValue];
}

/**
 * 返回每一行的估计高度
 * 只要返回了估计高度，那么就会先调用tableView:cellForRowAtIndexPath:方法创建cell，
 * 再调用tableView:heightForRowAtIndexPath:方法获取cell的真实高度
 */
- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 200;
}

```


## 方法二:把每个cell的高度储存在模型里边

- 每个cell里边的`数据` 都是储存在`模型`里边
- 所以每个cell的高度取决于数据的多少, 可以模型里边添加一个`高度属性`,来存储对应的cell的高度

- 1.在模型里边增加一个cell的高度属性数据

```objc
#import <UIKit/UIKit.h>

@interface XMGStatus : NSObject
@property (strong, nonatomic) NSString *name;
@property (strong, nonatomic) NSString *text;
@property (strong, nonatomic) NSString *icon;
@property (strong, nonatomic) NSString *picture;
@property (assign, nonatomic, getter=isVip) BOOL vip;

/** cell的高度 */
@property (assign, nonatomic) CGFloat cellHeight;

+ (instancetype)statusWithDict:(NSDictionary *)dict;
@end

```
- 2.在自定义cell里边`.m`中
    - 1`设置完控件数据`
        - 注意:控件只要设置`hidden=No`就一定要设置对应的`hidden=YES`
        - 否则`循环利用`cel的时候,`有数据的控件`会显示不出来
    - 2.`先强制布局[self layoutIfNeeded]`
    - 3.再根据`要显示的控件`计算cell的高度,
    - 4.**`给传进来的模型setter高度数据`**

```objc

- (void)setStatus:(XMGStatus *)status
{
    // 1.设置控件的数据
    _status = status;
    if (status.isVip) {
        self.nameLabel.textColor = [UIColor orangeColor];
        self.vipView.hidden = NO;
    } else {
        self.nameLabel.textColor = [UIColor blackColor];
        self.vipView.hidden = YES;
    }
    self.nameLabel.text = status.name;
    self.iconView.image = [UIImage imageNamed:status.icon];
    if (status.picture) {
        self.pictureView.hidden = NO;
        self.pictureView.image = [UIImage imageNamed:status.picture];
    } else {
        self.pictureView.hidden = YES;
    }
    self.contentLabel.text = status.text;

    // 2.强制布局
    [self layoutIfNeeded];

    // 3.计算cell的高度,
    if (self.pictureView.hidden) { // 没有配图

    // 4.给模型--添加---高度数据
        status.cellHeight = CGRectGetMaxY(self.contentLabel.frame) + 10;
    } else { // 有配图
        status.cellHeight = CGRectGetMaxY(self.pictureView.frame) + 10;
    }
}

```

- 3.在控制器里边, `tableView: heightForRowAtIndexPath`方法中
    - 1.根据`cell索引`拿到`对应的模型`
    - 2.把模型里边的高度数据(cellHeight)返回

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatusCell *cell = [XMGStatusCell cellWithTableView:tableView];
    cell.status = self.statuses[indexPath.row];
    return cell;
}
#pragma mark - 代理方法
/**
 *  返回每一行的高度
 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatus *staus = self.statuses[indexPath.row];
    return staus.cellHeight;
}
/**
 * 返回每一行的估计高度
 * 只要返回了估计高度，那么就会先调用tableView:cellForRowAtIndexPath:方法创建cell，
 * 再调用tableView:heightForRowAtIndexPath:方法获取cell的真实高度
 */
- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 200;
}

```


















