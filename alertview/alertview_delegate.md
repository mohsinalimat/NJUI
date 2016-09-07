# AlertView delegate

- 文本输入, 和获取文本输入,并监听按钮的点击
    - **`刷新tableView的数据`**

```objc

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    #pragma clang diagnostic ignored "-Wdeprecated-declarations"
    //1弹出一个弹窗,并指定控制器为弹窗的代理
    UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"请输入英雄姓名"
            delegate:self cancelButtonTitle:@"取消" otherButtonTitles:@"确认", nil];

    //设置弹框的样式,弹窗里有一个文本输入框
    alertView.alertViewStyle = UIAlertViewStylePlainTextInput;

    //根据被点击的行号获取到数据模型
    CZHeroModel *selectedHero = self.heroList[indexPath.row];

    //获取文本输入框
    [alertView textFieldAtIndex:0].text = selectedHero.name;
    [alertView show];
#pragma clang diagnostic pop
}
#pragma mark -alertView的代理方法
- (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex{
    NSLog(@"%zd",buttonIndex);
    //说明用户点击了确定按钮
    if (buttonIndex == 1) {
        //1.把文本输入框里的英雄的新名字拿出来
        NSString *newName =[alertView textFieldAtIndex:0].text;

        //2.获取到被点击cell的indexPath
       NSIndexPath *selectedIndexPath = [self.tableView indexPathForSelectedRow];

        //对tableviewcell的所有修改,全部都是对数据源的修改(CRUD  create read update delete)
        //获取到被点击的数据模型
        CZHeroModel *selectedHero = self.heroList[selectedIndexPath.row];
        //修改数据模型里的属性
        selectedHero.name = newName;

        //更新tableView的所有数据
//        [self.tableView reloadData];

        //更新tableView指定的行
        [self.tableView reloadRowsAtIndexPaths:@[selectedIndexPath]
        withRowAnimation:UITableViewRowAnimationRight];
    }
}

```


- 更新`tableView的数据`


```objc

 //对tableviewcell的所有修改,全部都是对数据源的修改(CRUD  create read update delete)
        //获取到被点击的数据模型
        CZHeroModel *selectedHero = self.heroList[selectedIndexPath.row];
        //修改数据模型里的属性
        selectedHero.name = newName;

        //更新tableView的所有数据
//        [self.tableView reloadData];

        //更新tableView指定的行
        [self.tableView reloadRowsAtIndexPaths:@[selectedIndexPath]
        withRowAnimation:UITableViewRowAnimationRight];

```
