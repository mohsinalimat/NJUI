# `TableViewDelegate`
 - 要遵守`UITableViewDelegate`协议, 这个协议继承自`UIScrollViewDelegate`
    - 所以可以使用scrollView的代理-所有监听方法

- 1>要设置代理, 代理要遵守`UITableViewDelegate`

# `代码展示`

![](../LibrarypPictures/Snip20160518_3.png)

- 2>几种要记住的代理方法,以及方法的注意点

# `代码展示`
- 如果不设置header的高度,只能显示头部文字,不能显示头部添加的View
- 不论有没有设置header的高度,只要设置了头部的View,头部文字都不能显示

![](../LibrarypPictures/Snip20160518_5.png)


- `- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    return [UIButton buttonWithType:UIButtonTypeContactAdd];
}`

![](../LibrarypPictures/Snip20160518_6.png)



