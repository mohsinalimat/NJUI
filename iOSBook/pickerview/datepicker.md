# datePicker


```objc

- (void)setUpBirthdayKeyboard
{
    //1. 手工创建日期选择器 默认有宽高
    UIDatePicker * datePicker = [[UIDatePicker alloc] init];
    //2.设置mode
    datePicker.datePickerMode = UIDatePickerModeDate;
    //3.设置语言
    datePicker.locale = [NSLocale localeWithLocaleIdentifier:@"zh-Hans"];

    //4.添加监听事件
    [datePicker addTarget:self action:@selector(valueChanged:) forControlEvents:UIControlEventValueChanged];
    //设置文本框inputView属性
    self.textField.inputView = datePicker;

    //1.创建工具条
    UIToolbar * toolBar = [[UIToolbar alloc] initWithFrame:CGRectMake(0, 0, 0, 44)];
    //2.设置bar的颜色
    toolBar.barTintColor = [UIColor redColor];
    //创建按钮
    UIBarButtonItem * done = [[UIBarButtonItem alloc] initWithTitle:@"完成" style:UIBarButtonItemStylePlain target:self action:@selector(doneClick)];
    //创建一根弹簧
    UIBarButtonItem * flx = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil action:nil];
    //把按钮添加到工具条上
    toolBar.items = @[flx,done];

    //设置文本框的inputAccessoryView的属性
    self.textField.inputAccessoryView = toolBar;
}

```
