# different-heights-Masonry

- 不用设置`contentLabel`的宽度约束
- 但是要设置`contentLabel`的` contentLabel.preferredMaxLayoutWidth = [UIScreen mainScreen].bounds.size.width-15;`属性,根据屏幕适配实际的宽度, 更重要的是的调用`[self layoutIfNeeded]`算的尺寸更准确
- `contentLabel.numberOfLines = 0;`的行数设置
- `contentLabel.lineBreakMode = NSLineBreakByCharWrapping` 换行怎么显示
    - 属性是枚举, 如下:<br>

```objc
// NSParagraphStyle
typedef NS_ENUM(NSInteger, NSLineBreakMode) {
    NSLineBreakByWordWrapping = 0,     	// Wrap at word boundaries, default
    NSLineBreakByCharWrapping,		// Wrap at character boundaries
    NSLineBreakByClipping,		// Simply clip
    NSLineBreakByTruncatingHead,	// Truncate at head of line: "...wxyz"
    NSLineBreakByTruncatingTail,	// Truncate at tail of line: "abcd..."
    NSLineBreakByTruncatingMiddle	// Truncate middle of line:  "ab...yz"
}

```


```objc

#import "LMJStatusCell.h"
#import "LMJStatus.h"
#import "Masonry/Masonry.h"
@interface LMJStatusCell ()
@property (weak, nonatomic) UIImageView *iconImageView;
@property (weak, nonatomic) UILabel *nameLabel;
@property (weak, nonatomic) UIImageView *vipImageView;
@property (weak, nonatomic) UILabel *contentLabel;
@property (weak, nonatomic) UIImageView *pictureImageView;

@end

@implementation LMJStatusCell
+ (instancetype)statusCell:(UITableView *)tableView
{
    static NSString *ID = @"status";

    LMJStatusCell *statusCell = [tableView dequeueReusableCellWithIdentifier:ID];
    if(statusCell == nil)
    {
        NSLog(@"xxxxx");
        statusCell  = [[LMJStatusCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:ID];
    }
    return statusCell;
}

- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if(self = [super initWithStyle:style reuseIdentifier:reuseIdentifier])
    {
        CGFloat margin = 10;

        UIImageView *iconImageView = [[UIImageView alloc] init];
        self.iconImageView = iconImageView;
        [self.contentView addSubview:iconImageView];
        [iconImageView makeConstraints:^(MASConstraintMaker *make) {
            make.size.equalTo(CGSizeMake(30, 30));
            make.left.top.equalTo(self.contentView).offset(margin);
        }];

        UILabel *nameLabel = [[UILabel alloc] init];
        nameLabel.textColor = [UIColor blackColor];
        nameLabel.textAlignment = NSTextAlignmentLeft;
        nameLabel.font = [UIFont systemFontOfSize:17];
        self.nameLabel = nameLabel;
        [self.contentView addSubview:nameLabel];
        [nameLabel makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(iconImageView.right).offset(margin);
            make.centerY.equalTo(iconImageView.centerY);
        }];

        UIImageView *vipImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"vip"]];
        self.vipImageView = vipImageView;
        [self.contentView addSubview:vipImageView];
        /** 约束digest */
        [vipImageView makeConstraints:^(MASConstraintMaker *make) {
            make.size.equalTo(CGSizeMake(14, 14));
            make.left.equalTo(nameLabel.right).offset(margin);
            make.centerY.equalTo(nameLabel.centerY);
        }];

        UILabel *contentLabel = [[UILabel alloc] init];
        contentLabel.textColor = [UIColor blackColor];
        contentLabel.textAlignment = NSTextAlignmentLeft;
        contentLabel.font = [UIFont systemFontOfSize:16];
        contentLabel.lineBreakMode = NSLineBreakByCharWrapping;
        contentLabel.numberOfLines = 0;
        contentLabel.preferredMaxLayoutWidth = [UIScreen mainScreen].bounds.size.width-15;
        [self.contentView addSubview:contentLabel];
        self.contentLabel = contentLabel;
        [contentLabel makeConstraints:^(MASConstraintMaker *make) {
            make.left.equalTo(iconImageView.left);
            make.top.equalTo(iconImageView.bottom).offset(margin);
//            make.right.equalTo(self.contentView.right).offset(-margin);
        }];

        UIImageView *pictureImageView = [[UIImageView alloc] init];
        [self.contentView addSubview:pictureImageView];
        self.pictureImageView = pictureImageView;
        [pictureImageView makeConstraints:^(MASConstraintMaker *make) {
            make.size.equalTo(CGSizeMake(100, 100));
            make.left.equalTo(contentLabel.left);
            make.top.equalTo(contentLabel.bottom).offset(margin);
        }];

    }
    return self;
}

- (void)setStatus:(LMJStatus *)status
{
    _status = status;
    if(status.isVip)
    {
        self.vipImageView.hidden = NO;
        self.nameLabel.textColor = [UIColor redColor];
    }
    else
    {
        self.vipImageView.hidden = YES;
        self.nameLabel.textColor = [UIColor blackColor];
    }

    self.iconImageView.image = [UIImage imageNamed:status.icon];
    self.nameLabel.text = status.name;
    self.contentLabel.text = status.text;

    if(status.picture)
    {
        self.pictureImageView.hidden = NO;
        self.pictureImageView.image = [UIImage imageNamed:status.picture];
    }
    else
    {
        self.pictureImageView.hidden = YES;
    }

    // 强制布局
    [self layoutIfNeeded];

    if(self.pictureImageView.isHidden)
    {
        status.cellHeight = CGRectGetMaxY(self.contentLabel.frame)+10;
    }else
    {
        status.cellHeight = CGRectGetMaxY(self.pictureImageView.frame)+10;
    }

}

```
