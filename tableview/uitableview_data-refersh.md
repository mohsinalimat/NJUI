# `UITableView Data-refersh 数据刷新`

```objc

- (void)loadMoreData{
    //操作tableView，更新tableView的数据
    //1.创建一个数据模型
    CZTgModel *model = [[CZTgModel alloc]init];

    //2.给创建好的数据模型赋值
    model.icon = @"37e4761e6ecf56a2d78685df7157f097";
    model.title = @"真好吃的手抓火锅";
    model.price = @"521";
    model.buyCount = @"2";

    //把创建好的模型添加到数据源数组里。
    [self.tgList addObject:model];

    //获取到最后一行的indexPath
    NSIndexPath *lastIndexPath = [NSIndexPath indexPathForRow:self.tgList.count - 1 inSection:0];

    //1.更新tableView的所有数据
    //    [self.tableView reloadData];

    //2. 如果对数据源数组的个数进行了修改，这个方法不能再调用,
    // 如果仅仅是对cell数据进行了更新, 调用这个方法
//    [self.tableView reloadRowsAtIndexPaths:@[lastIndexPath] withRowAnimation:UITableViewRowAnimationLeft];

    //3. 当向数据源数组里插入一条数据的时候(数据源数组的个数发生了变化)，需要调用这个方法对插入的数据进行更新
    [self.tableView insertRowsAtIndexPaths:@[lastIndexPath] withRowAnimation:UITableViewRowAnimationLeft];

    //把最后一行cell滚动到tableView的最上面显示
    [self.tableView scrollToRowAtIndexPath:lastIndexPath atScrollPosition:UITableViewScrollPositionTop animated:YES];
}

```
