# tabBarController

- UITabBarController添加控制器的方式有2种
    - 添加单个子控制器

```objc
- (void)addChildViewController:(UIViewController *)childController;
```

- 设置子控制器数组

```objc
@property(nonatomic,copy) NSArray *viewControllers;
```
---

![](../LibrarypPictures/Snip20160602_10.png
)

---

![](../LibrarypPictures/Snip20160602_11.png
)

---

![](../LibrarypPictures/Snip20160602_12.png
)

---

![](../LibrarypPictures/Snip20160602_23.png
)

- 隐藏控制器底部的导航条(是在控制器创建完成后push之前的操作)
    - `self.hidesBottomBarWhenPushed = YES;`
