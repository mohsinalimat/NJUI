## 怎么样获取一个lebel的`最大尺寸frame`

```objc

    //设置用户名的frame
    CGFloat nameX = CGRectGetMaxX(_iconFrame) + kSpace;
    CGFloat nameHeight = 20;
    CGFloat nameY = (iconHeight - nameHeight) / 2 + kSpace;

    //用户名的长度根据用户名来进行判断
    NSString *usrName = model.name;

    //指定计算字符串大小时的字体
    NSDictionary *attr = @{NSFontAttributeName : [UIFont systemFontOfSize:14]};

    //CGSize 代表字符串的最大范围
    //NSStringDrawingUsesLineFragmentOrigin 根据字符串的最大位置和字体来计算字符串实际大小
    //attr 指定计算时字体的大小
    CGFloat nameWidth = [usrName boundingRectWithSize:CGSizeMake(CGFLOAT_MAX, nameHeight)
                         options:NSStringDrawingUsesLineFragmentOrigin attributes:attr
                         context:nil].size.width;


                         - (CGRect)boundingRectWithSize:(CGSize)size
                         options:(NSStringDrawingOptions)options
                         attributes:(nullable NSDictionary<NSString *, id> *)attributes
                         context:(nullableNSStringDrawingContext *)context

    _nameFrame = CGRectMake(nameX, nameY, nameWidth, nameHeight);

```



