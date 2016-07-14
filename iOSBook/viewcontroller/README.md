# viewController


```objc
控制器常见的创建方式有以下几种
通过storyboard创建

直接创建
MJViewController *mj = [[MJViewController alloc] init];

指定xib文件来创建
MJViewController *mj = [[MJViewController alloc] initWithNibName:@"MJViewController" bundle:nil];

```

```objc

先加载storyboard文件（Test是storyboard的文件名）
UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Test" bundle:nil];

接着初始化storyboard中的控制器
初始化“初始控制器”（箭头所指的控制器）
MJViewController *mj = [storyboard instantiateInitialViewController];

通过一个标识初始化对应的控制器
MJViewController *mj = [storyboard instantiateViewControllerWithIdentifier:@”mj"];


```



- 加载storyboard控制器

```objc

    // 创建窗口
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];


    // 创建窗口的跟控制器
    // 加载storyboard
    // storyboard文件名，不需要带后缀
    // nil:  [NSBundle mainBundle]
    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
    // 通过storyboard创建控制器
    // instantiateInitialViewController：加载箭头指向的控制器
//    UIViewController *vc = [storyboard instantiateInitialViewController];

    UIViewController *vc = [storyboard instantiateViewControllerWithIdentifier:@"green"];

    NSLog(@"%@",[vc class]);
    self.window.rootViewController = vc;

    // 显示窗口
    [self.window makeKeyAndVisible];

```

- 加载xib控制器
- 注意点(设置owner和连线)
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160531_1.png)

```objc

    // 创建窗口
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];


    // 通过xib创建控制器
    ViewController *vc = [[ViewController alloc] initWithNibName:@"ViewController" bundle:nil];


    self.window.rootViewController = vc;


    [self.window makeKeyAndVisible];

```

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160531_2.png)



















