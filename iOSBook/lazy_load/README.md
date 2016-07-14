# lazy load

- 重写get方法
- 先把词典数组放到一个临时数组
- 再设置一个可变数组,用来存储模型
- 循环添加可变数组
- 最后把可变数组给到属性数组赋值

# `controller` 中的属性设置

````objc

- (NSArray *)shops
{
    if(_shops == nil)
    {
        // 设置所有商品的数组
        NSArray *arr = [NSArray arrayWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"shops" ofType:@"plist"]];

        NSMutableArray *arrM = [NSMutableArray array];
        for(NSDictionary *dict in arr)
        {
            LMJShop *shop = [LMJShop shopWithDictionary:dict];
            [arrM addObject:shop];
        }
        _shops = arrM.copy;
    }
    return _shops;
}

````
