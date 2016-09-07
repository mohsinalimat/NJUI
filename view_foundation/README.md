# View Foundation

````objc

[self.view addSubview:button];

[self.view insertSubview:stepper atIndex:..];
[self.view insertSubview:.. belowSubview:..];
[self.view insertSubview:..  aboveSubview:...];
[self.button removeFromSuperview];

// 自己从父控件中删除
- (void)removeFromSuperview;
// 父控件插入一个空间在数组数组中的什么位置
- (void)insertSubview:(UIView *)view atIndex:(NSInteger)index;
// 父控件添加子控件
- (void)addSubview:(UIView *)view;

// 父控件 添加子控件到某个控件的下边
- (void)insertSubview:(UIView *)view belowSubview:(UIView *)siblingSubview;
//....上边
- (void)insertSubview:(UIView *)view aboveSubview:(UIView *)siblingSubview;

---
//把self.view里的某个子控件带到前面来显示
 [self.view bringSubviewToFront:self.iconButton];


 // 获取一个控件是否是隐藏状态
         if(optionButton.isHidden) // 注意是isHidden
         {
            //让它不隐藏
            optionButton.hidden = NO;
            break;
        }

//设置待选项视图不可以响应用户的操作
    self.optionView.userInteractionEnabled = NO;

````


- 怎么隐藏iPhone上边的电池状态条

---
- 需要添加的代码
![](../LibrarypPictures/Snip20160517_2.png)
- 结果如下
![](../LibrarypPictures/Snip20160517_3.png)
---






