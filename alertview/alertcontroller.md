# AlertController

- 文本输入, 和获取文本输入,并监听按钮的点击
    - **`刷新tableView的数据`**

```objc

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{
    //获取到被点击的Cell所对应的英雄模型
    CZHeroModel *selectedHero = self.heroList[indexPath.row];

    //创建一个弹窗
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"提示"
    message:@"请输入新的英雄名称" preferredStyle:UIAlertControllerStyleAlert];

    //给弹窗添加一个文本输入框,并设置文本输入框里默认显示的内容
    [alertController addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
        //设置文本输入框里显示旧的英雄名称, 占位字符
        textField.placeholder = selectedHero.name;
    }];

    //创建一个取消按钮
    UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleDestructive handler:nil];

    //创建一个确定按钮,并给这个按钮添加点击事件
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"确认" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        //获取弹窗里所有的文本输入框
        UITextField *nameField = alertController.textFields.firstObject;

        //获取弹窗里文本输入框的内容,并设置给被选中的英雄模型
        selectedHero.name = nameField.text;

        //更新tableView
//        [self.tableView reloadData]; //更新所有的tableView
        //更新指定行的tableView
        [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationRight];
    }];

    //把确认和取消按钮添加到弹窗上
    [alertController addAction:cancelAction];
    [alertController addAction:okAction];

    //弹出弹窗
    [self presentViewController:alertController animated:YES completion:nil];

}

```

- typedef NS_ENUM(NSInteger, UIAlertControllerStyle)

    - UIAlertControllerStyleActionSheet = 0,<br>
    ![](../LibrarypPictures/Snip20160520_2.png)<br>

    - UIAlertControllerStyleAlert<br>
    ![](../LibrarypPictures/Snip20160520_1.png)<br>
 NS_ENUM_AVAILABLE_IOS(8_0)
