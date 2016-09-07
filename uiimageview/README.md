# UIImageView

````objc


//    UIImageView *imageView = [[UIImageView alloc] init];
//    imageView.image = [UIImage imageNamed:@"minion"];
//    imageView.frame = CGRectMake(0, 0, imageView.image.size.width, imageView.image.size.height);


// 等价的


    UIImageView *imageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"minion"]];
    [self.scrollView addSubview:imageView];

    // 设置内容大小
    self.scrollView.contentSize = imageView.image.size;

````


- 头像切圆

```objc

 self.iconImageView.layer.cornerRadius = self.iconImageView.frame.size.width/2;

 self.iconImageView.layer.masksToBounds = YES;

```
