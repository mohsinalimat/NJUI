## 动画

- 第一种动画 (首尾)

````objc
    [UIView beginAnimations:nil context:nil];
    [UIView setAnimationDuration:2.0];
    [UIView setAnimationDelegate:self];
    [UIView setAnimationWillStartSelector:@selector(start)];
    [UIView setAnimationDidStopSelector:@selector(stop)];


    [UIView setAnimationDelay:1.0];
    [UIView setAnimationCurve:UIViewAnimationCurveEaseOut];

    CGFloat offX = self.scrollView.contentSize.width - self.scrollView.frame.size.width;
    self.scrollView.contentOffset = CGPointMake(offX, self.scrollView.contentOffset.y);

    [UIView commitAnimations];
````


- 第二种动画 (有结束block的动画)

````objc
    [UIView animateWithDuration:2.0 animations:^{
        self.scrollView.contentOffset = CGPointMake(0, self.scrollView.contentOffset.y);
    } completion:^(BOOL finished) {
        NSLog(@"finishde left");
    }];

````

- 第三种动画 (contentOffset)

````objc

// 第三种 属于contentOffset 的专属动画
    [self.scrollView setContentOffset: CGPointMake(self.scrollView.contentOffset.x, 0) animated:YES];
````

- 第四种动画 (简单的block动画)

````objc
    [UIView animateWithDuration:2.0 animations:^{
         self.scrollView.contentOffset = CGPointMake(self.scrollView.contentOffset.x, offY);
    }];
````

- 第五种动画 (序列帧动画)

````objc
- (void)playAnimation:(NSInteger)imageCount andPreName:(NSString *)preName{
    NSLog(@"%@",[NSBundle mainBundle]);
    //创建一个可变数组,用来保存所有的图片
    NSMutableArray *images = [NSMutableArray array];
    for (NSInteger i = 0; i < imageCount; ++i) {
        //i 0 ~ 24
        //使用i拼接图片名称
        NSString *imageName = [NSString stringWithFormat:@"%@%03zd",preName,i + 1];

        //使用图片名称生成一个图片对象
//        UIImage *image = [UIImage imageNamed:imageName];
        NSString *path = [[NSBundle mainBundle]pathForResource:imageName ofType:@"png"];
        // 不常用的图片用这种方式
        UIImage *image = [UIImage imageWithContentsOfFile:path];
        /*
        UIImage *image = [UIImage imageNamed:father00%d, i+1];
        */
        //把图片添加到可变数组里
        [images addObject:image];
    }

    //给UImageView控件设置一个动画数组
    self.iconImage.animationImages = images;

    //配置序列帧动画的相关属性
    //设置动画的重复次数
    self.iconImage.animationRepeatCount = 1;

    //设置动画的时长
    self.iconImage.animationDuration = 0.1 * imageCount;

    //开启序列帧动画
    [self.iconImage startAnimating];
}

````

- 延迟执行动画
````objc
        [UIView animateWithDuration:<#(NSTimeInterval)#>
delay:<#(NSTimeInterval)#> options:<#(UIViewAnimationOptions)#>
animations:<#^(void)animations#> completion:<#^(BOOL finished)completion#>]

        [UIView animateWithDuration:1.5 animations:^{
            self.hud.alpha = 1.0;
        } completion:^(BOOL finished)
         {
             [UIView animateWithDuration:1.5 delay:2 options:UIViewAnimationOptionCurveLinear animations:^{
                 self.hud.alpha = 0;
             } completion:^(BOOL finished)
              {

              }];
         }];
````
