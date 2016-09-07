# application

- 获取用户系统版本号

![](../LibrarypPictures/Snip20160601_12.png)

```objc

#pragma mark - 控制器设置状态栏
// 在iOS7以后，状态栏默认由控制器决定
// 隐藏状态栏
//- (BOOL)prefersStatusBarHidden
//{
//    return YES;
//}

//- (UIStatusBarStyle)preferredStatusBarStyle
//{
//    return UIStatusBarStyleLightContent;
//}

#pragma mark - 设置提醒数字
- (void)application
{
    // 1.整个app中只有一个UIApplication

    //    UIApplication *app = [[UIApplication alloc] init];

    UIApplication *app = [UIApplication sharedApplication];

    // 2.UIApplication一般用来做一些应用级别的操作（app的提醒框，联网状态，打电话，打开网页，控制状态栏）
    //    UIApplication *app1 = [UIApplication sharedApplication];

    // 设置appIcon提醒数字，必须注册用户通知
    app.applicationIconBadgeNumber = 10;
    // 创建用户通知
    UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeBadge categories:nil];
    // 注册用户的通知
    [app registerUserNotificationSettings:settings];

    // 设置联网状态
    app.networkActivityIndicatorVisible = YES;

}

```

```objc

#pragma mark - 打开网页
- (IBAction)btnClick:(id)sender {

    // URL：资源路径
    // URL：协议头://域名+路径  http,https,file,tel
    // 协议头:
    // 打开网页 @"http://www.baidu.com"

    NSURL *url = [NSURL URLWithString:@"http://www.baidu.com"];

    [[UIApplication sharedApplication] openURL:url];

}
#pragma mark - 隐藏状态栏
- (void)statusHidden
{
    // 获取UIApplication
    UIApplication *app = [UIApplication sharedApplication];

    // 隐藏状态栏
    //    [app setStatusBarHidden:YES];

    [app setStatusBarHidden:YES withAnimation:UIStatusBarAnimationSlide];
}

```


- 程序的启动原理

```objc

// main:程序入口
int main(int argc, char * argv[]) {
    @autoreleasepool {
        // nil:@"UIApplication"

        ;
        // 类名转字符串好处: 1.防止输入错误

        // 字符串转类名 NSClassFromString(@"UIApplication")

        // 第三个参数：UIApplication
        // 第四个参数：AppDelegate：必须要遵守UIApplicationDelegate协议
        return UIApplicationMain(argc, argv, nil,NSStringFromClass([AppDelegate class]));
    }
}

/* UIApplicationMain底层实现
1.根据principalClassName提供类名创建UIApplication对象
2.创建UIApplicationDelegate对象，并且成为UIApplication对象代理，app.delete = delegate
 3.开启一个主运行循环，处理事件，可以保持程序一直运行。
 4.加载info.plist，并且判断有木有指定main.storyboard,如果指定，就会去加载

*/


```


