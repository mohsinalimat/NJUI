# QQFriendList

- 总结:头部视图监听点击, 点击后修改模型数据, 然后通知代理tableview,tableview重新加载数据,
- 在`numberOfRowsInSection`返回行数里边, 是根据模型的里边的开合状态属性, 返回每一组的行数,
- tableview重新加载数据, 又调用了headerview的创建dequeue方法, 传递了组模型,
- headerview又根据模型数据, 设置自己btn里边图片的显示transform

- `UITableViewHeaderFooterView` 头部视图的注意点
    - 制定协议, 设置代理
    - 监听里边按钮的点击,
    - 按钮点击后更改组模型数据的opened属性,
    - 然后通知tableview重新加载数据
    - 同时根据模型数据的opened属性设置按钮内部的imageView的transform
    - 注意点, 在`UITableViewHeaderFooterView`在`initWithReuseIdentifier`方法里边添加按钮,
        - 但是一定要在`layoutSubviews`里边重新布局button

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_17.png)

```objc

#import "LMJHeaderView.h"
#import "LMJHeaderButton.h"
#import "LMJSectionOfFriend.h"
@interface LMJHeaderView ()

/** <#digest#> */
@property (weak, nonatomic) LMJHeaderButton *btn;
@end

@implementation LMJHeaderView

+ (instancetype)headerViewWithTableView:(UITableView *)tableView withDelegate:(id<LMJHeaderViewDelegate>)delegate
{
    static NSString *ID = @"headerForFriend";
    LMJHeaderView *headerView = [tableView dequeueReusableHeaderFooterViewWithIdentifier:ID];

    if(headerView == nil)
    {
        headerView = [[self alloc] initWithReuseIdentifier:ID];
        headerView.delegate = delegate;
    }

    return headerView;
}

- (instancetype)initWithReuseIdentifier:(NSString *)reuseIdentifier
{
    if(self = [super initWithReuseIdentifier:reuseIdentifier])
    {
//        self.contentView.backgroundColor = [UIColor lightGrayColor];
//        self.userInteractionEnabled = YES;
//        self.contentView.userInteractionEnabled = YES;

        LMJHeaderButton *btn = [LMJHeaderButton buttonWithType:UIButtonTypeCustom];


        // 在这里设置frame没有作用
//        btn.frame = self.contentView.bounds;

        self.btn = btn;

        // 监听btn的点击
        [btn addTarget:self action:@selector(btnClick:) forControlEvents:UIControlEventTouchUpInside];

        // 记得一定是添加到self.contentView里边
        [self.contentView addSubview:btn];
    }
    return self;
}

- (void)setSection:(LMJSectionOfFriend *)section
{
    _section = section;

    self.btn.selected = section.isOpened;

    [self.btn setTitle:section.name forState:UIControlStateNormal];

    self.btn.onlineNum = [NSString stringWithFormat:@"%zd/%zd", section.online, section.friends.count];
}

- (void)btnClick:(UIButton *)btn
{
    // 一定是更改模型数据的属性
    self.section.opened = !self.section.isOpened;

    if([self.delegate respondsToSelector:@selector(headerViewOpenedStateChange:)])
    {
        [self.delegate headerViewOpenedStateChange:self];
    }

}

- (void)layoutSubviews
{
    [super layoutSubviews];
    self.btn.frame = self.contentView.bounds;
}
@end

```

---

- 自定义头部button的注意点
    - 重写setSelected方法, 一定要调用父类的方法

```objc

#import "LMJHeaderButton.h"

@interface LMJHeaderButton ()

/** <#digest#> */
@property (weak, nonatomic) UILabel *numLabel;
@end

@implementation LMJHeaderButton

- (instancetype)initWithFrame:(CGRect)frame
{
    if(self = [super initWithFrame:frame])
    {
        [self setUp];
    }
    return self;
}
- (void)awakeFromNib
{
    [super awakeFromNib];
    [self setUp];
}

- (void)setUp
{
//    self.backgroundColor = [UIColor redColor];

    self.imageView.contentMode = UIViewContentModeCenter;
    self.imageView.clipsToBounds = NO;
//    self.imageView.image = [UIImage imageNamed:@"buddy_header_arrow"];
    [self setImage:[UIImage imageNamed:@"buddy_header_arrow"] forState:UIControlStateNormal];
//    self.imageView.backgroundColor = [UIColor clearColor];

    self.titleLabel.textAlignment = NSTextAlignmentLeft;
    self.titleLabel.font = [UIFont systemFontOfSize:17];
//    self.titleLabel.backgroundColor = [UIColor clearColor];

    [self setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
//    [self setTitleColor:[UIColor lightGrayColor] forState:UIControlStateHighlighted];
//    [self setTitleColor:[UIColor purpleColor] forState:UIControlStateSelected];

    // 添加一个人数label在右边
    UILabel *numLabel = [[UILabel alloc] init];
    numLabel.textAlignment = NSTextAlignmentRight;
    numLabel.textColor = [UIColor blackColor];
    numLabel.font = [UIFont systemFontOfSize:17];
    numLabel.numberOfLines = 0;
    numLabel.backgroundColor = [UIColor clearColor];

    self.numLabel = numLabel;
    [self addSubview:numLabel];
}

// 重新布局
- (void)layoutSubviews
{
    [super layoutSubviews];

    CGFloat w = self.frame.size.width;
    CGFloat h = self.frame.size.height;

    self.imageView.frame = CGRectMake(h, 0, h, h);

    self.titleLabel.frame = CGRectMake(3 * h, 0, w - 3 * h, h);

    self.numLabel.frame = CGRectMake(3 * h, 0, w - 5 * h, h);
}

- (void)setHighlighted:(BOOL)highlighted
{

}

- (void)setOnlineNum:(NSString *)onlineNum
{
    _onlineNum = onlineNum;

    self.numLabel.text = onlineNum;
}


// 因为给button的右边添加了一个在线人数的label
// 重写setSelected 方法, 记得调用父类的方法
- (void)setSelected:(BOOL)selected
{
    [super setSelected:selected];

    if(self.isSelected)
    {
        self.imageView.transform = CGAffineTransformMakeRotation(M_PI_2);
    }
    else
    {
        self.imageView.transform = CGAffineTransformIdentity;
    }
}
@end

```

---
- `LMJSectionOfFriend.h`组模型里边的设置

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_18.png)


- `viewController.m`

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_20.png)

```objc


#import "LMJQQFriendController.h"
#import "LMJSectionOfFriend.h"
#import "LMJFriend.h"
#import "LMJHeaderView.h"
@interface LMJQQFriendController ()<LMJHeaderViewDelegate>
@property (nonatomic, strong) NSMutableArray<LMJSectionOfFriend *> *sections;

@end

@implementation LMJQQFriendController

- (void)viewDidLoad {
    [super viewDidLoad];


}

- (NSMutableArray<LMJSectionOfFriend *> *)sections
{
    if (_sections == nil) {
        NSString *path = [[NSBundle mainBundle] pathForResource:@"friends" ofType:@"plist"];
        NSArray *dictArr = [NSArray arrayWithContentsOfFile:path];

        NSMutableArray *arrM = [NSMutableArray array];

        [dictArr enumerateObjectsUsingBlock:^(NSDictionary * _Nonnull dict, NSUInteger idx, BOOL * _Nonnull stop) {
            LMJSectionOfFriend *section = [LMJSectionOfFriend sectionWithDict:dict];
            [arrM addObject:section];
        }];

        _sections = arrM;
    }
    return _sections;
}

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return self.sections.count;
}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.sections[section].isOpened ? self.sections[section].friends.count : 0;
//    return self.sections[section].friends.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *ID = @"friend";

    UITableViewCell *friendCell = [tableView dequeueReusableCellWithIdentifier:ID];

    if(friendCell == nil)
    {
        friendCell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }

    LMJFriend *friend = self.sections[indexPath.section].friends[indexPath.row];

    friendCell.imageView.image = friend.icon;
    friendCell.textLabel.text = friend.name;
    friendCell.detailTextLabel.text = friend.intro;
    friendCell.textLabel.textColor = friend.isVip ? [UIColor redColor] : [UIColor blackColor];

    return friendCell;

}

- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    LMJHeaderView *header = [LMJHeaderView headerViewWithTableView:tableView withDelegate:self];

//    header.delegate = self;
    header.section = self.sections[section];

    return header;
}

// 头部视图的代理方法
- (void)headerViewOpenedStateChange:(LMJHeaderView *)headerView
{
       NSInteger section = [self.sections indexOfObject:headerView.section];

//    [self.tableView reloadData];

    NSIndexSet *set = [NSIndexSet indexSetWithIndex:section];

    [self.tableView reloadSections:set withRowAnimation:UITableViewRowAnimationAutomatic];
}

- (BOOL)prefersStatusBarHidden
{
    return YES;
}
@end

```

