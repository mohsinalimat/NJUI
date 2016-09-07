# Update Data ` 更改 插入 删除 `

- 重新刷新屏幕上的所有cell<br>
```objc
[self.tableView reloadData];
```
- 更改特定行的cell<br>
```objc
[self.tableView reloadRowsAtIndexPaths:@[
        [NSIndexPath indexPathForRow:0 inSection:0],
        [NSIndexPath indexPathForRow:1 inSection:0]
        ]
        withRowAnimation:UITableViewRowAnimationLeft];
```
- 插入特定行数的cell<br>
```objc
[self.tableView insertRowsAtIndexPaths:@[
        [NSIndexPath indexPathForRow:0 inSection:0],
        [NSIndexPath indexPathForRow:1 inSection:0]
        ]
        withRowAnimation:UITableViewRowAnimationLeft];
```
- 删除特定行数的cell<br>
```objc
[self.tableView deleteRowsAtIndexPaths:@[
        [NSIndexPath indexPathForRow:0 inSection:0],
        [NSIndexPath indexPathForRow:1 inSection:0]
        ]
        withRowAnimation:UITableViewRowAnimationLeft];
```

- `withRowAnimation`

```objc
typedef NS_ENUM(NSInteger, UITableViewRowAnimation) {
    UITableViewRowAnimationFade,
    UITableViewRowAnimationRight,           // slide in from right (or out to right)
    UITableViewRowAnimationLeft,
    UITableViewRowAnimationTop,
    UITableViewRowAnimationBottom,
    UITableViewRowAnimationNone,            // available in iOS 3.0
    UITableViewRowAnimationMiddle,          // available in iOS 3.2.  attempts to keep cell centered in the space it will/did occupy
    UITableViewRowAnimationAutomatic = 100  // available in iOS 5.0.  chooses an appropriate animation style for you
};
```

## `数据刷新的原则`
- 通过修改模型数据，来修改tableView的展示
    - 先修改模型数据
    - 再调用数据刷新方法
- **不要直接修改cell上面子控件的属性**

