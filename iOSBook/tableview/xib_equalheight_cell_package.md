# xib EqualHeight Cell Package

- cell的高度实际上由tableViewController里边设置rowHeight决定,
    - 如果由tableViewController里边设置的rowHeight比xib里边cell高度高的话,就会把cell向下拉伸,但是由于自动布局, 所以里边的子控件也会拉伸
- **`反正提供一个手动创建cell的类方法,外边想怎么用怎么用`**
- **`反正提供一个手动创建cell的类方法,外边想怎么用怎么用`**
- **`反正提供一个手动创建cell的类方法,外边想怎么用怎么用`**


![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160519_4.png)


![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160519_3.png)


- 如果在`viewDidLoad`里边注册了xib,那么在下边用的时候也可以用下边图片实际用的类方法上边的注释的方法, 也可以使用类方法

```objc
/    static NSString *ID = @"deal";
//    // 获得或者创建cell
    LMJDealCellTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
```

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160519_9.png)





