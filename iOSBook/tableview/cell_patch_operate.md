# cell patch operate

### 方法一:

- 1.在模型里边增加一个打钩属性`BOOL`
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160523_4.png)

- 2.在自定义cell-xib里边曾加一个隐藏的打钩控件
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160523_1.png)


- 3.在自定义cell的`.m`中setter方法中根据传进来的模型确认控件的隐藏与否
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160523_2.png)

- 4.在控制器中`-didSelecteRow`方法中设置模型的隐藏属性
![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160523_3.png)


### 方法二:苹果自带的:

- 1.设置tableView的编辑模式属性

- 2.是否进入编辑模式

- 3.删除选中的cell对应的模型,并刷新数据

![](file:///Users/apple/Desktop/Library/LibrarypPictures/Snip20160523_5.png)
