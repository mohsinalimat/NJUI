# orderPassModel

```objc


/*
 来源控制器传递给目的控制器：顺传
 数据传值:
 1.接收方一定要有属性接收
 2.传递方必须要拿到接收方

 */

```

```objc

// 在执行跳转之前的时候调用
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{

    UIViewController *vc = segue.destinationViewController;
    vc.title = [NSString stringWithFormat:@"%@的联系人列表", _accountField.text];
    NSLog(@"%@--%@",segue.sourceViewController,segue.destinationViewController);
}

```
