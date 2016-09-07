# PickerView

```objc

@interface ViewController ()<UITextFieldDelegate,UIPickerViewDataSource,UIPickerViewDelegate>

@property (nonatomic, weak) UIPickerView *pickerView;
@property (weak, nonatomic) IBOutlet UITextField *cityField;

@property (nonatomic, strong) NSMutableArray *provinces;

@property (nonatomic, assign) NSInteger proIndex;

@end

@implementation ViewController

#pragma mark - UITextFieldDelegate

// 是否允许用户输入文字
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    return NO;
}

#pragma mark -viewDidLoad
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    // 设置文本框的代码

    _cityField.delegate = self;

    // 自定义生日键盘
    // 自定义城市键盘
    [self setUpCityKeyboard];

}
#pragma mark - 自定义城市键盘
- (void)setUpCityKeyboard
{
    UIPickerView *pickerView = [[UIPickerView alloc] init];

    _pickerView = pickerView;

    pickerView.dataSource = self;
    pickerView.delegate = self;

    _cityField.inputView = pickerView;
}
#pragma mark -UIPickerView
#pragma mark UIPickerView的数据源
- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView
{
    return 2;
}
// pickerView的第0列描述省会，有多少个省
// pickerView的第1列描述选中的省会，有多少个城市
- (NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component
{
    if (component == 0) { // 描述省会
        return self.provinces.count;
    }else{ // 描述选中的省会的城市
        // 获取省会
        XMGProvince *p = self.provinces[_proIndex];

        return p.cities.count;
    }
}
#pragma mark -UIPickerView的代理
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component
{
    if (component == 0) { // 描述省会

        // 获取省会
        XMGProvince *p = self.provinces[row];
        return p.name;

    }else{ // 描述选中的省会的城市
        // 获取选中的省会的角标
        // 获取选中省会
        XMGProvince *p = self.provinces[_proIndex];

        // 当前选中的内蒙古省，只有12个城市，角标0~11，但是右边城市是北京，北京的城市大于12个城市，所以滚动的时候会出现越界。

        NSLog(@"province:%@, count:%ld row:%ld",p.name,p.cities.count,row);


        return p.cities[row];
    }
}

// 全局断点就是帮我们定位到出bug的那一行。

// 滚动UIPickerView就会调用
- (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component
{
    if (component == 0) { // 滚动省会,刷新城市（1列）
        // 记录当前选中的省会
        _proIndex = [pickerView selectedRowInComponent:0];

        [pickerView reloadComponent:1];
    }
    // 给城市文本框赋值
    // 获取选中省会
    XMGProvince *p = self.provinces[_proIndex];

    // 获取选中的城市
    NSInteger cityIndex = [pickerView selectedRowInComponent:1];

    NSString *cityName = p.cities[cityIndex];

    _cityField.text = [NSString stringWithFormat:@"%@ %@",p.name,cityName];
}
- (void)textFieldDidBeginEditing:(UITextField *)textField
{
    [self pickerView:self.pickerView didSelectRow:0 inComponent:0];
}

```



