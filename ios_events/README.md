# iOS Events

![](../LibrarypPictures/Snip20160604_3.png)

![](../LibrarypPictures/Snip20160604_4.png)

![](../LibrarypPictures/Snip20160604_5.png)

![](../LibrarypPictures/Snip20160604_6.png)

![](../LibrarypPictures/Snip20160604_7.png)

![](../LibrarypPictures/Snip20160604_8.png)

![](../LibrarypPictures/Snip20160604_9.png)


![](../LibrarypPictures/Snip20160604_10.png)

![](../LibrarypPictures/Snip20160604_11.png)

![](../LibrarypPictures/Snip20160604_12.png)

![](../LibrarypPictures/Snip20160604_17.png)

![](../LibrarypPictures/Snip20160604_13.png)

![](../LibrarypPictures/Snip20160604_14.png)

![](../LibrarypPictures/Snip20160604_15.png)

---


# UIGestureRecognizer

```objc

为了完成手势识别，必须借助于手势识别器----UIGestureRecognizer

利用UIGestureRecognizer，能轻松识别用户在某个view上面做的一些常见手势

UIGestureRecognizer是一个抽象类，定义了所有手势的基本行为，使用它的子类才能处理具体的手势
UITapGestureRecognizer(敲击)
UIPinchGestureRecognizer(捏合，用于缩放)
UIPanGestureRecognizer(拖拽)
UISwipeGestureRecognizer(轻扫)
UIRotationGestureRecognizer(旋转)
UILongPressGestureRecognizer(长按)


```
---

## `UITapGestureRecognizer`
```objc
每一个手势识别器的用法都差不多，比如UITapGestureRecognizer的使用步骤如下
创建手势识别器对象
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];

设置手势识别器对象的具体属性
// 连续敲击2次
tap.numberOfTapsRequired = 2;
// 需要2根手指一起敲击
tap.numberOfTouchesRequired = 2;

添加手势识别器到对应的view上
[self.iconView addGestureRecognizer:tap];

监听手势的触发
[tap addTarget:self action:@selector(tapIconView:)];

```

### `手势识别状态`

```objc
typedef NS_ENUM(NSInteger, UIGestureRecognizerState) {
    // 没有触摸事件发生，所有手势识别的默认状态
    UIGestureRecognizerStatePossible,
    // 一个手势已经开始但尚未改变或者完成时
    UIGestureRecognizerStateBegan,
    // 手势状态改变
    UIGestureRecognizerStateChanged,
    // 手势完成
    UIGestureRecognizerStateEnded,
    // 手势取消，恢复至Possible状态
    UIGestureRecognizerStateCancelled,
    // 手势失败，恢复至Possible状态
    UIGestureRecognizerStateFailed,
    // 识别到手势识别
    UIGestureRecognizerStateRecognized = UIGestureRecognizerStateEnded
};


```

















