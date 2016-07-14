# transform

- `transform`是相对控件最初的状态而改变的量<br>

````objc
/*
 CGAffineTransformMakeTranslation 让控件移动,而且只移动一次
 tx:水平方向的移动距离  >0 向右  <0向左
 ty:垂直方向的移动距离  >0 向下  <0向上

 CGAffineTransformTranslate 可以实现控件的持续移动
 transform:传控件的transform,表示在修改之前获取一下当前的transform属性,然后再在这个transform上修改


 CGAffineTransformScale 实现控件的放大和缩小
 sx:水平方向的缩放比例   >1 放大  0<sx<1缩小
 sy:垂直方向的缩放比例

 CGAffineTransformRotate 实现控件的旋转
 angle >0 顺时针   <0 逆时针
 angle 需要的是弧度  M_PI = 180度

 如果transform值不是初始化的值,这个时候就不能再修改frame属性.
 如果修改了transform以后,
    还想重新设置控件的位置,需要修改控件的center
    还想重新设置控件的大小,需要修改控件的bounds.
 */

````

### 代码示例

````objc

- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event {

    // 清空transform，以前的平移、缩放、旋转都会消失
    [UIView animateWithDuration:2.0 animations:^{
    // 单位矩阵, 恢复原始状态
        self.tempView.transform = CGAffineTransformIdentity;

    }];
    // 一直在原来控件的transform上进行叠加, 叠加叠加
    [UIView animateWithDuration:1 animations:^{
        self.tempView.transform = CGAffineTransformTranslate(self.tempView.transform, 100, 0);
        self.tempView.transform = CGAffineTransformScale(self.tempView.transform, 0.5, 0.5);
        // 最后一行才起作用,累加了上边的2行
        self.tempView.transform = CGAffineTransformRotate(self.tempView.transform, M_PI_4);
    }];

````


- `CGAffineTransformMakeScale`-`make`的本质是创建一个`transfrom`;
- `CGAffineTransformMakeTranslation`-`make`的本质是创建一`个transfrom`;
- `CGAffineTransformMakeRotation`-`make`的本质是创建一个`transfrom`;

- `CGAffineTransformScale`-没有make`是叠加其它的``transform`
- `CGAffineTransformRotate`-没有make`是叠加其它的``transform`
- `CGAffineTransformTranslate`-没有make`是叠加其它的``transform`

````objc

     transform:形变属性，能完成的功能：缩放、平移、旋转
    [UIView animateWithDuration:2.0 animations:^{
        // 缩放 修改transform,会覆盖上一次的transform
        self.tempView.transform = CGAffineTransformMakeScale(0.5, 0.5);
        // 平移  修改transform ,会覆盖上一次的transform
      self.tempView.transform = CGAffineTransformMakeTranslation(-100, 100);
         // 旋转  修改transform,会覆盖上一次的transform, 前边2行都没用了!!!!!!!
        self.tempView.transform = CGAffineTransformMakeRotation(-M_PI_4);



        CGAffineTransform translation = CGAffineTransformMakeTranslation(-100, 100);

       CGAffineTransform scaleTranslation = CGAffineTransformScale(translation, 0.5, 0.5);
            // 叠加了上边2行的transform
       CGAffineTransform rotateScaleTranslation = CGAffineTransformRotate(scaleTranslation, M_PI_2);
            // 上边叠加的transform一起来, 上边的3行才起作用
        self.tempView.transform = rotateScaleTranslation;
    }];
}

````
