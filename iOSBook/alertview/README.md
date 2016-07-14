# `AlertView` and `UIAlertController`

# `弹出提示窗的方法`

 - 第一种,已经过时了

    ![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160516_12.png)

    ```objc

            //弹出提示框,告诉用户已经到最后一题了!!!
        // 弹窗
        /*
         'UIAlertView' is deprecated: first deprecated in iOS 9.0 - UIAlertView is deprecated.
         Use UIAlertController with a preferredStyle of UIAlertControllerStyleAlert instead(替代)

         UIAlertView 在 iOS9.0 的时候被废弃了.使用UIAlertController,并且指定它的类型为 UIAlertControllerStyleAlert 来替代 UIAlertView.
         */

       // 1.第1种方法:从中间弹出一个弹窗(过时)
        UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:@"提示" message:@"恭喜你,已经通关了." delegate:nil cancelButtonTitle:@"取消" otherButtonTitles:@"确定", nil];

       // 把弹窗显示出来
        [alertView show];

    ```


- 第二种
    - ![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160516_11.png)
    - 注意调用弹窗显示的时候是用控制器去调用的


    ```obj-c
            //调用控制器的 presentViewController方法弹出一个提示框.
        [self presentViewController:alertController animated:YES completion:nil];

    ```

---

    ```obj-c
            //2.弹窗的第二种方法
        //创建一个alertController
        // UIAlertControllerStyleAlert 从中间弹出一个弹窗
        // UIAlertControllerStyleActionSheet 从下面弹窗

        UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"提示" message:@"恭喜你,已经通关了." preferredStyle:``UIAlertControllerStyleActionSheet``];

        //创建一个AlertAction对象
        UIAlertAction *actionOK = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDefault handler:nil];
        // 创建第二个AlertAction对象
        UIAlertAction *actionCancel = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleDestructive handler:nil];

        //把创建好的alertAction添加到alertController里
        [alertController addAction:actionOK];
        [alertController addAction:actionCancel];

        //调用控制器的 presentViewController方法弹出一个提示框.
        [self presentViewController:alertController animated:YES completion:nil];

    ```

- 第三种弹窗(过时的)
    - ![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160516_10.png)

```objc
        UIActionSheet *actionSheet = [[UIActionSheet alloc] initWithTitle:@"提示" delegate:nil cancelButtonTitle:nil destructiveButtonTitle:@"取消" otherButtonTitles:@"确认", nil];
        [actionSheet showInView:self.view];

```




