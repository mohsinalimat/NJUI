# pch




```objc

 1.Products mac下使用
 2.Bundle identifier 应用程序的唯一标示
 3.Bundle display name 项目名称 （可改）不要与线上版本相差太大 有可能会被拒
 4.Bundle versions string, short 外部版本号 只能加大 不能减小
 5.Bundle version 内部版本号
 6.Launch screen interface file base name 程序启动文件
 7.Main storyboard file base name   storyboard文件名
 8.Supported interface orientations 程序支持屏幕方向

```




```objc

#ifdef __OBJC__

#define ABC 10
#import "UIImage+Image.h"
// 配置pch： buildSetting -> prefix ->
/* pch里面的所有内容都是共享，每个文件都会共有:
    作用:
    1.存放一些公用的宏
    2.存放一些公用的头文件
    3.自定义Log
 */
// 宏里面可变参数：...
// 函数中可变参数: __VA_ARGS__
#ifdef DEBUG // 调试阶段
#define XMGLog(...)  NSLog(__VA_ARGS__)
#else // 发布阶段
#define XMGLog(...)

#endif

#endif

```


- pch文件的提前编译
![](../LibrarypPictures/Snip20160529_1.png)

# 程序语言的调整

![](../LibrarypPictures/Snip20160601_15.png)

![](../LibrarypPictures/Snip20160601_14.png)
