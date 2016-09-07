# CAAnimationGroup
![](../LibrarypPictures/Snip20160613_1.png)

```objc

    // 同时缩放，平移，旋转
    CAAnimationGroup *group = [CAAnimationGroup animation];

    CABasicAnimation *scale = [CABasicAnimation animation];
    scale.keyPath = @"transform.scale";
    scale.toValue = @0.5;

    CABasicAnimation *rotation = [CABasicAnimation animation];
    rotation.keyPath = @"transform.rotation";
    rotation.toValue = @(arc4random_uniform(M_PI));

    CABasicAnimation *position = [CABasicAnimation animation];
    position.keyPath = @"position";
    position.toValue = [NSValue valueWithCGPoint:CGPointMake(arc4random_uniform(200), arc4random_uniform(200))];

    group.animations = @[scale,rotation,position];

    [_redView.layer addAnimation:group forKey:nil];

```
