# header-footer-View

```objc

#import "LMJHeaderView.h"

#import "LMJSection.h"

@interface LMJHeaderView ()
/** button */
@property (weak, nonatomic) UIButton *sectionHeaderButton;
/** label */
@property (weak, nonatomic) UILabel *onlineLabel;
@end

@implementation LMJHeaderView
+ (instancetype)headerViewWithTableView:(UITableView *)tableView
{
    static NSString *ID = @"header";
    LMJHeaderView *headerView = [tableView dequeueReusableHeaderFooterViewWithIdentifier:ID];
    if(headerView == nil)
    {
        headerView = [[self alloc] initWithReuseIdentifier:ID];
    }
    return headerView;
}

- (instancetype)initWithReuseIdentifier:(NSString *)reuseIdentifier
{
    if(self = [super initWithReuseIdentifier:reuseIdentifier])
    {
        self.contentView.backgroundColor = [UIColor redColor];

        UIButton *sectionHeaderButton = [[UIButton alloc] init];
        [self.contentView addSubview:sectionHeaderButton];
        self.sectionHeaderButton = sectionHeaderButton;
        [sectionHeaderButton setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
        // 设置按钮的内容水平方向左对齐
        sectionHeaderButton.contentHorizontalAlignment = UIControlContentHorizontalAlignmentLeft;
        // 设置按钮内容的税制方向的中心居中
        sectionHeaderButton.contentVerticalAlignment = UIControlContentVerticalAlignmentCenter;
        // 设置按钮的头部视图的iamge
        [sectionHeaderButton setImage:[UIImage imageNamed:@"buddy_header_arrow"] forState:UIControlStateNormal];
        // 设置按钮的内容的左 空隙
        sectionHeaderButton.contentEdgeInsets = UIEdgeInsetsMake(0, 20, 0, 0);
        // 设置按钮的标题的左 空隙
        sectionHeaderButton.titleEdgeInsets = UIEdgeInsetsMake(0, 10, 0, 0);
        // 设置按钮图片的中的imageView的内容 模式
        sectionHeaderButton.imageView.contentMode = UIViewContentModeCenter;
        // 设置图片的中的imageView的内容剪切模式
        sectionHeaderButton.imageView.clipsToBounds = NO;
        // 添加点击事件
        [sectionHeaderButton addTarget:self action:@selector(clickToChange) forControlEvents:UIControlEventTouchUpInside];


        UILabel *onlineLabel = [[UILabel alloc] init];
        self.onlineLabel = onlineLabel;
        [self.contentView addSubview:onlineLabel];
    }
    return self;
}

- (void)layoutSubviews
{
    [super layoutSubviews];

    CGFloat headerW = self.contentView.frame.size.width;
    CGFloat headerH = self.contentView.frame.size.height;

    self.sectionHeaderButton.frame = self.contentView.frame;


    // 在线人数的frmae
    CGFloat onlineW = 100;
    CGFloat onlineH = headerH;
    CGFloat onlineX = headerW - onlineW - 20;

    self.onlineLabel.frame = CGRectMake(onlineX, 0, onlineW, onlineH);
}

- (void)setSection:(LMJSection *)section
{
    _section = section;
    if(section.isOpened)
    {
        self.sectionHeaderButton.imageView.transform = CGAffineTransformMakeRotation(M_PI_2);
    }
    else
    {
        self.sectionHeaderButton.imageView.transform = CGAffineTransformIdentity;
    }

    [self.sectionHeaderButton setTitle:section.name forState:UIControlStateNormal];

    self.onlineLabel.text = [NSString stringWithFormat:@"%zd/%zd", section.online, section.friends.count];
}

- (void)clickToChange
{
    self.section.opened = !self.section.isOpened;

    if([self.delegate respondsToSelector:@selector(headerViewChangeOpenedState:)])
    {
        [self.delegate headerViewChangeOpenedState:self];
    }
}
@end

```
