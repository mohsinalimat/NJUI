# preference

- 在沙盒下的`preference`文件夹,下的`plist`文件储存

- 偏好设置

```objc

// 偏好设置存储:
// 好处：1.不需要关心文件名
// 2.快速做键值对存储

// 底层：就是封装了一个字典
// account:xmg pwd:123 rmbPwd:YES
- (IBAction)save:(id)sender {

    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];

    [userDefaults setObject:@"xmg" forKey:@"account"];
    [userDefaults setObject:@"123" forKey:@"pwd"];
    [userDefaults setBool:YES forKey:@"rmbPwd"];
    // 在iOS7之前，默认不会马上把跟硬盘同步
    // 同步
    [userDefaults synchronize];
}
- (IBAction)read:(id)sender {
   NSString *pwd = [[NSUserDefaults standardUserDefaults] objectForKey:@"pwd"];

    NSLog(@"%@",pwd);
}

```
