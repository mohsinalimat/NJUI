# navigationController

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160602_13.png
)

```objc
UINavigationController以栈的形式保存子控制器
@property(nonatomic,copy) NSArray *viewControllers;
@property(nonatomic,readonly) NSArray *childViewControllers;

使用push方法能将某个控制器压入栈
- (void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated;

使用pop方法可以移除控制器
将栈顶的控制器移除
- (UIViewController *)popViewControllerAnimated:(BOOL)animated;
回到指定的子控制器
- (NSArray *)popToViewController:(UIViewController *)viewController animated:(BOOL)animated;
回到根控制器（栈底控制器）
- (NSArray *)popToRootViewControllerAnimated:(BOOL)animated;
```

```objc

    // 创建窗口
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

    // 创建导航控制器的跟控制器,也属于导航控制器的子控制器
    UIViewController *vc = [[OneViewController alloc] init];
    vc.view.backgroundColor = [UIColor redColor];


    // 导航控制器也需要一个根控制器
    // 默认导航控制器把根控制器的view添加到导航控制器的view上
    UINavigationController *navVc = [[UINavigationController alloc] initWithRootViewController:vc];

    NSLog(@"%@",navVc);
    // 设置窗口的跟控制器
    self.window.rootViewController = navVc;

    [self.window makeKeyAndVisible];

```
---

```objc

    TwoViewController *vc = [[TwoViewController alloc] init];

    vc.view.backgroundColor = [UIColor yellowColor];
    // 跳转
    // 如果导航控制器调用push，就会把vc添加为导航控制器的子控制器
    [self.navigationController pushViewController:vc animated:YES];

```
---


```objc

// 返回上一个控制器
- (IBAction)backToPre:(id)sender {

    // pop不是马上把控制器销毁，
    // 回到上一个界面
    [self.navigationController popViewControllerAnimated:YES];

}

// 返回到导航控制器的跟控制器
- (IBAction)back2Root:(id)sender {


    // 方法1.注意：只能返回到栈里面的控制器
    [self.navigationController popToViewController:self.navigationController.childViewControllers[0] animated:YES];
    // 方法2
    [self.navigationController popToRootViewControllerAnimated:YES];
}

```
