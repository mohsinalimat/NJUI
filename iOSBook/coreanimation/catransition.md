# CATransition

## `转场动画过渡效果`

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160611_1.png)

---

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160612_24.png)

---

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160612_25.png)


```objc

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event
{
    static int i = 2;
    // 转场代码
    if (i == 4) {
        i = 1;
    }
    // 加载图片名称
    NSString *imageN = [NSString stringWithFormat:@"%d",i];

    _imageView.image = [UIImage imageNamed:imageN];

    i++;

    // 转场动画
    CATransition *anim = [CATransition animation];

    anim.type = @"pageCurl";

    anim.duration = 2;

    [_imageView.layer addAnimation:anim forKey:nil];

}

```
