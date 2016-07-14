# navigationItem

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160602_14.png
)

```objc

导航栏的内容由栈顶控制器的navigationItem属性决定

UINavigationItem有以下属性影响着导航栏的内容
左上角的返回按钮
@property(nonatomic,retain) UIBarButtonItem *backBarButtonItem;
中间的标题视图
@property(nonatomic,retain) UIView          *titleView;
中间的标题文字
@property(nonatomic,copy)   NSString        *title;
左上角的视图
@property(nonatomic,retain) UIBarButtonItem *leftBarButtonItem;
右上角的视图
@property(nonatomic,retain) UIBarButtonItem *rightBarButtonItem;


```

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160531_3.png)

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160531_7.png)

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160531_6.png)

