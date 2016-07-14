# TextField

```objc

通过UITextField的代理方法能够监听键盘最右下角按钮的点击
成为UITextField的代理
self.textField.delegate = self;

遵守UITextFieldDelegate协议,实现代理方法
- (BOOL)textFieldShouldReturn:(UITextField *)textField;

// 在UITextField左边放一个view
self.textField.leftView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 8, 0)];

// 控制左边的view什么时候显示
self.textField.leftViewMode = UITextFieldViewModeAlways;


```
