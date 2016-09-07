# Window

```objc
// main -> UIApplicationMain
/*
 UIApplicationMain
 1.创建UIApplication
 2.创建UIApplicationDelegate，并且成为UIApplication代理
 3.开启主运行循环，保持程序一直在运行
 4.加载info.plist,判断有没有指定main.stroyboard，指定了就加载


 加载main.stroyboard做的事情
 1.创建窗口
 2.加载main.storyboard,并且加载main.storyboard指定的控制器
 3.把新创建的控制器作为窗口的跟控制器，让窗口显示出来
```

```objc

    // 1.创建窗口,注意窗口必须要有尺寸，尺寸跟屏幕一样大的尺寸,窗口不要被释放
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor redColor];

    // 2.创建窗口的根控制器
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view.backgroundColor = [UIColor yellowColor];

    [vc.view addSubview:[UIButton buttonWithType:UIButtonTypeContactAdd]];

    // 如果设置窗口的跟控制器，默认就会把控制器的view添加到窗口上
    // 设置窗口的跟控制器，默认就有旋转功能
    self.window.rootViewController = vc;

//    [self.window addSubview:vc.view];

    // 3.显示窗口
    [self.window makeKeyAndVisible];

```
---

```objc

    // 设置窗口的层级关系
    self.window1.windowLevel = UIWindowLevelStatusBar;

    [self.window1 makeKeyAndVisible];


    // 窗口是有层级关系
    // UIWindowLevelNormal < UIWindowLevelStatusBar < UIWindowLevelAlert

```
