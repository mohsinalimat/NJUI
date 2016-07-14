# message-cell-last

- 注意点1
     - cellXib里边的button类型是custom
     - button的宽度约束是60-250之间, 不用添加按钮的高度约束
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_1.png)

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_2.png)

- 注意点2
    - 设置控件隐藏的时候注意类型
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_3.png)

- 注意点3
    - 1设置控件的隐藏
    - 2设置内容
    - 3根据内容, 设置button的约束,
    - 4再计算cell的高度
    - 计算之前都要强制更新
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_5.png)
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_4.png)

- 注意点4
    - 设置button的背景图片的拉伸
    - 设置button的内边距
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_6.png)
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_14.png)
- 注意点5, 返回cell的高度
    - 不要返回cell的预估高度, 会有bug
    - 在懒加载里边只用取出一个cell,传递模型
    - 算出cell的高度后, 模型就有高度属性了
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_8.png)

- 注意点6
    - 怎么让textfiled的左边空出一个输入距离
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_15.png)

- 注意点7
 - 设置键盘的弹出相应
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160616_16.png)


## `LMJMessageCell.m`

```objc

#import "LMJMessageCell.h"
#import "LMJMessage.h"
#import "Masonry/Masonry.h"
@interface LMJMessageCell ()

@property (weak, nonatomic) IBOutlet UILabel *timeLabel;
@property (weak, nonatomic) IBOutlet UIImageView *otherIcon;
@property (weak, nonatomic) IBOutlet UIImageView *meIcon;
@property (weak, nonatomic) IBOutlet UIButton *otherChatButton;
@property (weak, nonatomic) IBOutlet UIButton *meChatButton;

@end

@implementation LMJMessageCell

+ (instancetype)messageCellWithTableView:(UITableView *)tableView
{
    static NSString *ID = @"message";

    LMJMessageCell *msCell = [tableView dequeueReusableCellWithIdentifier:ID];
    if(msCell == nil)
    {
        msCell = [[NSBundle mainBundle] loadNibNamed:NSStringFromClass(self) owner:nil options:nil].firstObject;
    }
    return msCell;
}

- (void)awakeFromNib {
    [super awakeFromNib];

    self.meIcon.layer.cornerRadius = self.meIcon.bounds.size.width/2;
    self.otherIcon.layer.cornerRadius = self.otherIcon.bounds.size.width/2;

    self.meChatButton.titleLabel.numberOfLines = 0;
    self.otherChatButton.titleLabel.numberOfLines = 0;

    self.meChatButton.titleLabel.backgroundColor = [UIColor redColor];
    self.otherChatButton.titleLabel.backgroundColor = [UIColor redColor];

    [self setButton:self.otherChatButton backgroundImageForState:UIControlStateNormal withRequiredStretchImage:[UIImage imageNamed:@"chat_recive_nor"]];

    [self setButton:self.otherChatButton backgroundImageForState:UIControlStateHighlighted withRequiredStretchImage:[UIImage imageNamed:@"chat_recive_press_pic"]];


    [self setButton:self.meChatButton backgroundImageForState:UIControlStateNormal withRequiredStretchImage:[UIImage imageNamed:@"chat_send_nor"]];

    [self setButton:self.meChatButton backgroundImageForState:UIControlStateHighlighted withRequiredStretchImage:[UIImage imageNamed:@"chat_send_press_pic"]];
}


- (void)setButton:(UIButton *)btn backgroundImageForState:(UIControlState)state withRequiredStretchImage:(UIImage *)image
{
    UIImage *stretchImage = [image resizableImageWithCapInsets:UIEdgeInsetsMake(image.size.height/2, image.size.width/2, image.size.height/2, image.size.width/2) resizingMode:UIImageResizingModeStretch];

    [btn setBackgroundImage:stretchImage forState:state];
}


- (void)setMessage:(LMJMessage *)message
{
    _message = message;

    // 1 设置控件的显示
    [self controlCaputureHidden:message];

    // 2 设置内容
    [self setContentOfCaputures:message];

    // 3 实时更新button的尺寸
    [self updateConstraintOffButton:(message.type == LMJMessageTypeOher ? self.otherChatButton : self.meChatButton)];

    // 4 当模型里边的高度属性是0的时候, 计算cell的高度, 给到模型
    if(message.cellHeight == 0)
    {
        [self calculateCellHeightWithBtn:(message.type == LMJMessageTypeOher ? self.otherChatButton : self.meChatButton) andIcon:(message.type == LMJMessageTypeOher ? self.otherIcon : self.meIcon)];
    }
}

// 实时根据内容, 更新btn的宽高
- (void)updateConstraintOffButton:(UIButton *)btn
{
    [self layoutIfNeeded];

    CGFloat btnH = btn.titleLabel.frame.size.height + 30;

    [btn updateConstraints:^(MASConstraintMaker *make) {

        make.height.equalTo(btnH);
    }];
}

// 根据button设置约束后的, 实际显示, 强制更新后算出cell的高度, 给到模型
- (void)calculateCellHeightWithBtn:(UIButton *)btn andIcon:(UIImageView *)icon
{
    [self layoutIfNeeded];

    self.message.cellHeight = MAX(CGRectGetMaxY(icon.frame), CGRectGetMaxY(btn.frame)) + 10;

}

// 设置控件要显示的内容
- (void)setContentOfCaputures:(LMJMessage *)message
{
    self.timeLabel.text = message.time;

    [(message.type == LMJMessageTypeOher ? self.otherChatButton : self.meChatButton) setTitle:message.text forState:UIControlStateNormal];
}

// 设置控件的隐藏与否
- (void)controlCaputureHidden:(LMJMessage *)message
{
    self.timeLabel.hidden = message.isHideTime;
    [self.timeLabel updateConstraints:^(MASConstraintMaker *make) {
        make.height.equalTo(message.isHideTime ? 0 : 21);
    }];

    self.otherIcon.hidden = (message.type == LMJMessageTypeMe);
    self.otherChatButton.hidden = (message.type == LMJMessageTypeMe);

    self.meIcon.hidden = (message.type == LMJMessageTypeOher);
    self.meChatButton.hidden = (message.type == LMJMessageTypeOher);
}

```

##`controller.m`

```objc

#import "LMJMessageController.h"
#import "LMJMessage.h"
#import "LMJMessageCell.h"
#define LMJNotiCenter [NSNotificationCenter defaultCenter]
@interface LMJMessageController ()<UITableViewDelegate, UITableViewDataSource, UITextFieldDelegate>
@property (weak, nonatomic) IBOutlet UITableView *tableView;
@property (weak, nonatomic) IBOutlet UITextField *inputTextField;

/** <#digest#> */
@property (nonatomic, strong) NSMutableArray<LMJMessage *> *messageses;
@end

@implementation LMJMessageController

- (void)viewDidLoad {
    [super viewDidLoad];

    [LMJNotiCenter addObserver:self selector:@selector(keyboardWillChangeFrame:) name:UIKeyboardWillChangeFrameNotification object:nil];

    self.inputTextField.leftView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 10, 1)];
    self.inputTextField.leftViewMode = UITextFieldViewModeAlways;

#warning scroll
   [self.tableView scrollToRowAtIndexPath:[NSIndexPath indexPathForRow:self.messageses.count-1 inSection:0] atScrollPosition:UITableViewScrollPositionBottom animated:NO];

}

- (void)keyboardWillChangeFrame:(NSNotification *)note
{
    //    NSLog(@"%@", note);

    CGFloat durTime = [note.userInfo[UIKeyboardAnimationDurationUserInfoKey] floatValue];

    CGRect endRect = [note.userInfo[UIKeyboardFrameEndUserInfoKey] CGRectValue];

    CGFloat ty = [UIScreen mainScreen].bounds.size.height - endRect.origin.y;

    [UIView animateWithDuration:durTime animations:^{
        self.view.transform = CGAffineTransformMakeTranslation(0, -ty);
    }];
}

- (void)dealloc
{
    [LMJNotiCenter removeObserver:self forKeyPath:UIKeyboardWillChangeFrameNotification];
}

- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    LMJMessage *message = [[LMJMessage alloc] init];

    message.text = textField.text;
    message.type  = LMJMessageTypeMe;
    message.hideTime = YES;
    textField.text = nil;

    LMJMessageCell *messageCell = [LMJMessageCell messageCellWithTableView:self.tableView];

    messageCell.message = message;

    [self.messageses addObject:message];

    [self.tableView reloadData];

#warning scroll
   [self.tableView scrollToRowAtIndexPath:[NSIndexPath indexPathForRow:self.messageses.count-1 inSection:0] atScrollPosition:UITableViewScrollPositionBottom animated:YES];

    return YES;
}


- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    //    NSLog(@"%zd", self.messageses.count);
    return self.messageses.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    LMJMessageCell *messageCell = [LMJMessageCell messageCellWithTableView:tableView];

    messageCell.message = self.messageses[indexPath.row];

    return messageCell;
}

- (NSMutableArray<LMJMessage *> *)messageses
{
    if (_messageses == nil) {
        NSString *path = [[NSBundle mainBundle] pathForResource:@"messages" ofType:@"plist"];
        NSArray *dictArr = [NSArray arrayWithContentsOfFile:path];
        NSMutableArray<LMJMessage *> *arrM = [NSMutableArray array];
        // 懒加载每个cell的高度. 就不用再次计算了!!
        LMJMessageCell *messageCell = [LMJMessageCell messageCellWithTableView:self.tableView];
        [dictArr enumerateObjectsUsingBlock:^(NSDictionary * _Nonnull dict, NSUInteger idx, BOOL * _Nonnull stop) {
            LMJMessage *ms = [LMJMessage messageWithDict:dict];
            // 计算模型里边的高度
            messageCell.message = ms;
            ms.hideTime = ([arrM.lastObject.time isEqualToString:ms.time]);
            [arrM addObject:ms];
        }];
        _messageses = arrM;
    }
    return _messageses;
}
//- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath
//{
//    return 200;
//}
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return self.messageses[indexPath.row].cellHeight;
}


- (BOOL)prefersStatusBarHidden
{
    return YES;
}

- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    [self.view endEditing:YES];
}
@end

```

